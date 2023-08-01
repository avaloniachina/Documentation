# 📐 布局

## Panels <a id="panels"></a>

Avalonia包含一组派生自`Panel`的元素。这些`Panel`元素支持许多复杂的布局。例如，使用`StackPanel`元素可以轻松实现堆叠元素，而使用`Canvas`可实现更复杂和自由流动的布局。

下表汇总了可用的布局`Panel`控件。

| 名称              | 描述                                                                      |
|:----------------|:------------------------------------------------------------------------|
| `Panel`         | 排列所有子项填充`Panel`的边界                                                      |
| `Canvas`        | 定义一个区域，可在其中通过相对于`Canvas`区域的坐标显式定位子元素。                                   |
| `DockPanel`     | 定义一个区域，从中可以按相对位置水平或垂直排列各个子元素。                                           |
| `Grid`          | 定义由行和列组成的灵活的网格区域。                                                       |
| `RelativePanel` | 相对于其他元素或面板本身排列子元素。                                                      |
| `StackPanel`    | 将子元素排列成水平或垂直的一行。                                                        |
| `WrapPanel`     | 按从左到右的顺序位置定位子元素，在包含框的边缘处将内容切换到下一行。排序顺序是从上到下还是从右到左，取决于`Orientation`属性的值。 |

在WPF中，`Panel`是一个抽象类，如果需要排列多个控件来填充可用空间通常是通过一个没有行/列的`Grid`来完成的。在Avalonia中，`Panel`是一个可用的控件，它的布局行为与没有行/列的`Grid`相同，但运行时占用的空间较小。

## 元素边界框 <a id="element-bounding-boxes"></a>

在`Avalonia`中构思布局时，理解环绕所有元素的边界框非常重要。布局系统使用的每个`Control`都可以被视为嵌入到布局中的矩形。`Bounds`属性返回元素的布局分配的边界。矩形的大小是通过计算可用屏幕空间、约束的大小、特定于布局的属性（如`Margin`和`Padding`）以及父`Panel`元素的个别行为来确定的。处理此数据时，布局系统能够计算特定 `Panel`的所有子级的位置。请务必记住，调整在父元素（如`Border`）上定义的特性的大小会影响其子级。

## 布局系统 <a id="the-layout-system"></a>

简单地说，布局是一个递归系统，实现对元素进行大小调整、定位和绘制。更具体地说，布局描述测量和排列`Panel`元素的`Children`集合的成员的过程。布局是一个繁重的过程。`Children`集合越大，必要的计算次数就越多。根据拥有该集合的`Panel`元素所定义的布局行为，还可能会增加复杂性。相对简单的`Panel`（如`Canvas`）可以比更复杂的`Panel`（如`Grid`）具有更好的性能。

每当子`UIElement`改变其位置时，布局系统都可能触发一个新的传递。 因此，了解哪些事件会触发布局系统就很重要，因为不必要的调用可能导致应用程序性能变差。下面描述触发布局系统时发生的过程：

1. 子`UIElement`通过首先测量其核心属性来开始布局过程。
2. 计算`Control`上定义的尺寸属性，如`Width`、`Height`和`Margin`。
3. 应用`Panel`特定的逻辑，如`Dock`方向或`Orientation`堆叠方式。
4. 在测量所有子级后排列内容。
5. 在屏幕上绘制`Children`集合。
6. 如果向集合添加了其他`Children`，则会再次调用该过程。

以下各节更详细地定义了此过程及其调用方式。

## 测量和排列子元素 <a id="measuring-and-arranging-children"></a>

布局系统为`Children`集合的每个成员完成两个过程：一个测量过程和一个排列过程。每个子`Panel`都提供自己的`MeasureOverride`和`ArrangeOverride`方法来实现自己的特定布局行为。

此过程从调用`Measure`方法开始。 需要在父`Panel`元素的实现中调用此方法，而不必为要出现的布局显式调用该方法。

首先，计算`Visual`的本机尺寸属性，例如`Clip`和`IsVisible`。这将生成一个约束值，该值将传递给`MeasureCore`。

其次，处理影响约束值的框架属性。这些属性通常描述底层`Control`的尺寸特征，如`Width`、`Height`和`Margin`。每个属性都可以改变显示元素所需的空间。然后以约束作为参数调用`MeasureOverride`。

由于`Bounds`是计算所得的值，因此您应该注意，由于布局系统的各种操作，该值可能有多次或递增的报告的更改。布局系统可能会计算子元素所需的测量空间、父元素的约束等。

测量过程的最终目标是让子元素确定其期望尺寸(`DesiredSize`)，这发生在`MeasureCore`调用期间。`DesiredSize`值由`Measure`存储，以便在内容排列过程中使用。

排列过程从调用`Arrange`方法开始。在排列过程中，父`Panel`元素会生成一个表示子元素边界的矩形。该值将传递给`ArrangeCore`方法进行处理。

`ArrangeCore`方法计算子元素的`DesiredSize`，并且计算可能影响元素呈现大小的任何其他边距。`ArrangeCore`生成一个`arrangeSize`，后者作为参数传递给`Panel`的`ArrangeOverride`方法。`ArrangeOverride`生成子元素的`finalSize`。最后`ArrangeCore`方法执行偏移量属性（如边距和对齐）的最终计算，并将子元素放在其布局槽内。子元素不需要（并且通常不）填满整个分配空间。然后将控件返回给父级`Panel`，布局过程即告完成。

## 以下是本节中的文章 <a id="in-this-section"></a>

* [面板概述](panels-overview.md)
* [Alignment、Margins和Padding](alignment-margins-and-padding.md)
* [创建自定义面板](create-a-custom-panel.md)

