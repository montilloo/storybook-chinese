# 静态文件服务

## 托管静态文件

创建组件和stories时，加载图像和视频等静态文件通常很有用。

Storybook提供了两种方法

### 1.通过imports

您可以通过importing（或requeiring）它们来导入，如下所示。

```text
import React from 'react';
import imageFile from './static/image.png';

export default {
  title: 'img',
};

const image = {
  src: imageFile,
  alt: 'my image',
};

export const withAnImage = () => (
  <img src={image.src} alt={image.alt} />
);
```

这是通过我们的默认配置启用的。但是，如果您使用的是自定义Webpack配置，则需要将[file-loader](https://github.com/webpack/file-loader)添加到自定义Webpack配置中。

### 2.通过目录

启动Storybook时，还可以指定目录（或目录列表）以搜索静态内容。您可以使用-s标志来实现。

有关如何使用它，请参见以下npm脚本：

```text
{
  "scripts": {
    "start-storybook": "start-storybook -s ./public -p 9001"
  }
}
```

./public是我们的静态目录。现在，您可以在组件的公共目录或此类stories使用静态文件。

```text
import React from 'react';

export default {
  title: 'img',
};

// assume image.png is located in the "public" directory.
export const withAnImage = () => (
  <img src="/image.png" alt="my image" />
);
```

> 您还可以传递用逗号分隔的目录列表（不带空格），而不是单个目录。
>
> ```text
> {
>   "scripts": {
>     "start-storybook": "start-storybook -s ./public,./static -p 9001"
>   }
> }
> ```

#### 3.通过CDN

将文件上传到在线CDN并进行引用。在此示例中，我们使用占位符图像服务。

```text
import React from 'react';

export default {
  title: 'img',
};

// assume image.png is located in the "public" directory.
export const withAnImage = () => (
  <img src="https://placehold.it/350x150" alt="My CDN placeholder" />
);
```

## 绝对路径与相对路径

有时，您可能希望将storybook部署到子路径中，例如`https://example.com/storybook`。

在这种情况下，您需要使所有图像和媒体文件具有相对路径。否则，浏览器将找不到这些文件。

如果您通过导入加载静态内容，则这是自动的，您无需执行任何操作。

如果使用的是静态目录，则需要使用相对路径来加载图像或使用[基本元素](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/base)。







