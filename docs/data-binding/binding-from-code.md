# 从代码绑定

Avalonia中的代码绑定与WPF/UWP的工作方式有些不同。在低级别上，Avalonia的绑定系统基于Reactive Extensions`IObservable`，然后由XAML绑定构建（也可以在代码中实例化）。

## 订阅对属性的更改 <a id="subscribing-to-changes-to-a-property"></a>

您可以通过调用`GetObservable`方法订阅对属性的更改。这将返回一个`IObservable<T>`，它可用于监听属性的更改：

```csharp
var textBlock = new TextBlock();
var text = textBlock.GetObservable(TextBlock.TextProperty);
```

每个可以被订阅的属性都有一个名为`[PropertyName]Property`的静态只读字段，该字段被传递给`GetObservable`以订阅对属性的更改。

`IObservable`（Reactive Extensions的一部分，或者简称rx）超出了本指南的范围，但这里有一个示例，它使用返回的可观察对象(observable)将带有更改属性值的消息打印到控制台：

```csharp
var textBlock = new TextBlock();
var text = textBlock.GetObservable(TextBlock.TextProperty);
text.Subscribe(value => Console.WriteLine(value + " Changed"));
```

当订阅了返回的可观察对象后，它将立即返回属性的当前值，然后在每次属性更改时推送一个新值。如果不需要当前值，可以使用rx `Skip`运算符：

```csharp
var text = textBlock.GetObservable(TextBlock.TextProperty).Skip(1);
```

## 绑定到可观察对象 <a id="binding-to-an-observable"></a>

您可以使用`AvaloniaObject.Bind`方法将属性绑定到可观察对象：

```csharp
// 我们在这里使用Rx Subject，以便我们可以使用OnNext推送新值
var source = new Subject<string>();
var textBlock = new TextBlock();

// 将TextBlock.Text绑定到源
var subscription = textBlock.Bind(TextBlock.TextProperty, source);

// 将textBlock.Text设置为“hello”
source.OnNext("hello");
// 将textBlock.Text设置为“world!”
source.OnNext("world!");

// 终止绑定
subscription.Dispose();
```

请注意，`Bind`方法返回可用于终止绑定的`IDisposable`。如果您从未调用过，那么绑定将在可观察对象通过`OnCompleted`或`OnError`完成时自动终止。

## 在对象初始值设定项中设置绑定 <a id="setting-a-binding-in-an-object-initializer"></a>

在对象初始化器中设置绑定通常很有用。可以使用索引器执行此操作：

```csharp
var source = new Subject<string>();
var textBlock = new TextBlock
{
    Foreground = Brushes.Red,
    MaxWidth = 200,
    [!TextBlock.TextProperty] = source.ToBinding(),
};
```

使用此方法，您还可以轻松地将一个控件上的属性绑定到另一个控件的属性：

```csharp
var textBlock1 = new TextBlock();
var textBlock2 = new TextBlock
{
    Foreground = Brushes.Red,
    MaxWidth = 200,
    [!TextBlock.TextProperty] = textBlock1[!TextBlock.TextProperty],
};
```

当然，索引器也可以在对象初始化器的外部使用：

```csharp
textBlock2[!TextBlock.TextProperty] = textBlock1[!TextBlock.TextProperty];
```

此语法的唯一缺点是不返回`IDisposable`。如果需要手动终止绑定，则应使用`Bind`方法。

## 转换绑定值 <a id="transforming-binding-values"></a>

因为我们使用的是可观测对象，所以我们可以很容易地转换绑定的值！

```csharp
var source = new Subject<string>();
var textBlock = new TextBlock
{
    Foreground = Brushes.Red,
    MaxWidth = 200,
    [!TextBlock.TextProperty] = source.Select(x => "Hello " + x).ToBinding(),
};
```

## 使用代码中的XAML绑定 <a id="using-xaml-bindings-from-code"></a>

有时，当您需要XAML绑定提供的其他功能时，从代码中使用XAML绑定会更简便。例如，仅使用可观测对象，就可以绑定到`DataContext`上的属性，如下所示：

```csharp
var textBlock = new TextBlock();
var viewModelProperty = textBlock.GetObservable(TextBlock.DataContext)
    .OfType<MyViewModel>()
    .Select(x => x?.Name);
textBlock.Bind(TextBlock.TextProperty, viewModelProperty);
```

但是，在这种情况下，最好使用XAML绑定：

```csharp
var textBlock = new TextBlock
{
    [!TextBlock.TextProperty] = new Binding("Name")
};
```

或者，如果需要`IDisposable`来终止绑定：

```csharp
var textBlock = new TextBlock();
var subscription = textBlock.Bind(TextBlock.TextProperty, new Binding("Name"));

subscription.Dispose();
```

## 订阅任意对象的属性 <a id="subscribing-to-a-property-on-any-object"></a>

`GetObservable`方法返回跟踪单个实例上属性更改的可观察对象。但是，如果正在编写控件，则可能需要实现未绑定到对象实例的`OnPropertyChanged`方法。

要做到这一点，您可以订阅[`AvaloniaProperty.Changed`](http://reference.avaloniaui.net/api/Avalonia/AvaloniaProperty/65237C52)，这是一个可观察对象，**在任何实例上更改属性时都会触发**。

> 在WPF中，这是通过将静态`PropertyChangedCallback`传递给`DependencyProperty`注册方法来完成的，但这只允许控件开发者注册属性更改的回调。

此外，还有一个`AddClassHandler`扩展方法，它可以自动将事件路由到控件上的方法。

例如，如果您想监听控件的`Foo`属性的更改，可以这样做：

```csharp
static MyControl()
{
    FooProperty.Changed.AddClassHandler<MyControl>(x => x.FooChanged);
}

private void FooChanged(AvaloniaPropertyChangedEventArgs e)
{
    // e参数描述了更改的内容。
}
```

## 绑定到`INotifyPropertyChanged`对象

还可以绑定到实现了`INotifyPropertyChanged`的对象。

```csharp 
var textBlock = new TextBlock();

var binding = new Binding 
{ 
    Source = someObjectImplementingINotifyPropertyChanged, 
    Path = nameof(someObjectImplementingINotifyPropertyChanged.MyProperty)
}; 

textBlock.Bind(TextBlock.TextProperty, binding);
```
