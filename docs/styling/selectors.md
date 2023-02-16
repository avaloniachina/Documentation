# 选择器

## OfType <a id="oftype"></a>

{% tabs %}
{% tab title="XAML" %}

```markup
<Style Selector="Button">
<Style Selector="local|Button">
```

{% endtab %}

{% tab title="C\#" %}

```csharp
new Style(x => x.OfType<Button>());
new Style(x => x.OfType(typeof(Button)));
```

{% endtab %}
{% endtabs %}

按类型选择控件。上面的第一个示例选择`Avalonia.Controls.Button`类。若要在类型中引用XAML命名空间，请使用`|`字符分隔命名空间和类型。

此选择器不会匹配派生类。想要实现派生类匹配请使用[`Is`](selectors.md#is)选择器。

{% hint style="info" %}
注意：对象的类型实际上是通过查看其`IStyleable.StyleKey`属性来确定的。默认情况下，这只是返回当前实例的类型，但如果例如您确实希望从`Button`继承的控件样式为`Button`，则可以在类上实现`IStyleable.StyleKey`属性以返回`typeof(Button)`。
{% endhint %}

## Name <a id="name"></a>

{% tabs %}
{% tab title="XAML" %}

```markup
<Style Selector="#myButton">
<Style Selector="Button#myButton">
```

{% endtab %}

{% tab title="C\#" %}

```csharp
new Style(x => x.Name("myButton"));
new Style(x => x.OfType<Button>().Name("myButton"));
```

{% endtab %}
{% endtabs %}

使用`#`字符按控件的[`Name`](http://reference.avaloniaui.net/api/Avalonia/StyledElement/2362746E)属性选择一个控件。

## Class <a id="class"></a>

{% tabs %}
{% tab title="XAML" %}

```markup
<Style Selector="Button.large">
<Style Selector="Button.large:focus">
```

{% endtab %}

{% tab title="C\#" %}

```csharp
new Style(x => x.OfType<Button>().Class("large"));
new Style(x => x.OfType<Button>().Class("large").Class(":focus"));
```

{% endtab %}
{% endtabs %}

选择一个具有指定样式类的控件。多个类用`.`字符分开，如果是伪类则用`:`字符。如果指定了多个样式类，那么控件必须有所有必要的样式类才能够匹配。

## Is <a id="is"></a>

{% tabs %}
{% tab title="XAML" %}

```markup
<Style Selector=":is(Button)">
<Style Selector=":is(local|Button)">
```

{% endtab %}

{% tab title="C\#" %}

```csharp
new Style(x => x.Is<Button>());
new Style(x => x.Is(typeof(Button)));
```

{% endtab %}
{% endtabs %}

这与[`OfType`](selectors.md#ofType)选择器非常相似，只是它也匹配派生类型。

{% hint style="info" %}
同样，对象的类型实际上是通过查看其`IStyleable.StyleKey`属性来确定的。
{% endhint %}

## Child <a id="child"></a>

{% tabs %}
{% tab title="XAML" %}

```markup
<Style Selector="StackPanel > Button">
```

{% endtab %}

{% tab title="C\#" %}

```csharp
new Style(x => x.OfType<StackPanel>().Child().OfType<Button>());
```

{% endtab %}
{% endtabs %}

Child选择器是通过用`>`字符分隔两个选择器来定义的。此选择器匹配逻辑树中的直接子元素，因此在上面的例子中，选择器将匹配作为`StackPanel`的直接逻辑子元素的任何`Button`。

## Descendant <a id="descendant"></a>

{% tabs %}
{% tab title="XAML" %}

```markup
<Style Selector="StackPanel Button">
```

{% endtab %}

{% tab title="C\#" %}

```csharp
new Style(x => x.OfType<StackPanel>().Descendant().OfType<Button>());
```

{% endtab %}
{% endtabs %}

当两个选择器用空格分隔时，选择器将匹配逻辑树中的后代(Descendant)，因此在这种情况下，选择器将与作为`StackPanel`逻辑后代的任何`Button`匹配。

## PropertyEquals <a id="propertyequals"></a>

{% tabs %}
{% tab title="XAML" %}

```markup
<Style Selector="Button[IsDefault=true]">
```

{% endtab %}

{% tab title="C\#" %}

```csharp
new Style(x => x.OfType<Button>().PropertyEquals(Button.IsDefaultProperty, true));
```

{% endtab %}
{% endtabs %}

匹配将某个具体属性设置为某个具体值的所有控件。

{% hint style="info" %}
**注意：** 在XAML内部的选择器中使用`AttachedProperty`时，必须用小括号包装。

```markup
<Style Selector="TextBlock[(Grid.Row)=0]">
```

{% endhint %}

{% hint style="info" %}
**注意：** 在XAML中使用选择器时，属性必须支持[`TypeConverter`](https://learn.microsoft.com/dotnet/api/system.componentmodel.typeconverter)
{% endhint %}

## Template <a id="template"></a>

{% tabs %}
{% tab title="XAML" %}

```markup
<Style Selector="Button /template/ ContentPresenter">
```

{% endtab %}

{% tab title="C\#" %}

```csharp
new Style(x => x.OfType<Button>().Template().OfType<ContentPresenter>());
```

{% endtab %}
{% endtabs %}

匹配控件模板中的控件。这里列出的所有其他选择器都作用在逻辑树上。如果要在控件模板中选择控件，则必须使用此选择器。该示例选择`Button`模板中的`ContentPresenter`控件。

## Not <a id="not"></a>

{% tabs %}
{% tab title="XAML" %}

```markup
<Style Selector="TextBlock:not(.h1)">
```

{% endtab %}

{% tab title="C\#" %}

```csharp
new Style(x => x.OfType<TextBlock>().Not(y => y.Class("h1")));
```

{% endtab %}
{% endtabs %}

取反内部选择器。

## Or <a id="or"></a>

{% tabs %}
{% tab title="XAML" %}

```markup
<Style Selector="TextBlock, Button">
```

{% endtab %}

{% tab title="C\#" %}

```csharp
new Style(x => Selectors.Or(x.OfType<TextBlock>(), x.OfType<Button>()))
```

{% endtab %}
{% endtabs %}

查找任意一个与这些选择器中匹配的元素。每个选择器用`,`分开。

## Nth Child <a id="nth-child"></a>

{% tabs %}
{% tab title="XAML" %}

```markup
<Style Selector="TextBlock:nth-child(2n+3)">
```

{% endtab %}

{% tab title="C\#" %}

```csharp
new Style(x => x.OfType<TextBlock>().NthChild(2, 3));
```

{% endtab %}
{% endtabs %}

根据元素在一组邻近元素中的位置来匹配。

## Nth Last Child <a id="nth-last-child"></a>

{% tabs %}
{% tab title="XAML" %}

```markup
<Style Selector="TextBlock:nth-last-child(2n+3)">
```

{% endtab %}

{% tab title="C\#" %}

```csharp
new Style(x => x.OfType<TextBlock>().NthLastChild(2, 3));
```

{% endtab %}
{% endtabs %}

根据元素在一组邻近元素中的位置来匹配，从末端开始计算。

## Nth Child and Nth Last Child Syntax

`:nth-child()`和`:nth-last-child()`需要一个参数用以描述一个模式，这个模式用于匹配邻近列表中的元素索引。元素索引数是**从1开始**的。下面的例子使用`:nth-child()`，但这条规则也适用于`:nth-last-child()`。

### 关键字符号

`odd`代表在一系列邻近元素中索引为奇数的元素：1、3、5...。

`even`代表在一系列邻近元素中索引为偶数的元素：2、4、6...。

### 功能符号

`An+B`代表一个列表中的元素，其索引数与An+B定义的自定义数字模式中找到的索引一致，其中：

* `A`是整数，代表步长(step size)。
* `B`是整数，代表偏移量(offset)。
* `n`是所有从0开始的非负整数。

这可以这样理解： _从`B`开始的每第`A`个元素_。

### 功能符号示例

| 示例                 | 代表                                                                                                                 |
|:-------------------|:-------------------------------------------------------------------------------------------------------------------|
| `:nth-child(odd)`  | 奇数元素：**1**、**3**、**5**...                                                                                          |
| `:nth-child(even)` | 偶数元素: **2**、**4**、**6**...                                                                                         |
| `:nth-child(2n+1)` | 奇数元素：**1**_(2×0+1)_、**3**_(2×1+1)_、**5**_(2×2+1)_... 等效 `:nth-child(odd)`                                          |
| `:nth-child(2n)`   | 偶数元素：**2**_(2×1)_、**4**_(2×2)_、**6**_(2×3)_... 等效 `:nth-child(even)`。请注意，**0**_(2×0)_是一个有效的符号，然而它不匹配任何元素，因为索引从1开始。 |
| `:nth-child(7)`    | 第7个元素                                                                                                              |
| `:nth-child(n+7)`  | 所有从第7个开始的元素: **7**_(0+7)_、**8**_(1+7)_、**9**_(2+7)_...                                                             |
| `:nth-child(3n+4)` | 从第4开始的每第3个元素：**4**_(3×0+4)_、**7**_(3×1+4)_、**10**_(3×2+4)_、**13**_(3×3+4)_...                                      |
| `:nth-child(-n+3)` | 前3个元素: **3**_(-1×0+3)_、**2**_(-1×1+3)_、**1**_(-1×2+3)_。所有后续的索引数都小于1，所以没有元素能够匹配。                                    |

### 线上 nth-child 和 nth-last-child 测试工具 <a id="nth-last Online Tester"></a>

通过下面的网站可以很容易地测试`nth-child`和`nth-last-child`。
[nth-child-tester](https://css-tricks.com/examples/nth-child-tester/)
