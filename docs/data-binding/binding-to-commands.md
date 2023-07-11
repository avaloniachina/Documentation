# 绑定到命令

执行操作的控件，如[`Button`](http://reference.avaloniaui.net/api/Avalonia.Controls/Button/4AAA993D)具有可以绑定到[`ICommand`](https://docs.microsoft.com/en-gb/dotnet/api/system.windows.input.icommand?view=netstandard-2.0)的`Command`属性。激活控件时（例如单击按钮时），将调用`ICommand.Execute`方法。

在ReactiveUI的[`ReactiveCommand`](https://reactiveui.net/docs/handbook/commands/)中可以找到`ICommand`的良好实现。如果您使用[Avalonia MVVM应用程序](https://docs.avaloniaui.net/tutorials/todo-list-app/creating-a-new-project#net-core-cli)创建了应用程序模板，则默认情况下可用。请参阅[ReactiveUI](https://reactiveui.net/docs/handbook/commands/)文档获取更多信息。

一个例子:

```csharp
namespace Example
{
    public class MainWindowViewModel : ViewModelBase
    {
        public MainWindowViewModel()
        {
            DoTheThing = ReactiveCommand.Create(RunTheThing);
        }

        public ReactiveCommand<Unit, Unit> DoTheThing { get; }

        void RunTheThing()
        {
            // 此处执行命令的代码。
        }
    }
}
```

```markup
<Window xmlns="https://github.com/avaloniaui">
    <Button Command="{Binding DoTheThing}">Do the thing!</Button>
</Window>
```

## CommandParameter <a id="commandparameter"></a>

也可以使用`CommandParameter`属性将参数传递给命令：

```csharp
namespace Example
{
    public class MainWindowViewModel : ViewModelBase
    {
        public MainWindowViewModel()
        {
            DoTheThing = ReactiveCommand.Create<string>(RunTheThing);
        }

        public ReactiveCommand<string, Unit> DoTheThing { get; }

        void RunTheThing(string parameter)
        {
            // 此处执行命令的代码。
        }
    }
}
```

```markup
<Window xmlns="https://github.com/avaloniaui">
    <Button Command="{Binding DoTheThing}" CommandParameter="Hello World">Do the thing!</Button>
</Window>
```

注意：不会对`CommandParameter`执行类型转换，因此如果您需要的类型参数不是`string`，则必须在XAML中提供该类型的对象。例如，要传递`int`参数，可以使用：

```markup
<Window xmlns="https://github.com/avaloniaui"
        xmlns:sys="clr-namespace:System;assembly=mscorlib">
    <Button Command="{Binding DoTheThing}">
        <Button.CommandParameter>
            <sys:Int32>42</sys:Int32>
        </Button.CommandParameter>
        Do the thing!
    </Button>
</Window>
```

与任何其他属性一样，`CommandParameter`也可以被绑定。

## 绑定到方法 <a id="binding-to-methods"></a>

### ICommand.Execute <a id="icommandexecute"></a>

有时，您只想在单击按钮时调用一个方法，而不想专门创建命令，你也可以这样做！

```csharp
namespace Example
{
    public class MainWindowViewModel : ViewModelBase
    {
        public void RunTheThing(string parameter)
        {
            // 此处执行命令的代码。
        }
    }
}
```

```markup
<Window xmlns="https://github.com/avaloniaui">
  <Button Command="{Binding RunTheThing}" CommandParameter="Hello World">Do the thing!</Button>
</Window>
```

### ICommand.CanExecute <a id="icommandcanexecute"></a>

如果需要使执行依赖于CommandParameter或ViewModel属性，则可以定义由前缀“Can”和执行方法名称组成的命名方法；该方法将接受作为CommandParameter的对象参数，并返回一个布尔值。

```csharp
namespace Example
{
    public class MainWindowViewModel : ViewModelBase
    {
        public void RunTheThing(string parameter)
        {
            // 此处执行命令的代码。
        }

        bool CanRunTheThing(/* CommandParameter */object parameter)
        {
            return parameter != null;
        }
    }
}
```

**触发ICommand.CanExecute**

如果要从ViewModel中触发CanExecute，则必须用一个或多个DependsOn属性来修饰它，其中，当CanExecute方法更改值时，propertyName是将激活该方法的属性的名称。

```csharp
namespace Example
{
    public class MainWindowViewModel : ViewModelBase
    {
        bool _isTheThingEnabled = true;

        bool IsTheThingEnabled
        {
            get
            {
               return  _isTheThingEnabled;
            }
            set
            {
                if(value == _isTheThingEnabled)
                   return;
                _isTheThingEnabled = value;
                PropertyChanged?
                    .Invoke(this, new PropertyChangedEventArgs(nameof(IsTheThingEnabled)));
            }
        }

        public void RunTheThing(string parameter)
        {
            // 此处执行命令的代码。
        }

        [DependsOn(nameof(IsTheThingEnabled))]
        bool CanRunTheThing(/* CommandParameter */object parameter)
        {
            return IsTheThingEnabled && parameter != null;
        }
    }
}
```

## 范例

[Commands Example](https://github.com/AvaloniaUI/Avalonia.Samples/tree/main/src/Avalonia.Samples/MVVM/CommandSample)
