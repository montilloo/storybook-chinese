# Component Story Format \(CSF\)

从Storybook 5.2开始，推荐使用Storybook的组件故事格式（CSF）编写stories。

[阅读公告](https://medium.com/storybookjs/component-story-format-66f4c32366df)以了解更多信息。

在CSF中，故事和组件元数据被定义为ES模块。每个组件story文件均包含必需的默认导出和一个或多个命名导出。

除了React Native之外，所有框架都支持CSF，您应在该框架中使用storyOf API。

## 默认导出

默认导出定义有关组件的元数据，包括组件本身，标题（将在导航层次结构中显示的位置），装饰器和参数。组件字段是可选的（但鼓励使用！），插件使用该字段来自动生成prop table并显示组件其他元数据。标题应该是唯一的，即不能在文件之间重复使用。

```text
import MyComponent from './MyComponent';

export default {
  title: 'Path/To/MyComponent',
  component: MyComponent,
  decorators: [ ... ],
  parameters: { ... }
}
```

有关更多示例，请参见[编写故事](../basics/bian-xie-stories.md)。

## 命名story导出

使用CSF，默认情况下，文件中每个命名的导出都代表一个story功能。

```text
import MyComponent from './MyComponent';

export default { ... }

export const Basic = () => <MyComponent />;
export const WithProp = () => <MyComponent prop="value" />;
```

导出的标识符将使用Lodash的startCase函数转换为“首字母大写”。例如：

```text
name -> 'Name'
someName -> 'Some Name'
someNAME -> 'Some NAME'
some_custom_NAME -> 'Some Custom NAME'
someName1234 -> 'Some Name 1234'
someName1_2_3_4 -> 'Some Name 1 2 3 4
```

导出名称建议以大写字母开头。

Story 函数可以用`story`对象进行注释，以定义story级别的修饰符和参数，还可以定义story的名称。

如果要使用带有特殊字符的名称，与Javascript中受限制的关键字相对应的名称或与文件中其他变量冲突的名称，则该名称很有用。如果未指定，则将使用导出名称。

```text
export const Simple = () => <MyComponent />;

Simple.story = {
  name: 'So simple!',
  decorators: [ ... ],
  parameters: { ... }
};
```

## Storybook Export vs Name Handling

Storybook对命名的导出和story.name的处理略有不同。应该什么时候使用这一个或者另一个？

命名导出始终用于确定story ID / URL。

如果指定story.name，它将用作UI中的故事显示名称。如果您未指定story.name，则将使用命名导出来生成显示名称。Storybook通过storyNameFromExport函数（[＃7901](https://github.com/storybookjs/storybook/pull/7901)）传递命名的导出，该函数通过lodash.startCase实现：

```text
it('should format CSF exports with sensible defaults', () => {
  const testCases = {
    name: 'Name',
    someName: 'Some Name',
    someNAME: 'Some NAME',
    some_custom_NAME: 'Some Custom NAME',
    someName1234: 'Some Name 1234',
    someName1_2_3_4: 'Some Name 1 2 3 4',
  };
  Object.entries(testCases).forEach(([key, val]) => {
    expect(storyNameFromExport(key)).toBe(val);
  });
});
```

当您想更改story的名称时，只需重命名CSF导出即可。这将更改story的名称，并更改story的ID和URL。

在以下情况下，应该使用story.name:

1. 该名称以命名导出方式无法显示在Storybook UI中，例如保留关键字（例如“default”），特殊字符（如表情符号），空格/大写字母（不是storyNameFromExport提供的内容）
2. 您想独立保存story ID，而不更改其显示方式。拥有稳定的Story ID有助于与第三方工具集成。

## Non-story exports

在某些情况下，您可能希望导出story和non-story的混合体。例如，导出story中使用的数据会很有用。

为此，可以在默认导出中使用可选的includeStories和excludeStories配置字段，可以将其设置为字符串数组或正则表达式。

考虑以下story文件：

```text
import React from 'react';
import MyComponent from './MyComponent';
import someData from './data.json';

export default {
  title: 'MyComponent',
  component: MyComponent,
  includeStories: ['SimpleStory', 'ComplexStory']
}

export const simpleData = { foo: 1, bar: 'baz' };
export const complexData = { foo: 1, bar: { baz: someData } };

export const SimpleStory = () => <MyComponent data={simpleData} />;
export const ComplexStory = () => <MyComponent data={complexData} />;
```

当Storybook加载此文件时，它将看到所有导出，但将忽略数据导出，仅将SimpleStory和ComplexStory视为stories。

对于此特定示例，根据方便程度，可以通过几种方式获得等效的结果：

* `includeStories: ['SimpleStory', 'ComplexStory']`
* `includeStories: /.*Story$/`
* `excludeStories: ['simpleData', 'complexData']`
* `excludeStories: /.*Data$/`

