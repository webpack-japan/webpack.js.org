---
title: Configuration
sort: 6
contributors:
- TheLarkInn
- simon04
---

You may have noticed that few webpack configurations look exactly alike. This is because **webpack's configuration file is a JavaScript file that exports an object.** This object is then processed by webpack based upon its defined properties.

webpack の設定がまったく同じに見えることはほとんどありません。 これは **webpack の設定ファイルがオブジェクトをエクスポートする JavaScript ファイル** であるためです。 このオブジェクトは定義されたプロパティに基づいて webpack によって処理されます。

Because it's a standard Node.js CommonJS module, you **can do the following**:

標準的な Node.js の CommonJS モジュールのため、 **以下を行うことができます** 。

* import other files via `require(...)`
* use utilities on npm via `require(...)`
* use JavaScript control flow expressions i. e. the `?:` operator
* use constants or variables for often used values
* write and execute functions to generate a part of the configuration

* `require(...)` 経由で他のファイルをインポートする
* `require(...)` 経由で npm 上のユーティリティを使う
* 例えば `?:` 演算子のような JavaScript 制御フロー構文を使う
* 頻繁に使用される値の定数や変数を使う
* 設定の一部を生成するために関数を書き実行する

Use these features when appropriate.

これらの機能は必要に応じて使用してください。

While they are technically feasible, **the following practices should be avoided**:

技術的には可能ですが、 **以下の習慣は避けてください**。

* Access CLI arguments, when using the webpack CLI (instead write your own CLI, or [use `--env`](/configuration/configuration-types/))
* Export non-deterministic values (calling webpack twice should result in the same output files)
* Write long configurations (instead split the configuration into multiple files)

* webpack CLI を使用している際に CLI の引数にアクセスする ( 代わりに自身の CLI を書くか or [`--env` を使用 ](/configuration/configuration-types/) してください )
* 非決定性の値をエクスポートする (webpack を 2 度呼び出すと同じ出力ファイルになるべきです )
* 長い設定を書く ( 代わりに複数のファイルに設定を分割してください )

T> The most important part to take away from this document is that there are many different ways to format and style your webpack configuration. The key is to stick with something consistent that you and your team can understand and maintain.

T> このドキュメントの損なう最も重要な部分は webpack の設定をフォーマットしスタイルを設定する異なる方法が沢山あることです。 鍵はあなたとあなたのチームが理解しメンテナンスできる一貫性のあるものにし続けることです。

The following examples below describe how webpack's configuration object can be both expressive and configurable because _it is code_:

以下の例では webpack の設定が設定が _コードであるおかげで_ 表現力豊かで設定可能になる方法について示します。

## The Simplest Configuration

**webpack.config.js**

```javascript
var path = require('path');

module.exports = {
  entry: './foo.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'foo.bundle.js'
  }
};
```

## Multiple Targets

_See_: [Exporting multiple configurations](/configuration/configuration-types/#exporting-multiple-configurations)

_参照_ :  [Exporting multiple configurations](/configuration/configuration-types/#exporting-multiple-configurations)

## Using other Configuration Languages

webpack accepts configuration files written in multiple programming and data languages.

webpack は 複数のプログラミングまたはデータ言語で書かれたファイルを許容します。

_See_: [Configuration Languages](/configuration/configuration-languages/)

_参照_ : [Configuration Languages](/configuration/configuration-languages/)
