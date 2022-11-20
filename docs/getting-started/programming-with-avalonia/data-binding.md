# 数据绑定

Avalonia 完全支持控件和任意 .NET 对象 [绑定](../../data-binding/bindings.md)。可以在 XAML 或代码中设置数据绑定，并支持下列特性：

* 多种绑定模式：单向绑定、双向绑定、单次绑定和单向更新源
* 绑定 [数据上下文](../../data-binding/the-datacontext.md)
* 绑定 [其它控件](../../data-binding/binding-to-controls.md)
* 绑定 [`Tasks和Observables`](../../data-binding/binding-to-tasks-and-observables.md)
* [绑定转换器](../../data-binding/converting-binding-values.md) 以及绑定值取反

下面的示例展示了一个 `TextBlock` ，通过使用绑定，当它与 `TextBox` 关联时 `TextBox` 被禁用的情况：

```markup
<StackPanel>
    <TextBox Name="input" IsEnabled="False"/>
    <TextBlock IsVisible="{Binding !#input.IsEnabled}">Sorry, no can do!</TextBlock>
</StackPanel>
```

在这个示例中，`TextBlock` 的 `IsVisible` 属性通过 `#input.IsEnabled` 与名字为 `input` 的 `Textbox` 的 `IsEnabled` 属性建立绑定，取反后进行赋值。

