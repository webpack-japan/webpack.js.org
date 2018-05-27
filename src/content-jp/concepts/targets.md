---
title: Targets
sort: 10
contributors:
  - TheLarkInn
  - rouzbeh84
  - johnstew
  - srilman
---

Because JavaScript can be written for both server and browser, webpack offers multiple deployment _targets_ that you can set in your webpack [configuration](/configuration).

JavaScript がサーバとブラウザの両方に記述できるため、 webpack は webpack [configuration](/configuration) で設定できる複数のデプロイ _target_ を提供しています。

W> The webpack `target` property is not to be confused with the `output.libraryTarget` property. For more information see [our guide](/concepts/output) on the `output` property.

W> webpack の `target` プロパティを `output.libraryTarget` プロパティと混同しないでください。詳細は  `output` プロパティの [our guide](/concepts/output) を参照してください。

## Usage

To set the `target` property, you simply set the target value in your webpack config:

`target` プロパティを設定するために、以下のように webpack の設定で target の値を設定するだけです。

**webpack.config.js**

```javascript
module.exports = {
  target: 'node'
};
```

In the example above, using `node` webpack will compile for usage in a Node.js-like environment (uses Node.js `require` to load chunks and not touch any built in modules like `fs` or `path`).

上記の例では、 `node` を利用して webpack が Node.js のような環境でコンパイルされます ( チャンクをロードするために Node.js の `require` を利用し、 `fs` や `path` のようなモジュールを触れません ) 。

Each _target_ has a variety of deployment/environment specific additions, support to fit its needs. See what [targets are available](/configuration/target).

互いの _target_ は様々なデプロイ / 環境固有の追加機能があり、必要に応じてサポートします。どの [targets are available](/configuration/target) か参照してください。

?>Further expansion for other popular target values

?> 他の一般的なターゲットの拡張

## Multiple Targets

Although webpack does **not** support multiple strings being passed into the `target` property, you can create an isomorphic library by bundling two separate configurations:

webpack は `target` プロパティに渡される複数の文字列を **サポートしていません** が、 2 つの別れた設定をバンドルすることで同形のライブラリを作成することができます。

**webpack.config.js**

```javascript
var path = require('path');
var serverConfig = {
  target: 'node',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'lib.node.js'
  }
  //…
};

var clientConfig = {
  target: 'web', // <=== can be omitted as default is 'web'
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'lib.js'
  }
  //…
};

module.exports = [ serverConfig, clientConfig ];
```

The example above will create a `lib.js` and `lib.node.js` file in your `dist` folder.

上記の例では `dist` フォルダに `lib.js` と `lib.node.js` ファイルを生成しています。

## Resources

As seen from the options above there are multiple different deployment _targets_ that you can choose from. Below is a list of examples, and resources that you can refer to.

上記のオプションからわかるように複数の異なるデプロイ _target_ があります。以下は参照できる例とリソースのリストです。

*  **[compare-webpack-target-bundles](https://github.com/TheLarkInn/compare-webpack-target-bundles)**: A great resource for testing and viewing different webpack _targets_. Also great for bug reporting.
* **[Boilerplate of Electron-React Application](https://github.com/chentsulin/electron-react-boilerplate)**: A good example of a build process for electron's main process and renderer process.

* **[compare-webpack-target-bundles](https://github.com/TheLarkInn/compare-webpack-target-bundles)**: 異なる webpack _targets_ をテストし表示するための素晴らしいリソースです。バグ報告にも最適です。
* **[Boilerplate of Electron-React Application](https://github.com/chentsulin/electron-react-boilerplate)**: エレクトロンのメインプロセスと描画プロセス用のビルドプロセスの良い例です。

?> Need to find up to date examples of these webpack targets being used in live code or boilerplates.

?> ライブコードや定型文で使用されている webpack の targets の最新版の例を探す必要があります。
