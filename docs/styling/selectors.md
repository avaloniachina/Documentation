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

Selects a control by type. The first example above selects the `Avalonia.Controls.Button` class. To include a XAML namespace in the type separate the namespace and the type with a `|` character.

This selector does not match derived types. For that, use the [`Is`](selectors.md#is) selector.

{% hint style="info" %}
Note the type of an object is actually determined by looking at its `IStyleable.StyleKey` property. By default this simply returns the type of the current instance, but if, for example, you do want your control which inherits from `Button` to be styled as a `Button`, then you can implement the `IStyleable.StyleKey` property on your class to return `typeof(Button)`.
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

Selects a control by its [`Name`](http://reference.avaloniaui.net/api/Avalonia/StyledElement/2362746E) property with a `#` charactor.

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

Selects a control with the specified style classes. Multiple classes should be separated with a `.` character, or a `:` character in the case of pseudoclasses. If multiple classes are specified then the control must have all of the requested classes present in order to match.

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

This is very similar to the [`OfType`](selectors.md#ofType) selector except it also matches derived types.

{% hint style="info" %}
Again, the type of an object is actually determined by looking at its `IStyleable.StyleKey` property.
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

A child selector is defined by separating two selectors with a `>` character. This selector matches _direct children in the logical tree_, so in the example above the selector will match any `Button` that is a direct logical child of a `StackPanel`.

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

When two selectors are separated by a space, then the selector will match descendants in the logical tree, so in this case the selector will match any `Button` that is a logical descendant of a `StackPanel`.

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

Matches any control which has the specified property set to the specified value.

{% hint style="info" %}
**Note:** When using a `AttachedProperty` in selectors inside XAML, it has to be wrapped in parenthesis.

```markup
<Style Selector="TextBlock[(Grid.Row)=0]">
```

{% endhint %}

{% hint style="info" %}
**Note:** When using in selectors in XAML, properties must support [`TypeConverter`](https://learn.microsoft.com/dotnet/api/system.componentmodel.typeconverter)
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

Matches a control in a control template. All other selectors listed here work on the logical tree. If you wish to select a control in a control template then you must use this selector. The example selects `ContentPresenter` controls in the templates of `Button`s.

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

Negates an inner selector.

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

Finds the elements that match any of these selectors. Each selector is separated by ",".

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

Matches elements based on their position in a group of siblings.

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

Matches elements based on their position among a group of siblings, counting from the end.

## Nth Child and Nth Last Child Syntax

`:nth-child()` and `:nth-last-child()` takes a single argument that describes a pattern for matching element indices in a list of siblings. Element indices are **1-based**. Below samples use `:nth-child()`, but also applicable for `:nth-last-child()`.

### Keyword notation

`odd` Represents elements whose index in a series of siblings is odd: 1, 3, 5, etc.

`even` Represents elements whose index in a series of siblings is even: 2, 4, 6, etc.

### Functional notation

`An+B` Represents elements in a list whose indices match those found in a custom pattern of numbers, defined by An+B, where:

* `A` is an integer step size,
* `B` is an integer offset,
* `n` is all non-negative integers, starting from 0.

It can be understood as _every `A`th element starting from `B`th_

### Functional notation examples

|Example|Representation|
|:---|:---|
|`:nth-child(odd)`|The odd elements: **1**, **3**, **5**, etc.|
|`:nth-child(even)`|The even elements: **2**, **4**, **6**, etc.|
|`:nth-child(2n+1)`|The odd elements: **1**_(2×0+1)_, **3**_(2×1+1)_, **5**_(2×2+1)_, etc. equivalent to `:nth-child(odd)`|
|`:nth-child(2n)`|The even elements: **2**_(2×1)_, **4**_(2×2)_, **6**_(2×3)_, etc. equivalent to `:nth-child(even)`. Notice that **0**_(2×0)_ a valid notation, however it's not matching any element since index starts from 1. |
|`:nth-child(7)`|The 7th element|
|`:nth-child(n+7)`|Every element start from 7th: **7**_(0+7)_, **8**_(1+7)_, **9**_(2+7)_, etc|
|`:nth-child(3n+4)`|Every 3rd element start from 4th: **4**_(3×0+4)_, **7**_(3×1+4)_, **10**_(3×2+4)_, **13**_(3×3+4)_, etc|
|`:nth-child(-n+3)`|First 3 elements: **3**_(-1×0+3)_, **2**_(-1×1+3)_, **1**_(-1×2+3)_. All subsequent indices are less than 1 so they are not matching any elements.  |

### Online nth-child & nth-last-child Tester <a id="nth-last Online Tester"></a>

Using the link below, both `nth-child` and `nth-last-child` can be easily evaluated:
[nth-child-tester](https://css-tricks.com/examples/nth-child-tester/)
