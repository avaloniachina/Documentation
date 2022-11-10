# JetBrains Rider 设置



1. 下载并安装 .NET 5 SDK [下载 .NET \(Linux, macOS, and Windows\) \(microsoft.com\)](https://dotnet.microsoft.com/download)

   这是用于构建 `Avalonia` 应用程序的运行时开发工具包（编译器等）。

2. 安装 Avalonia Templates

   运行命令 `dotnet new -i Avalonia.Templates` 从计算机上的命令提示符中选择模板

   输出类似如下：

   ```bash
   $ dotnet new -i Avalonia.Templates
     Determining projects to restore...
     Restored /Users/danwalmsley/.templateengine/dotnetcli/v5.0.200/scratch/restore.csproj (in 706 ms).

   Templates                                     Short Name            Language    Tags
   .....

   Avalonia Resource Dictionary                  avalonia.resource                 ui/xaml/avalonia/avaloniaui
   Avalonia Styles                               avalonia.styles                   ui/xaml/avalonia/avaloniaui

   Examples:
       dotnet new mvc --auth Individual
       dotnet new mstest
       dotnet new --help
       dotnet new avalonia.mvvm --help
   $
   ```

3. 下载并安装 [Rider: JetBrains 出品的跨平台.NET IDE](https://www.jetbrains.com/zh-cn/rider/)

   Rider 将为您提供 Avalonia 最佳的开发体验。它适用于 Windows、Linux 和 macOS 。

   Rider 支持 XAML 开箱即用。但是，如果您想使用 XAML 预览器，则需要使用 Avalonia 插件。

4. 安装 Avalonia 插件

   Rider 打开后，会看到欢迎页面。单击 `Configure` ，在下拉列表并选择 `Plugins` 。

![rider-welcome](<../../../.gitbook/assets/jetbrains-rider-setup-1-rider-welcome.png>)

这会打开一个新的 Preferences 页面。如图所示，单击 `Settings` 图标，然后选择 `Manage Plugin Repositories...`

![configure-plugin-repos](<../../../.gitbook/assets/jetbrains-rider-setup-2-configure-plugin-repos.png>)

单击 `+` 图标并输入 URL `https://plugins.jetbrains.com/plugins/dev/14839` 之后点击 `OK`:

![enter-plugin-repo](<../../../.gitbook/assets/jetbrains-rider-setup-3-enter-plugin-repo.png>)

现在单击 `Marketplace` 选项卡并搜索 `Avalonia`。选择 `AvaloniaRider` 并单击 `Install`；安装完成后，单击出现的 `Restart IDE` 按钮。

![plugin-install](<../../../.gitbook/assets/jetbrains-rider-setup-4-plugin-install.png>)

现在 Rider 已经能开发 Avalonia 应用程序了。
