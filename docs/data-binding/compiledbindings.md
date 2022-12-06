# 编译绑定

在XAML中定义的绑定是通过反射来实现寻找和访问视图模型中的属性的。在Avalonia中你还可以使用编译绑定。这样有一些好处：
* 如果你使用编译绑定但没有找到你想要绑定的属性，会得到编译期错误，得到更好的调试体验。
* 众所周知，反射很慢（[可以阅读codeproject.com的这篇文章](https://www.codeproject.com/Articles/1161127/Why-is-reflection-slow)）。使用编译绑定可以提升应用的性能。

## 开启或禁用编译绑定

编译绑定默认不开启。想要开启编译绑定，你需要先通过`DataType`定义想要绑定的目标的类型。在[`DataTemplates数据模板`](https://docs.avaloniaui.net/misc/wpf/datatemplates)中有`DataType`属性，在其他元素中可以使用`x:DataType`。通常你会在根节点设置`x:DataType`，例如`Window`和`UserControl`。从Avalonia`0.10.12`开始，你还可以在`Binding`中直接指定`DataType`。

现在你可以开启或禁用编译绑定，方法是设置`x:CompileBindings="[True|False]"`。所有的子节点都会继承这一属性，需要的时候也可以为全局开启，为特定的子节点禁用。

```markup
<!--设置DataType并开启全局绑定 -->
<UserControl xmlns="https://github.com/avaloniaui"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:vm="using:MyApp.ViewModels"
             x:DataType="vm:MyViewModel"
             x:CompileBindings="True">
    <StackPanel>
        <TextBlock Text="Last name:" />
        <TextBox Text="{Binding LastName}" />
        <TextBlock Text="Given name:" />
        <TextBox Text="{Binding GivenName}" />
        <TextBlock Text="E-Mail:" />
        <!-- 在绑定标记中设置DataType -->
        <TextBox Text="{Binding MailAddress, DataType={x:Type vm:MyViewModel}}" />

        <!-- 我们无法在编译绑定中绑定到方法，所以对于按钮需要做出让步 -->
        <Button x:CompileBindings="False"
                Content="Send an E-Mail"
                Command="{Binding SendEmailCommand}" />
    </StackPanel>
</UserControl>
```

## CompiledBinding标记

如果你不想对所有子节点开启编译绑定，也可以使用`CompiledBinding`标记。你仍然需要设置`DataType`，但可以省略`x:CompileBindings="True"`。

```markup
<!-- Set DataType -->
<UserControl xmlns="https://github.com/avaloniaui"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:vm="using:MyApp.ViewModels"
             x:DataType="vm:MyViewModel">
    <StackPanel>
        <TextBlock Text="Last name:" />
        <!-- 使用CompiledBinding标记进行绑定 -->
        <TextBox Text="{CompiledBinding LastName}" />
        <TextBlock Text="Given name:" />
        <TextBox Text="{CompiledBinding GivenName}" />
        <TextBlock Text="E-Mail:" />
        <TextBox Text="{CompiledBinding MailAddress}" />

        <!--  我们无法在编译绑定中绑定到方法，所以使用正常的绑定（Binding） -->
        <Button Content="Send an E-Mail"
                Command="{Binding SendEmailCommand}" />
    </StackPanel>
</UserControl>
```

## ReflectionBinding标记

如果你在根节点开启了编译绑定（通过`x:CompileBindings="True"`），但对于特定的位置并不想用编译绑定，或者遇到了编译绑定的[局限](#known-limitations)，你可以使用`ReflectionBinding`标记。


```markup
<!-- 设置 DataType -->
<UserControl xmlns="https://github.com/avaloniaui"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:vm="using:MyApp.ViewModels"
             x:DataType="vm:MyViewModel"
             x:CompileBindings="True">
    <StackPanel>
        <TextBlock Text="Last name:" />
        <TextBox Text="{Binding LastName}" />
        <TextBlock Text="Given name:" />
        <TextBox Text="{Binding GivenName}" />
        <TextBlock Text="E-Mail:" />
        <TextBox Text="{Binding MailAddress}" />

        <!-- 我们无法在编译绑定中绑定到方法，所以使用ReflectionBinding -->
        <Button Content="Send an E-Mail"
                Command="{ReflectionBinding SendEmailCommand}" />
    </StackPanel>
</UserControl>
```

## 已知限制

编译绑定有一些已知的限制：

* 编译绑定无法绑定到通过名字指定的元素
* 编译绑定无法在样式中通过RelativeResource绑定到TemplatedParent（例如：{Binding Width, RelativeSource={RelativeSource TemplatedParent}}）

如果你遇到了上述的限制，你需要禁用编译绑定或使用`ReflectionBinding`。
