# 应用生命周期

平台生而不平等。我们在 Windows 窗体和 WPF 中习惯的生命周期管理只能在桌面风格的平台上工作。AvaloniaUI 是一个跨平台框架，为了确保应用程序的可移植性，我们为应用提供了不同的生命周期模型，并在平台允许的情况下能手动控制一切。

## 生命周期是如何运作的?

在桌面上初始化应用程序的首选方法是：

```csharp
class Program
{
  // This method is needed for IDE previewer infrastructure
  public static AppBuilder BuildAvaloniaApp() 
    => AppBuilder.Configure<App>().UsePlatformDetect();

  // The entry point. Things aren't ready yet, so at this point
  // you shouldn't use any Avalonia types or anything that expects
  // a SynchronizationContext to be ready
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

特定的生命周期可能在不同的方面起作用，有一组接口可以访问它们。

### IControlledApplicationLifetime

规定：

- `StartWithClassicDesktopLifetime`
- `StartLinuxFramebuffer`

订阅 `Startup` `Exit` 事件，并通过调用 `Shutdown` 方法显式关闭应用程序。还提供控制应用程序的退出代码。

### IClassicDesktopStyleApplicationLifetime

继承：`IControlledApplicationLifetime`

规定:

- `StartWithClassicDesktopLifetime`

允许使用 WindowsForms/WPF 方式控制应用程序的生命周期。提供一种方法，能够访问当前打开的窗口列表、设置主窗口和下列3种关闭模式：

- OnLastWindowClose - 关闭最后一个窗体时关闭应用程序
- OnMainWindowClose - 如果设置了MainWindow，则在关闭 MainWindow 时关闭应用程序
- OnExplicitShutdown - 禁用应用程序的自动关闭，需要手动调用 shutdown

### ISingleViewApplicationLifetime

规定:

- `StartLinuxFramebuffer`
- 移动平台 (WIP)

有些平台没有桌面上 `Window` 的概念，只允许同时在屏幕上显示一个视图。对于此类平台，生命周期允许您设置和更改 `MainView`。目前，我们没有提供堆栈导航器(navigation stack)的实现，但你可以使用 ReactiveUI 中的堆栈导航器。

## 手动管理生命周期

我们不会强迫你在平台上使用我们的生命周期模型。在桌面平台上，你可以将 `AppMain` 委托传递给 `BuildAvaloniaApp.Start` 手动管理：

之后将会提供更多的文档，目前请浏览：[Issue #2564](https://github.com/AvaloniaUI/Avalonia/issues/2564) 和  [PR 2676](https://github.com/AvaloniaUI/Avalonia/pull/2676)

```csharp
class Program
{
  // This method is needed for IDE previewer infrastructure
  public static AppBuilder BuildAvaloniaApp() 
    => AppBuilder.Configure<App>().UsePlatformDetect();

  // The entry point. Things aren't ready yet, so at this point
  // you shouldn't use any Avalonia types or anything that expects
  // a SynchronizationContext to be ready
  public static int Main(string[] args) 
    => BuildAvaloniaApp().Start(AppMain, args);

  // Application entry point. Avalonia is completely initialized.
  static void AppMain(Application app, string[] args)
  {
     // A cancellation token source that will be used to stop the main loop
     var cts = new CancellationTokenSource();
     
     // Do you startup code here
     new Window().Show();

     // Start the main loop
     app.Run(cts.Token);
  }
}
```
