# 使用环境变量

有时，我们可能会在Storybook中使用配置项。它可能是主题颜色，某些客户端secret或JSON字符串。因此，我们通常倾向于对它们进行硬编码。

但是您可以通过“environment variables.”暴露那些配置。为此，您需要为环境变量加上STORYBOOK\_前缀。

例如，让我们暴露两个这样的环境变量：

```text
STORYBOOK_THEME=red STORYBOOK_DATA_KEY=12345 npm run storybook
```

然后，我们可以在JS代码内的任何位置访问这些环境变量，如下所示：

```text
const out = console;

out.log(process.env.STORYBOOK_THEME);
out.log(process.env.STORYBOOK_DATA_KEY);
```

> 即使我们可以在客户端JS代码中的任何位置访问这些env变量，最好只在stories内部和主Storybook配置文件中使用它们。

## 在自定义 head/body中使用

这些环境变量可以在custom head和custom body文件中使用。

Storybook将用百分号分隔的变量名替换为其值；例如`％STORYBOOK_THEME％`将变为`red`。

如果将环境变量用作JavaScript中的属性或值，则可能需要添加引号，因为该值将直接插入。例如:

```text
<link rel="stylesheet" href="%STORYBOOK_STYLE_URL%" />
```

## 构建时传递环境变量

使用`build-storybook`构建storybook时，也可以传递这些环境变量。

然后，它们将被硬编码为您的Storybook的静态版本。

### NODE\_ENV env 变量 <a id="node_env-env-variable"></a>

除了上述带前缀的环境变量外，您还可以将NODE\_ENV变量传递给Storybook。但是，您通常不需要这样做，因为我们为此设置了合理的默认值。

* 在运行`npm run storybook`时，我们将NODE\_ENV设置为“development”。
* 在构建Storybook时，我们将NODE\_ENV设置为“production”。



