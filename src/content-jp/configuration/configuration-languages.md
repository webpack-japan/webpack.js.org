---
title: Configuration Languages
sort: 2
contributors:
  - sokra
  - skipjack
  - tarang9211
  - simon04
  - peterblazejewicz
  - youta1119
---

webpack accepts configuration files written in multiple programming and data languages. The list of supported file extensions can be found at the [node-interpret](https://github.com/js-cli/js-interpret) package. Using [node-interpret](https://github.com/js-cli/js-interpret), webpack can handle many different types of configuration files.

webpack  は複数のプログラミング言語やデータ言語で書かれた設定ファイルに対応します。サポートされたファイルの拡張子のリストは [node-interpret](https://github.com/js-cli/js-interpret) パッケージでみられます。 [node-interpret](https://github.com/js-cli/js-interpret) を使うことで、 webpack はたくさんの異なる設定ファイルの種類を取り扱うことができます。


## TypeScript

To write the webpack configuration in [TypeScript](http://www.typescriptlang.org/), you would first install the necessary dependencies:

[TypeScript](http://www.typescriptlang.org/) で webpack の設定を記述するために、まずは必要な依存関係をインストールします。

``` bash
npm install --save-dev typescript ts-node @types/node @types/webpack
```

and then proceed to write your configuration:

そして設定を記述します。

__webpack.config.ts__

```typescript
import * as webpack from 'webpack';
import * as path from 'path';

const config: webpack.Configuration = {
  entry: './foo.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'foo.bundle.js'
  }
};

export default config;
```

Note that you'll also need to check your `tsconfig.json` file. If the module in `compilerOptions` in `tsconfig.json` is `commonjs`, the setting is complete, else webpack will fail with an error. This occurs because `ts-node` does not support any module syntax other than `commonjs`.

`tsconfig.json` ファイルをチェックする必要があることに留意してください。 `tsconfig.json` の `compilerOptions` のモジュールが `common.js` の場合、設定は完璧ですが、それ以外の場合 webpack はエラーで失敗します。これは `ts-node` が `common.js` 以外のいかなるモジュール構文をサポートしていないために発生します。

There are two solutions to this issue:

この問題を解決する 2 つの方法があります。

- Modify `tsconfig.json`.
- Install `tsconfig-paths`.

- `tsconfig.json` を修正する
- `tsconfig-path` をインストールする。

The __first option__ is to open your `tsconfig.json` file and look for `compilerOptions`. Set `target` to `"ES5"` and `module` to `"CommonJS"` (or completely remove the `module` option).

__最初の項目__ は `tsconfig.json` を開き `compilerOptions` を探すためです。 `target` を `"ES5"` に `module` to `"CommonJS"` を設定 ( または `module` オプションを完全に削除 ) してください。

The __second option__ is to install the `tsconfig-paths` package:

__2 つめの項目__ は 以下の通り `tsconfig-paths` パッケージをインストールすることです。

``` bash
npm install --save-dev tsconfig-paths
```

And create a separate TypeScript configuration specifically for your webpack configs:

そして以下の通りにあなたの webpack の設定用に分離した TypeScript の設定を作成します。

__tsconfig-for-webpack-config.json__

``` json
{
  "compilerOptions": {
    "module": "commonjs",
    "target": "es5"
  }
}
```

T> `ts-node` can resolve a `tsconfig.json` file using the environment variable provided by `tsconfig-path`.

T> `ts-node` は `tsconfig-path` によって提供された環境変数を利用して `tsconfig.json` ファイルを解決します。

Then set the environment variable `process.env.TS_NODE_PROJECT` provided by `tsconfig-path` like so:

そして以下のように `tsconfig-path` によって提供された `process.env.TS_NODE_PROJECT` 環境変数を設定します。

__package.json__

```json
{
  "scripts": {
    "build": "TS_NODE_PROJECT=\"tsconfig-for-webpack-config.json\" webpack"
  }
}
```


## CoffeeScript

Similarly, to use [CoffeeScript](http://coffeescript.org/), you would first install the necessary dependencies:

同様に、 [CoffeeScript](http://coffeescript.org/) を利用して、以下の通り必要な依存関係を最初にインストールします。

``` bash
npm install --save-dev coffee-script
```

and then proceed to write your configuration:

そして以下の通り設定を記述します。

__webpack.config.coffee__

```javascript
HtmlWebpackPlugin = require('html-webpack-plugin')
webpack = require('webpack')
path = require('path')

config =
  entry: './path/to/my/entry/file.js'
  output:
    path: path.resolve(__dirname, 'dist')
    filename: 'my-first-webpack.bundle.js'
  module: rules: [ {
    test: /\.(js|jsx)$/
    use: 'babel-loader'
  } ]
  plugins: [
    new (webpack.optimize.UglifyJsPlugin)
    new HtmlWebpackPlugin(template: './src/index.html')
  ]

module.exports = config
```


## Babel and JSX

In the example below JSX (React JavaScript Markup) and Babel are used to create a JSON Configuration that webpack can understand.

以下の例では JSX (React JavaScript Markup) と Babel が webpack が理解できる JSON の設定を生成するために利用されています。

> Courtesy of [Jason Miller](https://twitter.com/_developit/status/769583291666169862)

> [Jason Miller](https://twitter.com/_developit/status/769583291666169862) のご厚意


First install the necessary dependencies:

はじめに以下の通り必要な依存関係をインストールします。

``` js
npm install --save-dev babel-register jsxobj babel-preset-es2015
```

__.babelrc__

``` json
{
  "presets": [ "es2015" ]
}
```

__webpack.config.babel.js__

``` js
import jsxobj from 'jsxobj';

// example of an imported plugin
const CustomPlugin = config => ({
  ...config,
  name: 'custom-plugin'
});

export default (
  <webpack target="web" watch>
    <entry path="src/index.js" />
    <resolve>
      <alias {...{
        react: 'preact-compat',
        'react-dom': 'preact-compat'
      }} />
    </resolve>
    <plugins>
      <uglify-js opts={{
        compression: true,
        mangle: false
      }} />
      <CustomPlugin foo="bar" />
    </plugins>
  </webpack>
);
```

W> If you are using Babel elsewhere and have `modules` set to `false`, you will have to either maintain two separate `.babelrc` files or use `const jsxobj = require('jsxobj');` and `module.exports` instead of the new `import` and `export` syntax. This is because while Node does support many new ES6 features, they don't yet support ES6 module syntax.

W> もしほかの場所で Babel を利用しており、 `modules` を `false` に設定している場合、 2 つの分割した `.babelrc` を保持するか新しい `import` と `export` 文の代わりに `const jsxobj = require('jsxobj');` と `module.exports` を利用する必要があります。これは Node が新しい ES6 の機能をサポートしているにもかかわらず、 ES6 のモジュール構文をまだサポートしていないためです。
