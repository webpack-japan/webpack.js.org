---
title: Configuration Types
sort: 3
contributors:
  - sokra
  - skipjack
  - kbariotis
  - simon04
---

Besides exporting a single config object, there are a few more ways that cover other needs as well.

単一の設定オブジェクトのエクスポートするだけでなく、他のニーズにも対応できる方法がいくつかあります。


## Exporting a Function

Eventually you will find the need to disambiguate in your `webpack.config.js` between [development](/guides/development) and [production builds](/guides/production). You have (at least) two options:

最終的に [development](/guides/development) と [production builds](/guides/production) の間の `webpack.config.js` の曖昧さをなくす必要があります。 ( 少なくとも ) 2 つの選択肢があります。

One option is to export a function from your webpack config instead of exporting an object. The function will be invoked with two arguments:

1 つ目の選択肢はオブジェクトをエクスポートする代わりに webpack の設定から関数をエクスポートすることです。関数は 2 つの引数を伴って呼び出されます。

* An environment as the first parameter. See the [environment options CLI documentation](/api/cli#environment-options) for syntax examples.
* An options map (`argv`) as the second parameter. This describes the options passed to webpack, with keys such as [`output-filename`](/api/cli/#output-options) and [`optimize-minimize`](/api/cli/#optimize-options).

* 最初のパラメータとして環境があります。構文の例は [environment options CLI documentation](/api/cli#environment-options) を参照してください。
* 2 つ目のパラメータとしてオプションのマップ (`argv`) があります。 [`output-filename`](/api/cli/#output-options) と [`optimize-minimize`](/api/cli/#optimize-options) というキーを使い、webpack へ渡されるためのオプションとして記述します。

```diff
-module.exports = {
+module.exports = function(env, argv) {
+  return {
+    devtool: env.production ? 'source-maps' : 'eval',
     plugins: [
       new webpack.optimize.UglifyJsPlugin({
+        compress: argv['optimize-minimize'] // only if -p or --optimize-minimize were passed
       })
     ]
+  };
};
```


## Exporting a Promise

webpack will run the function exported by the configuration file and wait for a Promise to be returned. Handy when you need to asynchronously load configuration variables.

webpack は設定ファイルによりエクスポートされた関数を実行し、 Promise が戻ってくるまで待機します。構成変数を非同期でロードする必要がある場合に便利です。

```js
module.exports = () => {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve({
        entry: './app.js',
        /* ... */
      })
    }, 5000)
  })
}
```


## Exporting multiple configurations

Instead of exporting a single configuration object/function, you may export multiple configurations (multiple functions are supported since webpack 3.1.0). When running webpack, all configurations are built. For instance, this is useful for [bundling a library](/guides/author-libraries) for multiple [targets](/configuration/output#output-librarytarget) such as AMD and CommonJS:

単一の設定オブジェクト / 関数をエクスポートすることの代わりに、複数の設定 ( 複数の関数は webpack 3.1.0 からサポート済 ) をエクスポートすることもできます。 webpack が実行している間、すべての設定はビルドあれます。例えば、以下のように AMD や CommonJS などの複数の [targets](/configuration/output#output-librarytarget) の [ライブラリをビルド](/guides/author-libraries) するのに便利です。

```js
module.exports = [{
  output: {
    filename: './dist-amd.js',
    libraryTarget: 'amd'
  },
  entry: './app.js',
}, {
  output: {
    filename: './dist-commonjs.js',
    libraryTarget: 'commonjs'
  },
  entry: './app.js',
}]
```
