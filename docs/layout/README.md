# 📐 布局

## Panels <a id="panels"></a>

Avalonia包含一组源自`Panel`的元素。这些`Panel`元素支持许多复杂的布局。例如，使用`StackPanel`元素可以轻松实现堆叠元素(Stacking Elements)，而使用`Canvas`可以实现更复杂、更自由的布局。

下表总结了可用的`Panel`控件：

| 名称              | 描述                                                                        |
|:----------------|:--------------------------------------------------------------------------|
| `Panel`         | 排列所有子项填充`Panel`的边界                                                        |
| `Canvas`        | 定义一个区域，在这个区域内，你可以通过相对于`Canvas`区域的坐标显式定位子元素。                               |
| `DockPanel`     | 定义一个区域，在该区域内可以相对于彼此水平或垂直排列子元素。                                            |
| `Grid`          | 定义由行和列组成的灵活的网格区域。                                                         |
| `RelativePanel` | 相对于其他元素或面板本身排列子元素。                                                        |
| `StackPanel`    | 将子元素排列成可以水平或垂直定向的单行。                                                      |
| `WrapPanel`     | 将子元素按顺序从左到右定位，在容器的边缘将内容分割到下一行。随后的排列取决于`Orientation`属性的值，按顺序从上到下或从右到左依次排列。 |

在WPF中，`Panel`是一个抽象类，如果需要排列多个控件来填充可用空间通常是通过一个没有行/列的`Grid`来完成的。在Avalonia中，`Panel`是一个可用的控件，它的布局行为与没有行/列的`Grid`相同，但运行时占用的空间较小。

## 元素边界框(Element Bounding Boxes) <a id="element-bounding-boxes"></a>

当讨论Avalonia的布局时，理解围绕所有元素的边界框是很重要的。布局系统所消耗的每个`Control`可以被认为是一个矩形，它被插入布局中。`Bounds`属性返回一个元素的布局分配的边界。矩形的尺寸是通过计算可用的屏幕空间、约束的尺寸、布局特定的属性（如margin和padding）以及父`Panel`元素的个别行为来决定的。处理这些数据，布局系统能够计算出一个特定的`Panel`的所有子元素的位置。记住，在父元素上定义的尺寸特征，如`Border`，会影响其子元素。

## 布局系统 <a id="the-layout-system"></a>

在最简单的情况下，布局是一个递归系统，把控元素的尺寸、位置和绘制。更具体地说，布局描述了测量和排列`Panel`元素的`Children`集合成员的过程。布局是一个繁重的过程，`Children`集合越大，必要的计算次数就越多，还可以根据集合中的`Panel`元素定义的布局行为增加复杂性。相对简单的`Panel`（如`Canvas`），对比更复杂的`Panel`（如`Grid`），有明显的性能优势。

每次子控件改变位置时，都有可能触发布局系统。因此，理解可以调用布局系统的事件非常重要，因为不必要的调用会导致应用程序性能下降。下面描述了调用布局系统时发生的过程：

1. 一个子UI元素开始布局过程，首先要测量其核心属性。
2. 计算在`Control`上定义的尺寸属性，如`Width`、`Height`和`Margin`。
3. 应用`Panel`特定的逻辑，如`Dock`方向或堆叠`Orientation`。
4. 在测量所有的`Children`后排列内容。
5. 在屏幕上绘制`Children`集合。
6. 如果有更多的`Children`被添加到集合中，这个过程将再次被调用。

这一过程以及如何调用这一过程将在以下章节中详细说明。

## 测量和排列Children <a id="measuring-and-arranging-children"></a>

布局系统为`Children`集合的每个成员完成两次传递，即测量传递和排列传递。每个子`Panel`都提供自己的`MeasureOverride`和`ArrangeOverride`方法来实现自己的特定布局行为。

在测量传递期间内，将计算`Children`集合的每个成员。该过程首先调用`Measure`方法，此方法是在父`Panel`元素的实现中调用的，不必显式调用该方法来进行布局。

首先，评估`Visual`的本地尺寸属性，如`Clip`和`IsVisible`。这将生成传递给`MeasureCore`的约束。

首先，处理影响约束值的框架属性。这些属性通常描述底层`Control`的尺寸特征，如`Width`、`Height`和`Margin`。每个属性都可以改变显示元素所需的空间。然后以约束作为参数调用`MeasureOverride`。

由于`Bounds`是一个计算值，因此您应该注意，由于布局系统的各种操作，可能会对其多次更改。布局系统有计算子元素所需的测量空间、父元素的约束等功能。

测量传递的最终目标是让子元素确定它的期望尺寸(`DesiredSize`)，这发生在`MeasureCore`调用中。`DesiredSize`值由`Measure`存储，以便在内容排列过程中使用。

排列过程从调用`Arrange`方法开始。在排列过程中，父`Panel`元素生成一个矩形，表示子元素的边界。此值将传递给`ArrangeCore`方法进行处理。

`ArrangeCore`方法计算子元素的`DesiredSize`，并计算可能影响元素渲染尺寸的任何其他边距。`ArrangeCore`生成一个排列尺寸，该尺寸作为参数传递给`Panel`的`ArrangeOverride`方法`ArrangeOverride`生成子级的最终尺寸。最后，`ArrangeCore`方法对偏移量属性（如`Margin`和`Alignment`）进行最终计算，并将子级放入其布局框中。子元素不必（而且经常不）填满整个分配的空间。控件返回到父`Panel`，完成布局过程。

## 以下是本节中的文章 <a id="in-this-section"></a>

* [Panels概述](panels-overview.md)
* [Alignment、Margins和Padding](alignment-margins-and-padding.md)
* [创建自定义面板](create-a-custom-panel.md)

