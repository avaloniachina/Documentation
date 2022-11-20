# UserControls

`UserControl` 表示 Avalonia 中的一个“视图”，它是预定义布局中可重用的控件集合。

`UserControl` 通常由两部分组成：XAML文件（例如`MyUserControl.axaml`）和代码隐藏文件（例如 `MyUserControl.axaml.cs` ）。代码隐藏文件中定义了一个表示控件的 .NET 类。

在使用 MVVM 模式时，`UserControl` 通常与“视图模型”配合使用。想要了解更多信息，请浏览 [教程](../../tutorials/todo-list-app/).

你可以从模板创建 `UserControl`：

### Visual Studio <a id="visual-studio"></a>

1. 右键单击解决方案资源管理器中要添加控件的文件夹
2. 选择 “添加->新增项”
3. 在出现的对话框中，导航到类别树中的 "Avalonia" 部分
4. 选择 “UserControl \(Avalonia\)”
5. 在“名称”栏输入控件名称
6. 单击“添加”按钮

### .NET Core 脚手架 <a id="net-core-cli"></a>

运行这条命令，将 `[namespace]` 替换为要在其中创建 `UserControl` 的命名空间，`[name]` 替换为控件的名称。

```bash
dotnet new avalonia.usercontrol -p:n [namespace] -n [name]
```

想要了解更多信息，请浏览 [.NET core 模板代码库](https://github.com/AvaloniaUI/avalonia-dotnet-templates/).

