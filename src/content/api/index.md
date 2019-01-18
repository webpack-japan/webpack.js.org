---
title: Introduction
sort: 1
contributors:
  - tbroadley
---

<!--
A variety of interfaces are available to customize the compilation process.
Some features overlap between interfaces, e.g. a configuration option may be
available via a CLI flag, while others exist only through a single interface.
The following high-level information should get you started.
-->

コンパイルプロセスをカスタマイズするためには様々なインターフェースがあります。  
いくつかの機能はインタフェース間で重複します、例えば、設定オプションはCLIフラグを経由して利用でき、他は単一のインターフェースを通してのみ存在します。
以下の大まかな情報から始めましょう。


<!--
## CLI

The Command Line Interface (CLI) to configure and interact with your build. It
is especially useful in the case of early prototyping and profiling. For the
most part, the CLI is simply used to kick off the process using a configuration
file and a few flags (e.g. `--env`).

[Learn more about the CLI!](/api/cli)
-->

# CLI

ビルドを構成して対話するためのコマンドラインインターフェース(CLI)です。これは初期のプロトタイピングやプロファイリングにおいて特に役立ちます。ほとんどの場合、CLIはプロセスを始めるために設定ファイルと少しのフラグ(例えば `--env`)を使ってシンプルに利用されます。

[CLIについてもっと学ぶ!](/api/cli)

<!--
## Module

When processing modules with webpack, it is important to understand the
different module syntaxes -- specifically the [methods](/api/module-methods)
and [variables](/api/module-variables) -- that are supported.

[Learn more about modules!](/api/module-methods)
-->

# モジュール

webpackでモジュールを処理するとき、サポートされているさまざまなモジュールの構文を理解することが重要です -- 具体的には[メソッド](/api/module-methods)と[変数](/api/module-variables)がサポートされています。

[モジュールについてもっと学ぶ!](/api/module-methods)


<!--
## Node

While most users can get away with just using the CLI along with a
configuration file, more fine-grained control of the compilation can be
achieved via the Node interface. This includes passing multiple configurations,
programmatically running or watching, and collecting stats.

[Learn more about the Node API!](/api/node)
-->

## Node

ほとんどのユーザーは設定ファイルと一緒にCLIをつかうだけでなんとかなりますが、Nodeインターフェースを経由することでコンパイルのより細かい制御が可能になります。これは複数の設定の受け渡し、プログラムの実行や監視、統計データの受け取りが含まれます。

[Node APIについてもっと学ぶ!](/api/node)


<!--
## Loaders

Loaders are transformations that are applied to the source code of a module.
They are written as functions that accept source code as a parameter and return
a new version of that code with transformations applied.

[Learn more about loaders!](/api/loaders)
-->

## ローダー

ローダーはモジュールのソースコードが適用されるための変換処理です。これらはソースコードをパラメーターとして受け入れる関数として書かれ、変換が適用されたコードの新しいバージョンを返します。

[ローダーについてもっと学ぶ!](/api/loaders)


<!--
## Plugins

The plugin interface allows users to tap directly into the compilation process.
Plugins can register handlers on lifecycle hooks that run at different points
throughout a compilation. When each hook is executed, the plugin will have full
access to the current state of the compilation.

[Learn more about plugins!](/api/plugins)
-->

## プラグイン

プラグインインターフェースはコンパイルプロセスで直接利用することができます。
プラグインはコンパイル中のさまざまなタイミングで実行されるライフサイクルフックにハンドラーを登録できます。
それぞれのフックが実行された時に、プラグインはコンパイルの現在の状態に完全にアクセスします。

[プラグインについてもっと学ぶ!](/api/plugins)
