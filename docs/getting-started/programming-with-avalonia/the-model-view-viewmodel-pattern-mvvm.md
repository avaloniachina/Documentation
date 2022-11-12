# Model-View-ViewModel (MVVM)部分

除了在代码隐藏中编写代码之外，Avalonia 还支持使用 [Model-View-ViewModel](https://docs.avaloniaui.net/guides/basics/mvvm) （MVVM）模式。MVVM 是一种构建 UI 应用程序的常用方法，它将视图逻辑与应用程序逻辑分离开，这样应用程序就能够单元测试。

MVVM 依赖 Avalonia 的 [绑定](https://docs.avaloniaui.net/docs/data-binding/bindings) 功能将应用程序分为标准 Avalonia 窗体、管理控件的 View 层和独立于 Avalonia 本身、用来实现应用程序功能的 ViewModel 层。

下面的示例展示了使用 MVVM 模式实现的上一个示例中的代码：

{% tabs %}
{% tab title="XAML" %}
```markup
<Window xmlns="https://github.com/avaloniaui"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        x:Class="AvaloniaApplication1.MainWindow"
        Title="Window with Button"
        Width="250" Height="100">

  <!-- Add button to window -->
  <Button Content="{Binding ButtonText}" Command="{Binding ButtonClicked}"/>

</Window>
```
{% endtab %}

{% tab title="C\#" %}
```csharp
using System.ComponentModel;
using Avalonia.Controls;
using Avalonia.Markup.Xaml;

namespace AvaloniaApplication1
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
            DataContext = new MainWindowViewModel();
        }
    }

    public class MainWindowViewModel : INotifyPropertyChanged
    {
        string buttonText = "Click Me!";

        public string ButtonText
        {
            get => buttonText;
            set 
            {
                buttonText = value;
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(nameof(ButtonText)));
            }
        }

        public event PropertyChangedEventHandler PropertyChanged;

        public void ButtonClicked() => ButtonText = "Hello, Avalonia!";
    }
}
```
{% endtab %}
{% endtabs %}

在本例中，代码隐藏将 `Window` 类的 [`DataContext`](https://docs.avaloniaui.net/docs/data-binding/the-datacontext) 属性赋值给了 `MainWindowViewModel` 类的实例。然后 XAML 使用 Avalonia [`｛Binding｝`](https://docs.avaloniaui.net/docs/data-binding/bindings) 将 `Button` 的 `Content` 属性绑定到 `MainWindowViewModel` 上的 `Button Text` 属性。它还将 `Button` 的 [`Command`](https://docs.avaloniaui.net/docs/data-binding/binding-to-commands) 属性绑定到 `MainWindowViewModel` 上的 `Button Clicked` 方法。

单击 `Button` 控件时，它会执行其 `Command` 命令，调用已绑定的 `MainWindowViewModel.ButtonClicked` 方法。这个方法就会去设置 `ButtonText` 属性，这样就会触发 `INotifyPropertyChanged.PropertyChanged` 事件，导致 `Button` 控件重新读取其绑定值并更新 UI。

