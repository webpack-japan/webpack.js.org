---
title: Plugins
sort: 5
contributors:
  - TheLarkInn
  - jhnns
  - rouzbeh84
  - johnstew
---

**Plugins** are the [backbone](https://github.com/webpack/tapable) of webpack. webpack itself is built on the **same plugin system** that you use in your webpack configuration!

**Plugins** は webpack の [backbone](https://github.com/webpack/tapable) です。 webpack 自身は webpack の設定で使用するのと **同じプラグインシステム** でビルドされています !

They also serve the purpose of doing **anything else** that a [loader](/concepts/loaders) cannot do.

Plugins は [loader](/concepts/loaders) が行うことができないことを行うという目的にかないます。


## Anatomy

A webpack **plugin** is a JavaScript object that has an [`apply`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/apply) property. This `apply` property is called by the webpack compiler, giving access to the **entire** compilation lifecycle.

webpack **plugin** は [`apply`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/apply) プロパティをもつ JavaScript オブジェクトです。 この `apply` プロパティは webpack コンパイラに呼ばれ 、 *全体の* コンパイルライフサイクルにアクセスできます。

**ConsoleLogOnBuildWebpackPlugin.js**

```javascript
function ConsoleLogOnBuildWebpackPlugin() {

};

ConsoleLogOnBuildWebpackPlugin.prototype.apply = function(compiler) {
  compiler.plugin('run', function(compiler, callback) {
    console.log("The webpack build process is starting!!!");

    callback();
  });
};
```

T> As a clever JavaScript developer you may remember the `Function.prototype.apply` method. Because of this method you can pass any function as plugin (`this` will point to the `compiler`). You can use this style to inline custom plugins in your configuration.


T> 賢い JavaScript 開発者なら `Function.prototype.apply` メソッドを覚えているかもしれません。 このメソッドのため、プラグインとして関数を渡すことができます ( `this` は `compiler` を指します ) 。このスタイルを利用して設定内のカスタムプラグインをインライン化することができます。

## Usage

Since **plugins** can take arguments/options, you must pass a `new` instance to the `plugins` property in your webpack configuration.

**plugin** が 引数やオプションを受け取ることができるため、 webpack の設定内で `new` インスタンスを `plugins` プロパティに渡す必要があります。

Depending on how you are using webpack, there are multiple ways to use plugins.

どのように webpack を使うかによって、 plugin を利用する複数の方法があります。

### Configuration

**webpack.config.js**

```javascript
const HtmlWebpackPlugin = require('html-webpack-plugin'); //installed via npm
const webpack = require('webpack'); //to access built-in plugins
const path = require('path');

const config = {
  entry: './path/to/my/entry/file.js',
  output: {
    filename: 'my-first-webpack.bundle.js',
    path: path.resolve(__dirname, 'dist')
  },
  module: {
    rules: [
      {
        test: /\.(js|jsx)$/,
        use: 'babel-loader'
      }
    ]
  },
  plugins: [
    new webpack.optimize.UglifyJsPlugin(),
    new HtmlWebpackPlugin({template: './src/index.html'})
  ]
};

module.exports = config;
```


### Node API

?> Even when using the Node API, users should pass plugins via the `plugins` property in the configuration. Using `compiler.apply` should not be the recommended way.

?> Node API を利用している場合でも、設定内で `plugins` プロパティを介して plugin を渡す必要があります。 `compiler.apply` を使うことはお勧めできません。

**some-node-script.js**

```javascript
  const webpack = require('webpack'); //to access webpack runtime
  const configuration = require('./webpack.config.js');

  let compiler = webpack(configuration);
  compiler.apply(new webpack.ProgressPlugin());

  compiler.run(function(err, stats) {
    // ...
  });
```

T> Did you know: The example seen above is extremely similar to the [webpack runtime itself!](https://github.com/webpack/webpack/blob/e7087ffeda7fa37dfe2ca70b5593c6e899629a2c/bin/webpack.js#L290-L292) There are lots of great usage examples hiding in the [webpack source code](https://github.com/webpack/webpack) that you can apply to your own configurations and scripts!

T> 知っていましたか ? 上記の例は [webpack のランタイム自身](https://github.com/webpack/webpack/blob/e7087ffeda7fa37dfe2ca70b5593c6e899629a2c/bin/webpack.js#L290-L292) ととても似ています ! [webpack source code](https://github.com/webpack/webpack) にはあなたの設定やスクリプトに適用できる素晴らしい利用例があります !
