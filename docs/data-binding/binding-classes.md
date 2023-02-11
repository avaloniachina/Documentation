# 绑定Class

在Avalonia中，您还可以绑定Classes。有时，根据某些逻辑切换类可能很有用，为此，可以使用Binding Classes API。下面是绑定Classes的示例用法：这里有两种不同的样式，我们希望根据`MyProperty`的状态在它们之间切换。

```markup
 <ListBox Items="{Binding MyItems}">
    <ListBox.Styles>
        <Style Selector="TextBlock.myClass">
            <Setter Property="Background" Value="Red" />
        </Style>
        <Style Selector="TextBlock.myClass2">
            <Setter Property="Background" Value="Green" />
        </Style>
    </ListBox.Styles>
    <ListBox.ItemTemplate>
        <DataTemplate>
            <StackPanel>
                <TextBlock
                    Classes.myClass="{Binding MyProperty}"
                    Classes.myClass2="{Binding !MyProperty}"
                    Text="{Binding Name}"/>
            </StackPanel>
        </DataTemplate>
    </ListBox.ItemTemplate>
 </ListBox>
```

当您绑定到Classes时，Avalonia将期望布尔值。此API在0.10.1中引入

