---
description: The TransitioningContentControl is a ContentControl which can render PageTransitions when the Content changes.
---

# TransitioningContentControl

## Common Properties

| Property          | Description                                                                                                                                 |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| `Content`         | Gets or sets the content to display in the control                                                                                          |
| `ContentTemplate` | Gets or sets the [DataTemplate](https://docs.avaloniaui.net/docs/templates/data-templates) used to display the content                      |
| `PageTransition`  | Gets or sets the [PageTransition](https://docs.avaloniaui.net/docs/animations/PageTransitions) which will be shown when the content changes |

## Reference

[TransitioningContentControl](http://reference.avaloniaui.net/api/Avalonia.ReactiveUI/TransitioningContentControl/)

## Source code

[TransitioningContentControl.cs](https://github.com/AvaloniaUI/Avalonia/blob/master/src/Avalonia.Controls/TransitioningContentControl.cs)

## Example

Let's assume we have a collection of different images and we want to show them in a slideshow like view. In order to do this we can setup our `TransitioningContentControl` like this:

```markup
<TransitioningContentControl Content="{Binding SelectedImage}" >
    <TransitioningContentControl.ContentTemplate>
        <DataTemplate DataType="Bitmap">
            <Image Source="{Binding}" />
        </DataTemplate>
    </TransitioningContentControl.ContentTemplate>
</TransitioningContentControl>
```

![TransitioningContentControl Example](../../.gitbook/assets/TransitioningContentControl\_01.webp)

## Changing the PageTransition

If you don't like the `PageTransition` which is provided by the applied theme, you can also provide your own [PageTransition](https://docs.avaloniaui.net/docs/animations/PageTransitions). This can be done in XAML, provided via `Binding` or via `DynamicResource`.

In the sample below we will change the [PageTransition](https://docs.avaloniaui.net/docs/animations/PageTransitions) to slide the images horizontally.

```markup
<TransitioningContentControl Content="{Binding SelectedImage}" >
    <TransitioningContentControl.PageTransition>
        <PageSlide Orientation="Horizontal" Duration="0:00:00.500" />
    </TransitioningContentControl.PageTransition>
    <TransitioningContentControl.ContentTemplate>
        <DataTemplate DataType="Bitmap">
            <Image Source="{Binding}"  />
        </DataTemplate>
    </TransitioningContentControl.ContentTemplate>
</TransitioningContentControl>
```

![TransitioningContentControl Example](../../.gitbook/assets/TransitioningContentControl\_02.webp)

## Disable the PageTransition

If you want to disable the transition, set the `PageTransition` to `null`.

```markup
<TransitioningContentControl Content="{Binding SelectedImage}" PageTransition="{x:Null}" >
    <TransitioningContentControl.ContentTemplate>
        <DataTemplate DataType="Bitmap">
            <Image Source="{Binding}"  />
        </DataTemplate>
    </TransitioningContentControl.ContentTemplate>
</TransitioningContentControl>
```
