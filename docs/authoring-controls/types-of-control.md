# 控件类别

如果你想要创建自己的控件，Avalonia有三个主要的控件类别。在创建自定义控件之前，你需要根据你的使用场景选择一个最合适的控件类别。

### User Control

`UserControl` 是最简单的创建自定义控件的方法。这种类型的控件最适合创建特定于程序的“Views”或者“Pages” 。

 `UserControl`和创建Window的方法一样，创建一个新的UserControl，并向其添加控件即可。

### Templated Controls

`TemplatedControl`s 是最长使用的在多个程序之间共享的通用控件。他们初始状态下没有外观,这意味着它们可以为不同的主题、程序重新设计外观。  Avalonia定义的大多数标准控件都属于这一类。

{% hint style="info" %}  在 WPF/UWP你需要继承自 Control 类型来创建一个新的控件, 但是在Avalonia 你需要继承自`TemplatedControl.` {% endhint %}

{% hint style="info" %} 如果你想在一个单独的文件中，为你创建的`TemplatedControl`提供一个`Style`,记住在你的程序中使用 [`StyleInclude`](https://docs.avaloniaui.net/docs/styling/styles) 导入你的文件. {% endhint %}

### Basic Control

`Control`是用户界面的基础 - 它们通过重写  `Visual.Render`  来使用几何图形来绘制自己。渲染方法。 像`TextBlock` 和 `Image` 这样的控件就属于这一类.

{% hint style="info" %} 在 WPF/UWP你需要继承自 FrameworkElement 类型来创建一个新的控件, 但是在Avalonia 你需要继承自 `Control.` {% endhint %}
