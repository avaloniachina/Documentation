# 在控件模板中绑定

当您创建控件模板并希望绑定到模板的父级时，可以使用：

```markup
<TextBlock Name="tb" Text="{TemplateBinding Caption}"/>

<!-- 也能写成这样 -->
<TextBlock Name="tb" Text="{Binding Caption, RelativeSource={RelativeSource TemplatedParent}}"/>
```

尽管这里展示的两种语法在大多数情况下是等效的，但也存在一些差异：

1. `TemplateBinding`只接受单个属性而不是属性路径，因此如果要使用属性路径进行绑定，则必须使用第二种语法：

   ```markup
   <!-- 这不起作用，因为TemplateBinding只接受单个属性 -->
   <TextBlock Name="tb" Text="{TemplateBinding Caption.Length}"/>

   <!-- 在这种情况下，必须使用此语法 -->
   <TextBlock Name="tb" Text="{Binding Caption.Length, RelativeSource={RelativeSource TemplatedParent}}"/>
   ```

2. 出于性能原因，`TemplateBinding`仅支持单向绑定(`OneWay`)（[这与WPF相同](https://docs.microsoft.com/en-us/dotnet/desktop/wpf/advanced/templatebinding-markup-extension#remarks)）。这意味着`TemplateBinding`实际上等同于`{Binding RelativeSource={RelativeSource TemplatedParent}, Mode=OneWay}`。如果控件模板中需要双向绑定(`TwoWay`)，则需要完整的语法，如下所示。注意，与`TemplateBinding`不同，`Binding`也将使用默认绑定模式。

   ```markup
   {Binding RelativeSource={RelativeSource TemplatedParent}, Mode=TwoWay}
   ```

3. `TemplateBinding`只能用于`IStyledElement`.

   ```markup
   <!-- 这不起作用，因为GeometryDrawing不是IStyledElement -->
   <GeometryDrawing Brush="{TemplateBinding Foreground}"/>

   <!-- 在这种情况下，必须使用此语法 -->
   <GeometryDrawing Brush="{Binding Foreground, RelativeSource={RelativeSource TemplatedParent}}"/>
   ```
