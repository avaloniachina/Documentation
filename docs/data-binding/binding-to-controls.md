# 与控件绑定

类似绑定到控件的[`DataContext`](https://docs.avaloniaui.net/docs/data-binding/the-datacontext)，您还可以绑定到其他控件。

{% hint style="info" %}
注意：当您这样做时，绑定源是到**控件本身**的，而不是控件的`DataContext`。如果要绑定到控件的`DataContext`，则需要指定在绑定路径中。
{% endhint %}

## 绑定到已命名控件

如果要绑定到另一个已命名控件的属性，可以使用以`#`字符为前缀的控件名称。

```markup
<TextBox Name="other">

<!-- 绑定到命名为“other”控件的Text属性 -->
<TextBlock Text="{Binding #other.Text}"/>
```

这相当于WPF和UWP开发者熟悉的长格式绑定：

```markup
<TextBox Name="other">
<TextBlock Text="{Binding Text, ElementName=other}"/>
```

这两种语法Avalonia均支持，但缩写`#`语法显得不那么冗长。

## 绑定到祖先<a id="binding-to-an-ancestor"></a>

您可以使用`$parent`符号绑定到目标在逻辑上的父级：

```markup
<Border Tag="Hello World!">
  <TextBlock Text="{Binding $parent.Tag}"/>
</Border>
```

也可以通过在`$parent`符号添加索引器绑定到祖先级：

```markup
<Border Tag="Hello World!">
  <Border>
    <TextBlock Text="{Binding $parent[1].Tag}"/>
  </Border>
</Border>
```

索引器从0开始，因此`$parent[0]`等同于`$parent`。

还可以按类型绑定到祖先：

```markup
<Border Tag="Hello World!">
  <Decorator>
    <TextBlock Text="{Binding $parent[Border].Tag}"/>
  </Decorator>
</Border>
```

最后，您可以组合索引器和类型：

```markup
<Border Tag="Hello World!">
  <Border>
    <Decorator>
    <TextBlock Text="{Binding $parent[Border;1].Tag}"/>
    </Decorator>
  </Border>
</Border>
```

如果需要在祖先类型中包含XAML命名空间，一般使用`:`字符：

```markup
<local:MyControl Tag="Hello World!">
  <Decorator>
    <TextBlock Text="{Binding $parent[local:MyControl].Tag}"/>
  </Decorator>
</local:MyControl>
```

{% hint style="warning" %}
Avalonia还支持WPF/UWP的`RelativeSource`语法，它做了一些类似的事情，但**并不相同**。`RelativeSource`在**视觉树**上工作，而这里给出的语法在**逻辑树**上运行。
{% endhint %}

