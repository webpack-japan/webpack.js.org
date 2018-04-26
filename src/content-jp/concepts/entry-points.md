---
title: Entry Points
sort: 2
contributors:
  - TheLarkInn
  - chrisVillanueva
---

As mentioned in [Getting Started](/guides/getting-started/#using-a-configuration), there are multiple ways to define the `entry` property in your webpack configuration. We will show you the ways you **can** configure the `entry` property, in addition to explaining why it may be useful to you.

[Getting Started](/guides/getting-started/#using-a-configuration) で触れたように、 webpack の設定で `entry` プロパティを定義する方法は複数あります。 ここでは `entry` プロパティを設定する方法と、それがなぜあなたにとって役立つかを説明します。

## Single Entry (Shorthand) Syntax

## 単一エントリ ( 簡潔な ) 構文

Usage: `entry: string|Array<string>`

使い方: `entry: string|Array<string>`

**webpack.config.js**

```javascript
const config = {
  entry: './path/to/my/entry/file.js'
};

module.exports = config;
```

The single entry syntax for the `entry` property is a shorthand for:

`entry` プロパティの単一エントリ構文は以下を簡潔にしたものです。

```javascript
const config = {
  entry: {
    main: './path/to/my/entry/file.js'
  }
};
```

T> **What happens when you pass an array to `entry`?** Passing an array of file paths to the `entry` property creates what is known as a **"multi-main entry"**. This is useful when you would like to inject multiple dependent files together and graph their dependencies into one "chunk".

T> **`entry` の配列を渡したときに何が起こるのでしょうか？** ファイルパスの配列が `entry` プロパティに渡されると **multi-main エントリ** として知られるものを生成します。これは複数の依存するファイルを一緒に注入したいときに便利で、それらの依存性を一つの "chunk" としてくれます。

This is a great choice when you are looking to quickly setup a webpack configuration for an application or tool with one entry point (IE: a library). However, there is not much flexibility in extending or scaling your configuration with this syntax.

1 つのエントリポイント ( 例えばライブラリ ) でアプリケーションやツール用の webpack を設定する場合は最適な選択です。しかしながら、この構文では継承の柔軟性や設定の拡張性は少ししかありません。


## Object Syntax

## オブジェクト構文

Usage: `entry: {[entryChunkName: string]: string|Array<string>}`

使い方: `entry: {[entryChunkName: string]: string|Array<string>}`

**webpack.config.js**

```javascript
const config = {
  entry: {
    app: './src/app.js',
    vendors: './src/vendors.js'
  }
};
```

The object syntax is more verbose. However, this is the most scalable way of defining entry/entries in your application.

オブジェクト構文はより冗長です。アプリケーションでエントリ ( たち ) の定義を行う方法で最も拡張性があります。

T> **"Scalable webpack configurations"** are ones that can be reused and combined with other partial configurations. This is a popular technique used to separate concerns by environment, build target and runtime. They are then merged using specialized tools like [webpack-merge](https://github.com/survivejs/webpack-merge).

T> **「 webpack 設定の拡張性」** の 1 つに再利用や他の部分的な設定と結合する手法があります。これは環境、ビルド対象、ランタイムによって関心事を分離するために使用される一般的なテクニックです。　[webpack-merge](https://github.com/survivejs/webpack-merge)　のような特別なツールを使用してマージされます。

## Scenarios

## シナリオ

Below is a list of entry configurations and their real-world use cases:

以下は entry の設定と実際の使用例のリストです。


### Separate App and Vendor Entries

### アプリケーションとベンダのエントリを分ける

**webpack.config.js**

```javascript
const config = {
  entry: {
    app: './src/app.js',
    vendors: './src/vendors.js'
  }
};
```

**What does this do?** At face value this tells webpack to create dependency graphs starting at both `app.js` and `vendors.js`. These graphs are completely separate and independent of each other (there will be a webpack bootstrap in each bundle). This is commonly seen with single page applications which have only one entry point (excluding vendors).

**何をしているのでしょうか ?** 表面上の値は `app.js` と `vendors.js` の両方から始まる依存関係グラフを生成するように webpack に指示しています。これらのグラフは完全に分離し互いに独立しています ( 互いの bundle には webpack bootstrap があります ) 。これは一般的に ( vendors を除く ) エントリポイントが 1 つしかないシングルページアプリケーションで見られます。

**Why?** This setup allows you to leverage `CommonsChunkPlugin` and extract any vendor references from your app bundle into your vendor bundle, replacing them with `__webpack_require__()` calls. If there is no vendor code in your application bundle, then you can achieve a common pattern in webpack known as [long-term vendor-caching](/guides/caching).

**なぜ使われるのでしょうか ?** この設定では `CommonsChunkPlugin` を利用し、app bundle から vendor bundle への vendor 参照を抽出し、それらを `__webpack_require__()` に置換することができます。あなたのアプリケーションバンドルに vendor コードがないのであれば、 [long-term vendor-caching](/guides/caching) として知られる webpack での共通パターンを実現できます。

?> Consider removing this scenario in favor of the DllPlugin, which provides a better vendor-splitting.

?> このシナリオを削除して、より良い vendor 分割を提供する DllPlugin を利用することを検討してください。


### Multi Page Application

## 複数ページのアプリケーション

**webpack.config.js**

```javascript
const config = {
  entry: {
    pageOne: './src/pageOne/index.js',
    pageTwo: './src/pageTwo/index.js',
    pageThree: './src/pageThree/index.js'
  }
};
```

**What does this do?** We are telling webpack that we would like 3 separate dependency graphs (like the above example).

**何が起きているでしょうか？** webpack に 3 つの独立した依存関係グラフを指定しています ( 上記の例のように ) 。

**Why?** In a multi-page application, the server is going to fetch a new HTML document for you. The page reloads this new document and assets are redownloaded. However, this gives us the unique opportunity to do multiple things:

**なぜ分けたのでしょうか ?** 複数ページのアプリケーションでは、サーバがあなたへ新しい HTML ドキュメントをフェッチしています。ページは新しい document でリロードされ、アセットも再ダウンロードされます。しかしながら、これは以下のような複数のことを行うユニークな機会を与えます。

- Use `CommonsChunkPlugin` to create bundles of shared application code between each page. Multi-page applications that reuse a lot of code/modules between entry points can greatly benefit from these techniques, as the amount of entry points increase.

- `CommonsChunkPlugin` を使うことで互いのページで共通化された bundle を生成します。たくさんのコード / モジュールをエントリポイント間で再利用する複数ページのアプリケーションはこれらのテクニックによりエントリポイントのマウントが増えた際に大きな恩恵を受けます。

T> As a rule of thumb: for each HTML document use exactly one entry point.

T> 経験則として、各 HTML document に対してただ 1 つのエントリポイントを使用します。
