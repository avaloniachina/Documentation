# 绑定到任务和可观察对象

您可以使用`^`流绑定运算符订阅任务或可观察对象。

## 示例1：绑定到可观察对象

例如，如果`DataContext.Name`是一个`IObservable<string>`，那么下面的示例将绑定到每个值生成时可观察对象的每个字符串的长度。

```markup
<TextBlock Text="{Binding Name^.Length}"/>
```

## 示例2: 绑定到任务

如果需要做一些繁重的工作来加载属性的内容，则可以绑定到`async Task<TResult>`的结果。

假设您有下列的视图模型，它在一个耗时的任务中生成一些文本：

```csharp
public Task<string> MyAsyncText => GetTextAsync();

private async Task<string> GetTextAsync()
{
  await Task.Delay(1000); // 延时只是为了演示
  return "Hello from async operation";
}
```

您可以通过以下方式绑定到结果：
```markup
<TextBlock Text="{Binding MyAsyncText^, FallbackValue='Wait a second'}" />
```


{% hint style="info" %}
注：您可以使用`FallbackValue`显示一些加载指示符。 
{% endhint %}


