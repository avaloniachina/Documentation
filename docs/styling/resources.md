# 资源

通常，样式和控件需要共享资源，比如（但不限于）笔刷(brush)和颜色(color)。你可以把这些资源放在每个样式和控件共有的 `Resources`字典里，然后在其他地方引用这些资源。

## 定义资源 <a id="declaring-resources"></a>

如果一个资源要作用在整个应用程序上，你可以在`App.xaml`/`App.axaml`中定义它。

```markup
<Application xmlns="https://github.com/avaloniaui"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             x:Class="MyApp.App">
  <Application.Resources>
    <SolidColorBrush x:Key="Warning">Yellow</SolidColorBrush>
  </Application.Resources>
</Application>
```

另外，你可以在`Window`或`UserControl`上声明资源：该资源将可用于`Window`/`UserControl`及其子代。

```markup
<UserControl xmlns="https://github.com/avaloniaui"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             x:Class="MyApp.MyUserControl">
  <UserControl.Resources>
    <SolidColorBrush x:Key="Warning">Yellow</SolidColorBrush>
  </UserControl.Resources>
</UserControl>
```

或者实际上对任何控件都可用：

```markup
<Window xmlns="https://github.com/avaloniaui"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             x:Class="MyApp.MainWindow">
  <StackPanel>
    <StackPanel.Resources>
      <SolidColorBrush x:Key="Warning">Yellow</SolidColorBrush>
    </StackPanel.Resources>
  </StackPanel>
</Window>
```

你也可以在样式上声明资源。

```markup
<Style Selector="TextBlock.warn">
  <Style.Resources>
    <SolidColorBrush x:Key="Warning">Yellow</SolidColorBrush>
  </Style.Resources>
</Style>
```

## 引用资源 <a id="referencing-resources"></a>

你可以使用`{DynamicResource}`标记扩展从控件中引用资源，例如：

```markup
<Border Background="{DynamicResource Warning}">
  Look out!
</Border>
```

另外，还有`StaticResource`标记扩展，与`DynamicResource`相比，它有一些限制：

* 它不会响应资源的变化
* 该资源需要在同一个XAML文件中声明。

作为交换，`StaticResource`不需要添加事件处理程序来监听资源的变化，这意味着它使用的内存较少。

## 覆盖资源 <a id="overriding-resources"></a>

通过从`DynamicResource`或`StaticResource`节点向上遍历逻辑树，直到找到满足要求的键(key)的资源。这意味着资源可以在应用程序的子树中被**覆盖**，例如：

```markup
<UserControl xmlns="https://github.com/avaloniaui"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             x:Class="MyApp.MyUserControl">
  <UserControl.Resources>
    <SolidColorBrush x:Key="Warning">Yellow</SolidColorBrush>
  </UserControl.Resources>

  <StackPanel>
    <StackPanel.Resources>
      <SolidColorBrush x:Key="Warning">Orange</SolidColorBrush>
    </StackPanel.Resources>

    <Border Background="{DynamicResource Warning}">
      Look out!
    </Border>
  </StackPanel>
</UserControl>
```

这里`Border`的背景色是`Orange`，因为它的父级`StackPanel`已经**覆盖**了从`UserControl`上声明的`Yellow`中的`Warning`资源。

## 合并的资源字典 <a id="merged-resource-dictionaries"></a>

每个控件和样式的`Resources`属性的类型为`ResourceDictionary`。通过使用`MergedDictionaries`属性，资源字典还可以包括其他资源字典。要在另一个资源字典中包括资源字典，可以使用`ResourceInclude`类，例如：

```markup
<Window.Resources>
  <ResourceDictionary>
    <ResourceDictionary.MergedDictionaries>
      <ResourceInclude Source='/AnotherResourceDictionary.xaml'/>
    </ResourceDictionary.MergedDictionaries>
    <SolidColorBrush x:Key="Warning">Yellow</SolidColorBrush>
  </ResourceDictionary>
</Window.Resources>
```

其中，`AnotherResourceDictionary`是根为`ResourceDictionary`的XAML文件，并作为[资产(asset)](../getting-started/assets.md)包含在应用程序中。

## 资源解析 <a id="resource-resolution"></a>

如上所述，通过从`DynamicResource`或`StaticResource`节点向上遍历逻辑树来解析资源，直到找到满足条件的键(key)的资源。然而，样式和合并字典的存在使这一点有些复杂。优先级如下：

* 控件资源(Control resources)
  * 合并字典(Merged dictionaries)
* 样式资源(Style resources)
  * 合并字典(Merged dictionaries)

如下所示的应用程序，`Border`控件上定义的资源的查找顺序将遵循`[]`括号中指示的顺序：

```text
Application
 |- Resources [11]
     |- Merged dictionary [12]
     |- Merged dictionary [13]
 |- Styles
     |- Resources [14]
         |- Merged dictionary [15]
         |- Merged dictionary [16]

Window
 |- Resources [6]
     |- Merged dictionary [7]
 |- Styles
     |- Resources [8]
         |- Merged dictionary [9]
         |- Merged dictionary [10]
 |- StackPanel
     |- Resources [1]
         |- Merged dictionary [2]
         |- Merged dictionary [3]
     |- Styles
         |- Resources [4]
             |- Merged dictionary [5]
     |- Border
```

## 主题资源 <a id="theme-resources"></a>

主题通常将笔刷、颜色和其他设置定义为资源。通过改变这些资源，例如可以从暗黑主题切换到明亮主题。定义的资源通常特定于使用中的主题，但您可以[在这里](https://github.com/AvaloniaUI/Avalonia/blob/master/src/Avalonia.Themes.Simple/Accents/Base.xaml)看到默认主题定义的资源。

