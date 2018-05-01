---
title: Output
sort: 3
contributors:
  - TheLarkInn
  - chyipin
  - rouzbeh84
---

Configuring the `output` configuration options tells webpack how to write the compiled files to disk. Note that, while there can be multiple `entry` points, only one `output` configuration is specified.

`output` 設定オプションを設定することでコンパイルされたファイルを書き出す方法を webpack に指示します。複数の `entry` ポイントを存在させることができますが、 ただ 1 つの `output` 設定が指定されることに留意してください。

## Usage

The minimum requirements for the `output` property in your webpack config is to set its value to an object including the following two things:

webpack の設定で `output` プロパティの最小要件は以下の 2 つを含むオブジェクトに値を設定することです。

- A `filename` to use for the output file(s).
- An absolute `path` to your preferred output directory.

- 出力ファイル名に使う `filename`
- あなたの好みの出力ディレクトリへの絶対 `path`

**webpack.config.js**

```javascript
const config = {
  output: {
    filename: 'bundle.js',
    path: '/home/proj/public/assets'
  }
};

module.exports = config;
```

This configuration would output a single `bundle.js` file into the `/home/proj/public/assets` directory.

この設定は単一の `bundle.js` を `/home/proj/public/assets` ディレクトリへ出力します。

## Multiple Entry Points

If your configuration creates more than a single "chunk" (as with multiple entry points or when using plugins like CommonsChunkPlugin), you should use [substitutions](/configuration/output#output-filename) to ensure that each file has a unique name.

もし 設定で 1 つ以上の "chunk" を生成したい場合 ( 複数のエントリポイントの場合または CommonChunkPlugin のようなプラグインを使用した場合 ) 、各ファイルに固有の名前がついていることを保障するために [substitutions](/configuration/output#output-filename) を使用すべきです。

```javascript
{
  entry: {
    app: './src/app.js',
    search: './src/search.js'
  },
  output: {
    filename: '[name].js',
    path: __dirname + '/dist'
  }
}

// writes to disk: ./dist/app.js, ./dist/search.js
```


## Advanced

Here's a more complicated example of using a CDN and hashes for assets:

CDN とアセットのハッシュを使用したより複雑な例です。

**config.js**

```javascript
output: {
  path: "/home/proj/cdn/assets/[hash]",
  publicPath: "http://cdn.example.com/assets/[hash]/"
}
```

In cases when the eventual `publicPath` of output files isn't known at compile time, it can be left blank and set dynamically at runtime in the entry point file. If you don't know the `publicPath` while compiling, you can omit it and set `__webpack_public_path__` on your entry point.

出力ファイルの最終的な `publicPath` がコンパイル時に不明な場合は、空白のままにし実行時にエントリポイントファイルを動的に設定します。コンパイル中に `publicPath` を知らないのであれば、それを省略しエントリポイントで `__webpack_public_path__` を設定することもできます。

```javascript
__webpack_public_path__ = myRuntimePublicPath

// rest of your application entry
```
