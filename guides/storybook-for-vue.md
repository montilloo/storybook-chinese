# Vue Storybook

## 自动开始

您可能已尝试使用我们的快速入门指南来为Storybook设置项目。如果由于无法检测到您正在使用Vue而失败，则可以尝试强制其使用Vue：

```text
npx -p @storybook/cli sb init --type vue
```

## 手动设置

如果您想为自己的Vue项目手动设置Storybook，这是适合您的指南。

### **Step 1: 添加依赖**

#### **添加@storybook/vue**

```text
npm install @storybook/vue --save-dev
```

**添加同等依赖**

确保您在依赖项中也有`vue`, `vue-loader`, `vue-template-compiler`, `@babel/core`, `babel-loader` 和 `babel-preset-vue` 因为我们将它们列为对等依赖项：

```text
npm install vue --save
npm install vue-loader vue-template-compiler @babel/core babel-loader babel-preset-vue --save-dev
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

> 您可能正在使用全局组件或vue插件（例如vuex），在这种情况下，您需要将其注册到此Preview.js文件中。
>
> ```text
> import { configure } from '@storybook/vue';
>
> import Vue from 'vue';
>
> // Import Vue plugins
> import Vuex from 'vuex';
>
> // Import your global components.
> import Mybutton from '../src/stories/Button.vue';
>
> // Install Vue plugins.
> Vue.use(Vuex);
>
> // Register global components.
> Vue.component('my-button', Mybutton);
>
> configure(require.context('../src', true, /\.stories\.js$/), module);
> ```
>
> 本示例注册了您的自定义Button.vue组件，安装了Vuex插件，并加载了../src/index.stories.js中定义的Storybook stories。
>
> 在调用configure（）之前，应注册所有自定义组件和Vue插件。

### Step4: 撰写stories

现在创建一个`../src/index.stories.js`文件，并像这样写第一个story：

```text
import Vue from 'vue';
import MyButton from './Button.vue';

export default { title: 'Button' };

export const withText = () => '<my-button>with text</my-button>';

export const withEmoji = () => '<my-button>😀 😎 👍 💯</my-button>';

export const asAComponent = () => ({
  components: { MyButton },
  template: '<my-button :rounded="true">rounded</my-button>'
});
```



每个story都是组件的单个状态。在上述情况下，演示按钮组件有两个stories：

Button 

          ├── With Text 

          └── With Emoji

          └── As A Component

如果您的story返回的是纯模板，则只能使用全局注册的组件。要注册它们，请在`preview.js`文件中使用`Vue.component('my-button', Mybutton)`

如果您的story返回如下所示的纯字符串，则需要全局注册它使用的每个VueJs组件。

```text
export const withText = () => '<my-component>with text</my-component>';
```

在大型解决方案中，全局注册的组件可能会相互冲突。

这是在stories中使用组件而不进行全局注册的另外两种方法。

* 在vue组件对象的“ components”成员中本地注册组件。请参见“作为一个组成部分”。
* 使用如下所示的JSX渲染函数。无需注册任何东西。

### 最后：运行Storybook

现在一切就绪。使用以下命令运行您的Storybook：

```text
npm run storybook
```

Storybook应在开发模式下的随机开放端口上启动。

现在，由于Storybook使用Webpack的热模块重载功能，因此您可以开发自己的组件并编写故事并立即查看Storybook中的更改。

