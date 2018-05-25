---
title: Dependency Graph
sort: 9
contributors:
  - TheLarkInn
---

Any time one file depends on another, webpack treats this as a _dependency_. This allows webpack to take non-code assets, such as images or web fonts, and also provide them as _dependencies_ for your application.

1 つのファイルが他のファイルに依存するたびに、 webpack はそれを _dependency_ として取り扱います。これにより webpack は画像や web フォントのような非コードのアセットを取得し、アプリケーションに依存関係を提供します。

When webpack processes your application, it starts from a list of modules defined on the command line or in its config file.
Starting from these _entry points_, webpack recursively builds a _dependency graph_ that includes every module your application needs, then packages all of those modules into a small number of _bundles_ - often, just one - to be loaded by the browser.

Webpack がアプリケーションを処理した際、コマンドラインまたは webpack 自身の設定ファイル上で定義されたモジュールのリストから開始します。
これらの _entry points_ から始まり、 webpack はアプリケーションで必要なすべてのモジュールを含んだ _dependency graph_ を再帰的にビルドし、それらのモジュールすべてを少数の ( しばしば 1 つの ) _bundle_ にパッケージ化してブラウザからロードされます。

T> Bundling your application is especially powerful for *HTTP/1.1* clients, as it minimizes the number of times your app has to wait while the browser starts a new request. For *HTTP/2*, you can also use Code Splitting and bundling through webpack for the [best optimization](https://medium.com/webpack/webpack-http-2-7083ec3f3ce6#.7y5d3hz59).

T> *HTTP/1.1* クライアントではアプリケーションのバンドルが特に効果的で、ブラウザが新しいリクエストを開始している間にアプリケーションが待機する回数を最小限にするためです。 *HTTP/2* では、 [best optimization](https://medium.com/webpack/webpack-http-2-7083ec3f3ce6#.7y5d3hz59) のためにコード分割や webpack を通したバンドルもまた利用できます。
