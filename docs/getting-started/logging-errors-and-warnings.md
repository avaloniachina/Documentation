# 错误和警告日志

Avalonia 可以使用 [`System.Diagnostics.Trace`](https://docs.microsoft.com/zh-cn/dotnet/api/system.diagnostics.trace) 记录警告和错误。要启用日志记录，程序中需要调用 `LogToTrace` 方法。cs文件：

```csharp
public static AppBuilder BuildAvaloniaApp()
    => AppBuilder.Configure<App>()
        .UsePlatformDetect()
        .LogToTrace();
```

默认情况下，此日志记录设置会将严重性为 `Warning` 或更高的日志消息写入 `System.Diagnostics.Trace`。可以通过向 `LogToTrace()` 方法传递 `level` 参数来控制日志级别。

默认情况下，这些跟踪消息将被记录到 IDE 的输出窗口中。如果要将这些消息重新路由到不同的位置，请使用 [`System.Diagnostics.Trace`](https://docs.microsoft.com/zh-cn/dotnet/api/system.diagnostics.trace) 提供的 API

### Areas <a id="areas"></a>

每个 Avalonia 日志消息都有一个 `Area`，它用于过滤日志，只筛选你感兴趣的事件类型。这些 `Avalonia.Logging.LogArea` 静态类的成员有：

* `Property`
* `Binding`
* `Animations`
* `Visual`
* `Layout`
* `Control`

`LogToTrace` 方法用于指定记录特定 `Area`：

```csharp
public static AppBuilder BuildAvaloniaApp()
    => AppBuilder.Configure<App>()
        .UsePlatformDetect()
        .LogToTrace(LogEventLevel.Debug, LogArea.Property, LogArea.Layout);
```

