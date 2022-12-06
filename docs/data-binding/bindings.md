# 绑定

你可以使用`{Binding}`标记拓展在XAML中进行绑定。通过绑定，所有数据上下文（假设你已经为其实现了[变化通知](https://docs.avaloniaui.net/docs/data-binding/change-notifications)）的变化都将自动更新到控件上。

绑定默认是[`DataContext`的属性](https://docs.avaloniaui.net/docs/data-binding/the-datacontext)。例如:

```markup
<!-- 绑定 TextBlock 的 DataContext.Name 属性 -->
<TextBlock Text="{Binding Name}"/>

<!-- 如下等同 ('Path' 是可选的) -->
<TextBlock Text="{Binding Path=Name}"/>
```

空白的绑定是绑定在`DataContext`本身

```markup
<!-- 绑定至 TextBlock 的 DataContext 属性 -->
<TextBlock Text="{Binding}"/>

<!-- 等同如下 -->
<TextBlock Text="{Binding .}"/>
```

我们将控件的属性称为绑定的 _目标_，而`DataContext`的属性称为绑定的 _源_

## 绑定路径 <a id="binding-path"></a>

绑定的路径可以是一个属性，也可以是连续调用的一串属性。例如，如果`DataContext`所赋值有`Student`属性，而这个属性的值有`Name`属性，那么你可以用如下方法绑定学生的名字：

```markup
<TextBlock Text="{Binding Student.Name}"/>
```

你还可以在绑定路径中使用列表或数组的索引器

```markup
<TextBlock Text="{Binding Students[0].Name}"/>
```

## 绑定模式 <a id="binding-modes"></a>

你可以通过指定`{Binding}`的`Mode`来修改绑定的行为:

```markup
<TextBlock Text="{Binding Name, Mode=OneTime}">
```

可用的绑定模式包括:

| 模式 | 详情 |
| :--- | :--- |
| `OneWay` | 源的变化自动传播到目标 |
| `TwoWay` | 源和目标的变化互相传播 |
| `OneTime` | 源的初始值会应用到目标，但后续的变化会被无视 |
| `OneWayToSource` | 目标的变化自动传播到源 |
| `Default` | 依据属性而定 |

如果没有设定模式，则默认为`Default`模式。通常情况下如果控件属性不需要相应用户输入则是`OneWay`（例如`TextBlock.Text`）；如果需要依用户输入而变则是`TwoWay`（例如`TextBox.Text`）。

## 格式字符串 <a id="binding-stringformat"></a>

你可以为绑定设置格式字符串，从而影响在UI上如何展现值：

```markup
<!-- 选项 1: 使用花括号对格式字符串进行转义 -->
<TextBlock Text="{Binding FloatValue, StringFormat={}{0:0.0}}" />

<!-- 选项 2: 使用反斜线对格式字符串进行转义 -->
<TextBlock Text="{Binding FloatValue, StringFormat=\{0:0.0\}}" />

<!-- 选项 3: 如果格式字符串并非以{0}开始，则不需要转义 -->
<!-- 注意：如果在格式字符串中存在空格，则需要用单引号''包裹起来 -->
<TextBlock Text="{Binding Animals.Count, StringFormat='I have {0} animals.'}" />
<!-- 注意: 如果格式字符串以{0}开始则需要转义，例如：-->
<TextBlock Text="{Binding Animals.Count, StringFormat='{}{0} animals live in the farm.'}" />
```

当`StringFormat`属性存在时，绑定的值将会由`StringFormatValueConverter`利用指定的格式字符串进行转换。

与WPF不同，你需要将格式字符串用花括号包裹起来并以`0:`开始（`{0:TheStringFormat}`）。如果格式字符串中花括号出现在开始，那么即使用单引号包裹起来，也需要进行转义。可以在前面加`{}`，也可以使用反斜线`\{...\}`：

**WPF:**

```markup
<TextBlock Text="{Binding FloatValue, StringFormat=0.0}" />
```

**Avalonia:**

```markup
<TextBlock Text="{Binding FloatValue, StringFormat={}{0:0.0}}" />
<TextBlock Text="{Binding FloatValue, StringFormat=\{0:0.0\}}" />
<TextBlock Text="{Binding FloatValue, StringFormat='{}{0:0.0}'}" />
```

{% hint style="info" %} 
字符串格式详情请阅读[微软文档](https://docs.microsoft.com/en-us/dotnet/api/system.string.format)
{% endhint %}


## 示例

[Basic MVVM Sample](https://github.com/AvaloniaUI/Avalonia.Samples/tree/main/src/Avalonia.Samples/MVVM/BasicMvvmSample)
