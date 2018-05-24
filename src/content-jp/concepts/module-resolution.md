---
title: Module Resolution
sort: 8
contributors:
    - pksjce
    - pastelsky
---

A resolver is a library which helps in locating a module by its absolute path.
A module can be required as a dependency from another module as:

リゾルバはモジュールの絶対パスでモジュールを探すのに役立つライブラリです。
モジュールは次のように他のモジュールからの依存として要求することができます。

```js
import foo from 'path/to/module'
// or
require('path/to/module')
```

The dependency module can be from the application code or a third party library. The resolver helps
webpack find the module code that needs to be included in the bundle for every such `require`/`import` statement.
webpack uses [enhanced-resolve](https://github.com/webpack/enhanced-resolve) to resolve file paths while bundling modules.

依存するモジュールはアプリケーションのコードまたはサードパーティライブラリがありえます。
リゾルバはすべての `require`/`import` ステートメント用のバンドルに含まれる必要なモジュールコードを webpack が探すのを手助けします。
webpack はモジュールをバンドルする間ファイルパスを解決するために [enhanced-resolve](https://github.com/webpack/enhanced-resolve) を利用します。


## Resolving rules in webpack

Using `enhanced-resolve`, webpack can resolve three kinds of file paths:

`enhanced-resolve` を使うため、 webpack は 3 種類のファイルパスを解決します。


### Absolute paths

```js
import "/home/me/file";

import "C:\\Users\\me\\file";
```

Since we already have the absolute path to the file, no further resolution is required.

ファイルへの絶対パスがすでにあるため、それ以上の解決は必要ありません。


### Relative paths

```js
import "../src/file1";
import "./file2";
```

In this case, the directory of the resource file where the `import` or `require` occurs is taken to be the context directory. The relative path specified in the `import/require` is joined to this context path to produce the absolute path to the module.

この場合、 `import` または `require` が存在するリソースファイルのディレクトリはコンテキストディレクトリとみなされます。 `import/require` で指定された相対パスがこのコンテキストパスに結合され、モジュールへの絶対パスが生成されます。


### Module paths

```js
import "module";
import "module/lib/file";
```

Modules are searched for inside all directories specified in [`resolve.modules`](/configuration/resolve/#resolve-modules).
You can replace the original module path by an alternate path by creating an alias for it using [`resolve.alias`](/configuration/resolve/#resolve-alias) configuration option.

モジュールは [`resolve.modules`](/configuration/resolve/#resolve-modules) 内で指定されたすべてのディレクトリ内部で走査されます。
[`resolve.alias`](/configuration/resolve/#resolve-alias) 設定オプションを利用しエイリアスを生成することで元のモジュールパスを代替パスで置き換えることができます。

Once the path is resolved based on the above rule, the resolver checks to see if the path points to a file or a directory. If the path points to a file:

上記のルールに従いパスが解決されると、リゾルバはパスがファイルまたはディレクトリを指定しているかチェックします。パスがファイルを指定している場合、

* If the path has a file extension, then the file is bundled straightaway.
* Otherwise, the file extension is resolved using the [`resolve.extensions`](/configuration/resolve/#resolve-extensions) option, which tells the resolver which extensions (eg - `.js`, `.jsx`) are acceptable for resolution.

* パスにファイル拡張子がある場合、ファイルはすぐにバンドルされます。
* それ以外は、ファイル拡張子が [`resolve.extensions`](/configuration/resolve/#resolve-extensions) オプションを利用して解決され、リゾルバが拡張子 ( 例 : `.js` 、 `.jsx`) が解決可能か教えます。

If the path points to a folder, then the following steps are taken to find the right file with the right extension:

パスがフォルダを指定している場合、正しい拡張子をもつファイルを見つけるために次のステップが実行されます。

* If the folder contains a `package.json` file, then fields specified in [`resolve.mainFields`](/configuration/resolve/#resolve-mainfields) configuration option are looked up in order, and the first such field in `package.json` determines the file path. 
* If there is no `package.json` or if the main fields do not return a valid path, file names specified in the [`resolve.mainFiles`](/configuration/resolve/#resolve-mainfiles) configuration option are looked for in order, to see if a matching filename exists in the imported/required directory .
* The file extension is then resolved in a similar way using the `resolve.extensions` option.

* フォルダが `package.json` ファイルを含むディレクトリの場合、 [`resolve.mainFields`](/configuration/resolve/#resolve-mainfields) 設定オプション内で指定されたフィールドが順番に調べられ、 `package.json` 内のフィールドの最初のパスがファイルパスを決定します。
* `package.json` がない、またはメインフィールドが有効なパスを返せない場合、 [`resolve.mainFiles`](/configuration/resolve/#resolve-mainfiles) 設定オプションで指定されたファイル名が順番に調べられ、 import/require されたディレクトリ内で一致するファイルが存在するか確認します。
* ファイル拡張子は `resolve.extensions` オプションを利用して同様の方法で解決されます。

webpack provides reasonable [defaults](/configuration/resolve) for these options depending on your build target.

webpack はビルドターゲットに依存してこれらのオプション用の妥当な [defaults](/configuration/resolve) を提供します。


## Resolving Loaders

This follows the same rules as those specified for file resolution. But the [`resolveLoader`](/configuration/resolve/#resolveloader) configuration option can be used to have separate resolution rules for loaders.

これはファイル解決に指定された同じルールに従います。しかし [`resolveLoader`](/configuration/resolve/#resolveloader) 設定オプションは loader 用に別々の解決ルールをもつために利用できます。


## Caching

Every filesystem access is cached, so that multiple parallel or serial requests to the same file occur faster. In [watch mode](/configuration/watch/#watch), only modified files are evicted from the cache. If watch mode is off, then the cache gets purged before every compilation.

すべてのファイルシステムのアクセスはキャッシュされるため、同じファイルに対して複数のパラレルまたはシリアルなリクエストがより高速に実行されます。 [watch mode](/configuration/watch/#watch) では、変更されたファイルのみキャッシュから削除されます。　watch mode をオフの場合、キャッシュはすべてのコンパイル前に開放されます。


See [Resolve API](/configuration/resolve) to learn more on the configuration options mentioned above.

上記の設定オプションの詳細については [Resolve API](/configuration/resolve) を参照してください。
