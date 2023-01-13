# Windows

`Window` 是 Avalonia 的顶层控件。

`Window` 由两部分组成：[XAML 文件](../../guides/basics/introduction-to-xaml.md) （例如 `MainWindow.axaml`）和[代码隐藏](../../guides/basics/code-behind.md) 文件（例如 `MainWindow.axaml.cs`）。代码隐藏文件中定义了一个表示窗体的 .NET 类。

想要了解更多信息和示例，请浏览 [`Window`](../controls/window.md) 控件。

默认应用程序模板创建一个名为 `MainWindow` 的 `Window`。你也可以从模板创建其他 `Window`：

### Visual Studio <a id="visual-studio"></a>

1. 右键单击解决方案资源管理器中要添加窗体的文件夹
2. 选择 “添加->新增项”
3. 在出现的对话框中，导航到类别树中的 "Avalonia" 部分
4. 选择 “Window \(Avalonia\)”
5. 在“名称”栏输入窗体名称
6. 单击“添加”按钮

### .NET Core 命令行接口 <a id="net-core-cli"></a>

运行这条命令，将 `[namespace]` 替换为要在其中创建窗体的命名空间，`[name]` 替换为窗体的名称

```bash
dotnet new avalonia.window -na [namespace] -n [name]
```

想要了解更多信息，请浏览 [.NET core 模板代码库](https://github.com/AvaloniaUI/avalonia-dotnet-templates/).

