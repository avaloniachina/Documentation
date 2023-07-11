# 资产

大量应用程序需要在其可执行文件中包含位图和 [资源字典](../styling/resources.md) 等资产，并在 XAML 中引用这些资产。

### 使用资产 <a id="including-assets"></a>

通过使用项目文件中的 `<AvaloniaResource>` 项，可以在应用程序中使用资产。MVVM 应用程序模板默认将 `Assets` 目录中的所有文件视为 `<AvaloniaResource>`：

```markup
<ItemGroup>
  <AvaloniaResource Include="Assets\**"/>
</ItemGroup>
```

你可以通过添加其他 `<AvaloniaResource>` 元素来使用所需的任何文件。

注意到，我们在这里提到的是**资产** (Assets) ，而 MSBuild 项称其为 Avalonia **资源** (resource)。资产在内部存储为[.NET 资源](https://docs.microsoft.com/zh-cn/visualstudio/ide/managing-application-resources-dotnet)。但因为术语“资源”与[XAML 资源](../styling/resources.md)冲突，因此我们统一将其称为“资产”。

### 引用资产 <a id="referencing-assets"></a>

资产可以通过指定资产的相对路径在 XAML 中引用：

```markup
<Image Source="icon.png"/>
<Image Source="images/icon.png"/>
<Image Source="../icon.png"/>
```

或者在资产的根目录路径下：

```markup
<Image Source="/Assets/icon.png"/>
```

如果资产位于与 XAML 文件不同的程序集中，则使用 `avares:` URI 方案。例如，如果资产包含在名为 `MyAssembly.dll` 的程序集中，则可以这样使用：

```markup
<Image Source="avares://MyAssembly/Assets/icon.png"/>
```

Avalonia 提供了转换器，可以开箱即用地加载位图、图标和字体的资源。因此，资产 Uri 可以被自动转换为以下任何类型：[IImage](http://reference.avaloniaui.net/api/Avalonia.Media/IImage)，[IBitmap](http://reference.avaloniaui.net/api/Avalonia.Media.Imaging/IBitmap)，[WindowIcon](http://reference.avaloniaui.net/api/Avalonia.Controls/WindowIcon) 和 [FontFamily](http://reference.avaloniaui.net/api/Avalonia.Media/FontFamily)

### 引用清单资源 <a id="referencing-manifest-resources"></a>

使用“`<EmbeddedResource>` MSBuild”项将资产引用在.NET应用程序中，该项会导致文件作为[清单资源](https://docs.microsoft.com/zh-cn/dotnet/api/system.reflection.assembly.getmanifestresourcenames) 引用在程序集中。

使用“`resm:`”URL 方案引用清单资源：

```markup
<Image Source="resm:MyApp.Assets.icon.png"/>
```

或者将资源嵌入到 XAML 文件的其他程序集中：

```markup
<Image Source="resm:MyApp.Assets.icon.png?assembly=MyAssembly"/>
```

资产的名称由 MSBuild 根据程序集名称、文件路径和文件名自动生成，这些部分都用句点分隔。如果 Avalonia 找不到清单资源，请使用 [`Assembly.GetManifestResourceNames`](https://docs.microsoft.com/zh-cn/dotnet/api/system.reflection.assembly.getmanifestresourcenames) 检查资源名称。

### 在代码中加载资产 <a id="loading-assets-from-code"></a>

使用 `IAssetLoader` 接口在代码中加载资产：

```csharp
var assets = AvaloniaLocator.Current.GetService<IAssetLoader>();
var bitmap = new Bitmap(assets.Open(new Uri(uri)));
```

代码片段中的 `uri` 变量可以包含任何有效的 URI，包括之前提到的 `avares:` 和 `resm:`。默认情况下，Avalonia 不支持 `file://`、`http://` 和 `https://` 方案。如果你想从磁盘或 web 中加载文件，你可以自己实现该功能或者参考社区里的实现例子，比如 https://github.com/AvaloniaUtils/AsyncImageLoader.Avalonia 。
