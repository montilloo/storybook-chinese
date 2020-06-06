# 自定义Babel配置

默认情况下，Storybook会加载您的.babelrc根文件并加载这些配置。但是有时其中一些选项可能会导致Storybook引发错误。

在这种情况下，您可以仅为Storybook提供自定义.babelrc。为此，请在Storybook配置目录（默认为.storybook）中创建一个名为.babelrc文件。

然后，Storybook将仅从该文件加载Babel配置。

> 目前，我们不支持从package.json加载Babel配置。

