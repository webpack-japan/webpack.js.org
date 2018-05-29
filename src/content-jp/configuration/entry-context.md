---
title: Entry and Context
sort: 4
contributors:
  - sokra
  - skipjack
  - tarang9211
---

The entry object is where webpack looks to start building the bundle. The context is an absolute string to the directory that contains the entry files.

エントリオブジェクトは webpack がバンドルの構築を開始するための場所です。コンテキストはエントリファイルを含むディレクトリへの絶対パスです。


## `context`

`string`

The base directory, an **absolute path**, for resolving entry points and loaders from configuration.

設定からエントリポイントとローダを解決するための **絶対パス** のベースディレクトリ。

``` js
context: path.resolve(__dirname, "app")
```

By default the current directory is used, but it's recommended to pass a value in your configuration. This makes your configuration independent from CWD (current working directory).

デフォルトではカレントディレクトリが利用されますが、設定で値を渡すことが推奨されています。これは CWD ( カレントワーキングディレクトリ ) から独立して設定を作成します。

---


## `entry`

`string | [string] | object { <key>: string | [string] } | (function: () => string | [string] | object { <key>: string | [string] })`

The point or points to enter the application. At this point the application starts executing. If an array is passed all items will be executed.

アプリケーションを入力するポイントです。このポイントからアプリケーションは実行を開始します。配列が渡された場合、すべての項目が実行されます。

A dynamically loaded module is **not** an entry point.

同時にロードされたモジュールはエントリポイント **ではありません** 。

Simple rule: one entry point per HTML page. SPA: one entry point, MPA: multiple entry points.
単純なルールは HTML のページごとに一つのエントリポイントにすることです。 SPA は一つのエントリポイント、 MPA は複数のエントリポイントです。

```js
entry: {
  home: "./home.js",
  about: "./about.js",
  contact: "./contact.js"
}
```


### Naming

If a string or array of strings is passed, the chunk is named `main`. If an object is passed, each key is the name of a chunk, and the value describes the entrypoint for the chunk.

文字列または文字列の配列が渡された場合、チャンクは `main` と名付けられます。 オブジェクトが渡された場合、各キーはチャンク名になり、値はチャンクのエントリポイントに記述されます。


### Dynamic entry

```js
entry: () => './demo'
```

or

```js
entry: () => new Promise((resolve) => resolve(['./demo', './demo2']))
```

When combining with the [`output.library`](/configuration/output#output-library) option: If an array is passed only the last item is exported.

[`output.library`](/configuration/output#output-library) オプションと組み合わせる場合、配列が渡された際は最後の項目がエクスポートされます。
