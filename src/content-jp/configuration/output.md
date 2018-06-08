---
title: Output
sort: 5
contributors:
  - sokra
  - skipjack
  - tomasAlabes
  - mattce
  - irth
  - fvgs
  - dhurlburtusa
  - MagicDuck
---

The top-level `output` key contains set of options instructing webpack on how and where it should output your bundles, assets and anything else you bundle or load with webpack.

トップレベルの `output` キーはバンドル、アセット。 webpack でバンドルまたはロードする方法や場所を webpack に指示するオプションの集合が含まれています。


## `output.auxiliaryComment`

`string` `object`

When used in tandem with [`output.library`](#output-library) and [`output.libraryTarget`](#output-librarytarget), this option allows users to insert comments within the export wrapper. To insert the same comment for each `libraryTarget` type, set `auxiliaryComment` to a string:

[`output.library`](#output-library) と [`output.libraryTarget`](#output-librarytarget) が連携して利用あれる場合、本オプションはエクスポートのラッパーにコメントを総ニュすることができます。 各 `libraryTarget` タイプに同じコメントを挿入するために、 以下のように `auxiliaryComment` に文字列を設定します。

``` js
output: {
  library: "someLibName",
  libraryTarget: "umd",
  filename: "someLibName.js",
  auxiliaryComment: "Test Comment"
}
```

which will yield the following:

次の結果が得られます。

``` js
(function webpackUniversalModuleDefinition(root, factory) {
  // Test Comment
  if(typeof exports === 'object' && typeof module === 'object')
    module.exports = factory(require("lodash"));
  // Test Comment
  else if(typeof define === 'function' && define.amd)
    define(["lodash"], factory);
  // Test Comment
  else if(typeof exports === 'object')
    exports["someLibName"] = factory(require("lodash"));
  // Test Comment
  else
    root["someLibName"] = factory(root["_"]);
})(this, function(__WEBPACK_EXTERNAL_MODULE_1__) {
  // ...
});
```

For fine-grained control over each `libraryTarget` comment, pass an object:

それぞれの `libraryTarget` コメントに対するきめ細かい制御のために、以下のようにオブジェクトを渡します。

``` js
auxiliaryComment: {
  root: "Root Comment",
  commonjs: "CommonJS Comment",
  commonjs2: "CommonJS2 Comment",
  amd: "AMD Comment"
}
```


## `output.chunkFilename`

`string` `function`

This option determines the name of non-entry chunk files. See [`output.filename`](#output-filename) option for details on the possible values.

このオプションはエントリされていないチャンクファイルの名前を決定します。設定できる値の詳細については [`output.filename`](#output-filename) を参照してください。

Note that these filenames need to be generated at runtime to send the requests for chunks. Because of this, placeholders like `[name]` and `[chunkhash]` need to add a mapping from chunk id to placeholder value to the output bundle with the webpack runtime. This increases the size and may invalidate the bundle when placeholder value for any chunk changes.

これらのファイル名はチャンクへリクエストを送信するためのランタイムによって生成される必要があります。そのために、 `[name]` のようなプレースホルダーや `[chunkhash]` は webpack ランタイムでバンドルを出力するためにチャンク ID からプレースホルダーの値へのマッピングを追加する必要があります。これによりサイズが大きくなり、チャンクのプレースホルダーの値が変更した時にバンドルが無効になる可能性があります。

By default `[id].js` is used or a value inferred from [`output.filename`](#output-filename) (`[name]` is replaced with `[id]` or `[id].` is prepended).


## `output.chunkLoadTimeout`

`integer`

Number of milliseconds before chunk request expires, defaults to 120 000. This option is supported since webpack 2.6.0.

チャンクのリクエストが完了するまでのミリ秒。デフォルトは 120,000 。本オプションは webpack 2.6.0 からサポートされています。


## `output.crossOriginLoading`

`boolean` `string`

Only used when [`target`](/configuration/target) is web, which uses JSONP for loading on-demand chunks, by adding script tags.

[`target`](/configuration/target) が web の場合にのみ利用され、スクリプトタグの追加によりオンデマンドのチャンクのロードに JSONP が利用されます。

Enable [cross-origin](https://developer.mozilla.org/en/docs/Web/HTML/Element/script#attr-crossorigin) loading of chunks. The following values are accepted...

チャンクの [cross-origin](https://developer.mozilla.org/en/docs/Web/HTML/Element/script#attr-crossorigin) ローディングが可能です。以下の値が許可されています。

`crossOriginLoading: false` - Disable cross-origin loading (default)

`crossOriginLoading: false` - cross-origin ロ＝ディングを許可しません ( デフォルト )

`crossOriginLoading: "anonymous"` - Enable cross-origin loading **without credentials**

`crossOriginLoading: "anonymous"` - cross-origin ローディングを **認証なしに** 許可します

`crossOriginLoading: "use-credentials"` - Enable cross-origin loading **with credentials**

`crossOriginLoading: "use-credentials"` - cross-origin ローディングを **認証付きで** 許可します


## `output.jsonpScriptType`

`string`

Allows customization of the `script` type webpack injects `script` tags into the DOM to download async chunks. The following options are available:

`script` タイプのカスタムすることで webpack は非同期チャンクをダウンロードするために `script` タグを DOM へ注入します。以下のオプションが設定可能です。

- `"text/javascript"` (default)
- `"module"`: Use with ES6 ready code.

- `"text/javascript"` ( デフォルト )
- `"module"`: ES6 のコードを利用します。


## `output.devtoolFallbackModuleFilenameTemplate`

`string | function(info)`

A fallback used when the template string or function above yields duplicates.

テンプレート文字列または上記の関数が重複したときに利用されるフォールバックです。

See [`output.devtoolModuleFilenameTemplate`](#output-devtoolmodulefilenametemplate).

[`output.devtoolModuleFilenameTemplate`](#output-devtoolmodulefilenametemplate) を参照してください。


## `output.devtoolLineToLine`

`boolean | object`

> Avoid using this option as it is __deprecated__ and will soon be removed.

> 本オプションは __廃止__ され、早めに削除されるので利用しないでください。

Enables line to line mapping for all or some modules. This produces a simple source map where each line of the generated source is mapped to the same line of the original source. This is a performance optimization and should only be used if all input lines match generated lines.

すべてまたはいくつかのモジュールを行から行へのマッピングを許可します。これにより、生成されたソースの各行が元のソースの同じ行にマップされる単純なソースマップが生成されます。これはパフォーマンスの最適化であり、すべての入力行が生成された行と一致する場合にのみ使用してください。

Pass a boolean to enable or disable this feature for all modules (defaults to `false`). An object with `test`, `include`, `exclude` is also allowed. For example, to enable this feature for all javascript files within a certain directory:

すべてのモジュールに対して本機能を有効または無効にするための boolean を渡します ( デフォルトは `false` ) 。 `test` 、 `include` 、 `exclude` をもつオブジェクトも許可されます。例として、特定のディレクトリ内のすべての JavaScript ファイルに対して本機能を有効にするために以下を設定します。

``` js
devtoolLineToLine: { test: /\.js$/, include: 'src/utilities' }
```


## `output.devtoolModuleFilenameTemplate`

`string | function(info)`

This option is only used when [`devtool`](/configuration/devtool) uses an options which requires module names.

本オプションは [`devtool`](/configuration/devtool) がモジュール名を要求するオプションを利用する場合にのみ利用されます。

Customize the names used in each source map's `sources` array. This can be done by passing a template string or function. For example, when using `devtool: 'eval'`, this is the default:

各ソースマップの `sources` 配列で利用される名前をカスタムします。これはテンプレート文字列または関数を渡すことで実行されます。例として、 `devtool: 'eval'` を利用する場合、以下がデフォルトになります。

``` js
devtoolModuleFilenameTemplate: "webpack://[namespace]/[resource-path]?[loaders]"
```

The following substitutions are available in template strings (via webpack's internal [`ModuleFilenameHelpers`](https://github.com/webpack/webpack/blob/master/lib/ModuleFilenameHelpers.js)):

以下の代入は ( webpack の内部 [`ModuleFilenameHelpers`](https://github.com/webpack/webpack/blob/master/lib/ModuleFilenameHelpers.js) 経由の ) テンプレート文字列で利用可能です。

| Template                 | Description |
| ------------------------ | ----------- |
| [absolute-resource-path] | The absolute filename |
| [all-loaders]            | Automatic and explicit loaders and params up to the name of the first loader |
| [hash]                   | The hash of the module identifier |
| [id]                     | The module identifier |
| [loaders]                | Explicit loaders and params up to the name of the first loader |
| [resource]               | The path used to resolve the file and any query params used on the first loader |
| [resource-path]          | The path used to resolve the file without any query params |
| [namespace]              | The modules namespace. This is usually the library name when building as a library, empty otherwise |

| テンプレート               | 概要         |
| ------------------------ | ----------- |
| [absolute-resource-path] | 絶対パスのファイル名 |
| [all-loaders]            | 最初のローダーの名前までの自動および明示的なローダーやパラメーター |
| [hash]                   | モジュール識別子のハッシュ |
| [id]                     | モジュール識別子 |
| [loaders]                | 明示的なローダーと最初のローダーの名前までのパラメーター |
| [resource]               | ファイルを解決するために利用されるパスと最初のローダーで利用されるクエリパラメータ |
| [resource-path]          | クエリパラメータなしでファイルを解決するために利用されるパス |
| [namespace]              | モジュールの名前空間。 これは通常ライブラリとしてビルドするときのライブラリ名で、他の場合は空 |


When using a function, the same options are available camel-cased via the `info` parameter:

関数を利用する時、同じオプションは `info` パラメータを経由しキャメルケースで利用できます。

``` js
devtoolModuleFilenameTemplate: info => {
  return `webpack:///${info.resourcePath}?${info.loaders}`
}
```

If multiple modules would result in the same name, [`output.devtoolFallbackModuleFilenameTemplate`](#output-devtoolfallbackmodulefilenametemplate) is used instead for these modules.

複数のモジュールが同じ名前になる場合、 [`output.devtoolFallbackModuleFilenameTemplate`](#output-devtoolfallbackmodulefilenametemplate) がそれらのモジュールの代わりに利用されます。


## `output.devtoolNamespace`

`string`

This option determines the modules namespace used with the [`output.devtoolModuleFilenameTemplate`](#output-devtoolmodulefilenametemplate). When not specified, it will default to the value of: [`output.library`](#output-library). It's used to prevent source file path collisions in sourcemaps when loading multiple libraries built with webpack.

このオプションは [`output.devtoolModuleFilenameTemplate`](#output-devtoolmodulefilenametemplate) で利用されるモジュールの名前空間を決定します。指定しない場合、デフォルトの [`output.library`](#output-library) の値になります。これは webpack でビルドされた複数のライブラリのロード時にソースファイルパスが衝突するのを防ぐために使われます。

For example, if you have 2 libraries, with namespaces `library1` and `library2`, which both have a file `./src/index.js` (with potentially different contents), they will expose these files as `webpack://library1/./src/index.js` and `webpack://library2/./src/index.js`.

例として、`library1` と `library2` という名前空間の 2 つのライブラリがあり、それぞれで ( 潜在的に異なるコンテンツをもつ ) `./src/index.js` というファイルがある場合、それらのファイルは `webpack://library1/./src/index.js` と `webpack://library2/./src/index.js` として出力します。


## `output.filename`

`string` `function`

This option determines the name of each output bundle. The bundle is written to the directory specified by the [`output.path`](#output-path) option.

このオプションは出力する各バンドルの名前を決定します。バンドルは [`output.path`](#output-path) オプションで指定されたディレクトリに書き込まれます。

For a single [`entry`](/configuration/entry-context#entry) point, this can be a static name.

単一の [`entry`](/configuration/entry-context#entry) ポイントの場合、静的な名前にすることができます。

``` js
filename: "bundle.js"
```

However, when creating multiple bundles via more than one entry point, code splitting, or various plugins, you should use one of the following substitutions to give each bundle a unique name...

しかしながら 1 つ以上のエントリポイントや、コード分割、または様々なプラグインを経由して複数のバンドルを生成した場合、各バンドルにユニークな名前を与えるために以下の代替法の 1 つを利用しなければなりません。

Using entry name:

エントリ名を利用 :

``` js
filename: "[name].bundle.js"
```

Using internal chunk id:

内部チャンク ID を利用 : 

``` js
filename: "[id].bundle.js"
```

Using the unique hash generated for every build:

すべてのバンドルに対して生成されたユニークなハッシュを利用 :

``` js
filename: "[name].[hash].bundle.js"
```

Using hashes based on each chunks' content:

各チャンクのコンテンツを基にしたハッシュを利用 :

``` js
filename: "[chunkhash].bundle.js"
```

Make sure to read the [Caching guide](/guides/caching) for details. There are more steps involved than just setting this option.

詳細は [Caching guide](/guides/caching) を必ず読んでください。本オプションを設定するだけでなくより多くのステップがあります。

Note this option is called filename but you are still allowed to use something like `"js/[name]/bundle.js"` to create a folder structure.

このオプションは filename と呼ばれますが、未だフォルダ構成を生成するために `"js/[name]/bundle.js"` のように利用できることに注意してください。

Note this option does not affect output files for on-demand-loaded chunks. For these files the [`output.chunkFilename`](#output-chunkfilename) option is used. Files created by loaders also aren't affected. In this case you would have to try the specific loader's available options.

このオプションはオンデマンドロードされたチャンクの出力ファイルに影響しないことに注意してください。これらのファイルには [`output.chunkFilename`](#output-chunkfilename) オプションが使用されます。ローダーによってファイルが生成された場合も影響されません。このケースでは特定のローダーの利用可能なオプションを試す必要があります。

The following substitutions are available in template strings (via webpack's internal [`TemplatedPathPlugin`](https://github.com/webpack/webpack/blob/master/lib/TemplatedPathPlugin.js)):

以下の置き換えは ( webpack 内部の  [`TemplatedPathPlugin`](https://github.com/webpack/webpack/blob/master/lib/TemplatedPathPlugin.js) を介した ) テンプレート文字列にて利用可能です。

| Template    | Description                                                                         |
| ----------- | ----------------------------------------------------------------------------------- |
| [hash]      | The hash of the module identifier                                                   |
| [chunkhash] | The hash of the chunk content                                                       |
| [name]      | The module name                                                                     |
| [id]        | The module identifier                                                               |
| [query]     | The module query, i.e., the string following `?` in the filename                    |

| テンプレート  | 概要                                                                                |
| ----------- | ----------------------------------------------------------------------------------- |
| [hash]      | モジュール識別子のハッシュ                                                              |
| [chunkhash] | チャンクコンテンツのハッシュ                                                            |
| [name]      | モジュール名                                                                          |
| [id]        | モジュール識別子                                                                      |
| [query]     | モジュールクエリ 例として、ファイル名の `?` 以降の文字列                                    |

The lengths of `[hash]` and `[chunkhash]` can be specified using `[hash:16]` (defaults to 20). Alternatively, specify [`output.hashDigestLength`](#output-hashdigestlength) to configure the length globally.

`[hash]`　と `[chunkhash]` の長さは `[hash:16]` を使用して指定されます ( デフォルトは 20 ) 。あるいはグローバルに長さを設定するために [`output.hashDigestLength`](#output-hashdigestlength) を指定します。

If using a function for this option, the function will be passed an object containing the substitutions in the table above.

このオプションに関数を利用している場合、関数は上記のテーブルの置換を含むオブジェクトが渡されます。

T> When using the [`ExtractTextWebpackPlugin`](/plugins/extract-text-webpack-plugin), use `[contenthash]` to obtain a hash of the extracted file (neither `[hash]` nor `[chunkhash]` work).

T> [`ExtractTextWebpackPlugin`](/plugins/extract-text-webpack-plugin) を利用している場合、抽出されたファイルのハッシュを入手するために `[contenthash]` を利用します ( `[hash]` も `[chunkhash]` もどちらも動作しません ) 。


## `output.hashDigest`

The encoding to use when generating the hash, defaults to `'hex'`. All encodings from Node.JS' [`hash.digest`](https://nodejs.org/api/crypto.html#crypto_hash_digest_encoding) are supported.

ハッシュを生成するときに利用されるエンコーディングで、デフォルトは `'hex'` です。 Node.JS の [`hash.digest`](https://nodejs.org/api/crypto.html#crypto_hash_digest_encoding) のすべてのエンコーディングがサポートされます。


## `output.hashDigestLength`

The prefix length of the hash digest to use, defaults to `20`.

使用するハッシュダイジェストのプレフィックス長で、デフォルトは `20` です。


## `output.hashFunction`

`string|function`

The hashing algorithm to use, defaults to `'md5'`. All functions from Node.JS' [`crypto.createHash`](https://nodejs.org/api/crypto.html#crypto_crypto_createhash_algorithm) are supported. Since `4.0.0-alpha2`, the `hashFunction` can now be a constructor to a custom hash function. You can provide a non-crypto hash function for performance reasons.

使用するハッシュアルゴリズムで、デフォルトは `'md5'` です。すべての Node.JS の [`crypto.createHash`](https://nodejs.org/api/crypto.html#crypto_crypto_createhash_algorithm) 関数がサポートされています。 `4.0.0-alpha2` からは、 `hashFunction` はカスタムハッシュ関数のコンストラクタになりました。パフォーマンス上の理由により、非暗号ハッシュ関数を提供することができます。

``` js
hashFunction: require('metrohash').MetroHash64
```

Make sure that the hashing function will have `update` and `digest` methods available.

ハッシュ関数に `update` メソッドと `digest` メソッドがあることを確認してください。

## `output.hashSalt`

An optional salt to update the hash via Node.JS' [`hash.update`](https://nodejs.org/api/crypto.html#crypto_hash_update_data_input_encoding).

Node.JS の [`hash.update`](https://nodejs.org/api/crypto.html#crypto_hash_update_data_input_encoding) を介してハッシュを更新するオプションソルトです。


## `output.hotUpdateChunkFilename`

`string` `function`

Customize the filenames of hot update chunks. See [`output.filename`](#output-filename) option for details on the possible values.

チャンクをほっとアップデートするファイル名をカスタマイズします。設定可能な値の詳細については [`output.filename`](#output-filename) オプションを参照してください。

The only placeholders allowed here are `[id]` and `[hash]`, the default being:

プレースホルダーは `[id]` と `[hash]` だけが利用可能で、デフォルトは以下です。

``` js
hotUpdateChunkFilename: "[id].[hash].hot-update.js"
```

Here is no need to change it.

変更する必要はありません。


## `output.hotUpdateFunction`

`function`

Only used when [`target`](/configuration/target) is web, which uses JSONP for loading hot updates.

[`target`](/configuration/target) が web の場合にのみ利用され、ホットアップデートをロードするために JSONP を利用します。

A JSONP function used to asynchronously load hot-update chunks.

JSONP 関数は非同期的にホットアップデートチャンクをロードするために使用されます。

For details see [`output.jsonpFunction`](#output-jsonpfunction).

詳細は [`output.jsonpFunction`](#output-jsonpfunction) を参照してください。


## `output.hotUpdateMainFilename`

`string` `function`

Customize the main hot update filename. See [`output.filename`](#output-filename) option for details on the possible values.

メインのホットアップデートファイル名をカスタマイズします。設定可能な値の詳細については [`output.filename`](#output-filename) を参照してください。

`[hash]` is the only available placeholder, the default being:

`[hash]` のみ設定可能なプレースホルダーで、デフォルトは以下です。

``` js
hotUpdateMainFilename: "[hash].hot-update.json"
```

Here is no need to change it.

変更する必要はありません。


## `output.jsonpFunction`

`string`

Only used when [`target`](/configuration/target) is web, which uses JSONP for loading on-demand chunks.

[`target`](/configuration/target) が web の場合にのみ利用され、オンデマンドチャンクをロードするために JSONP を利用します。

A JSONP function name used to asynchronously load chunks or join multiple initial chunks (CommonsChunkPlugin, AggressiveSplittingPlugin).

JSONP の関数名は非同期にチャンクをロード、または複数の初期チャンク ( CommonsChunkPlugin 、 AggressiveSplittingPlugin ) を結合するために使用されます。

This needs to be changed if multiple webpack runtimes (from different compilation) are used on the same webpage.

複数の ( 異なるコンパイルからの ) webpack ランタイムが同じ web ページで利用されている場合に変更する必要がありあす。

If using the [`output.library`](#output-library) option, the library name is automatically appended.

[`output.library`](#output-library) オプションを利用している場合、ライブラリ名は自動的に追加されます。


## `output.library`

`string`

`string` or `object` (since webpack 3.1.0; for `libraryTarget: "umd"`)

How the value of the `output.library` is used depends on the value of the [`output.libraryTarget`](#output-librarytarget) option; please refer to that section for the complete details. Note that the default option for `output.libraryTarget` is `var`, so if the following configuration option is used:

``` js
output: {
  library: "MyLibrary"
}
```

The variable `MyLibrary` will be bound with the return value of your entry file, if the resulting output is included as a script tag in an HTML page.

もたらされた出力が HTML ページ内のスクリプトタグとして含まれている場合、変数 `MyLibrary` はエントリファイルの戻り値にバインドされます。

W> Note that if an `array` is provided as an `entry` point, only the last module in the array will be exposed. If an `object` is provided, it can exposed using an `array` syntax (see [this example](https://github.com/webpack/webpack/tree/master/examples/multi-part-library) for details).

W> `entry` ポイントとして `array` が提供された場合、配列の最後のモジュールのみが公開されることに注意してください。 `object` が提供された場合、 `array` 構文を利用して公開されることになります ( 詳細は [this example](https://github.com/webpack/webpack/tree/master/examples/multi-part-library) を参照してください ) 。

T> Read the [authoring libraries guide](/guides/author-libraries) guide for more information on `output.library` as well as `output.libraryTarget`.

T> `output.libraryTarget` と同様に `output.library` の詳細については [authoring libraries guide](/guides/author-libraries) のガイドを読んでください。


## `output.libraryExport`

`string` or `string[]` (since webpack 3.0.0)

> Default: `_entry_return_`

Configure which module or modules will be exposed via the `libraryTarget`. The default `_entry_return_` value is the namespace or default module returned by your entry file. The examples below demonstrate the effect of this config when using `libraryTarget: "var"`, but any target may be used.

`libraryTarget` を介してモジュールまたはモジュールたちが公開されるときに設定します。デフォルトの `_entry_return_` の値は名前空間かエントリファイルで戻されたデフォルトモジュールになります。以下の例は `libraryTarget: "var"` を利用した場合の設定の効果を示していますが、任意のターゲットも使用できます。

The following configurations are supported:

以下の設定がサポートされています。

`libraryExport: "default"` - The **default export of your entry point** will be assigned to the library target:

`libraryExport: "default"` - **エントリポイントのデフォルトエクスポート** はライブラリターゲットに割り当てられます。


``` js
// if your entry has a default export of `MyDefaultModule`
var MyDefaultModule = _entry_return_.default;
```

`libraryExport: "MyModule"` - The **specified module** will be assigned to the library target:

`libraryExport: "MyModule"` - **指定したモジュール** はライブラリターゲットに割り当てられます。


``` js
var MyModule = _entry_return_.MyModule;
```

`libraryExport: ["MyModule", "MySubModule"]` - The array is interpreted as a **path to a module** to be assigned to the library target:

`libraryExport: ["MyModule", "MySubModule"]` - 配列は **モジュールへのパス** として解釈され、ライブラリターゲットに割り当てられます。


``` js
var MySubModule = _entry_return_.MyModule.MySubModule;
```

With the `libraryExport` configurations specified above, the resulting libraries could be utilized as such:

上記で指定された `libraryExport` 設定では、得られるライブラリは以下のように利用できます。

``` js
MyDefaultModule.doSomething();
MyModule.doSomething();
MySubModule.doSomething();
```


## `output.libraryTarget`

`string`

> Default: `"var"`

Configure how the library will be exposed. Any one of the following options can be used. Please note that this option works in conjunction with the value assigned to [`output.library`](#output-library). For the following examples, it is assumed that this value is configured as `MyLibrary`.

ライブラリがどのように公開されるか設定します。任意の以下のオプションが利用されます。このオプションは [`output.library`](#output-library) に割り当てられた値と連動して動作することに注意してください。以下の例では、値が `MyLibrary` として設定されたとみなしています。

T> Note that `_entry_return_` in the example code below is the value returned by the entry point. In the bundle itself, it is the output of the function that is generated by webpack from the entry point.

T> 以下のサンプルコード内の `entry_return` はエントリポイントで戻された値であることに注意してください。バンドル自身では、 webpack がエントリポイントから生成された関数の出力になります。

### Expose a Variable

These options assign the return value of the entry point (e.g. whatever the entry point exported) to the name provided by `output.library` at whatever scope the bundle was included at.

これらのオプションはバンドルがどのような範囲に含まれているとしても `output.library` によって提供された名前にエントリポイントの戻り値 ( 例えば、エントリポイントがエクスポートされたもの ) が割り当てます。

`libraryTarget: "var"` - (default) When your library is loaded, the **return value of your entry point** will be assigned to a variable:

`libraryTarget: "var"` - ( デフォルト ) ライブラリがロードされたとき、 **エントリポイントの返り値** が変数に割り当てられます。


``` js
var MyLibrary = _entry_return_;

// In a separate script...
MyLibrary.doSomething();
```

W> When using this option, an empty `output.library` will result in no assignment.

W> 本オプションを利用するとき、空の `output.library` は割り当てしません。


`libraryTarget: "assign"` - This will generate an implied global which has the potential to reassign an existing value (use with caution).

`libraryTarget: "assign"` - これは既存の値を再割り当てする可能性のある暗黙のグローバルが生成されます ( 注意して使用してください ) 。


``` js
MyLibrary = _entry_return_;
```

Be aware that if `MyLibrary` isn't defined earlier your library will be set in global scope.

`MyLibrary` があらかじめ定義されていないと、ライブラリはグローバルスコープで設定されることに注意してください。

W> When using this option, an empty `output.library` will result in a broken output bundle.

W> 本オプションを利用するとき、空の `output.library` は出力バンドルが壊れます。


### Expose Via Object Assignment

These options assign the return value of the entry point (e.g. whatever the entry point exported) to a specific object under the name defined by `output.library`.

このオプションはエントリポイントの返り値 ( 例えばエントリポイントがエクスポートされたもの ) を `output.library` で定義された名前の指定したオブジェクトに割り当てます。

If `output.library` is not assigned a non-empty string, the default behavior is that all properties returned by the entry point will be assigned to the object as defined for the particular `output.libraryTarget`, via the following code fragment:

`output.library` が空ではない文字列に割り当てられない場合、デフォルトの振る舞いは、エントリポイントで返されたすべてのプロパティが以下のようなコードの断片を介して、特定の `output.libraryTarget` に対して定義されたオブジェクトに割り当てられます。

``` js
(function(e, a) { for(var i in a) e[i] = a[i]; }(${output.libraryTarget}, _entry_return_)
```

W> Note that not setting a `output.library` will cause all properties returned by the entry point to be assigned to the given object; there are no checks against existing property names.

W> `output.library` を設定しないとエントリポイントから返されたすべてのプロパティが与えられたオブジェクトに割り当てられることに注意してください。既存のプロパティ名に対するチェックはありません。

`libraryTarget: "this"` - The **return value of your entry point** will be assigned to this under the property named by `output.library`. The meaning of `this` is up to you:

`libraryTarget: "this"` - **エントリポイントの返り値** は `output.library` で指定されたプロパティ名に指定されます。 `this` の意味はあなた次第です。

``` js
this["MyLibrary"] = _entry_return_;

// In a separate script...
this.MyLibrary.doSomething();
MyLibrary.doSomething(); // if this is window
```

`libraryTarget: "window"` - The **return value of your entry point** will be assigned to the `window` object using the `output.library` value.

`libraryTarget: "window"` - **エントリポイントの返り値** は `output.library` の値を利用して `window` オブジェクトに割り当てられます。

``` js
window["MyLibrary"] = _entry_return_;

window.MyLibrary.doSomething();
```


`libraryTarget: "global"` - The **return value of your entry point** will be assigned to the `global` object using the `output.library` value.

`libraryTarget: "global"` - **エントリポイントの返り値** は `output.library` の値を利用して `global` オブジェクトに割り当てられます。

``` js
global["MyLibrary"] = _entry_return_;

global.MyLibrary.doSomething();
```


`libraryTarget: "commonjs"` - The **return value of your entry point** will be assigned to the `exports` object using the `output.library` value. As the name implies, this is used in CommonJS environments.

`libraryTarget: "commonjs"` - **エントリポイントの返り値** は `output.library` の値を利用して `exports` オブジェクトに割り当てられます。名前が示すように、これは CommonJS の環境で利用されます。

``` js
exports["MyLibrary"] = _entry_return_;

require("MyLibrary").doSomething();
```

### Module Definition Systems

These options will result in a bundle that comes with a more complete header to ensure compatibility with various module systems. The `output.library` option will take on a different meaning under the following `output.libraryTarget` options.

これらのオプションはさまざまなモジュールシステムと互換性を保証するためにより完全なヘッダが付属するバンドルになります。 `output.library` オプションは次の `output.libraryTarget` オプションにおいて異なる意味を持ちます。


`libraryTarget: "commonjs2"` - The **return value of your entry point** will be assigned to the `module.exports`. As the name implies, this is used in CommonJS environments:

`libraryTarget: "commonjs2"` - **エントリポイントの返り値** は `module.exports` に割り当てられます。名前が示すように、これは CommonJS の環境で利用されます。

``` js
module.exports = _entry_return_;

require("MyLibrary").doSomething();
```

Note that `output.library` is omitted, thus it is not required for this particular `output.libraryTarget`.

`output.library` は省略されているので、特定の `output.libraryTarget` では必須ではありません。

T> Wondering the difference between CommonJS and CommonJS2 is? While they are similar, there are some subtle differences between them that are not usually relevant in the context of webpack. (For further details, please [read this issue](https://github.com/webpack/webpack/issues/1114).)

T> CommonJS と CommonJS2 の違いは？それらは似ていますが、 webpack のコンテストでは通常関係ない微妙な違いがあります ( 詳細は [read this issue](https://github.com/webpack/webpack/issues/1114) を参照してください ) 。


`libraryTarget: "amd"` - This will expose your library as an AMD module.

`libraryTarget: "amd"` - これは AMD モジュールとしてライブラリを公開します。

AMD modules require that the entry chunk (e.g. the first script loaded by the `<script>` tag) be defined with specific properties, such as `define` and `require` which is typically provided by RequireJS or any compatible loaders (such as almond). Otherwise, loading the resulting AMD bundle directly will result in an error like `define is not defined`.

AMD モジュールは RequireJS または ( almond など ) 任意の互換性のあるローダーによって通常提供される `define` や `require` のような特定のプロパティによって定義されたエントリチャンク ( 例えば `<script>` タグでロードされた最初のスクリプト ) を要求します。

So, with the following configuration...

つまり、以下の設定で…

``` js
output: {
  library: "MyLibrary",
  libraryTarget: "amd"
}
```

The generated output will be defined with the name "MyLibrary", i.e.

例として、生成された output は "MyLibrary" という名前で定義されます。

``` js
define("MyLibrary", [], function() {
  return _entry_return_;
});
```

The bundle can be included as part of a script tag, and the bundle can be invoked like so:

バンドルは script タグの一部として含まれ、バンドルは以下のように呼び出されます。

``` js
require(['MyLibrary'], function(MyLibrary) {
  // Do something with the library...
});
```

If `output.library` is undefined, the following is generated instead.

`output.library` が undefined の場合、代わりに以下が生成されます。

``` js
define([], function() {
  return _entry_return_;
});
```

This bundle will not work as expected, or not work at all (in the case of the almond loader) if loaded directly with a `<script>` tag. It will only work through a RequireJS compatible asynchronous module loader through the actual path to that file, so in this case, the `output.path` and `output.filename` may become important for this particular setup if these are exposed directly on the server.

このバンドルは `<script>` タグで直接ロードされた場合、想定通りに動かず、 ( almond ローダーの場合は ) まったく動作しません。ファイルへの実際のパスを通して RequireJS 互換の非同期モジュールローダーを介してのみ動作するので、この場合、 `output.path` と `output.filename` はサーバ上で直接公開されている場合、特定のセットアップにとって重要になります。


`libraryTarget: "umd"` - This exposes your library under all the module definitions, allowing it to work with CommonJS, AMD and as global variable. Take a look at the [UMD Repository](https://github.com/umdjs/umd) to learn more.

`libraryTarget: "umd"` - すべてのモジュール定義の下にライブラリが公開され、 CommonJS 、 AMD やグローバル変数として動作します。詳細はは [UMD Repository](https://github.com/umdjs/umd) を参照してください。

In this case, you need the `library` property to name your module:

この場合、 モジュールを命名するために `library` プロパティが必要となります。

``` js
output: {
  library: "MyLibrary",
  libraryTarget: "umd"
}
```

And finally the output is:

また、最終的は出力は以下になります。

``` js
(function webpackUniversalModuleDefinition(root, factory) {
  if(typeof exports === 'object' && typeof module === 'object')
    module.exports = factory();
  else if(typeof define === 'function' && define.amd)
    define([], factory);
  else if(typeof exports === 'object')
    exports["MyLibrary"] = factory();
  else
    root["MyLibrary"] = factory();
})(typeof self !== 'undefined' ? self : this, function() {
  return _entry_return_;
});
```

Note that omitting `library` will result in the assignment of all properties returned by the entry point be assigned directly to the root object, as documented under the [object assignment section](#exposing-the-library-via-object-assignment). Example:

`library` を省略することで [object assignment section](#exposing-the-library-via-object-assignment) で文書化されたように、ルートオブジェクトにて直接割り当てられたエントリポイントで戻されたすべてのプロパティの割り当てになることに注意してください。例は以以下になります。

``` js
output: {
  libraryTarget: "umd"
}
```

The output will be:

出力は次のようになります。

``` js
(function webpackUniversalModuleDefinition(root, factory) {
  if(typeof exports === 'object' && typeof module === 'object')
    module.exports = factory();
  else if(typeof define === 'function' && define.amd)
    define([], factory);
  else {
    var a = factory();
    for(var i in a) (typeof exports === 'object' ? exports : root)[i] = a[i];
  }
})(typeof self !== 'undefined' ? self : this, function() {
  return _entry_return_;
});
```

Since webpack 3.1.0, you may specify an object for `library` for differing names per targets:

``` js
output: {
  library: {
    root: "MyLibrary",
    amd: "my-library",
    commonjs: "my-common-library"
  },
  libraryTarget: "umd"
}
```

Module proof library.

モジュールがライブラリを検査します。


### Other Targets

`libraryTarget: "jsonp"` - This will wrap the return value of your entry point into a jsonp wrapper.

`libraryTarget: "jsonp"` - これはエントリポイントの戻り値を json ラッパーにラップします。

``` javascript
MyLibrary(_entry_return_);
```

The dependencies for your library will be defined by the [`externals`](/configuration/externals/) config.

ライブラリの依存関係は [`externals`](/configuration/externals/) の設定に定義されています。


## `output.path`

`string`

The output directory as an **absolute** path.

**絶対** パスとしてディレクトリを出力します。

```js
path: path.resolve(__dirname, 'dist/assets')
```

Note that `[hash]` in this parameter will be replaced with an hash of the compilation. See the [Caching guide](/guides/caching) for details.

このパラメータの `[hash]` はコンパイルのハッシュと置換されます。詳細は [Caching guide](/guides/caching) を参照してください。


## `output.pathinfo`

`boolean`

Tell webpack to include comments in bundles with information about the contained modules. This option defaults to `false` and **should not** be used in production, but it's very useful in development when reading the generated code.

webpack に、バンドルに含まれるモジュールについての情報をコメントを含めるか指定します。このオプションのデフォルトは `false` で、本番環境では使用 **すべきではありません** が、開発時に生成されたコードを読む時にとても便利です。

``` js
pathinfo: true
```

Note it also adds some info about tree shaking to the generated bundle.

また、生成されたバンドルへのツリーシェイキングについての情報も追加しています。


## `output.publicPath`

`string` `function`

This is an important option when using on-demand-loading or loading external resources like images, files, etc. If an incorrect value is specified you'll receive 404 errors while loading these resources.

オンデマンドロードを利用、または画像、ファイルなどのような外部リソースをロードするときに重要なオプションです。誤った値が指定された場合、これらのリソースをロードする間ずっと 404 エラーを受け取るでしょう。

This option specifies the **public URL** of the output directory when referenced in a browser. A relative URL is resolved relative to the HTML page (or `<base>` tag). Server-relative URLs, protocol-relative URLs or absolute URLs are also possible and sometimes required, i. e. when hosting assets on a CDN.

このオプションはブラウザ内で参照された時、 output ディレクトリの **public URL** を指定します。相対 URL は HTML ページ ( または `<base>` タグ ) と関連して解決されます。サーバに関係する URL 、プロトコルに関係する URL または 絶対 URL もまた設定可能でたまに要求されます。例えば、 CDN 上でアセットをホスティングしている時です。

The value of the option is prefixed to every URL created by the runtime or loaders. Because of this **the value of this option ends with `/`** in most cases.

オプションの値はランタイムまたはローダーによって生成されたあらゆる URL に付けられます。そんなわけでほとんどの場合 **`/` で終わるオプションの値になります** 。

The default value is an empty string `""`.

デフォルト値は空文字 `""` です。

Simple rule: The URL of your [`output.path`](#output-path) from the view of the HTML page.

シンプルなルール : HTML ページのビューからの [`output.path`](#output-path) の URL。

```js
path: path.resolve(__dirname, "public/assets"),
publicPath: "https://cdn.example.com/assets/"
```

For this configuration:

この構成の場合

```js
publicPath: "/assets/",
chunkFilename: "[id].chunk.js"
```

A request to a chunk will look like `/assets/4.chunk.js`.

チャンクへのリクエストは `/assets/4.chunk.js` のようになります。

A loader outputting HTML might emit something like this:

HTML を出力するローダーは次のようなものを出力します。

```html
<link href="/assets/spinner.gif" />
```

or when loading an image in CSS:

または、 CSS で画像をロードするときは以下のようになります。

```css
background-image: url(/assets/spinner.gif);
```

The webpack-dev-server also takes a hint from `publicPath`, using it to determine where to serve the output files from.

webpack-dev-server もまた `publicPath` からのヒントを受け取り、出力ファイルがどこから提供するか決定するのに利用します。

Note that `[hash]` in this parameter will be replaced with an hash of the compilation. See the [Caching guide](/guides/caching) for details.

このパラメータの `[hash]` はコンパイルのハッシュに置換されることに注意してください。詳細は [Caching guide](/guides/caching) を参照してください。

Examples:

例 :

``` js
publicPath: "https://cdn.example.com/assets/", // CDN (always HTTPS)
publicPath: "//cdn.example.com/assets/", // CDN (same protocol)
publicPath: "/assets/", // server-relative
publicPath: "assets/", // relative to HTML page
publicPath: "../assets/", // relative to HTML page
publicPath: "", // relative to HTML page (same directory)
```

In cases where the `publicPath` of output files can't be known at compile time, it can be left blank and set dynamically at runtime in the entry file using the [free variable](http://stackoverflow.com/questions/12934929/what-are-free-variables) `__webpack_public_path__`.

コンパイル時に出力ファイルの `publicPath` がどこか知ることができない場合、空白のままにして実行時に [free variable](http://stackoverflow.com/questions/12934929/what-are-free-variables) `__webpack_public_path__` を利用してエントリファイルに動的に設定します。

``` js
 __webpack_public_path__ = myRuntimePublicPath

// rest of your application entry
```

See [this discussion](https://github.com/webpack/webpack/issues/2776#issuecomment-233208623) for more information on `__webpack_public_path__`.

`__webpack_public_path__` の詳細な情報については [this discussion](https://github.com/webpack/webpack/issues/2776#issuecomment-233208623) を参照してください。


## `output.sourceMapFilename`

`string`

This option is only used when [`devtool`](/configuration/devtool) uses a SourceMap option which writes an output file.

このオプションは [`devtool`](/configuration/devtool) が出力ファイルを書き込むソースマップオプションを利用するときにのみに利用されます。

Configure how source maps are named. By default `"[file].map"` is used.

ソースマップにどのように命名されるか設定します。デフォルトとして `"[file].map"` が使用されています。

The `[name]`, `[id]`, `[hash]` and `[chunkhash]` substitutions from [#output-filename](#output-filename) can be used. In addition to those, you can use substitutions listed below. The `[file]` placeholder is replaced with the filename of the original file. We recommend __only using the `[file]` placeholder__, as the other placeholders won't work when generating SourceMaps for non-chunk files.

[#output-filename](#output-filename) からの `[name]` 、 `[id]` 、 `[hash]` や `[chunkhash]` の置き換えが利用できます。これらに加えて、以下にリストされている置き換えが利用できます。 `[file]` プレースホルダーはオリジナルファイルのファイル名と置き換えます。他のプレースホルダーはノンチャンクファイル用のソースマップを生成するときには動作しないので __`[file]` プレースホルダーのみ使用する__ ことをおすすめします。

| Template                   | Description                                                                         |
| -------------------------- | ----------------------------------------------------------------------------------- |
| [file]                     | The module filename                                                                 |
| [filebase]                 | The module [basename](https://nodejs.org/api/path.html#path_path_basename_path_ext) |

| テンプレート                 | 概要                                                                                 |
| -------------------------- | ----------------------------------------------------------------------------------- |
| [file]                     | モジュールのファイル名                                                                  |
| [filebase]                 | [basename](https://nodejs.org/api/path.html#path_path_basename_path_ext) モジュール   |



## `output.sourcePrefix`

`string`

Change the prefix for each line in the output bundles.

出力バンドルの各行のプレフィックスを変更します。

``` js
sourcePrefix: "\t"
```

Note by default an empty string is used. Using some kind of indentation makes bundles look more pretty, but will cause issues with multi-line strings.

デフォルトでは空文字が利用されることに注意してください。いくつかの種類のインデントを利用するとより良く見えますが、複数行の文字列にて問題が発生するでしょう。

There is no need to change it.

変更する必要はありません。


## `output.strictModuleExceptionHandling`

`boolean`

Tell webpack to remove a module from the module instance cache (`require.cache`) if it throws an exception when it is `require`d.

`require` された際にエラーが発生した場合、 webpackにモジュールインスタンスキャッシュ (`require.cache`) からモジュールを削除するために指定します。

It defaults to `false` for performance reasons.

デフォルトはパフォーマンス上の理由から `false` です。

When set to `false`, the module is not removed from cache, which results in the exception getting thrown only on the first `require` call (making it incompatible with node.js).

`false` に設定すると、モジュールはキャッシュから削除されず、最初の `require` 呼び出しで例外がスローされます ( node.js と互換性がなくなります ) 。

For instance, consider `module.js`:

例えば、 `module.js` について考えてみます。

``` js
throw new Error("error");
```

With `strictModuleExceptionHandling` set to `false`, only the first `require` throws an exception:

`strictModuleExceptionHandling` を `false` に設定すると、最初の `require` でのみ例外がスローされます。

``` js
// with strictModuleExceptionHandling = false
require("module") // <- throws
require("module") // <- doesn't throw
```

Instead, with `strictModuleExceptionHandling` set to `true`, all `require`s of this module throw an exception:

代わりに、 `strictModuleExceptionHandling` を `true` に設定すると、モジュールのすべての `require` で例外がスローされます。

``` js
// with strictModuleExceptionHandling = true
require("module") // <- throws
require("module") // <- also throw
```


## `output.umdNamedDefine`

`boolean`

When using `libraryTarget: "umd"`, setting:

`libraryTarget: "umd"` を使用しているときに、以下のように設定します。

``` js
umdNamedDefine: true
```

will name the AMD module of the UMD build. Otherwise an anonymous `define` is used.

UMD ビルドの AMD モジュールに名前を付けます。それ以外の場合は `define` が使用されます。
