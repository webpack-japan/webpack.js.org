---
title: Concepts
sort: 1
contributors:
  - TheLarkInn
  - jhnns
  - grgur
  - johnstew
  - jimrfenner
  - TheDutchCoder
  - adambraimbridge
  - EugeneHlushko
---

At its core, *webpack* is a _static module bundler_ for modern JavaScript applications. When webpack processes your application, it recursively builds a _dependency graph_ that includes every module your application needs, then packages all of those modules into one or more _bundles_.

核となる *webpack* は、最新の JavaScript アプリケーション用の _スタティックモジュールバンドラ_ です。 webpack がアプリケーションを処理すると、アプリケーションに必要なすべてのモジュールが含まれた _依存関係グラフ_ が再帰的に作成され、それらのモジュールのすべてが1つ以上の _バンドル_ にパッケージ化されます。

T> Learn more about JavaScript modules and webpack modules [here](/concepts/modules).

T> JavaScript モジュールと Webpack モジュールの詳細は [こちら](/concepts/modules).

Since v4.0.0 webpack does not require a configuration file. Nevertheless, it is [incredibly configurable](/configuration). To get started you only need to understand four **Core Concepts**:

v4.0.0 の webpack は設定ファイルを必要としません。それにもかかわらず、それは [非常に設定可能](/configuration) です。開始するには、4 つの**コアの概念**を理解する必要があります。

- Entry
- Output
- Loaders
- Plugins

This document is intended to give a **high-level** overview of these concepts, while providing links to detailed concept specific use cases.

このドキュメントはこれらのコンセプトの **高レベルな** 概要を提供することを目的としていますが、詳細なコンセプト固有のユースケースへのリンクを提供します。


## Entry

An **entry point** indicates which module webpack should use to begin building out its internal *dependency graph*. After entering the entry point, webpack will figure out which other modules and libraries that entry point depends on (directly and indirectly).

**entry point**は内部モジュール*を webpack が *依存関係グラフ*の構築をするために仕様されるべきモジュールを示します。エントリポイントに入ると、webpack はエントリポイントが依存する他のモジュールとライブラリを（直接的および間接的に）把握します。

Every dependency is then processed and outputted into files called *bundles*, which we'll discuss more in the next section.

すべての依存関係は処理され、*bundles*というファイルに出力されます。これについては、次のセクションで詳しく説明します。

You can specify an entry point (or multiple entry points) by configuring the `entry` property in the [webpack configuration](/configuration). It defaults to `./src`.

[webpack configuration]（/configuration）の `entry`プロパティを設定することで、エントリポイント（または複数のエントリポイント）を指定することができます。デフォルトは `./src`です。

Here's the simplest example of an `entry` configuration:

`entry`設定の最も単純な例を次に示します：

__webpack.config.js__

``` js
module.exports = {
  entry: './path/to/my/entry/file.js'
};
```

T> You can configure the `entry` property in various ways depending the needs of your application. Learn more in the [entry points](/concepts/entry-points) section.

T> アプリケーションのニーズに応じてさまざまな方法で `entry`プロパティを設定することができます。 [entry points](/concepts/entry-points) セクションで詳しく学んでください。

## Output

The **output** property tells webpack where to emit the *bundles* it creates and how to name these files, it defaults to `./dist`. You can configure this part of the process by specifying an `output` field in your configuration:

**output** プロパティは w​​ebpack に *bundle* を生成する場所と、これらのファイルの名前を付ける方法を指示します。デフォルトは `./dist`です。あなたの設定に `output`フィールドを指定することで、プロセスのこの部分を設定することができます：

__webpack.config.js__

```javascript
const path = require('path');

module.exports = {
  entry: './path/to/my/entry/file.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'my-first-webpack.bundle.js'
  }
};
```

In the example above, we use the `output.filename` and the `output.path` properties to tell webpack the name of our bundle and where we want it to be emitted to.

上記の例では、 `output.filename` と ` output.path` プロパティを使用して、webpackにバンドル名と出力先を指定します。


T> You may see the term **emitted** or **emit** used throughout our documentation and [plugin API](/api/plugins). This is a fancy term for 'produced' or 'discharged'.

T> 私たちのドキュメントと [plugin API](/api/plugins) の中で**emitted**または**emit**という用語が使われることがあります。これは、「生成された」または「排出された」という言葉です。

T> The `output` property has [many more configurable features](/configuration/output) and if you like to know more about the concepts behind the `output` property, you can [read more in the concepts section](/concepts/output).

T> 
`output`プロパティには [より多くの設定可能な機能](/configuration/output)があり、` output` プロパティの背後にあるコンセプトについてもっと知りたければ、[concept のセクションで読むことができます](/concepts/output)。

## Loaders

*Loaders* enable webpack to process more than just JavaScript files (webpack itself only understands JavaScript). They give you the ability to leverage webpack's bundling capabilities for all kinds of files by converting them to valid [modules](/concepts/modules) that webpack can process.

*Loader* は webpack に JavaScriptファイル以外の処理を可能にします（webpack自体はJavaScriptのみを理解します）。それらは、 webpack が処理できる有効な[modules]（/concepts/modules）に変換することで、あらゆる種類のファイルに対する webpack のバンドル機能を活用する能力を与えます。

Essentially, webpack loaders transform all types of files into modules that can be included in your application's dependency graph (and eventually a bundle).

基本的に、webpack loader は、すべてのタイプのファイルを、アプリケーションの依存関係グラフに（そして最終的にはバンドルに）含まれるモジュールに変換します。

W> Note that the ability to `import` any type of module, e.g. `.css` files, is a feature specific to webpack and may not be supported by other bundlers or task runners. We feel this extension of the language is warranted as it allows developers to build a more accurate dependency graph.

W> どのようなタイプのモジュールも `import` できることに注意してください。 `.css` ファイルは webpack 特有の機能であり、他のバンドルやタスクランナーがサポートしていない可能性があります。私たちは、開発者がより正確な依存関係グラフを作成できるので、言語のこの拡張が正当であると感じています。

At a high level, __loaders__ have two purposes in your webpack configuration:

高いレベルでは、__loaders__ は webpack の設定に2つの目的を持っています：

1. The `test` property identifies which file or files should be transformed.
2. The `use` property indicates which loader should be used to do the transforming.

1. `test` プロパティはどのファイルを変換すべきかを識別します。
2. `use`プロパティは、どのローダが変換を行うために使用されるべきかを示します。

__webpack.config.js__

```javascript
const path = require('path');

const config = {
  output: {
    filename: 'my-first-webpack.bundle.js'
  },
  module: {
    rules: [
      { test: /\.txt$/, use: 'raw-loader' }
    ]
  }
};

module.exports = config;
```

The configuration above has defined a `rules` property for a single module with two required properties: `test` and `use`. This tells webpack's compiler the following:

上記の設定では、 `test`と` use`の2つの必須プロパティを持つ単一モジュールの `rules`プロパティを定義しています。これはwebpackのコンパイラに次のように伝えます：

> "Hey webpack compiler, when you come across a path that resolves to a '.txt' file inside of a `require()`/`import` statement, **use** the `raw-loader` to transform it before you add it to the bundle."

> "おーい webpack compiler、`require()`/`import` の中の'.txt'ファイルに解決するパスを見つけたら、 buldle に追加する前に `raw-loader` を **使って** 変換しておいてね。"

W> It is important to remember that **when defining rules in your webpack config, you are defining them under `module.rules` and not `rules`**. For your benefit, webpack will 'yell at you' if this is done incorrectly.

W>あなたの webpack 設定でルールを定義するときは、 **`rules` ではなく`module.rules`の中で定義することを覚えておくことが重要** です。あなたのために、これが間違って行われた場合 webpack はあなたを怒るでしょう。

There are other, more specific properties to define on loaders that we haven't yet covered.

他にも、まだカバーしていないローダーで定義する他のより具体的なプロパティがあります。

[詳しく知る!](/concepts/loaders)


## Plugins

While loaders are used to transform certain types of modules, plugins can be leveraged to perform a wider range of tasks. Plugins range from bundle optimization and minification all the way to defining environment-like variables. The [plugin interface](/api/plugins) is extremely powerful and can be used to tackle a wide variety of tasks.

特定のタイプのモジュールを変換するためにローダーが使用されますが、プラグインを活用してより広い範囲のタスクを実行することができます。プラグインはバンドルの最適化と縮小から、環境に似た変数の定義に至るまで幅広く使用されています。 [plugin interface](/api/plugins)は非常に強力で、さまざまなタスクに取り組むことができます。

In order to use a plugin, you need to `require()` it and add it to the `plugins` array. Most plugins are customizable through options. Since you can use a plugin multiple times in a config for different purposes, you need to create an instance of it by calling it with the `new` operator.

プラグインを使用するには、 `require()` して `plugins` 配列に追加する必要があります。ほとんどのプラグインはオプションでカスタマイズできます。あなたは別の目的のために設定でプラグインを何度も使うことができるので、 `new` 演算子でそれを呼び出すことによってインスタンスを作成する必要があります。

**webpack.config.js**

```javascript
const HtmlWebpackPlugin = require('html-webpack-plugin'); //installed via npm
const webpack = require('webpack'); //to access built-in plugins

const config = {
  module: {
    rules: [
      { test: /\.txt$/, use: 'raw-loader' }
    ]
  },
  plugins: [
    new webpack.optimize.UglifyJsPlugin(),
    new HtmlWebpackPlugin({template: './src/index.html'})
  ]
};

module.exports = config;
```

There are many plugins that webpack provides out of the box! Check out our [list of plugins](/plugins) for more information.

webpackがすぐに提供する多くのプラグインがあります。詳しくは、[list of plugins](/plugins)を参照してください。

Using plugins in your webpack config is straightforward - however, there are many use cases that are worth further exploration.

Webpack の設定でプラグインを使用するのは簡単ですが、さらに検討する価値のあるユースケースが数多くあります。

[詳しく知る!](/concepts/plugins)


## Mode

By setting the `mode` parameter to either `development` or `production`, you can enable webpack's built-in optimizations that correspond with the selected mode.

`mode` パラメータを `development` または `production` に設定することで、選択されたモードに対応する webpack の組み込み最適化を有効にすることができます。

```javascript
module.exports = {
  mode: 'production'
};
```

[詳しく知る!](/concepts/mode)
