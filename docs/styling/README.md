# 🎨 样式

[Avalonia中的样式](styles.md)用于在控件之间共享属性设置。Avalonia的样式系统可以被看作是CSS样式和WPF/UWP样式的混合。最基本的样式由选择器(selector)和设置器(setters)的集合组成。

以下样式选择带有`h1`样式类(style class)的任何`TextBlock`，并将其字体大小设置为24磅，字体加粗。

```markup
<Window xmlns="https://github.com/avaloniaui"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
    <Window.Styles>
        <Style Selector="TextBlock.h1">
            <Setter Property="FontSize" Value="24"/>
            <Setter Property="FontWeight" Value="Bold"/>
        </Style>
    </Window.Styles>

    <TextBlock Classes="h1">I'm a Heading!</TextBlock>
</Window>
```

