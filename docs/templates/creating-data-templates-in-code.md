# 在代码中创建数据模板

Avalonia also supports creating data templates in code with the [`FuncDataTemplate<T>`](http://reference.avaloniaui.net/api/Avalonia.Controls.Templates/FuncDataTemplate_1/) class.

At its simplest you can create a data template by passing a lambda which accepts an instance to the `FuncDataTemplate<T>` constructor:

```csharp
var template = new FuncDataTemplate<Student>((value, namescope) =>
    new TextBlock
    {
        [!TextBlock.TextProperty] = new Binding("FirstName"),
    });
```

Is equivalent to:

```markup
<DataTemplate DataType="{x:Type local:Student}">
    <TextBlock Text="{Binding FirstName}"/>
</DataTemplate>
```

## Samples

[FuncDataTemplate Sample](https://github.com/AvaloniaUI/Avalonia.Samples/blob/main/src/Avalonia.Samples/DataTemplates/FuncDataTemplateSample)
