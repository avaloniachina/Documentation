# 样式

Avalonia中的样式用于在控件之间共享属性设置。Avalonia的样式系统可以被看作是CSS样式和WPF/UWP样式的混合。最基本的样式由选择器(selector)和设置器(setters)的集合组成。

样式适用于它定义的控件以及所有的后代控件。

以下样式选择带有`h1`样式类(style class)的任何`TextBlock`，并将其字体大小设置为24磅，字体加粗。

```markup
<Style Selector="TextBlock.h1">
    <Setter Property="FontSize" Value="24"/>
    <Setter Property="FontWeight" Value="Bold"/>
</Style>
```

通过将样式添加到[`Control.Styles`](http://reference.avaloniaui.net/api/Avalonia/StyledElement/0A46A84A)或[`Application.Styles`](http://reference.avaloniaui.net/api/Avalonia/Application/04017CAF)集合，可以在任何控件或`Application`对象上定义样式。

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

还可以使用`StyleInclude`类从其他文件中引用样式，例如：

```markup
<Window xmlns="https://github.com/avaloniaui"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
    <Window.Styles>
        <StyleInclude Source="/CustomStyles.xaml" />
    </Window.Styles>

    <TextBlock Classes="h1">I'm a Heading!</TextBlock>
</Window>
```

其中`CustomStyles.xaml`是一个XAML文件，它的根为`Style`或`Styles`，并作为[asset](../getting-started/assets.md)引用在应用程序中，例如：

CustomStyles.xaml

```markup
<Styles xmlns="https://github.com/avaloniaui"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
    <Style Selector="TextBlock.h1">
        ...
    </Style>
</Styles>
```

请注意，与WPF/UWP不同，如果将样式添加到控件或应用程序`ResourceDictionary`中，则样式将不起作用。这是因为在Avalonia中定义样式的顺序很重要，而`ResourceDictionary`是一个无序字典。

### 样式类(Style Class) <a id="style-classes"></a>

与CSS一样，控件可以被赋予**样式类**，这些类可以在选择器中使用。通过将`Classes`属性设置为以空格分隔的字符串列表，可以在XAML中分配样式类。以下示例将`h1`和`blue`样式类作用于`Button`：

```markup
<Button Classes="h1 blue"/>
```

如果需要按条件添加或删除类，可以使用以下特殊语法：
```markup
<Button Classes.accent="{Binding IsSpecial}" />
```

还可以使用`Classes`集合在代码中控制样式类：

```csharp
control.Classes.Add("blue");
control.Classes.Remove("red");
```

### 伪类(Pseudoclasses) <a id="pseudoclasses"></a>

与CSS一样，控件也可以有伪类。这些是由控件本身而不是用户定义的类。伪类以`:`字符开头。

一个伪类的例子，`:pointerover`（类似于CSS中的`:hover`）。

伪类提供WPF中的`Triggers`和UWP中的`VisualStateManager`功能：

```markup
<StackPanel>
  <StackPanel.Styles>
    <Style Selector="Border:pointerover">
      <Setter Property="Background" Value="Red"/>
    </Style>
  </StackPanel.Styles>
  <Border>
    <TextBlock>I will have red background when hovered.</TextBlock>
  </Border>
</StackPanel>
```

另一个涉及更改控件[模板](selectors.md#template)内部属性的示例：

```markup
<StackPanel>
  <StackPanel.Styles>
    <Style Selector="Button:pressed /template/ ContentPresenter">
        <Setter Property="TextBlock.Foreground" Value="Red"/>
    </Style>
  </StackPanel.Styles>
  <Button>I will have red text when pressed.</Button>
</StackPanel>
```

其他伪类包括`:focus`、`:disabled`、按钮的`:pressed`、复选框的`:checked`等。

### 自定义伪类(Custom PseudoClasses)  <a id="custom-pseudoclasses"></a>

您可以为`CustomControl`或`TemplatedControl`创建自己的伪类。
下面的函数根据`StyledElement`上的布尔值添加或删除伪类。
```csharp
PseudoClasses.Set(":className", bool);
```
**记住：** 伪类总是以`:`开头 !

### 选择器(Selectors) <a id="selectors"></a>

**选择器**使用自定义选择器语法选取控件，该语法与CSS选择器使用的语法非常相似。一些选择器的示例：

| 选择器                                  | 描述                                         |
|:-------------------------------------|:-------------------------------------------|
| `Button`                             | 选取所有`Button`控件                             |
| `Button.red`                         | 选取所有带有`red`样式类的`Button`控件                  |
| `Button.red.large`                   | 选取所有带有`red`和`large`样式类的`Button`控件          |
| `Button:focus`                       | 选取所有带有`:focus`伪类的`Button`控件                |
| `Button.red:focus`                   | 选取所有带有`red`样式类和`:focus`伪类的`Button`控件       |
| `Button#myButton`                    | 选取一个命名为`myButton`的`Button`控件               |
| `StackPanel Button.foo`              | 选取在`StackPanel`的后代中带有`foo`类的所有`Button`控件   |
| `StackPanel > Button.foo`            | 选取在`StackPanel`的直接子代中带有`foo`类的所有`Button`控件 |
| `Button /template/ ContentPresenter` | 选取Button的模板内所有ContentPresenter控件           |

想要了解更多信息，请浏览[选择器文档](selectors.md).

### 设置器(Setters) <a id="setters"></a>

样式的设置器描述了当选择器与控件匹配时会发生什么。它们是简单的属性-值对，格式如下：

```markup
<Setter Property="FontSize" Value="24"/>
<Setter Property="Padding" Value="4 2 0 4"/>
```

您还可以使用复杂元素语法来声明更复杂的对象值：

```markup
<Setter Property="MyProperty">
   <MyObject Property1="My Value"/>
</Setter>
```

绑定也可以应用在设置器(Setter)上，可以绑定到目标控件的`DataContext`：

```markup
<Setter Property="FontSize" Value="{Binding SelectedFontSize}"/>
```

每当样式与控件匹配时，所有设置器都将应用于该控件。如果样式选择器导致样式不再与控件匹配，则该属性值将恢复为其下一个最高优先级的值。

注意：`Setter`创建了一个`Value`的单一实例，它将被应用于样式匹配的所有控件：如果对象是可变的，那么变化将反映在所有控件上。继而，在设置器`Value`内的任何对象的绑定将不能访问目标控件的`DataContext`，因为可能有多个目标控件。

```markup
<Style Selector="local|MyControl">
  <Setter Property="MyProperty">
     <MyObject Property1="{Binding MyViewModelProperty}"/>
  </Setter>
</Style>
```

在上面的例子中，绑定源是`MyObject.DataContext`而不是`MyControl.DataContext`，如果`MyObject`没有数据上下文，那么绑定将不能产生值。

注意：在这种情况下，如果您正在使用已编译的绑定，则需要在`<Style>`元素中显式设置绑定源的数据类型：

```markup
<Style Selector="MyControl" x:DataType="MyViewModelClass">
  <Setter Property="ControlProperty" Value="{Binding MyViewModelProperty}" />
</Style>
```

### 设置器中的模板 <a id="templates-in-setters"></a>

如上所述，通常会创建设置器的`Value`的单个实例，并在所有匹配控件之间共享。因此，要将控件作为设置器的值，必须将控件包装在`<Template>`中：

```markup
<Style Selector="Border.empty">
  <Setter Property="Child">
    <Template>
      <TextBlock>No content available.</TextBlock>
    </Template>
  </Setter>
</Style>
```

### 样式优先级 <a id="style-precedence"></a>

如果多个样式与一个控件匹配，并且它们都试图设置相同的属性，则该控件优先使用**最近**的样式。如下示例：

```markup
<Window xmlns="https://github.com/avaloniaui"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
    <Window.Styles>
        <Style Selector="TextBlock.h1">
            <Setter Property="FontSize" Value="24"/>
            <Setter Property="FontWeight" Value="Bold"/>
        </Style>
    </Window.Styles>

    <StackPanel>
        <StackPanel.Styles>
            <Style Selector="TextBlock.h1">
                <Setter Property="FontSize" Value="48"/>
                <Setter Property="Foreground" Value="Red"/>
            </Style>
        </StackPanel.Styles>

        <TextBlock Classes="h1">
            <StackPanel.Styles>
                <Style Selector="TextBlock.h1">
                    <Setter Property="Foreground" Value="Blue"/>
                </Style>
            </StackPanel.Styles>

            I'm a Heading!
        </TextBlock>
    </StackPanel>
</Window>
```

这里在多个地方定义了`h1`样式。`TextBlock`最后将使用以下设置：

| 属性           | 值    | 源            |
|:-------------|:-----|:-------------|
| `FontSize`   | 48   | `StackPanel` |
| `FontWeight` | Bold | `Window`     |
| `Foreground` | Blue | `TextBlock`  |

如果一个属性应用了多个样式设置器，则优先级为：

* 在最接近控件的祖先中定义的样式的值
* 对于同一`Styles`集合中声明的两种样式，显示更靠后的样式

