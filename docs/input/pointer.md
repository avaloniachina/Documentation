# 鼠标与触控设备

Avalonia 可工作于我们称之为“定点设备”的基础上。这类设备包括但不限于鼠标、触摸板与数位笔等。Avalonia中的每个控件都提供了如下事件，允许开发者追踪定点设备的移动、点击与滚动：
- PointerEnter
- PointerLeave
- PointerMoved
- PointerPressed
- PointerReleased
- PointerWheelChanged

以下是一个如何响应指针按钮按下的实例。

```csharp
control.PointerPressed += (args) =>
{
    var point = args.GetCurrentPoint();
    var x = point.Position.X;
    var y = point.Position.Y;
    if (point.Properties.IsLeftButtonPressed)
    {
        // left button pressed
    }
    if (point.Properties.IsRightButtonPressed)
    {
        // right button pressed
    }
}
```

在上述实例中，坐标 `x` 和 `y` 是相对于窗口原点定位的。如果你希望这些坐标相对定位于某个控件, 你可以将 `PointerEventArgs.GetCurrentPoint` 函数改成这样： `var pointControlCoords = args.GetCurrentPoint(control);`

# 点击事件的参数：

每个控件都有如下两个事件：单击（Tapped）和双击（DoubleTapped）。
当指针在控件上按下并抬起时，响应为单击（Tapped）。
当指针在同一位置上按下两次后，双击（DoubleTapped）被触发。允许的大小（size，两次“点击”间的距离）和时间（time，两次“点击”间的延迟）取决于程序工作的平台。通常来说，在触摸设备上这两个值会更大。

### 参考 <a id="reference"></a>

[控件（Control）](http://reference.avaloniaui.net/api/Avalonia.Controls/Control/)
[指针事件参数（PointerEventArgs）](http://reference.avaloniaui.net/api/Avalonia.Input/PointerEventArgs/)
