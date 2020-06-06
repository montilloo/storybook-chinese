# 独立配置

还有一个API，用于从Node运行Storybook。

```text
const storybook = require('@storybook/{APP}/standalone');

storybook({
  // options
});
```

其中APP是受支持的应用之一。例如：

```text
const storybook = require('@storybook/react/standalone');

storybook({
  // options
});
```

## Mode

模式定义了将应用哪种Storybook模式：

### dev

在开发模式下运行Storybook-类似于CLI中的`start-storybook`

```text
const storybook = require('@storybook/react/standalone');

storybook({
  mode: 'dev',
  // other options
});
```

### static

构建Storybook的静态版本-类似于CLI中的`build-storybook`

```text
const storybook = require('@storybook/react/standalone');

storybook({
  mode: 'static',
  // other options
});
```

其他选项与CLI中的选项相似。

## For “dev” mode:

```text
port [number]           Port to run Storybook
host [string]           Host to run Storybook
staticDir <dir-names>   Directory where to load static files from, array of strings
configDir [dir-name]    Directory where to load Storybook configurations from
https                   Serve Storybook over HTTPS. Note: You must provide your own certificate information.
sslCa <ca>              Provide an SSL certificate authority. (Optional with "https", required if using a self-signed certificate)
sslCert <cert>          Provide an SSL certificate. (Required with "https")
sslKey <key>            Provide an SSL key. (Required with "https")
smokeTest               Exit after successful start
ci                      CI mode (skip interactive prompts, don't open browser)
quiet                   Suppress verbose build output
```

## For “static” mode:

```text
staticDir <dir-names>   Directory where to load static files from, array of strings
outputDir [dir-name]    Directory where to store built files
configDir [dir-name]    Directory where to load Storybook configurations from
watch                   Enable watch mode
quiet                   Suppress verbose build output
```

例子：

```text
const storybook = require('@storybook/angular/standalone');

storybook({
  mode: 'dev',
  port: 9009,
  configDir: './.storybook',
});
```



