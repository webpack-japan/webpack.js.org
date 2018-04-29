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
---

At its core, *webpack* is a _static module bundler_ for modern JavaScript applications. When webpack processes your application, it recursively builds a _dependency graph_ that includes every module your application needs, then packages all of those modules into one or more _bundles_.

中核となる *webpack* は モダンな Javascript アプリケーション用の _static module bundler_ です。 アプリケーションで webpack を処理したとき、アプリケーションで必要となるすべてのモジュールが含まれた  _dependency graph_ を再帰的にビルドし、それらのモジュールすべてが 1 つ以上の _bundle_ にパッケージ化されます。

T> Learn more about JavaScript modules and webpack modules [here](/concepts/modules).

T> Javascript モジュールや webpack モジュールの詳細は [こちら](/concepts/modules) 。

It is [incredibly configurable](/configuration), but to get started you only need to understand four **Core Concepts**:

webpack は [色々と設定できます](/configuration) が、始めるには 4 つの **Core Concepts** を理解する必要があります。

- Entry
- Output
- Loaders
- Plugins

This document is intended to give a **high-level** overview of these concepts, while providing links to detailed concept specific use cases.

本ドキュメントはこれらコンセプトの **high-level** な概要を提供することを目的とする一方、固有のユースケースの詳細なコンセプトへのリンクを提供しています。


## Entry

An **entry point** indicates which module webpack should use to begin building out its internal *dependency graph*. After entering the entry point, webpack will figure out which other modules and libraries that entry point depends on (directly and indirectly).

**entry point** は内部の *dependency graph* の構築を開始するために webpack がどのモジュールを使用すべきか指定します。 entry point に入力すると、 webpack は entry point が依存する他のモジュールやライブラリを ( 直接および間接的に ) 理解します。

Every dependency is then processed and outputted into files called *bundles*, which we'll discuss more in the next section.

すべての依存関係は処理され、 *bundles* と呼ばれるファイルに出力されます。 これらについては次のセクションで詳しく説明します。

You can specify an entry point (or multiple entry points) by configuring the `entry` property in the [webpack configuration](/configuration).

[webpack configuration](/configuration) の `entry` プロパティを設定することで entry point ( または 複数の entry point ) を指定できます。

Here's the simplest example of an `entry` configuration:

これが `entry` 設定の最もシンプルな例です。

__webpack.config.js__

``` js
module.exports = {
  entry: './path/to/my/entry/file.js'
};
```

T> You can configure the `entry` property in various ways depending the needs of your application. Learn more in the [entry points](/concepts/entry-points) section.

T> アプリケーションのニーズに応じてさまざまな方法で `entry` プロパティを設定できます。 詳細は [entry points](/concepts/entry-points) セクションにあります.


## Output

The **output** property tells webpack where to emit the *bundles* it creates and how to name these files. You can configure this part of the process by specifying an `output` field in your configuration:

**outpu** プロパティは webpack に生成する **bundles** の場所とどう名前をつけるか示します。あなたの設定の `output` フィールドに指定することで処理の一部を設定することができます。

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

上記の例では、 webpack にバンドル名と出力先を指定するために `output.filename` と `output.path` プロパティを使用しています。

T> You may see the term **emitted** or **emit** used throughout our documentation and [plugin API](/api/plugins). This is a fancy term for 'produced' or 'discharged'.

T> 私たちのドキュメントと [plugin API](/api/plugins) の中では **出力された** または **出力する** という用語が使われることがあります。これは **生み出された** や **排出された** への聞こえのいい言葉です。

T> The `output` property has [many more configurable features](/configuration/output) and if you like to know more about the concepts behind the `output` property, you can [read more in the concepts section](/concepts/output).

T> `output` プロパティは [より多くの設定可能な機能](/configuration/output) を持ち、 `output` の背後にあるコンセプトについてより知りたいのであれば、[concept セクションでもっと読むことができます](/concepts/output) 。


## Loaders

*Loaders* enable webpack to process more than just JavaScript files (webpack itself only understands JavaScript). They give you the ability to leverage webpack's bundling capabilities for all kinds of files by converting them to valid [modules](/concepts/modules) that webpack can process.

*Loader* は webpack が Javascript 以外のファイルを処理できるようにします ( webpack 自身は Javascript のみ理解します ) 。 webpack が処理できる有効な [modules](/concepts/modules) に変換することによりあらゆる種類のファイルに対して Loader は webpack のバンドル機能を利用する能力を与えます。 

Essentially, webpack loaders transform all types of files into modules that can be included in your application's dependency graph (and eventually a bundle).

基本的に、 webpack loader は全種類のファイルをアプリケーションの dependency graph ( や最終的にはバンドル ) に含まれているモジュールへ変換します。

W> Note that the ability to `import` any type of module, e.g. `.css` files, is a feature specific to webpack and may not be supported by other bundlers or task runners. We feel this extension of the language is warranted as it allows developers to build a more accurate dependency graph.

W> どのようなタイプのモジュールでも `import` できる点に注意してください。 例えば、 `.css` ファイルは webpack 特有の機能であり、他のばんどらやタスクランナーはサポートしていないかもしれません。開発者がより正確な dependency graph をビルドすることができるため我々はこの言語拡張は当然であると感じています。

At a high level, __loaders__ have two purposes in your webpack configuration:

ハイレベルでは、 __loaders__ は webpack の設定に 2 つの目的を持っています。

1. The `test` property identifies which file or files should be transformed.
2. The `use` property indicates which loader should be used to do the transforming.

1. `test` プロパティはどのファイル ( たち ) を変換すべきか指定します。　
2. `use` プロパティはどの loader が変換を行う上で使われるべきか指定します。


__webpack.config.js__

```javascript
const path = require('path');

const config = {
  entry: './path/to/my/entry/file.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
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

上記の設定では `test` と `use` の 2 つのプロパティを持つ単一モジュールの `rules` プロパティが定義されています。これは webpack コンパイラにに以下の様に伝えます。

> "Hey webpack compiler, when you come across a path that resolves to a '.txt' file inside of a `require()`/`import` statement, **use** the `raw-loader` to transform it before you add it to the bundle."

>" " おーい webpack コンパイラ、 `require()`/`import` 文で '.txt' で解決するパスを見つけたら、バンドルに追加する前に `raw-loader` を **使って** 変換してね。"

W> It is important to remember that **when defining rules in your webpack config, you are defining them under `module.rules` and not `rules`**. For your benefit, webpack will 'yell at you' if this is done incorrectly.

W> **webpack の設定でルールを定義するときは、 `rules` ではなく `module.rules` で定義することを覚えておいてください。** あなたのために、その設定が間違って設定されると webpack は 'あなたを叱る' でしょう。

There are other, more specific properties to define on loaders that we haven't yet covered.

他にも、まだ取り上げていない loader で定義するより詳細なプロパティがあります。

[Learn more!](/concepts/loaders)

[もっと知る !](/concepts/loaders)



## Plugins

While loaders are used to transform certain types of modules, plugins can be leveraged to perform a wider range of tasks. Plugins range from bundle optimization and minification all the way to defining environment-like variables. The [plugin interface](/api/plugins) is extremely powerful and can be used to tackle a wide variety of tasks.

loader はモジュールの特定のタイプを変換するために使用される一方、 plugin はより幅広いタスクを行うために利用されます。 plugin はバンドルの最適化および圧縮から環境変数のような変数の定義へと多岐にわたり使用されます。 [plugin interface](/api/plugins) はとてもパワフルでさまざまなタスクに取り組むために使用されます。

In order to use a plugin, you need to `require()` it and add it to the `plugins` array. Most plugins are customizable through options. Since you can use a plugin multiple times in a config for different purposes, you need to create an instance of it by calling it with the `new` operator.

plugin をしようするためには、 `require()` して `plugins` 配列に追加する必要があります。ほとんどの plugin は option を通してカスタムできます。 plugin は異なる目的で何度も利用できるため、 `new` 演算子で呼び出し、インスタンスを生成する必要があります。

**webpack.config.js**

```javascript
const HtmlWebpackPlugin = require('html-webpack-plugin'); //installed via npm
const webpack = require('webpack'); //to access built-in plugins
const path = require('path');

const config = {
  entry: './path/to/my/entry/file.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'my-first-webpack.bundle.js'
  },
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

webpack が提供するすぐに使えるたくさんの plugin があります ! 詳しくは [list of plugins](/plugins) を参照してください。

Using plugins in your webpack config is straightforward - however, there are many use cases that are worth further exploration.

webpack の設定で plugin を使用するのは簡単ですが、さらに検討する価値のあるユースケースが多々あります。

[Learn more!](/concepts/plugins)

[もっと知る !](/concepts/plugins)
