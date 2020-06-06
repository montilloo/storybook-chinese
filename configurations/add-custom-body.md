# 添加自定义Body

有时，您可能需要向HTML主体添加不同的标签。这对于添加一些自定义内容根很有用。

您可以通过在Storybook的配置目录中创建一个名为`Preview-body.html`的文件并添加如下标签来完成此操作：

```text
<div id="custom-root"></div>
```

如果在项目中使用相对大小调整（例如rem或em），则可以通过在`Preview-body.html`中添加`style`标签来更新基本字体大小：

```text
<style>
  body {
    font-size: 15px;
  }
</style>
```

就是这样，Storybook会将这些标签注入html正文。

> **重要**
>
> Storybook会将这些标签注入渲染组件的iframe中。因此，这些内容不会加载到主Storybook UI中。



