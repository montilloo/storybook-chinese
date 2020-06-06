# 常见问题

以下是一些常见问题的答案。如果有问题，可以通过在[Storybook Repository](https://github.com/storybookjs/storybook/)上打开一个问题来提出。

## 如何使用Create React App运行覆盖率测试并撰写Stories？

`Create React App`不允许在`package.json`中为`Jest`提供参数，但是您可以使用命令行参数运行`jest`：

```text
npm test -- --coverage --collectCoverageFrom='["src/**/*.{js,jsx}","!src/**/stories/*"]'
```

#### I see `ReferenceError: React is not defined` when using storybooks with Next.js <a id="i-see-referenceerror-react-is-not-defined-when-using-storybooks-with-nextjs"></a>

Next通过babel插件自动为所有文件定义`React`。您必须定义`React`才能使JSX正常工作。您可以通过以下方法解决此问题：

1. 将`import React from 'React'`添加到您的组件文件中。
2. 添加一个包含`babel-plugin-react-require`的`.babelrc`文件

## 如何设置Storybook与Next.js共享Webpack配置？

通常，您可以通过将`webpack`规则放在`next.config.js`和`.storybook / main.js`文件中的`require（）-ed`文件中来重用它们。例如：

```text
module.exports = {
  webpackFinal: async (baseConfig) => {
    const nextConfig = require('/path/to/next.config.js');

    // merge whatever from nextConfig into the webpack config storybook will use
    return { ...baseConfig };
  },
};
```

## 我可以修改stories中的React组件状态吗？

不可直接修改。如果您具有对组件的完全控制权，可以执行以下操作：

```text
import React, { Component } from 'react';

export default {
  title: 'MyComponent',
};

class MyComponent extends Component {
  constructor(props) {
    super(props);

    this.state = {
      someVar: 'defaultValue',
      ...props.initialState,
    };
  }
  // ...
};

export const defaultView = () => <MyComponent initialState={} />;
```

