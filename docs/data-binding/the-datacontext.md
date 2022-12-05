# 数据上下文

`Control.DataContext` 属性描述了使用绑定时控件默认从何处取值。数据上下文通常在[`Window`](http://reference.avaloniaui.net/api/Avalonia.Controls/Window)这样的顶级控件中赋值，它的子控件会自动继承数据上下文。

当使用MVVM模式时，数据上下文通常都是视图模型的实例。

如果你使用 [Avalonia MVVM Application](https://docs.avaloniaui.net/tutorials/todo-list-app/creating-a-new-project#net-core-cli) 模板创建应用，那么您将在 `Program.cs` 文件中看到如下代码：

```csharp
private static void AppMain(Application app, string[] args)
{
    var window = new MainWindow
    {
        DataContext = new MainWindowViewModel(),
    };

    app.Run(window);
}
```

这段代码表明，当`MainWindow`创建时，一个新的`MainWindowViewModel`实例将会被创建出来，并赋值给窗体的`DataContext`属性。所有绑定都会默认绑定到这个实例的属性上。

```markup
<Window>
    <Button Content="{Binding ButtonCaption}"/>
</Window>
```

将会把`Button`的`Content`属性绑定到`Window.DataContext.ButtonCaption`上。

## 绑定数据上下文<a id="binding-datacontext"></a>

绑定`DataContext`时, 绑定的源是父级控件的`DataContext`:

```markup
<Window>
    <!-- 会将`DataContext`绑定到`Window.DataContext.Content -->
    <StackPanel DataContext="{Binding Content}"/>
</Window>
```

使用[数据模板](https://docs.avaloniaui.net/docs/templates/data-templates)显示内容的控件会自动将控件的`DataContext`赋值给模板内部的控件。例如对于`ContentControl`：

```markup
<Window>
    <ContentControl DataContext="{Binding Content}">
        <ContentControl.ContentTemplate>
            <DataTemplate>
                <!-- 会将`Text`绑定到`Window.DataContext.Content.Header -->
                <TextBlock Text="{Binding Header}"/>
            </DataTemplate>
        </ContentControl.ContentTemplate>
    </ContentControl>
</Window>
```
