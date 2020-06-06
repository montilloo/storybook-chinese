# 配置概览

有关CLI选项，请参见：[此处](https://storybook.js.org/docs/configurations/cli-options)。

迁移指南：此页面记录了5.3.0中最近引入的配置故事书的方法，如果要迁移到这种配置故事书的格式，请查阅[迁移指南](https://github.com/storybookjs/storybook/blob/next/MIGRATION.md)。

## 主要配置

Storybook有一些用于配置的文件，它们被组织到一个目录中（默认：`.storybook`）。

最重要的文件是main.js文件。这是声明常规配置的地方。

这是该文件的最小示例：

```text
module.exports = {
  stories: ['../src/components/**/*.stories.js'],
  addons: [
    '@storybook/addon-essentials',
  ],
};
```

stories是一个全局模式列表，用于告诉您的story相对于配置文件的位置。

`addons`字段可以引用传统插件，但也可以包含预设，这些预设可以进一步扩展配置。

**`main.js`是一个预设**

`main.js`文件实际上是一个预设！因此，如果您知道如何配置故事书，那么您就会知道如何编写预设，反之亦然！因此`main.js` API等于预设的API。

## 管理&预览

Storybook的工作方式分为两个应用程序（“manager”和“preview”），它们通过postMessage通道相互通信。

预览应用程序本质上只是带有框架无关的“router”的stories。它会呈现manager应用程序告诉它呈现的任何story。

manager应用程序呈现插件的UI，导航器和工具栏。

如果需要配置Manager或Preview的运行时，则有两个额外的配置文件。

{% embed url="https://在preview.js中，您可以添加全局装饰器和参数：" %}

```text
// preview.js
import { addDecorator } from '@storybook/svelte';
import { withA11y } from '@storybook/addon-a11y';

addDecorator(withA11y);
```

在`manager.js`中，您可以添加UI选项。

```text
// manager.js
import { themes } from '@storybook/theming/create';
import { addons } from '@storybook/addons';

addons.setConfig({
  theme: themes.dark,
});
```

## webpack

有关如何自定义Webpack的信息，请查看“[自定义Webpack](custom-webpack-config.md)”部分

