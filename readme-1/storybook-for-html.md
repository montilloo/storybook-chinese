# HTML Storybook

## 自动开始

您可能已尝试使用我们的快速入门指南来为Storybook设置项目。如果由于无法检测到您无法链接到html而失败，则可以尝试强制其使用html：

```text
npx -p @storybook/cli sb init --type html
```

## 手动设置

如果要为您的html项目手动设置Storybook，这是适合您的指南。

### Step 1: 添加依赖 <a id="step-1-add-dependencies"></a>

#### 初始化npm

如果您的项目中没有`package.json`，则需要首先对其进行初始化：

```text
npm init
```

#### 添加@storybook/html

将@ storybook / html添加到您的项目。为此，请运行：

```text
npm install @storybook/html --save-dev
```

#### 添加@babel/core 和 babel-loader

确保您的依赖项中也有`@ babel / core`和`babel-loader`，因为我们将它们列为对等依赖项：

```text
npm install babel-loader @babel/core --save-dev 
```

### Step 2: 添加npm script <a id="step-2-add-a-npm-script"></a>

然后，将以下NPM脚本添加到`package.json`中，以便在本指南的后面部分启动：

```text
{
  "scripts": {
    "storybook": "start-storybook"
  }
}
```

### Step 3: 创建main文件

对于一个基本的Storybook配置，您唯一需要做的就是告诉Storybook在哪里找到stories。

为此，请在`.storybook / main.js`中创建一个包含以下内容的文件：

```text
module.exports = {
  stories: ['../src/**/*.stories.[tj]s'],
};
```

这将在../src目录下加载与模式`*.stories.[tj]s`匹配的所有stories。我们建议您将故事与源文件放在同一位置，但是您可以将它们放置在任意位置。

### Step 4: 撰写stories <a id="step-4-write-your-stories"></a>

现在创建一个`../src/index.stories.js`文件，并像这样写第一个story：

```text
export default { title: 'Button' };

export const withText = () => '<button class="btn">Hello World</button>';

export const withEmoji = () => {
  const button = document.createElement('button');
  button.innerText = '😀 😎 👍 💯';
  return button;
};
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







