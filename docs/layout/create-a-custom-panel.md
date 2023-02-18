# 创建自定义Panel

此示例展示了如何覆盖`Panel`元素的默认布局行为，并创建了从`Panel`派生的自定义布局元素。

该示例定义了一个名为`PlotPanel`的简单自定义`Panel`元素，它根据两个硬编码的x和y坐标定位子元素。在此示例中，`x`和`y`都设置为`50`；因此，所有子元素都位于x和y轴上的该位置。

要实现自定义的`Panel`行为，该示例使用`MeasureOverride`和`ArrangeOverride`方法。每个方法都返回定位和渲染子元素所需的`Size`数据。

```csharp
public class PlotPanel : Panel
{
    // 重写Panel的默认测量方法
    protected override Size MeasureOverride(Size availableSize)
    {
        var panelDesiredSize = new Size();

        // 在我们的例子中，我们只有一个Child。
        // 我们的Panel只需要它唯一的Child的大小。
        foreach (var child in Children)
        {
            child.Measure(availableSize);
            panelDesiredSize = child.DesiredSize;
        }

        return panelDesiredSize;
    }

    protected override Size ArrangeOverride(Size finalSize)
    {
        foreach (var child in Children)
        {
            double x = 50;
            double y = 50;

            child.Arrange(new Rect(new Point(x, y), child.DesiredSize));
        }
        
        return finalSize; // 返回最终排列大小
    }
}
```

