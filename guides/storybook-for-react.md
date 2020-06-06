# React Storybook

## 自动开始

在尝试以下命令之前，应尝试以下命令。在大多数情况下，Storybook会检测到您正在使用`react`或`react-scripts`，并安装适当的软件包。

```text
npx -p @storybook/cli sb init
```

您可能已尝试使用我们的快速入门指南来为Storybook设置项目。如果由于无法检测到您正在使用React而失败，则可以尝试强制其使用React：

```text
npx -p @storybook/cli sb init --type react
```

如果您使用的是Create React App（或React脚本的分支），则应改用以下命令：

```text
npx -p @storybook/cli sb init --type react_scripts
```

注意：您的项目中必须包含package.json，否则上述命令将失败。

## 手动设置

如果您想为自己的React项目手动设置Storybook，这是适合您的指南。

**给创建React App用户的注释**

现在，您可以使用`@ storybook / preset-create-react-app`代表您配置Storybook。这是由Storybook在自动设置（Storybook 5.3或更高版本）期间安装的。

### **Step 1: 添加依赖**

#### **添加@storybook/react**

```text
npm install @storybook/react --save-dev
```

**添加react, react-dom, @babel/core, 和 babel-loader**

确保您在依赖项中也有`react`，`react-dom`，`@ babel / core`和`babel-loader`，因为我们将它们列为对等依赖项：

```text
npm install react react-dom --save
npm install babel-loader @babel/core --save-dev
```

### Step 2: 添加npm script <a id="step-2-add-an-npm-script"></a>

然后，将以下NPM脚本添加到`package.json`以便在本指南的后面部分启动：

```text
module.exports = {
  stories: ['../src/**/*.stories.[tj]s'],
};
```

### Step 3: 创建main文件 <a id="step-3-create-the-main-file"></a>

对于基本的Storybook配置，您唯一需要做的就是告诉Storybook在哪里找到stories。

为此，请在`.storybook / main.js`中创建一个包含以下内容的文件：

```text
module.exports = {
  stories: ['../src/**/*.stories.[tj]s'],
};
```

这将在../src目录下加载与模式`*.stories.[tj]s`匹配的所有stories。我们建议您将故事与源文件放在同一位置，但是您可以将它们放置在任意位置。

### Step4: 撰写stories

现在创建一个`../src/index.stories.js`文件，并像这样写第一个story：

```text
import React from 'react';
import { Button } from '@storybook/react/demo';

export default { title: 'Button' };

export const withText = () => <Button>Hello Button</Button>;

export const withEmoji = () => (
  <Button>
    <span role="img" aria-label="so cool">
      😀 😎 👍 💯
    </span>
  </Button>
);
```

每个story都是组件的单个状态。在上述情况下，演示按钮组件有两个stories：

Button 

          ├── With Text 

          └── With Emoji



### 最后：运行Storybook

现在一切就绪。使用以下命令运行您的Storybook：

```text
npm run storybook
```

Storybook应在开发模式下的随机开放端口上启动。

现在，由于Storybook使用Webpack的热模块重载功能，因此您可以开发自己的组件并编写故事并立即查看Storybook中的更改。

