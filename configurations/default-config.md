# 默认配置

让我们了解Storybook随附的默认配置。

## Babel

我们使用Babel进行JavaScript（ES6）转换。这是Storybook的Babel配置的一些主要功能。

#### 支持ES2016+

我们在Babel中添加了ES2016支持，用于转译JS代码。除此之外，我们还添加了一些实验性功能，例如Object Spreading 和async await。查看我们的[资源](https://github.com/storybookjs/storybook/blob/master/lib/core/src/server/common/babel.js)以了解有关这些插件的更多信息。

#### 支持.babelrc <a id="babelrc-support"></a>

如果您的项目有一个.babelrc文件，我们将使用它而不是默认的配置文件。因此，您可以在Storybook中使用在项目中使用的任何babel插件或预设。

## Webpack

我们使用Webpack为Web服务和加载JavaScript模块。webpack配置是可配置的，默认设置取决于您使用的框架以及是否使用过诸如Create React App或Angular CLI等脚手架。

> 我们正在努力使故事书随着时间的推移更加零配置，非常欢迎您加入生成器的配置。

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

#### 支持CSS

您可以将CSS文件导入到storybook配置文件，UI组件中或story定义文件中的任意位置。

通常，您可以像这样导入CSS：

```text
// from NPM modules
import 'bootstrap/dist/css/bootstrap.css';

// from local path
import './styles.css';
```

> 注意：在某些框架/ clis中，我们仅注入纯CSS。如果需要像SASS这样的预处理器，则需要[自定义webpack配置](custom-webpack-config.md)。
>
> 警告：默认情况下，使用Angular CLI的项目的storybook无法导入CSS。他们必须自定义webpack配置，或使用内联加载器语法：
>
> ```text
> import '!style-loader!css-loader!./styles.css';
> ```

#### 图片和静态文件支持

您也可以直接通过JavaScript导入图像和媒体文件。这可以帮助您用媒体文件编写story：

```text
import React from 'react';
import { storiesOf } from '@storybook/react';

import imageFile from './static/image.png';

storiesOf('<img />', module)
  .add('with an image', () => (
    <img src={imageFile} alt="covfefe" />
  ));
```

在制作storybook时，我们还将导出导入的图像。因此，这是加载所有静态内容的好方法。

> 备选方案：故事书还可以通过`start-storybook`和`build-storybook`命令的`-s`选项来提及静态目录。[阅读更多](https://storybook.js.org/docs/configurations/serving-static-files/)

#### JSON Loader

您可以像使用Node.js一样导入.json文件。这也将允许您使用在其中导入.json文件的NPM项目。

```text
import React from 'react';
import { storiesOf } from '@storybook/react';

import data from './data.json';

storiesOf('Component', module)
  .add('with data', () => (
    <pre>{JSON.stringify(data, null, 2)}</pre>
  ));
```

## NPM Modules

