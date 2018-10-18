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

様々なインターフェースがコンパイルプロセスをカスタマイズすることができます。  
いくつかの機能はインタフェース間で重複します、例えば、設定オプションはCLIフラグを経由して利用でき、他は単一のインターフェースを通してのみ存在します。
以下が始めるべきハイレベルな情報です。


## CLI
<!--
The Command Line Interface (CLI) to configure and interact with your build. It
is especially useful in the case of early prototyping and profiling. For the
most part, the CLI is simply used to kick off the process using a configuration
file and a few flags (e.g. `--env`).

[Learn more about the CLI!](/api/cli)
-->

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

webpackでモジュールを処理するとき、モジュールは異なるモジュールの構文 -- 具体的には[メソッド](/api/module-methods)と[変数](/api/module-variables) -- がサポートされています。

[モジュールについてもっと学ぶ!](/api/module-methods)


<!--
## Node

While most users can get away with just using the CLI along with a
configuration file, more fine-grained control of the compilation can be
achieved via the Node interface. This includes passing multiple configurations,
programmatically running or watching, and collecting stats.

[Learn more about the Node API!](/api/node)
-->

## ノード

ほとんどのユーザーはCLIを使用するだけで設定ファイルと一緒に、コンパイルのより細かい制御がノードインターフェースを経由して得られます。これは複数の設定の通過、プログラムの実行や監視、統計データの受け取りが含まれます。

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

プラグインインターフェースはコンパイル処理で直接利用することを許可します。  
プラグインは コンパイル中のさまざまな時点で実行するライフサイクルのハンドラーに登録できます。
互いのフックが実行された時、プラグインはコンパイルの現在の状態にフルアクセスします。

[プラグインについてもっと学ぶ!](/api/plugins)
