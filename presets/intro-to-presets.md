# 预设介绍

Storybook预设是支持特定用例的babel，webpack和插件配置的分组集合。

例如，要使用Typescript编写stories，而不是使用单独的babel和webpack配置手动为Storyscript配置Storybook，您可以使用@ storybook / preset-typescript软件包，该软件包可以为您完成繁重的工作。

## 基本使用

每个预设都有其自己的安装说明，预设的思想是安装插件，然后加载其预设。

例如，要获得typescript支持，请先安装插件：

```text
yarn add @storybook/preset-typescript --dev
```

然后将其加载到您的Storybook文件夹中的main.js文件中（默认情况下为.storybook\)

```text
module.exports = {
  addons: ['@storybook/preset-typescript'],
};
```

Storybook启动时，它将自动配置为typescript，而无需任何进一步配置。有关更多信息，请参见Typescript预设[自述文件](https://github.com/storybookjs/presets/tree/master/packages/preset-typescript)。

## 预设配置

预设也可以采用可选参数。这些可以由预设本身使用，也可以通过配置来配置由预设使用的webpack加载程序。

考虑以下示例：

```text
const path = require('path');
module.exports = {
  addons: [
    {
      name: '@storybook/preset-typescript',
      options: {
        tsLoaderOptions: {
          configFile: path.resolve(__dirname, '../tsconfig.json'),
        },
        include: [path.resolve(__dirname)],
      },
    },
  ],
};
```

这会使用应用程序的tsconfig.json配置typescript加载程序，并告知typescript加载程序仅应用于当前目录。

每个预设都有自己的选项，这些选项应记录在预设的自述文件中。

## 深入

要查看可用的预设，请参阅预设库。要了解有关预设如何工作以及如何编写自己的内容的更多信息，请参阅编写预设。

