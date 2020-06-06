# 导出为静态应用

Storybook通过其开发时的迅捷（例如通过Webpack的HMR进行热更新）为开发人员带来了绝佳的体验。

但是Storybook还是一个可以用来向他人展示组件的工具。 React Native Web和React Dates的演示就是一个很好的例子。

为此，Storybook附带了一个将您的Storybook导出到静态Web应用程序的工具。然后，您可以将其部署到GitHub页面或任何静态托管服务。

添加以下NPM脚本：

```text
{
  "scripts": {
    "build-storybook": "build-storybook -c .storybook -o .out"
  }
}
```

然后，运行`yarn build-storybook`.

这会将Storybook目录中配置的storybook构建到静态Web应用程序中，并将其放置在`.out`目录中。现在，您可以部署.out目录中的内容到您需要的地方。

如果在本地测试：

```text
npx http-server .out
```

## 部署到Github  Pages

此外，您可以使用我们的[Storybook-deployer](https://github.com/storybookjs/storybook-deployer)工具将Storybook直接部署到GitHub Pages中。

或者，您可以将故事书导出到docs目录中，并将其用作GitHub Pages的根目录。请参阅本[指南](https://github.com/blog/2233-publish-your-project-documentation-with-github-pages)以获取更多信息。

##  部署到Vercel

Vercel是一个用于托管静态站点和具备Serverless的云平台，您可以使用该平台将Storybook项目部署到您的个人域（或免费的.now.sh后缀URL）。  
  
要将您的Storybook项目部署到Vercel，只需连接Git存储库并导入该项目。导入时将自动检测到构建命令，项目目录和项目类型。

如果您不仅仅在存储库中使用Storybook项目，而且只想为您的部署构建Storybook，请确保将package.json文件中的构建脚本设置为以下内容：  


```text
`"build": "build-storybook -c .storybook -o build"`
```

  
导入后，将创建一个部署。从现在开始，每当您进行`git push`操作时，都会创建一个新的`Preview Deployment`。如果推送或合并到默认分支，将触发生产部署。

