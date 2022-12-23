# Panel

The `Panel` is the base class for controls that can contain multiple children like `DockPanel` or `StackPanel`.

The `Panel` class can be useful on its own for very basic layouts, or simply to allow multiple controls to be to be contained.

```markup
<Panel Height="300" Width="300">
    <Rectangle Fill="Red" Height="100" VerticalAlignment="Top"/>
    <Rectangle Fill="Blue" Width="100" HorizontalAlignment="Right" />
    <Rectangle Fill="Green" Height="100" VerticalAlignment="Bottom"/>
    <Rectangle Fill="Orange" Width="100" HorizontalAlignment="Left"/>
</Panel>
```

![](../../.gitbook/assets/image%20%287%29.png)

There are other more useful panels that derive from `Panel`, these include:

{% page-ref page="stackpanel.md" %}

{% page-ref page="dockpanel.md" %}

{% page-ref page="relativepanel.md" %}

{% page-ref page="wrappanel.md" %}

### Reference <a id="reference"></a>

[Panel](http://reference.avaloniaui.net/api/Avalonia.Controls/Panel/)

### Source code <a id="source-code"></a>

[Panel.cs](https://github.com/AvaloniaUI/Avalonia/blob/master/src/Avalonia.Controls/Panel.cs)

### Related Docs

{% page-ref page="../layout/panels-overview.md" %}

{% page-ref page="../layout/create-a-custom-panel.md" %}

