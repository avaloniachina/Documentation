# 面板概述

`Panel`元素是控制元素呈现（它们的大小和尺寸、位置和子内容的排列）的组件。Avalonia提供许多预定义的`Panel`元素以及构造自定义`Panel`元素的功能。

## Panel类 <a id="the-panel-class"></a>

`Panel`是Avalonia中提供布局支持的所有元素的基类。派生的`Panel`元素用于在XAML和代码中定位和排列元素。

Avalonia有一套全面的派生自`Panel`的实现，支持许多复杂的布局。这些派生类暴露了支持大多数标准用户界面(UI)场景的属性和方法。如果开发者找不到满足其需求的子排列行为，可以通过重写`ArrangeOverride`和`MeasureOverride`方法创建新的布局。想要了解自定义布局行为的更多信息，请浏览[创建自定义Panel](create-a-custom-panel.md)。

### Panel公共成员 <a id="panel-common-members"></a>

所有`Panel`元素都支持由`Control`定义的关于尺寸调整和定位的基本属性，包括`Height`、`Width`、`HorizontalAlignment`、`VerticalAlignment`和`Margin`。想要了解关于`Control`中定义的定位属性的其他信息，请浏览[Alignment、Margins和Padding概述](alignment-margins-and-padding.md)。

`Panel`暴露了对理解和使用布局至关重要的属性。`Background`属性作用在用`Brush`填充派生`Panel`元素边界之间的区域，`Children`表示`Panel`所包含的子元素集合。

**附加属性(Attached Properties)**

派生自`Panel`的元素广泛使用了附加属性。附加属性是一种特殊形式的依赖属性，它没有常规的公共语言运行库(CLR)属性`wrapper`。附加的属性在XAML中有一个特殊的语法，可以在下面的几个示例中看到。

附加属性的一个用途是允许子元素存储实际上由父元素定义的属性的唯一值。此功能的一项应用是让子元素通知父级它们希望如何在用户界面(UI)中呈现，这对应用程序布局非常有用。

### 用户界面Panel <a id="user-interface-panels"></a>

UI方案中有几个面板类可用：`Panel`、`Canvas`、`DockPanel`、`Grid`、`StackPanel`、`WrapPanel`和`RelativePanel`。这些面板元素易于使用、功能齐全并且可扩展，足以适用于大多数应用程序。

**画布(Canvas)** <a id="canvas-class"></a>

`Canvas`元素支持根据x和y的绝对坐标定位内容。元素可以在唯一的位置绘制，如果元素占据相同的坐标，则它们在标记中出现的顺序决定了它们的绘制顺序。

`Canvas`是所有`Panel`中最灵活的布局。高度(`Height`)和宽度(`Width`)属性用于定义`Canvas`的区域，并为内部的元素分配相对于父`Canvas`的区域的绝对坐标。对于附加属性`Canvas.Left`、`Canvas.Top`、`Canvas.Right`和`Canvas.Bottom`，可以控制对象在`Canvas`中的精确位置，使开发者可以在屏幕上精确定位和排列元素。

**Canvas中的ClipToBounds**

`Canvas`可以将子元素放置在屏幕上的任何位置，甚至在其自身定义的`Height`和`Width`之外的坐标处。此外，`Canvas`不受其子对象大小的影响。因此，子元素可能会在父`Canvas`的边框之外绘制其他元素。`Canvas`默认允许在父`Canvas`边界之外绘制子对象。如果不希望出现这种情况，可以将`ClipToBounds`属性设置为`true`，这将导致`Canvas`剪裁到其自身大小。`Canvas`是唯一允许在其边界之外绘制子对象的布局元素。

**声明和使用Canvas**

只需使用XAML或代码即可实例化`Canvas`。下面的示例演示如何使用`Canvas`对内容进行绝对定位。此代码生成三个100像素的正方形。第一个正方形为红色，其左上角的\(_x, y_)位置指定为\(0, 0\)。第二个正方形为绿色，其左上角的位置是\(100, 100\)，在第一个正方形的右下方。第三个正方形为蓝色，其左上角为\(50, 50\)，因此包含了第一个正方形的右下四分之一部分和第二个正方形的左上四分之一部分。由于第三个正方形最后布置，因此它看起来在另外两个正方形上方，即，重叠部分采用第三个正方形的颜色。

C\#

```csharp
// 创建Canvas
myParentCanvas = new Canvas();
myParentCanvas.Width = 400;
myParentCanvas.Height = 400;

// 声明子画布元素
myCanvas1 = new Canvas();
myCanvas1.Background = Brushes.Red;
myCanvas1.Height = 100;
myCanvas1.Width = 100;
Canvas.SetTop(myCanvas1, 0);
Canvas.SetLeft(myCanvas1, 0);

myCanvas2 = new Canvas();
myCanvas2.Background = Brushes.Green;
myCanvas2.Height = 100;
myCanvas2.Width = 100;
Canvas.SetTop(myCanvas2, 100);
Canvas.SetLeft(myCanvas2, 100);

myCanvas3 = new Canvas();
myCanvas3.Background = Brushes.Blue;
myCanvas3.Height = 100;
myCanvas3.Width = 100;
Canvas.SetTop(myCanvas3, 50);
Canvas.SetLeft(myCanvas3, 50);

// 将子元素添加到Canvas的Children集合
myParentCanvas.Children.Add(myCanvas1);
myParentCanvas.Children.Add(myCanvas2);
myParentCanvas.Children.Add(myCanvas3);
```

XAML

```markup
<Canvas Height="400" Width="400">
  <Canvas Height="100" Width="100" Top="0" Left="0" Background="Red"/>
  <Canvas Height="100" Width="100" Top="100" Left="100" Background="Green"/>
  <Canvas Height="100" Width="100" Top="50" Left="50" Background="Blue"/>
</Canvas>
```

**DockPanel** <a id="dockpanel-class"></a>

`DockPanel`元素使用在子内容元素中设置的`DockPanel.Dock`附加属性沿容器边缘定位内容。`DockPanel.Dock`设置为`Top`或`Bottom`时，它会将子元素放置在彼此的上方或下方。`DockPanel.Dock`设置为`Left`或`Right`时，它会将子元素放置在彼此的左侧或右侧。`LastChildFill`属性确定添加为`DockPanel`的子级的最后一个元素的位置。

可以使用`DockPanel`定位一组相关控件，如一组按钮。或者，可以使用它创建“平移”的UI。

**根据内容调整尺寸**

如果未指定其`Height`和`Width`属性，则`DockPanel`会调整大小以适应其内容。大小可以增大或减小以容纳其子元素的大小。但是，当已指定这些属性，并且不再有空间能容纳下一个指定子元素时，`DockPanel`将不会显示该子元素或后续子元素，并且不会测量后续子元素。

**LastChildFill**

默认情况下，`DockPanel`元素的最后一个子元素将“填充”剩余的未分配空间。如果不希望这么做，请将`LastChildFill`属性设置为`false`。

**声明和使用DockPanel**

以下示例演示如何使用`DockPanel`为空间分区。五个`Border`元素添加为父`DockPanel`的子级。 每个都使用不同的`DockPanel`位置属性来为空间分区。最后一个元素"填充"剩余的未分配空间。

C\#

```csharp
// 创建DockPanel
DockPanel myDockPanel = new DockPanel();
myDockPanel.LastChildFill = true;

// 声明子内容
Border myBorder1 = new Border();
myBorder1.Height = 25;
myBorder1.Background = Brushes.SkyBlue;
myBorder1.BorderBrush = Brushes.Black;
myBorder1.BorderThickness = new Thickness(1);
DockPanel.SetDock(myBorder1, Dock.Top);
TextBlock myTextBlock1 = new TextBlock();
myTextBlock1.Foreground = Brushes.Black;
myTextBlock1.Text = "Dock = Top";
myBorder1.Child = myTextBlock1;

Border myBorder2 = new Border();
myBorder2.Height = 25;
myBorder2.Background = Brushes.SkyBlue;
myBorder2.BorderBrush = Brushes.Black;
myBorder2.BorderThickness = new Thickness(1);
DockPanel.SetDock(myBorder2, Dock.Top);
TextBlock myTextBlock2 = new TextBlock();
myTextBlock2.Foreground = Brushes.Black;
myTextBlock2.Text = "Dock = Top";
myBorder2.Child = myTextBlock2;

Border myBorder3 = new Border();
myBorder3.Height = 25;
myBorder3.Background = Brushes.LemonChiffon;
myBorder3.BorderBrush = Brushes.Black;
myBorder3.BorderThickness = new Thickness(1);
DockPanel.SetDock(myBorder3, Dock.Bottom);
TextBlock myTextBlock3 = new TextBlock();
myTextBlock3.Foreground = Brushes.Black;
myTextBlock3.Text = "Dock = Bottom";
myBorder3.Child = myTextBlock3;

Border myBorder4 = new Border();
myBorder4.Width = 200;
myBorder4.Background = Brushes.PaleGreen;
myBorder4.BorderBrush = Brushes.Black;
myBorder4.BorderThickness = new Thickness(1);
DockPanel.SetDock(myBorder4, Dock.Left);
TextBlock myTextBlock4 = new TextBlock();
myTextBlock4.Foreground = Brushes.Black;
myTextBlock4.Text = "Dock = Left";
myBorder4.Child = myTextBlock4;

Border myBorder5 = new Border();
myBorder5.Background = Brushes.White;
myBorder5.BorderBrush = Brushes.Black;
myBorder5.BorderThickness = new Thickness(1);
TextBlock myTextBlock5 = new TextBlock();
myTextBlock5.Foreground = Brushes.Black;
myTextBlock5.Text = "This content will Fill the remaining space";
myBorder5.Child = myTextBlock5;

// 将子元素添加到DockPanel Children集合
myDockPanel.Children.Add(myBorder1);
myDockPanel.Children.Add(myBorder2);
myDockPanel.Children.Add(myBorder3);
myDockPanel.Children.Add(myBorder4);
myDockPanel.Children.Add(myBorder5);
```

XAML

```markup
<DockPanel LastChildFill="True">
  <Border Height="25" Background="SkyBlue" BorderBrush="Black" BorderThickness="1" DockPanel.Dock="Top">
    <TextBlock Foreground="Black">Dock = "Top"</TextBlock>
  </Border>
  <Border Height="25" Background="SkyBlue" BorderBrush="Black" BorderThickness="1" DockPanel.Dock="Top">
    <TextBlock Foreground="Black">Dock = "Top"</TextBlock>
  </Border>
  <Border Height="25" Background="LemonChiffon" BorderBrush="Black" BorderThickness="1" DockPanel.Dock="Bottom">
    <TextBlock Foreground="Black">Dock = "Bottom"</TextBlock>
  </Border>
  <Border Width="200" Background="PaleGreen" BorderBrush="Black" BorderThickness="1" DockPanel.Dock="Left">
    <TextBlock Foreground="Black">Dock = "Left"</TextBlock>
  </Border>
  <Border Background="White" BorderBrush="Black" BorderThickness="1">
    <TextBlock Foreground="Black">这部分内容“填充”剩余的空间</TextBlock>
  </Border>
</DockPanel>
```

**网格(Grid)** <a id="grid-class"></a>

`Grid`元素合并了绝对定位和表格式数据控件的功能，使您能够轻松定位元素并设置元素样式。`Grid`允许您定义灵活的行和列分组，甚至提供了一种在多个`Grid`元素之间共享尺寸信息的机制。

**行和列的尺寸调整机制**

在`Grid`中定义的行和列可以充分利用`Star`大小调整来按比例分配剩余空间。当选择`Star`作为行或列的`Height`或`Width`时，该行或列将收到剩余可用空间的加权比例。 这与`Auto`相反，后者根据行或列内的内容大小平均分配空间。

使用XAML时，此值表示为`*`或`2*`。在第一种情况下，行或列将得到一倍的可用空间，在第二种情况下，将得到两倍的可用空间，依此类推。通过将这一按比例分配空间的技术与`Stretch`的`HorizontalAlignment`和`VerticalAlignment`值结合使用，可以按屏幕空间的百分比为布局空间分区。`Grid`是唯一可以按此方式分配空间的布局面板。

**声明和使用Grid**

以下示例演示了如何构建与Windows“开始”菜单上的“运行”对话框类似的UI。

C\#

```csharp
// 创建Grid
grid1 = new Grid ();
grid1.Background = Brushes.Gainsboro;
grid1.HorizontalAlignment = HorizontalAlignment.Left;
grid1.VerticalAlignment = VerticalAlignment.Top;
grid1.ShowGridLines = true;
grid1.Width = 425;
grid1.Height = 165;

// 声明Columns
colDef1 = new ColumnDefinition();
colDef1.Width = new GridLength(1, GridUnitType.Auto);
colDef2 = new ColumnDefinition();
colDef2.Width = new GridLength(1, GridUnitType.Star);
colDef3 = new ColumnDefinition();
colDef3.Width = new GridLength(1, GridUnitType.Star);
colDef4 = new ColumnDefinition();
colDef4.Width = new GridLength(1, GridUnitType.Star);
colDef5 = new ColumnDefinition();
colDef5.Width = new GridLength(1, GridUnitType.Star);
grid1.ColumnDefinitions.Add(colDef1);
grid1.ColumnDefinitions.Add(colDef2);
grid1.ColumnDefinitions.Add(colDef3);
grid1.ColumnDefinitions.Add(colDef4);
grid1.ColumnDefinitions.Add(colDef5);

// 声明Rows
rowDef1 = new RowDefinition();
rowDef1.Height = new GridLength(1, GridUnitType.Auto);
rowDef2 = new RowDefinition();
rowDef2.Height = new GridLength(1, GridUnitType.Auto);
rowDef3 = new RowDefinition();
rowDef3.Height = new GridLength(1, GridUnitType.Star);
rowDef4 = new RowDefinition();
rowDef4.Height = new GridLength(1, GridUnitType.Auto);
grid1.RowDefinitions.Add(rowDef1);
grid1.RowDefinitions.Add(rowDef2);
grid1.RowDefinitions.Add(rowDef3);
grid1.RowDefinitions.Add(rowDef4);

// 添加Image
img1 = new Image();
img1.Source = runicon;
Grid.SetRow(img1, 0);
Grid.SetColumn(img1, 0);

// 添加主应用程序对话框
txt1 = new TextBlock();
txt1.Text = "Type the name of a program, folder, document, or Internet resource, and Windows will open it for you.";
txt1.TextWrapping = TextWrapping.Wrap;
Grid.SetColumnSpan(txt1, 4);
Grid.SetRow(txt1, 0);
Grid.SetColumn(txt1, 1);

// 将第二个文本单元格添加到Grid中
txt2 = new TextBlock();
txt2.Text = "Open:";
Grid.SetRow(txt2, 1);
Grid.SetColumn(txt2, 0);

// 添加TextBox控件
tb1 = new TextBox();
Grid.SetRow(tb1, 1);
Grid.SetColumn(tb1, 1);
Grid.SetColumnSpan(tb1, 5);

// 添加按钮
button1 = new Button();
button2 = new Button();
button3 = new Button();
button1.Content = "OK";
button2.Content = "Cancel";
button3.Content = "Browse ...";
Grid.SetRow(button1, 3);
Grid.SetColumn(button1, 2);
button1.Margin = new Thickness(10, 0, 10, 15);
button2.Margin = new Thickness(10, 0, 10, 15);
button3.Margin = new Thickness(10, 0, 10, 15);
Grid.SetRow(button2, 3);
Grid.SetColumn(button2, 3);
Grid.SetRow(button3, 3);
Grid.SetColumn(button3, 4);

grid1.Children.Add(img1);
grid1.Children.Add(txt1);
grid1.Children.Add(txt2);
grid1.Children.Add(tb1);
grid1.Children.Add(button1);
grid1.Children.Add(button2);
grid1.Children.Add(button3);
```

**StackPanel** <a id="stackpanel-class"></a>

`StackPanel`使您能够按指定方向“堆叠”元素，默认排列方向为垂直，`Orientation`属性控制内容排列方向。

**StackPanel对比DockPanel**

尽管`DockPanel`也可以“堆叠”子元素，但在某些使用场景中，`DockPanel `和`StackPanel`不会产生类似的效果。例如，子元素的顺序会影响其在`DockPanel`中的尺寸，而在`StackPanel`中不会受到影响。这是因为`StackPanel`在`PositiveInfinity`处定位堆叠方向，而`DockPanel`仅测量可用尺寸。

**声明和使用StackPanel**

下面的示例演示如何使用`StackPanel`创建一组垂直放置的按钮。如果想要水平放置，请将`Orientation`属性设置为`Horizontal`。

C\#

```csharp
// 声明StackPanel
myStackPanel = new StackPanel();
myStackPanel.HorizontalAlignment = HorizontalAlignment.Left;
myStackPanel.VerticalAlignment = VerticalAlignment.Top;

// 声明子内容
Button myButton1 = new Button();
myButton1.Content = "Button 1";
Button myButton2 = new Button();
myButton2.Content = "Button 2";
Button myButton3 = new Button();
myButton3.Content = "Button 3";

// 将子元素添加到父StackPanel
myStackPanel.Children.Add(myButton1);
myStackPanel.Children.Add(myButton2);
myStackPanel.Children.Add(myButton3);
```

**WrapPanel** <a id="wrappanel-class"></a>

`WrapPanel`用于将子元素从左到右按顺序放置，当内容到达其父容器的边缘时，将剩余内容拆分到下一行，内容可以以水平或垂直方向放置。`WrapPanel`适用于简单的流动UI场景，它还能够统一其所有子元素的尺寸大小。

下面的示例演示如何创建`WrapPanel`用以显示`Button`控件，这些控件在到达容器边缘时换行。

C\#

```csharp
// 实例化WrapPanel并设置属性
myWrapPanel = new WrapPanel();
myWrapPanel.Background = System.Windows.Media.Brushes.Azure;
myWrapPanel.Orientation = Orientation.Horizontal;
myWrapPanel.Width = 200;
myWrapPanel.HorizontalAlignment = HorizontalAlignment.Left;
myWrapPanel.VerticalAlignment = VerticalAlignment.Top;

// 声明3个按钮元素。最后三个按钮的宽度为75，因此第四个按钮将换行到下一行。
btn1 = new Button();
btn1.Content = "Button 1";
btn1.Width = 200;
btn2 = new Button();
btn2.Content = "Button 2";
btn2.Width = 75;
btn3 = new Button();
btn3.Content = "Button 3";
btn3.Width = 75;
btn4 = new Button();
btn4.Content = "Button 4";
btn4.Width = 75;

// 使用Children.Add方法将按钮添加到父WrapPanel。
myWrapPanel.Children.Add(btn1);
myWrapPanel.Children.Add(btn2);
myWrapPanel.Children.Add(btn3);
myWrapPanel.Children.Add(btn4);
```

XAML

```markup
<Border HorizontalAlignment="Left" VerticalAlignment="Top" BorderBrush="Black" BorderThickness="2">
  <WrapPanel Background="LightBlue" Width="200" Height="100">
    <Button Width="200">Button 1</Button>
    <Button>Button 2</Button>
    <Button>Button 3</Button>
    <Button>Button 4</Button>
  </WrapPanel>
</Border>
```

### 嵌套Panel元素 <a id="nested-panel-elements"></a>

`Panel`元素可以相互嵌套，以生成复杂的布局。在一个`Panel`非常适合UI的一部分，但无法满足UI的不同部分的需求的情况下非常有用。

应用程序可以支持的嵌套数量没有实际限制，但是，通常情况下最好将应用程序限制使用为实际需要的`Panel`。在多数情况下，由于布局容器`Grid`元素的灵活性，可以使用它来代替嵌套Panel。这可以通过将不必要的元素排除在树之外来提高应用程序的性能。

