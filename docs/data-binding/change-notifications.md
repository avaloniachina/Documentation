# 变化通知

### 属性变化 <a id="property-changes"></a>

为了让Avalonia知晓视图模型中的属性已经发生了变化，视图模型必须要实现变化通知。最简单的方法是使你的视图模型类继承`ReactiveUI`中的`ReactiveObject`。

然后在属性的setter中调用`RaiseAndSetIfChanged`：

```csharp
using ReactiveUI;

public class MyViewModel : ReactiveObject
{
    private string caption;

    public string Caption
    {
        get => caption;
        set => this.RaiseAndSetIfChanged(ref caption, value);
    }
}
```

详情请参考[ReactiveUI文档](https://reactiveui.net/docs/handbook/view-models/).

如果你不想引入对ReactiveUI的依赖，可以自己手动实现[`INotifyPropertyChanged`](https://docs.microsoft.com/en-us/dotnet/api/system.componentmodel.inotifypropertychanged).

### 集合变化 <a id="collection-changes"></a>

集合也需要实现变化通知。这里有一些开箱即用的集合类：

* [ObservableCollection](https://docs.microsoft.com/en-us/dotnet/api/system.collections.objectmodel.observablecollection-1) 在.NET的基础类库中
* [DynamicData](https://github.com/reactiveui/DynamicData) 可以用在更加高级的场景
* [ReactiveList](https://reactiveui.net/docs/handbook/obsolete/collections/reactive-list) 在ReactiveUI中 \(已被DynamicData淘汰\)
* Avalonia中搭载了`AvaloniaList`，但API在未来可能会更新，当下不建议使用

如果你想要自己实现集合变化通知，你可以手动实现[`INotifyCollectionChanged`](https://docs.microsoft.com/en-us/dotnet/api/system.collections.specialized.inotifycollectionchanged)
