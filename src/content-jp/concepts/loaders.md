---
title: Loaders
sort: 4
contributors:
  - manekinekko
  - ev1stensberg
  - SpaceK33z
  - gangachris
  - TheLarkInn
  - simon04
  - jhnns
---

Loaders are transformations that are applied on the source code of a module. They allow you to pre-process files as you `import` or “load” them. Thus, loaders are kind of like “tasks” in other build tools, and provide a powerful way to handle front-end build steps. Loaders can transform files from a different language (like TypeScript) to JavaScript, or inline images as data URLs. Loaders even allow you to do things like `import` CSS files directly from your JavaScript modules!

Loader は モジュールのソースコードに適用される変換処理です。モジュールを `import` または "load" する際にファイルを前処理することができます。たがって、ローダーは他のビルドツールと同様の "task" であり、フロントエンドのビルドステップを処理するパワフルな手段を提供します。 Loader は ( TypeScript のような ) 異なる言語を Javascript またはデータ URL からインラインイメージへ変換できます。 Loader は Javascript モジュールから直接 CSS ファイルを `import` することもできます !


## Example

For example, you can use loaders to tell webpack to load a CSS file or to convert TypeScript to JavaScript. To do this, you would start by installing the loaders you need:

例として、　Loader を使用して webpack に CSS ファイルをロードを指示したり TypeScript を JavaScript への変換できます。それをするためには、必要な loader をインストールすることから始めます。

``` bash
npm install --save-dev css-loader
npm install --save-dev ts-loader
```

And then instruct webpack to use the [`css-loader`](/loaders/css-loader) for every `.css` file and the [`ts-loader`](https://github.com/TypeStrong/ts-loader) for all `.ts` files:

そして webpack にすべての `.css` ファイルに [`css-loader`](/loaders/css-loader) を、すべての `.ts` ファイルに対して [`ts-loader`](https://github.com/TypeStrong/ts-loader) を使用することを指定します。

**webpack.config.js**

``` js
module.exports = {
  module: {
    rules: [
      { test: /\.css$/, use: 'css-loader' },
      { test: /\.ts$/, use: 'ts-loader' }
    ]
  }
};
```


## Using Loaders

There are three ways to use loaders in your application:

アプリケーションで loader を使用するには 3 通りの方法があります。

* [Configuration](#configuration) (recommended): Specify them in your __webpack.config.js__ file.
* [Inline](#inline): Specify them explicitly in each `import` statement.
* [CLI](#cli): Specify them within a shell command.

* [Configuration](#configuration) ( 推奨 ): __webpack.config.js__ ファイルで指定します。
* [Inline](#inline): それぞれの `import` ステートメントで明示的に指定します。
* [CLI](#cli): shell コマンド内で指定します。


### Configuration

[`module.rules`](/configuration/module/#module-rules) allows you to specify several loaders within your webpack configuration.
This is a concise way to display loaders, and helps to maintain clean code. It also offers you a full overview of each respective loader:

[`module.rules`](/configuration/module/#module-rules) は webpack の設定内で複数の loader を指定することができます。
これは loader を表示するための完結な方法であり、クリーンなコードを維持するのに役立ちます。またこれはそれぞれ個別の loader の概要を示しています。

```js-with-links-with-details
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          { loader: ['style-loader'](/loaders/style-loader) },
          {
            loader: ['css-loader'](/loaders/css-loader),
            options: {
              modules: true
            }
          }
        ]
      }
    ]
  }
```


### Inline

It's possible to specify loaders in an `import` statement, or any [equivalent "importing" method](/api/module-methods). Separate loaders from the resource with `!`. Each part is resolved relative to the current directory.

`import` 文または任意の [equivalent "importing" method](/api/module-methods) で loader を指定することができます。 `!` でリソースと loader を区切ります。 各部分はカレントディレクトリに対して相対的に解決されます。

```js
import Styles from 'style-loader!css-loader?modules!./styles.css';
```

It's possible to overwrite any loaders in the configuration by prefixing the entire rule with `!`.

ルール全体に `!` をつけることで設定内のすべての loader を上書きすることができます。

Options can be passed with a query parameter, e.g. `?key=value&foo=bar`, or a JSON object, e.g. `?{"key":"value","foo":"bar"}`.

オプションは `?key=value&foo=bar` のようなクエリパラメータ、または `?{"key":"value","foo":"bar"}` のような JSON オブジェクトで渡すことができます。

T> Use `module.rules` whenever possible, as this will reduce boilerplate in your source code and allow you to debug or locate a loader faster if something goes south.

T> できるだけ `module.rules` を使用してください。ソースコード内のボイラープレートを減らし、何かが失敗した際に早くデバッグや特定できるようにするためです。


### CLI

You can also use loaders through the CLI:

CLI 経由でも loader を使用することができます。

```sh
webpack --module-bind jade-loader --module-bind 'css=style-loader!css-loader'
```

This uses the `jade-loader` for `.jade` files, and the [`style-loader`](/loaders/style-loader) and [`css-loader`](/loaders/css-loader) for `.css` files.

これは `.jade` ファイル用に `jade-loader` を使用し、 `.css` ファイル用に [`style-loader`](/loaders/style-loader) と [`css-loader`](/loaders/css-loader) を使用します。


## Loader Features

* Loaders can be chained. They are applied in a pipeline to the resource. A chain of loaders are executed in reverse order. The first loader in a chain of loaders returns a value to the next. At the end loader, webpack expects JavaScript to be returned.
* Loaders can be synchronous or asynchronous.
* Loaders run in Node.js and can do everything that’s possible there.
* Loaders accept query parameters. This can be used to pass configuration to the loader.
* Loaders can also be configured with an `options` object.
* Normal modules can export a loader in addition to the normal `main` via `package.json` with the `loader` field.
* Plugins can give loaders more features.
* Loaders can emit additional arbitrary files.

* loader は連結できます。これらはリソースにパイプラインで適用されます。 loader のチェーンは逆の順序で実行されます。 loader のチェーンの最初の loader は次の loader に値を返します。最後の loader では webpack が JavaScript が返されることを期待しています。
* loader は同期または非同期にすることができます。
* loader は Node.js で実行し、そこで可能なすべてのことができます。
* loader はクエリパラメータを受け入れます。これは設定を loader に渡すために使用できます。
* loader は `options` オブジェクトでも設定することができます。
* 通常のモジュールは `loader` フィールドで `package.json` を通して通常の `main` に加えて loader をエクスポートすることができます。
* plugin は loader により多くの機能を与えることができます。
* loader は任意の追加ファイルを出力することができます。

Loaders allow more power in the JavaScript ecosystem through preprocessing
functions (loaders). Users now have more flexibility to include fine-grained logic such as compression, packaging, language translations and [more](/loaders).

loader はプリプロセッサ関数 (loader) を介して JavaScript のエコシステムでより強力になります。ユーザは、圧縮、パッケージング、言語翻訳、 [more](/loader) などのきめ細かなロジックを組み込むことができるようになりました。


## Resolving Loaders

Loaders follow the standard [module resolution](/concepts/module-resolution/). In most cases it will be loaders from the [module path](/concepts/module-resolution/#module-paths) (think `npm install`, `node_modules`).

loader は標準の [module resolution](/concepts/module-resolution/) に従います。ほとんどの場合、それは [module path](/concepts/module-resolution/#module-paths) (think `npm install`, `node_modules`) からの loader になります。

A loader module is expected to export a function and be written in Node.js compatible JavaScript. They are most commonly managed with npm, but you can also have custom loaders as files within your application. By convention, loaders are usually named `xxx-loader` (e.g. `json-loader`). See ["How to Write a Loader?"](/development/how-to-write-a-loader) for more information.

loader モジュールは関数をエクスポートし、 Node.js 互換の JavaScript で記述される必要があります。これらは npm で通常は管理されていますが、アプリケーション内のファイルとしてカスタム loader をもつことができます。詳細は ["How to Write a Loader?"](/development/how-to-write-a-loader) を参照してください。
