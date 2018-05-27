---
title: Hot Module Replacement
sort: 11
contributors:
  - SpaceK33z
  - sokra
  - GRardB
  - rouzbeh84
  - skipjack
---

Hot Module Replacement (HMR) exchanges, adds, or removes [modules](/concepts/modules/) while an application is running, without a full reload. This can significantly speed up development in a few ways:

Hot Module Replacement (HMR) はアプリケーションが実行している間完全なリロードをせず、 [modules](/concepts/modules/) を変更、追加、削除します。いくつかの方法で開発をスピードアップできます。

- Retain application state which is lost during a full reload.
- Save valuable development time by only updating what's changed.
- Tweak styling faster -- almost comparable to changing styles in the browser's debugger.

- フルリロード中に失われたアプリケーションの状態を保持します。
- 変更箇所を変更するだけなので貴重な開発時間を節約できます。
- ブラウザのデバッガでスタイルを変更するのと同じようにすばやくスタイルを微調整します。


## How It Works

Let's go through some different viewpoints to understand exactly how HMR works...

HMR がどのように動いているか正確に理解するためにいくつかの異なる視点で見てみましょう…

### In the Application

The following steps allow modules to be swapped in and out of an application:

次のステップではモジュールをアプリケーションにスワップインまたはスワップアウトできます。

1. The application asks the HMR runtime to check for updates.
2. The runtime asynchronously downloads the updates and notifies the application.
3. The application then asks the runtime to apply the updates.
4. The runtime synchronously applies the updates.

1. アプリケーションは HMR ランタイムに更新をチェックするように要求します。
2. ランタイムはアプリケーションの更新を非同期でダウンロードし、アプリケーションに通知します。
3. そしてアプリケーションは更新を適用するようにランタイムに要求します。
4. ランタイムは更新を同期的に適用します。

You can set up HMR so that this process happens automatically, or you can choose to require user interaction for updates to occur.

この処理が自動的に行われるように HMR に設定することも、更新が発生するためのユーザインタラクションを要求するように選択することもできます。


### In the Compiler

In addition to normal assets, the compiler needs to emit an "update" to allow updating from previous version to the new version. The "update" consists of two parts:

通常のアセットに加えて、コンパイラは以前のバーションから新しいバージョンへの更新できるようにするために "update" を発行する必要があります。 "update" は 2 つ部分から構成されています。

1. The updated [manifest](/concepts/manifest) (JSON)
2. One or more updated chunks (JavaScript)

1. 更新された [manifest](/concepts/manifest) (JSON)
2. 1 つ以上のチャンク (JavaScript)

The manifest contains the new compilation hash and a list of all updated chunks. Each of these chunks contains the new code for all updated modules (or a flag indicating that the module was removed).

マニフェストは新しいコンパイルハッシュとすべての更新されたチャンクのリストが含まれています。これら各チャンクは更新されたモジュールの新しいコード ( またはモジュールが削除されたことを示すフラグ ) が含まれています。

The compiler ensures that module IDs and chunk IDs are consistent between these builds. It typically stores these IDs in memory (e.g. with [webpack-dev-server](/configuration/dev-server/)), but it's also possible to store them in a JSON file.

コンパイラはモジュールの ID とチャンク ID がビルドの間で一貫していることを保証します。通所これらの ID はメモリに保存されます ( 例 :  [webpack-dev-server](/configuration/dev-server/) ) が、 JSON ファイルに保存することも可能です。


### In a Module

HMR is an opt-in feature that only affects modules containing HMR code. One example would be patching styling through the [`style-loader`](https://github.com/webpack/style-loader). In order for patching to work, the `style-loader` implements the HMR interface; when it receives an update through HMR, it replaces the old styles with the new ones.

HMR は HMR コードに含まれるモジュールのみに影響するオプトイン機能です。 1 つの例は [`style-loader`](https://github.com/webpack/style-loader) を通してスタイルをパッチすることです。パッチを動かすために、 `style-loader` は HMR インタフェースを実施しています。 HMR を通して更新を受け取ると、古いスタイルを新しいスタイルへ置換します。

Similarly, when implementing the HMR interface in a module, you can describe what should happen when the module is updated. However, in most cases, it's not mandatory to write HMR code in every module. If a module has no HMR handlers, the update bubbles up. This means that a single handler can update a complete module tree. If a single module from the tree is updated, the entire set of dependencies is reloaded.

同様に、モジュールが HMR インタフェースを実装する場合、モジュールが更新されたときに何が起こるべきか記述することができます。しかしながら、ほとんどの場合、すべてのモジュールに HMR コードを記述する必要はありません。モジュールに HMR ハンドラがない場合、更新はバブルアップします。これは単一のハンドラが完全なモジュールツリーを更新することができることを意味しています。ツリーの 1 つのモジュールが更新された場合、依存関係のセット全体がリロードされます。

See the [HMR API page](/api/hot-module-replacement) for details on the `module.hot` interface.

`module.hot` インタフェースの詳細については [HMR API page](/api/hot-module-replacement) を参照してください。


### In the Runtime

Here things get a bit more technical... if you're not interested in the internals, feel free to jump to the [HMR API page](/api/hot-module-replacement) or [HMR guide](/guides/hot-module-replacement).

ここで少し技術的な話があります…もし内部に興味がないのであれば、 [HMR API page](/api/hot-module-replacement) または [HMR guide](/guides/hot-module-replacement) にジャンプするとよいでしょう。

For the module system runtime, additional code is emitted to track module `parents` and `children`. On the management side, the runtime supports two methods: `check` and `apply`.

モジュールシステムランタイム用に、追加コードが `parents` や `children` モジュールを追跡するために発行されます。管理側では、ランタイムは `check` と `apply` という 2 つのメソッドをサポートしています。

A `check` makes an HTTP request to the update manifest. If this request fails, there is no update available. If it succeeds, the list of updated chunks is compared to the list of currently loaded chunks. For each loaded chunk, the corresponding update chunk is downloaded. All module updates are stored in the runtime. When all update chunks have been downloaded and are ready to be applied, the runtime switches into the `ready` state.

`check` は 更新マニフェストへ HTTP リクエストを行います。 もしリクエストが失敗した場合、利用可能な更新はありません。成功した場合、更新されたチャンクのリストが現在ロードされているチャンクのリストと比較します。ロードされた各チャンクについて対応する更新チャンクがダウンロードされます。すべての更新はランタイムに保存されます。すべての更新チャンクがダウンロードされ適用可能になった時、ランタイムは `ready` state に切り替わります。

The `apply` method flags all updated modules as invalid. For each invalid module, there needs to be an update handler in the module or in its parent(s). Otherwise, the invalid flag bubbles up and invalidates parent(s) as well. Each bubble continues until the app's entry point or a module with an update handler is reached (whichever comes first). If it bubbles up from an entry point, the process fails.

`apply` メソッドは更新されたモジュールを無効としてフラグを立てます。無効なモジュールごとに、モジュールまたは自身の親に更新ハンドラが必要です。それ以外の場合、無効なフラグがバブルアップし、親も無効になります。書くバブルはアプリケーションのエントリポイントまはた更新ハンドラに到達するまで ( のどちらか早いほう ) 続きます 。エントリポイントまでバブルアップした場合、この処理は失敗します。

Afterwards, all invalid modules are disposed (via the dispose handler) and unloaded. The current hash is then updated and all `accept` handlers are called. The runtime switches back to the `idle` state and everything continues as normal.

その後、すべての無効なモジュールは ( dispose ハンドラを介して ) 廃棄されアンロードされます。そして現在のハッシュは更新され、すべての `accept` ハンドラが呼び出されます。ランタイムは `idle` 状態に戻り、すべてが正常に続きます。


## Get Started

HMR can be used in development as a LiveReload replacement. [webpack-dev-server](/configuration/dev-server/) supports a `hot` mode in which it tries to update with HMR before trying to reload the whole page. See the [Hot Module Replacement guide](/guides/hot-module-replacement) for details.

HMR は LiveReload の代わりとして開発に利用できます。 [webpack-dev-server](/configuration/dev-server/)  はページ全体をリロードしようとする前にHMRで更新を試みる `hot` モードをサポートしています。詳細は [Hot Module Replacement guide](/guides/hot-module-replacement) を参照してください。

T> As with many other features, webpack's power lies in its customizability. There are _many_ ways of configuring HMR depending on the needs of a particular project. However, for most purposes, `webpack-dev-server` is a good fit and will allow you to get started with HMR quickly.

T> 他の機能と同様に webpack のパワーはカスタマイズ性にあります。特定のプロジェクトのニーズに応じて HMR を設定する方法は _たくさん_ あります。しかしながら、ほとんどの目的に `webpack-dev-server` はとてもフィットし、 HMR をすぐに使い始めることができます。
