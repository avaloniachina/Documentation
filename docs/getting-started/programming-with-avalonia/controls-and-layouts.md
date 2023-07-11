# 控制和布局

### 控件 <a id="controls"></a>

Avalonia 提供了许多核心控件。以下是一些最常见的：

* 按钮：[`Button`](../../controls/buttons/button.md), [`RepeatButton`](../../controls/buttons/repeatbutton.md)
* 数据展示：[`ItemsControl`](../../controls/itemscontrol.md), [`ItemsRepeater`](../../controls/itemsrepeater.md), [`ListBox`](../../controls/listbox.md), [`TreeView`](../../controls/treeview.md)
* 输入：[`CheckBox`](../../controls/checkbox.md), [`ComboBox`](../../controls/combobox.md), [`RadioButton`](../../controls/buttons/radiobutton.md), [`Slider`](../../controls/slider.md), [`TextBox`](../../controls/textbox.md)
* 布局：[`Border`](../../controls/border.md), [`Canvas`](../../controls/canvas.md), [`DockPanel`](../../controls/dockpanel.md), [`Expander`](../../controls/expander.md), [`Grid`](../../controls/grid.md), [`GridSplitter`](../../controls/gridsplitter.md), [`Panel`](../../controls/panel.md), [`Separator`](../../controls/separator.md), [`ScrollBar`](../../controls/scrollbar.md), [`ScrollViewer`](../../controls/scrollviewer.md), [`StackPanel`](../../controls/stackpanel.md), [`Viewbox`](../../controls/viewbox.md), [`WrapPanel`](../../controls/wrappanel.md)
* 菜单：[`ContextMenu`](../../controls/contextmenu.md), [`Menu`](../../controls/menu.md), [`NativeMenu`](../../controls/nativemenu.md)
* 导航: [`TabControl`](../../controls/tabcontrol.md), [`TabStrip`](../../controls/tabstrip.md)
* 用户信息：[`ProgressBar`](../../controls/progressbar.md), [`TextBlock`](../../controls/textblock.md), [`ToolTip`](../../controls/tooltip.md)

## 输入和命令

控件经常需要检测和响应用户输入。Avalonia 的 [输入系统](../../input/) 使用 [直接路由事件](../../input/routed-events.md) 支持文本输入、焦点管理和鼠标定位功能。

应用程序通常有复杂的输入要求。Avalonia 提供了一个 [命令系统](../../data-binding/binding-to-commands.md)，将用户输入的动作与响应这些动作的代码分开。

### 布局 <a id="layout"></a>

创建用户界面时，可以按控件的位置和大小加以排列形成布局。任何布局的关键要求是适应窗体大小和显示设置的变化。Avalonia 提供了一流的、可扩展的布局系统，不通过强迫编写代码来适应这些情况下的布局。

相对定位是布局系统的基石，它能适应不断变化的窗口和显示条件。此外，确定布局依靠布局系统协调管理控件。协调是一个两步过程：第一步，子控件告诉其父控件子控件需要的位置和大小；第二步，父控件告诉子控件子控件允许有多少空间。

布局系统通过 Avalonia 基础类型暴露给子控件。对于 `grids`、`stacking` 和 `docking` 等常见布局，Avalonia 拥有多种布局控件。

* [`Panel`](../../controls/panel.md): 子控件用堆叠在一起的方式填充面板
* [`DockPanel`](../../controls/dockpanel.md): 子控件与 `panel` 边缘对齐
* [`StackPanel`](../../controls/stackpanel.md): 子控件垂直或水平堆叠
* [`WrapPanel`](../../controls/wrappanel.md): 当某一行上的控件数量超过空间允许的数量时，子控件将按从左到右的顺序排列并换行
* [`Grid`](../../controls/grid.md): 子控件按行和列定位
* [`Canvas`](../../controls/canvas.md): 子控件提供自己的布局

你还可以通过从 [`Panel`](../../controls/panel.md) 类派生来创建自己的布局。

