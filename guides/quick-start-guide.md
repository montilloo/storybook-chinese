# 快速入门

Storybook支持许多不同的前端视图，将来还会有更多！当前支持React，Vue，Angular，Mithril，Marko，HTML，Svelte，Meteor，Ember，Riot和Preact。请按照以下步骤开始使用Storybook。

开始使用自动化命令行工具。此命令在您的项目中为Storybook添加了一组样板文件：

```text
cd my-project-directory
npx -p @storybook/cli sb init
```

该工具检查您的`package.json`，以确定您使用的是哪个前端视图。如果您想在Storybook中开发HTML代码片段，则无法自动确定。因此，要安装HTML的Storybook，请使用`--type`标志强制设置HTML项目类型：

```text
npx -p @storybook/cli sb init --type html
```

如果我们的自动检测失败，这也很有用。

默认情况下，npx将使用最新版本，如果您想尝试下一个版本（或任何特定版本），则可以：

```text
npx -p @storybook/cli@5.0.0-rc.6 sb init
```

要手动设置项目，请参阅[《慢速入门》](slow-start-guide.md)。

启动Storybook：

```text
npm run storybook
```

故事书现在应该可以根据控制台中提供的链接在浏览器中使用。

要了解有关Storybook CLI命令`sb init`命令的功能的更多信息，请查看慢速入门指南：

* [React](https://storybook.js.org/docs/guides/guide-react/)
* [React Native](https://storybook.js.org/docs/guides/guide-react-native/)
* [Vue](https://storybook.js.org/docs/guides/guide-vue/)
* [Angular](https://storybook.js.org/docs/guides/guide-angular/)
* [Mithril](https://storybook.js.org/docs/guides/guide-mithril/)
* [Marko](https://storybook.js.org/docs/guides/guide-marko/)
* [HTML](https://storybook.js.org/docs/guides/guide-html/)
* [Svelte](https://storybook.js.org/docs/guides/guide-svelte/)
* [Ember](https://storybook.js.org/docs/guides/guide-ember/)
* [Riot](https://storybook.js.org/docs/guides/guide-riot/)
* [Preact](https://storybook.js.org/docs/guides/guide-preact/)

[学习Storybook](https://www.learnstorybook.com/)中提供了分步教程。



