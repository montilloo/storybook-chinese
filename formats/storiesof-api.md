# StoriesOf API

storyOf是Storybook的API，用于添加stories。直到Storybook 5.2为止，它一直是在Storybook中创建stories的主要方法。

在Storybook 5.2中，我们引入了更简单，更可移植的Component Story Format，并且所有未来的工具和基础结构都将面向CSF。因此，我们建议您从StoryOfAPI中迁移您的stories，甚至提供[自动化工具](https://storybook.js.org/docs/formats/storiesof-api/#component-story-format-migration)来执行此操作。

也就是说，不推荐使用storyOfOf API。目前，大多数现有的Storybook都使用storiesOf API，因此，我们计划在可预见的将来为其提供支持。

## storiesOf API

Storybook是stories的集合。每个story代表组件的单个视觉状态。有关storybook story概念的概述，请参见“[编写stories](../basics/bian-xie-stories.md)”。

这是StoryOf API中的基本story文件：

```text
import React from 'react';
import { storiesOf } from '@storybook/react';
import { action } from '@storybook/addon-actions';
import Button from '../components/Button';

storiesOf('Button', module)
  .add('with text', () => <Button onClick={action('clicked')}>Hello Button</Button>)
  .add('with some emoji', () => (
    <Button onClick={action('clicked')}>
      <span role="img" aria-label="so cool">
        😀 😎 👍 💯
      </span>
    </Button>
  ));
```

`storyOf`的字符串参数是组件标题。如果您传递“ Widgets \| Button / Button”之类的字符串，它也可以用于将组件的story放置在Storybook的层次结构中。每个.add调用都带有一个story名称，一个返回可渲染对象的story函数（在React中为JSX）以及一些可选参数，如下所述。

## 装饰器和参数

可以在全局组件级或在story级本地指定装饰器和参数。

全局装饰器和参数在Storybook配置中指定：

```text
addDecorator((storyFn) => <blink>{storyFn()}</blink>);
addParameters({ foo: 1 });
```

链式API调用支持组件级装饰器和参数：

```text
storiesOf('Button', module).addDecorator(withKnobs).addParameters({ notes: someNotes });
```

story级参数作为.add的第三个参数提供

```text
storiesOf('Button', module).add(
  'with text',
  () => <Button onClick={action('clicked')}>Hello Button</Button>,
  { notes: someNotes }
);
```

最后，通过参数提供了story级装饰器：

```text
storiesOf('Button', module).add(
  'with text',
  () => <Button onClick={action('clicked')}>Hello Button</Button>,
  { decorators: [withKnobs] }
);
```

## CSF迁移

为了更轻松地采用新的组件故事格式（CSF），我们创建了一个自动迁移工具，可以将StoryOf API转换为模块格式。

```text
sb migrate storiesof-to-csf --glob=src/**/*.stories.js
```

有关更多信息，请参见CLI的[Codemod README](https://github.com/storybookjs/storybook/tree/next/lib/codemod)。









