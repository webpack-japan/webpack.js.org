---
title: The Manifest
sort: 10
contributors:
  - skipjack
related:
  - title: Separating a Manifest
    url: https://survivejs.com/webpack/optimizing/separating-manifest/
  - title: Predictable Long Term Caching with Webpack
    url: https://medium.com/webpack/predictable-long-term-caching-with-webpack-d3eee1d3fa31
  - title: Caching
    url: /guides/caching
---

In a typical application or site built with webpack, there are three main types of code:

webpack でビルドされた典型的なアプリケーションまたはサイトでは、主に以下の 3 種類のコードがあります。

1. The source code you, and maybe your team, have written.
2. Any third-party library or "vendor" code your source is dependent on.
3. A webpack runtime and _manifest_ that conducts the interaction of all modules.

1. あなたやたぶんあなたのチームが書いたソースコード
2. ソースコードが依存しているサードパーティライブラリまたは "vendor" コード
3. すべてのモジュールのインタラクションを行う webpack ランタイムや _manifest_

This article will focus on the last of these three parts, the runtime and in particular the manifest.

本記事ではランタイムや特に manifest の 3 つの部分の最後に焦点を当てます。


## Runtime

As mentioned above, we'll only briefly touch on this. The runtime, along with the manifest data, is basically all the code webpack needs to connect your modularized application while it's running in the browser. It contains the loading and resolving logic needed to connect your modules as they interact. This includes connecting modules that have already been loaded into the browser as well as logic to lazy-load the ones that haven't.

上記のように、これに簡単に触れるだけです。 manifest データと連動しているランタイムは基本的にブラウザで実行されている間モジュール化されたアプリケーションを接続するために webpack が必要とするすべてのコードです。ランタイムは相互作用するモジュールを接続するために必要なロードや解決ロジックが含まれます。これにはブラウザにすでにロードされているモジュールとそれ以外を遅延ロードするロジックが含まれます。


## Manifest

So, once your application hits the browser in the form of an `index.html` file, some bundles, and a variety of other assets, what does it look like? That `/src` directory you meticulously laid out is now gone, so how does webpack manage the interaction between all of your modules? This is where the manifest data comes in...

つまり、一度アプリケーションが `index.html` ファイル、いくつかのバンドル、様々なアセットの形式でブラウザでヒットしたらどのように見えるでしょうか？最新の注意を払ってレイアウトした `/src` ディレクトリはいなくなったので、 webpack はｄのようにすべてのモジュール間のインタラクションを管理しているのでしょうか？ここに manifest データが入るのです…

As the compiler enters, resolves, and maps out your application, it keeps detailed notes on all your modules. This collection of data is called the "Manifest" and it's what the runtime will use to resolve and load modules once they've been bundled and shipped to the browser. No matter which [module syntax](/api/module-methods) you have chosen, those `import` or `require` statements have now become `__webpack_require__` methods that point to module identifiers. Using the data in the manifest, the runtime will be able to find out where to retrieve the modules behind the identifiers.

コンパイラがアプリケーションを入力、解決、およびマッピングするとき、すべてのモジュールに関する詳細なメモを保持します。このデータのコレクションは "Manifest" と呼ばれ、一度バンドルされブラウザに送られた後ランタイムが解決とロードするために利用します。どの [module syntax](/api/module-methods) を選択しても、 `import` または `require` 構文はモジュール識別子を指す `__webpack_require__` メソッドになりました。 manifest 内のデータを利用して、ランタイムは拡張子の後ろのモジュールをどこから取得するか見つけることができます。


## The Problem

So now you have a little bit of insight about how webpack works behind the scenes. "But, how does this affect me?", you might ask. The simple answer is that most of the time it doesn't. The runtime will do its thing, utilizing the manifest, and everything will appear to just magically work once your application hits the browser. However, if you decide to improve your projects performance by utilizing browser caching, this process will all of a sudden become an important thing to understand.

そのため、 webpack が背後でどのように動いているかについて少し考えています。 " しかし、どのような影響があるのだろう？ " 、と尋ねるかもしれません。簡単な答えは殆どの場合影響がないということです。ランタイムは manifest を利用して行い、アプリケーションがブラウザにヒットしたらすべてが魔法のように動いているように見えます。しかしながら、ブラウザキャッシュを利用してプロジェクトのパフォーマンスを向上しようと決めた場合、本プロセスは突然理解すべき重要なものになります。

By using content hashes within your bundle file names, you can indicate to the browser when the contents of a file has changed thus invalidating the cache. Once you start doing this though, you'll immediately notice some funny behavior. Certain hashes change even when their contents apparently does not. This is caused by the injection of the runtime and manifest which changes every build.

バンドルファイル名でコンテンツハッシュを利用することでファイルが変更された場合にブラウザに指示しキャッシュを無効にすることができます。一度これを始めるとすぐに面白いふるまいに気づくでしょう。特定のハッシュはコンテンツが明らかでない時も変更されます。これはすべてのビルドを変更するランタイムと manifest の注入によって引き起こされます。

See [the manifest section](/guides/output-management#the-manifest) of our _Managing Built Files_ guide to learn how to extract the manifest, and read the guides below to learn more about the intricacies of long term caching.

manifest を抽出する方法を学ぶには、 _Managing Built Files_ ガイドの [the manifest section](/guides/output-management#the-manifest) を参照し、長期間のキャッシュの複雑さを学ぶためには以下のガイドを参照してください。
