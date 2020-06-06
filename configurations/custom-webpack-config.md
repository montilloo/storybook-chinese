# 自定义Webpack配置

您可以通过在main.js文件中提供webpackFinal字段来自定义Storybook的webpack设置。该值应该是一个异步函数，该函数接收一个webpack配置，并最终返回一个webpack配置。  
  
Storybook具有自己的Webpack设置和开发服务器。 webpack配置是可配置的，默认设置取决于您使用的框架以及是否使用过诸如Create React App或Angular CLI等脚手架。

> 我们正在努力使storybook随着时间的推移更加零配置，非常欢迎您加入。

这是在开发人员模式下使用Create React App时，storybook的配置：

```text
{
  mode: 'development',
  bail: false,
  devtool: '#cheap-module-source-map',
  entry: [
    '@storybook/core/dist/server/common/polyfills.js',
    '@storybook/core/dist/server/preview/globals.js',
    '<your-storybook-dir>/preview.js',
    'webpack-hot-middleware/client.js?reload=true',
  ],
  output: {
    path: './',
    filename: '[name].[hash].bundle.js',
    publicPath: '',
  },
  plugins: [
    HtmlWebpackPlugin {
      options: {
        template: '@storybook/core/dist/server/templates/index.ejs',
        templateContent: false,
        templateParameters: [Function: templateParameters],
        filename: 'iframe.html',
        hash: false,
        inject: false,
        compile: true,
        favicon: false,
        minify: undefined,
        cache: true,
        showErrors: true,
        chunks: 'all',
        excludeChunks: [],
        chunksSortMode: 'none',
        meta: {},
        title: 'Webpack App',
        xhtml: false,
        alwaysWriteToDisk: true,
      },
    },
    DefinePlugin {
      definitions: {
        'process.env': {
          NODE_ENV: '"development"',
          NODE_PATH: '""',
          PUBLIC_URL: '"."',
          '<storybook-environment-variables>'
          '<dotenv-environment-variables>'
        },
      },
    },
    WatchMissingNodeModulesPlugin {
      nodeModulesPath: './node_modules',
    },
    HotModuleReplacementPlugin {},
    CaseSensitivePathsPlugin {},
    ProgressPlugin {},
    DefinePlugin {
      definitions: {
        '<storybook-environment-variables>'
        '<dotenv-environment-variables>'
      },
    },
  ],
  module: {
    rules: [
      { test: /\.(mjs|jsx?)$/,
        use: [
          { loader: 'babel-loader', options:
            { cacheDirectory: './node_modules/.cache/storybook',
              presets: [
                [ './node_modules/@babel/preset-env/lib/index.js', { shippedProposals: true, useBuiltIns: 'usage', corejs: '3' } ],
                './node_modules/@babel/preset-react/lib/index.js',
                './node_modules/@babel/preset-flow/lib/index.js',
              ],
              plugins: [
                './node_modules/@babel/plugin-proposal-object-rest-spread/lib/index.js',
                './node_modules/@babel/plugin-proposal-class-properties/lib/index.js',
                './node_modules/@babel/plugin-syntax-dynamic-import/lib/index.js',
                [ './node_modules/babel-plugin-emotion/dist/babel-plugin-emotion.cjs.js', { sourceMap: true, autoLabel: true } ],
                './node_modules/babel-plugin-macros/dist/index.js',
                './node_modules/@babel/plugin-transform-react-constant-elements/lib/index.js',
                './node_modules/babel-plugin-add-react-displayname/index.js',
                [ './node_modules/babel-plugin-react-docgen/lib/index.js', { DOC_GEN_COLLECTION_NAME: 'STORYBOOK_REACT_CLASSES' } ],
              ],
            },
          },
        ],
        include: [ './' ],
        exclude: [ './node_modules' ],
      },
      { test: /\.md$/,
        use: [
          { loader: './node_modules/raw-loader/index.js' },
        ],
      },
      { test: /\.css$/,
        use: [
          './node_modules/style-loader/index.js',
          { loader: './node_modules/css-loader/dist/cjs.js', options: { importLoaders: 1 } },
          { loader: './node_modules/postcss-loader/src/index.js', options: { ident: 'postcss', postcss: {}, plugins: [Function: plugins] } },
        ],
      },
      { test: /\.(svg|ico|jpg|jpeg|png|gif|eot|otf|webp|ttf|woff|woff2|cur|ani)(\?.*)?$/,
        loader: './node_modules/file-loader/dist/cjs.js',
        query: { name: 'static/media/[name].[hash:8].[ext]' },
      },
      { test: /\.(mp4|webm|wav|mp3|m4a|aac|oga)(\?.*)?$/,
        loader: './node_modules/url-loader/dist/cjs.js',
        query: { limit: 10000, name: 'static/media/[name].[hash:8].[ext]' },
      },
    ],
  },
  resolve: {
    extensions: [ '.mjs', '.js', '.jsx', '.json' ],
    modules: [ 'node_modules' ],
    mainFields: [ 'browser', 'main', 'module' ],
    alias: {
      'core-js': './node_modules/core-js',
      react: './node_modules/react',
      'react-dom': './node_modules/react-dom',
    },
  },
  optimization: {
    splitChunks: { chunks: 'all' },
    runtimeChunk: true,
    minimizer: [ [Object] ],
  },
  performance: { hints: false },
}
```

#### 调试默认的webpack配置

为了有效地自定义webpack配置，您可能需要获取其使用的完整默认配置。

* 创建`.storybook/main.js`文件
* 编辑  module.exports = { webpackFinal: \(config\) =&gt; console.dir\(config, { depth: null }\) \|\| config, };
* 运行 yarn storybook --debug-webpack

控制台应记录整个配置，以供您检查。

## 示例

该值应导出一个函数，它是第一个参数就是stroybook将使用的配置（如果您不自定义它的话）。第二个参数是storybook中的一个options对象，它将提供有关配置来自何处，我们是否处于开发模式等方面的信息。

例如，这是一个`.storybook / main.js`来添加Sass支持：

```text
const path = require('path');

// Export a function. Accept the base config as the only param.
module.exports = {
  webpackFinal: async (config, { configType }) => {
    // `configType` has a value of 'DEVELOPMENT' or 'PRODUCTION'
    // You can change the configuration based on that.
    // 'PRODUCTION' is used when building the static version of storybook.

    // Make whatever fine-grained changes you need
    config.module.rules.push({
      test: /\.scss$/,
      use: ['style-loader', 'css-loader', 'sass-loader'],
      include: path.resolve(__dirname, '../'),
    });

    // Return the altered config
    return config;
  },
};
```

Storybook使用从上述函数返回的配置，在Storybook的“预览” iframe中呈现您的组件。请注意，Storybook对于其自己的UI（也称为“manager”）具有完全独立的webpack配置，因此您进行的自定义仅适用于stories的呈现，即，您可以根据需要完全替换`config.module.rules`。

不过，请谨慎编辑配置。确保保留以下配置选项：

* entry
* output

此外，`config`需要使用`HtmlWebpackplugin`来生成预览页面，因此相比覆盖`config.plugins`，您更不应该覆盖它（或小心覆盖），有关示例，请参见[issue＃6020](https://github.com/storybookjs/storybook/issues/6020)：

```text
module.exports = {
  webpackFinal: (config) => {
    config.plugins.push(...);
    return config;
  },
}
```

最后，如果您的自定义Webpack配置使用的加载程序未通过`test`属性明确包含特定文件扩展名，则有必要从该加载程序中`exclude` .ejs文件扩展名。

如果您使用的是非标准的Storybook配置目录，则应在其中放置main.js而不是.storybook并更​​新包含路径，以确保其解析为您的项目根目录。

## 使用现有配置

如果您的项目已有Webpack配置，并且想重复使用此应用程序的配置，则可以将主要Webpack配置导入Storybook的.storybook / main.js并合并这两个配置：

{% embed url="https://使用应用程序的webpack.config.js中的加载器替换storybook中的加载器的示例：" %}

```text
const path = require('path');

// your app's webpack.config.js
const custom = require('../webpack.config.js');

module.exports = {
  webpackFinal: (config) => {
    return { ...config, module: { ...config.module, rules: custom.module.rules } };
  },
};
```









