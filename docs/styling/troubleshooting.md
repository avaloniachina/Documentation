# 疑难解答

Avalonia样式系统是XAML和CSS样式方法的混合体，因此只了解其中一种技术的开发人员可能会被另一种技术细节所迷惑。

让我们想象一个问题，当一个或所有样式设置器都未应用于控件时，下面我们列出常见的可能原因和解决方案。

### 选择器以不存在的控件为目标

类似CSS选择器，当没有控件可以被选择器匹配时，Avalonia选择器不会引发任何错误或警告。这包括使用不存在的名称或类，或者当没有子选择器可以匹配内部选择器时使用一个子选择器。原因很简单，一种样式可以针对许多控件，这些控件可以在运行时创建或删除，因此不可能验证选择器。

### 目标属性被另一种样式覆盖

样式按声明顺序应用。如果有**多个**样式文件以同一控件属性为目标，则一种样式可以覆盖另一种样式：

{% code title="Styles2.axaml" %}

```markup
<Style Selector="TextBlock.header">
    <Style Property="Foreground" Value="Green" />
</Style>
```

{% endcode %}

{% code title="Styles1.axaml" %}

```markup
<Style Selector="TextBlock.header">
    <Style Property="Foreground" Value="Blue" />
    <Style Property="FontSize" Value="16" />
</Style>
```

{% endcode %}

{% code title="App.axaml" %}

```markup
<StyleInclude Source="Style1.axaml" />
<StyleInclude Source="Style2.axaml" />
```

{% endcode %}

这里首先应用了文件**Styles1.axaml**中的样式，因此优先应用文件**Styles2.axaml**中的样式设置器。生成的TextBlock属性有FontSize="16"和Foreground="Green"。样式文件中也会发生相同的顺序优先级排序。

### 本地设置的属性覆盖了样式

与WPF类似，Avalonia属性可以有多个值，通常具有不同的优先级；

在本例中，您可以看到局部值（直接在控件上定义）的优先级高于样式值，因此文本块显示红色的前景色：

```markup
<TextBlock Classes="header" Foreground="Red" />
...
<Style Selector="TextBlock.header">
    <Setter Property="Foreground" Value="Green" />
</Style>
```

您可以在[BindingPriority](http://reference.avaloniaui.net/api/Avalonia.Data/BindingPriority/)中查看值优先级的完整枚举列表，其中较小的枚举值具有较高的优先级。例如，`Animation`值具有最高优先级，甚至能覆盖本地值。

{% hint style="info" %}
一些默认的Avalonia样式在其模板中使用本地值而不是模板绑定或样式设置器，这使得在不替换整个模板的情况下无法更新模板属性。
{% endhint %}

### 缺少样式伪类（触发器）选择器

让我们设想一种情况，在这种情况下，您可能希望第二种样式覆盖前一种样式，但它不会：

```markup
<Style Selector="Border:pointerover">
    <Setter Property="Background" Value="Blue" />
</Style>
<Style Selector="Border">
    <Setter Property="Background" Value="Red" />
</Style>
...
<Border Width="100" Height="100" Margin="100" />
```

在这个代码示例中，`Border`正常情况下是红色背景，当指针悬停在上方时为蓝色。这是因为与CSS一样，更具体的选择器优先级更高。当您想要用单一样式覆盖任何状态（pointerover、pressed或其他）的默认样式时，这是一个问题。为了实现这一点，您还需要为这些状态创建新的样式；

{% hint style="info" %}
访问Avalonia源代码查找[原始模板](https://github.com/AvaloniaUI/Avalonia/tree/master/src/Avalonia.Themes.Fluent/Controls)。当这种情况发生时，将带有伪类的样式复制并粘贴到代码中。
{% endhint %}

### 带有伪类的选择器不会覆盖默认值

以下代码示例展示了作用在默认样式之上的样式：

```markup
<Style Selector="Button">
    <Setter Property="Background" Value="Red" />
</Style>
<Style Selector="Button:poinverover">
    <Setter Property="Background" Value="Blue" />
</Style>
```

您可能希望默认情况下`Button`为红色，当指针悬停在上方时为蓝色。实际上，只有第一个样式的设置器会被应用，而第二个设置器会被忽略。

原因隐藏在Button的模板中。你可以在Avalonia源代码中找到默认模板（旧的[Default](https://github.com/AvaloniaUI/Avalonia/blob/master/src/Avalonia.Themes.Default/Button.xaml)主题和新的[Fluent](https://github.com/AvaloniaUI/Avalonia/blob/master/src/Avalonia.Themes.Fluent/Controls/Button.xaml)主题），但为了方便，我们在这里简化了Fluent主题中的一个：

```markup
<Style Selector="Button">
    <Setter Property="Background" Value="{DynamicResource ButtonBackground}"/>
    <Setter Property="Template">
        <ControlTemplate>
            <ContentPresenter Name="PART_ContentPresenter"
                              Background="{TemplateBinding Background}"
                              Content="{TemplateBinding Content}"/>
        </ControlTemplate>
    </Setter>
</Style>
<Style Selector="Button:pointerover /template/ ContentPresenter#PART_ContentPresenter">
    <Setter Property="Background" Value="{DynamicResource ButtonBackgroundPointerOver}" />
</Style>
```

实际的背景是由`ContentPresenter`渲染的，在默认情况下，它与Button的 `Background`属性绑定。然而，在pointer-over状态下，选择器直接将背景应用于`ContentPresenter (Button:pointerover /template/ ContentPresenter#PART_ContentPresenter)`。这就是为什么在前面的代码示例中，第二个设置器被忽略了。更正后的代码也应该直接针对内容展示。

```markup
<!-- 这里的#PART_ContentPresenter名称选择器是不必要的，但被添加成更具体的样式。 -->
<Style Selector="Button:pointerover /template/ ContentPresenter#PART_ContentPresenter">
    <Setter Property="Background" Value="Blue" />
</Style>
```

{% hint style="info" %}
不仅仅是Button，你可以在默认主题中的所有控件（包括旧的Default和新的Fluent）看到这种行为。而且不仅仅是Background，还有其他与状态相关的属性也有这种行为。
{% endhint %}

{% hint style="info" %}
为什么默认样式直接改变ContentPresenter的`Background`属性，而不是改变`Button.Background`属性？

这是因为如果用户在按钮上设置一个本地值，就会覆盖所有的样式，并使按钮永远是同一个颜色。更多细节请看这个[reverted PR](https://github.com/AvaloniaUI/Avalonia/pull/2662#issuecomment-515764732)。
{% endhint %}

### 样式不再应用时，不会恢复特定属性的历史值

在Avalonia中有多种类型的属性，其中之一，直接属性(Direct Property)，完全不支持样式。这些属性以简化的方式工作，以实现更低的开销和更高的性能，并且不根据优先级存储多个值。它只保存最新的值，并且不能恢复。你可以在[这里](../authoring-controls/defining-properties.md#direct-avaloniaproperties)找到更多关于属性的详细信息。

典型的例子是[CommandProperty](http://reference.avaloniaui.net/api/Avalonia.Controls/Button/B9689B29)。它被定义为一个直接属性(DirectProperty)，它将永远无法正常工作。
在未来，尝试设置直接属性的样式将导致编译错误，详情请看[#6837](https://github.com/AvaloniaUI/Avalonia/issues/6837)。
