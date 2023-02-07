# 转换绑定值

### 否定值 <a href="negating-values" id="negating-values"></a>

您经常需要对绑定的值取反。常见的用法有显示/隐藏控件或启用/禁用控件。您可以通过在绑定值前面加上运算符`!`来取反。

例如，您可能希望在禁用一个控件时显示另一个控件。

```markup
<StackPanel>
  <TextBox Name="input" IsEnabled="{Binding AllowInput}"/>
  <TextBlock IsVisible="{Binding !AllowInput}">Sorry, no can do!</TextBlock>
</StackPanel>
```

当绑定到非布尔值时，取反也有效。首先，使用`Convert.ToBoolean`将值转换为布尔值，并对结果取反。由于整数值`0`被视为`false`，而所有其他整数值都被视为`true`，因此当集合为空时，可以使用此方式显示消息：

```markup
<Panel>
  <ListBox Items="{Binding Items}"/>
  <TextBlock IsVisible="{Binding !Items.Count}">No results found</TextBlock>
</Panel>
```

双感叹号可用于将非布尔值转换为布尔值。例如，要在集合为空时隐藏控件：

```markup
<Panel>
  <ListBox Items="{Binding Items}" IsVisible="{Binding !!Items.Count}"/>
</Panel>
```

### 绑定转换器 <a href="binding-converters" id="binding-converters"></a>

对于更高级的转换，和其他XAML框架一样，Avalonia支持[`IValueConverter`](https://docs.microsoft.com/en-gb/dotnet/api/system.windows.data.ivalueconverter?view=netframework-4.7.1)

> 注意：`IValueConverter`接口在.NET标准2.0中不可用，因此我们在`Avalonia.Data.Converters`命名空间中提供了自己的接口。

用法与其他XAML框架相同：

```markup
<Window xmlns="https://github.com/avaloniaui"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="clr-namespace:ExampleApp;assembly=ExampleApp">

  <Window.Resources>
    <local:MyConverter x:Key="myConverter"/>
  </Window.Resources>

  <TextBlock Text="{Binding Value, Converter={StaticResource myConverter}}"/>
</Window>
```

### 内置转换器 <a href="built-in-converters" id="built-in-converters"></a>

Avalonia为常见场景提供了许多内置值转换器：

| 转换器                                 | 描述                                 |
|-------------------------------------|------------------------------------|
| `StringConverters.IsNullOrEmpty`    | 如果输入的字符串为null或empty，则返回`true`      |
| `StringConverters.IsNotNullOrEmpty` | 如果输入的字符串不为null或empty，则返回`true`     |
| `ObjectConverters.IsNull`           | 如果输入为null，则返回`true`                |
| `ObjectConverters.IsNotNull`        | 如果输入为null，则返回`false`               |
| `BoolConverters.And`                | 一种多值转换器，如果所有输入均为true，则返回`true`。    |
| `BoolConverters.Or`                 | 一种多值转换器，如果其中任意输入为true，则返回`true`。   |

您可以在此处查看默认转换器列表： [Avalonia.Data.Converters 命名空间](https://docs.avaloniaui.net/api/untitled/avalonia-ui-framework-23/avalonia-ui-framework-24#classtypes).

### 示例

如果绑定文本为null或empty，则隐藏`TextBlock`：

```markup
<TextBlock Text="{Binding MyText}"
           IsVisible="{Binding MyText, Converter={x:Static StringConverters.IsNotNullOrEmpty}}"/>
```

如果绑定内容为null或empty，则隐藏`ContentControl`：

```markup
<ContentControl Content="{Binding MyContent}"
                IsVisible="{Binding MyContent, Converter={x:Static ObjectConverters.IsNotNull}}"/>
```

> 从现在开始，假设转换器已导入，如上一节“绑定转换器”所示

根据不同参数将文本转换为特定大小写格式
```markup
<TextBlock Text="{Binding TheContent, 
    Converter={StaticResource textCaseConverter},
    ConverterParameter=lower}" />
```
```csharp
public class TextCaseConverter : IValueConverter
{
    public static readonly TextCaseConverter Instance = new();

    public object? Convert( object? value, Type targetType, object? parameter, CultureInfo culture )
    {
        if (value is string sourceText && parameter is string targetCase
            && targetType.IsAssignableTo(typeof(string)))
        {
            switch (targetCase)
            {
                case "upper":
                case "SQL":
                    return sourceText.ToUpper();
                case "lower":
                    return sourceText.ToLower();
                case "title": // 首字母大写
                    var txtinfo = new System.Globalization.CultureInfo("en-US",false).TextInfo;
                    return txtinfo.ToTitleCase(sourceText);
                default:
                    // 无效参数，在这里返回异常
                    break;
            }
        }
        // 用于错误类型的转换器
        return new BindingNotification(new InvalidCastException(), BindingErrorType.Error);
    }

    public object ConvertBack( object? value, Type targetType, object? parameter, CultureInfo culture )
    {
      throw new NotSupportedException();
    }
}
```

在上下文中将绑定对象转换为不同的目标类型

```markup
<Image Width="42" 
       Source="{Binding Animal, Converter={StaticResource animalConverter}}"/>
<TextBlock 
       Text="{Binding Animal, Converter={StaticResource animalConverter}}" />
```

```csharp
public class AnimalConverter : IValueConverter
{
    public static readonly AnimalConverter Instance = new();

    public object? Convert( object? value, Type targetType, object? parameter, CultureInfo culture )
    {
        if (value is Animal animal)
        {
            if (targetType.IsAssignableTo(typeof(IImage)))
            {
                img = @"icons/generic-animal-placeholder.png"
                switch (animal)
                {
                    case Dog d:
                      img = d.IsGoodBoy ? @"icons/dog-happy.png" : @"icons/dog.png";
                      break;
                    case Cat:
                      img = @"icons/cat.png";
                      break;
                    // 等等
                }
                // 请看 https://docs.avaloniaui.net/docs/controls/image
                return BitmapAssetValueConverter.Instance
                    .Convert(img, typeof(Bitmap), parameter, culture);
            }
            else if (targetType.IsAssignableTo(typeof(string)))
            {
                return !string.IsNullOrEmpty(animal.NickName) ? 
                    $"{animal.Name} \"{animal.NickName}\"" : animal.Name;
            }
        }
        // 用于错误类型的转换器
        return new BindingNotification(new InvalidCastException(), BindingErrorType.Error);
        
    }

    public object ConvertBack( object? value, Type targetType, object? parameter, CultureInfo culture )
    {
      throw new NotSupportedException();
    }
}
```

### 范例

[ValueConverter Sample](https://github.com/AvaloniaUI/Avalonia.Samples/tree/main/src/Avalonia.Samples/MVVM/ValueConversionSample)
