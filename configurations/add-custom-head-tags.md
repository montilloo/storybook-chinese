# 添加自定义Head标签

有时，您可能需要向HTML头添加不同的标签。这对于添加Web字体或某些外部脚本很有用。

您可以通过在Storybook的配置目录中创建一个名为Preview-head.html的文件并添加如下标签来实现此目的：

```text
<script src="https://use.typekit.net/xxxyyy.js"></script>
<script>try{ Typekit.load(); } catch(e){ }</script>
```

> Storybook会将这些标签注入渲染组件的iframe中。因此，这些内容不会加载到主Storybook UI中。

## 将Tags或Scripts添加到Main UI

另外，您可能需要向主Storybook UI添加不同的脚本或标签。当您的自定义Webpack配置输出或需要其他块时，可能会出现这种情况。在Storybook的配置目录内创建一个名为`manager-head.html`的文件，并添加所需的所有标签。

> **重要**
>
> 您的脚本将在Storybook的React UI之前运行。另请注意，这是一种不常见的情况，并有可能破坏Storybook的UI。因此，请谨慎使用。

