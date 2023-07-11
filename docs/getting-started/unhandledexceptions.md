# 未处理的异常

Avalonia 不提供任何全局异常处理并将其标记为已处理的机制。这是因为这样做无法知道异常是否已被正确处理，这样会导致应用程序可能处于不可用状态。如果应用程序可以处理异常，强烈建议在本地处理异常。总之，记录任何未处理的异常日志，之后支持和调试这些异常，这仍然是一个不错的选择。

## 记录日志

我们建议将异常日志记录到控制台、文件或其他地方。这里有几个可用的日志库：[Serilog](https://serilog.net) 和 [NLog](https://nlog-project.org).

## 全局 try-catch

您可以在 `Program.cs` 文件中从主线程（也是 Avalonia 中的 UI 线程）捕获任何异常。为此，只需将整个 `void Main` 包装在 `try` 和 `catch` 代码块中。在 `catch` 代码块中，您可以记录错误、通知用户、发送日志文件或重新启动应用程序。

```csharp
// 文件: Program.cs

public static void Main(string[] args)
{
    try
    {
        // 在此准备和运行您的应用
        BuildAvaloniaApp()
            .StartWithClassicDesktopLifetime(args);
    }
    catch (Exception e)
    {
        // 在此处理异常，例如新增到日志文件
        Log.Fatal(e, "Something very bad happened");
    }
    finally
    {
        // 这段代码块是可选项
        // 使用 finally 代码块释放资源
        Log.CloseAndFlush();
    }
}
```

## 来自其他线程的异常

如果您正在使用 `Task` 异步执行某些工作，则可以设置 `TaskScheduler.UnobservedTaskException`。想要了解更多信息，请浏览[Microsoft .NET 文档](https://docs.microsoft.com/dotnet/api/system.threading.tasks.taskscheduler.unobservedtaskexception)

## 来自Reactive UI 的异常

如果您将 Avalonia 与 [ReactiveUI](https://docs.avaloniaui.net/guides/basics/mvvm#frameworks) 一起使用，可以订阅他们的`RxApp.DefaultExceptionHandler`。想要了解更多信息，请浏览 [ReactiveUI Default Exception Handler](https://www.reactiveui.net/docs/handbook/default-exception-handler/)。

注意：请在创建任何 ReactiveCommand 之前设置 `RxApp.DefaultExceptionHandler`，否则将不会使用自定义处理器。
您可以在应用程序入口点或创建任何 Avalonia 视图或窗体之前设置它。
