# 应用生命周期

平台生而不平等。我们在 WinForms 和 WPF 中习惯的生命周期管理只能在桌面风格的平台上工作。AvaloniaUI 是一个跨平台框架，为了确保应用程序的可移植性，我们为应用提供了不同的生命周期模型，并在平台允许的情况下能手动控制一切。

## 生命周期是如何运作的?

在桌面上初始化应用程序的首选方法是：

```csharp
class Program
{
  // 此方法被用在IDE预览器的基础结构上
  public static AppBuilder BuildAvaloniaApp() 
    => AppBuilder.Configure<App>().UsePlatformDetect();

  // 程序入口点。一切还没有准备就绪，
  // 因此在这里你不应该使用任何Avalonia的类型，
  // 也不要妄想SynchronizationContext已经就绪
  public static int Main(string[] args) 
    => BuildAvaloniaApp().StartWithClassicDesktopLifetime(args);
}
```

那么，主窗体设置在哪里？现在它被移动到应用程序类：

```csharp
public override void OnFrameworkInitializationCompleted()
{
  if (ApplicationLifetime is IClassicDesktopStyleApplicationLifetime desktop)
    desktop.MainWindow = new MainWindow();
  else if (ApplicationLifetime is ISingleViewApplicationLifetime singleView)
    singleView.MainView = new MainView();
  base.OnFrameworkInitializationCompleted();
}
```

当框架准备好使用并且 `ApplicationLifetime` 属性包含所选的生命周期（如果有的话）时，将调用此方法。如果应用程序由 IDE 预览器进程在设计模式下运行，则 `ApplicationLifetime` 为 `null` 。

## 生命周期类型

每种特定的生命周期有其独有的关注点，我们提供了一组接口用以访问。

### IControlledApplicationLifetime

由以下方法提供：

- `StartWithClassicDesktopLifetime`
- `StartLinuxFramebuffer`

你可以订阅 `Startup` `Exit` 事件，或通过调用 `Shutdown` 方法显式关闭应用程序。还提供控制应用程序的退出代码。

### IClassicDesktopStyleApplicationLifetime

继承：`IControlledApplicationLifetime`

由以下方法提供:

- `StartWithClassicDesktopLifetime`

允许使用 WinForms/WPF 的方式控制应用程序的生命周期。它提供一个方法，能够访问当前打开的窗口列表、设置主窗口及设置下列3种关闭模式：

- OnLastWindowClose - 关闭最后一个窗体时关闭应用程序
- OnMainWindowClose - 如果设置了MainWindow，则在关闭 MainWindow 时关闭应用程序
- OnExplicitShutdown - 禁用应用程序的自动关闭，需要手动调用 shutdown

### ISingleViewApplicationLifetime

由以下方法提供:

- `StartLinuxFramebuffer`
- 移动平台 (WIP)

有些平台没有桌面上 `Window` 的概念，只允许同时在屏幕上显示一个视图。对于此类平台，生命周期允许您设置和更改 `MainView`。目前，我们没有提供堆栈导航器(navigation stack)的实现，但你可以使用 ReactiveUI 中的堆栈导航器。

## 手动管理生命周期

我们不会强迫你在平台上使用我们的生命周期模型。在桌面平台上，你可以将 `AppMain` 委托传递给 `BuildAvaloniaApp.Start` 手动管理：

之后将会提供更多的文档，目前请浏览：[Issue #2564](https://github.com/AvaloniaUI/Avalonia/issues/2564) 和  [PR 2676](https://github.com/AvaloniaUI/Avalonia/pull/2676)

```csharp
class Program
{
  // 此方法被用在IDE预览器的基础结构上
  public static AppBuilder BuildAvaloniaApp() 
    => AppBuilder.Configure<App>().UsePlatformDetect();

  // 程序入口点。一切还没有准备就绪，
  // 因此在这里你不应该使用任何Avalonia的类型，
  // 也不要妄想SynchronizationContext已经就绪
  public static int Main(string[] args) 
    => BuildAvaloniaApp().Start(AppMain, args);

  // 应用程序入口点。Avalonia已完全初始化。
  static void AppMain(Application app, string[] args)
  {
     // 用于停止主循环的取消令牌源
     var cts = new CancellationTokenSource();
     
     // 这里有启动代码吗
     new Window().Show();

     // 启动主循环
     app.Run(cts.Token);
  }
}
```
