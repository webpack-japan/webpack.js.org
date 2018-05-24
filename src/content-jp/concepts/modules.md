---
title: Modules
sort: 7
contributors:
  - TheLarkInn
  - simon04
  - rouzbeh84
related:
   - title: JavaScript Module Systems Showdown
     url: https://auth0.com/blog/javascript-module-systems-showdown/
---

In [modular programming](https://en.wikipedia.org/wiki/Modular_programming), developers break programs up into discrete chunks of functionality called a _module_.

[modular programming](https://en.wikipedia.org/wiki/Modular_programming) では、開発者はプログラムを _module_ と呼ばれる個別の機能のチャンクに分割します。

Each module has a smaller surface area than a full program, making verification, debugging, and testing trivial.
Well-written _modules_ provide solid abstractions and encapsulation boundaries, so that each module has a coherent design and a clear purpose within the overall application.

各モジュールはフルプログラムより表面積が小さく、検証、デバッグ、テストが簡単です。
よく書かれた _modules_ は堅実な抽象化とカプセル化の境界を提供するので、各モジュールは理解しやすいデザインとアプリケーション全体の中で明確な目的をもちます。

Node.js has supported modular programming almost since its inception.
On the web, however, support for _modules_ has been slow to arrive.
Multiple tools exist that support modular JavaScript on the web, with a variety of benefits and limitations.
webpack builds on lessons learned from these systems and applies the concept of _modules_ to any file in your project.

Node.js は当初からモジュール JavaScript をサポートしてきました。
しかしながら web 上では _module_ のサポートは遅れていました。
web 上でモジュールプログラミングをサポートする複数のツールは存在し、さまざまなメリットと制限があります。
webpack ではこれらのシステムから学んだ教訓を素に構築し、プロジェクトのファイルに _modules_ の概念を適用しています。

## What is a webpack Module

In contrast to [Node.js modules](https://nodejs.org/api/modules.html), webpack _modules_ can express their _dependencies_ in a variety of ways. A few examples are:

[Node.js modules](https://nodejs.org/api/modules.html) と対象的に、 webpack _modules_ はさまざまは方法でそれらの _dependencies_ を記述することができます。いくつかの例があります。

* An [ES2015 `import`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import) statement
* A [CommonJS](http://www.commonjs.org/specs/modules/1.0/) `require()` statement
* An [AMD](https://github.com/amdjs/amdjs-api/blob/master/AMD.md) `define` and `require` statement
* An [`@import` statement](https://developer.mozilla.org/en-US/docs/Web/CSS/@import) inside of a css/sass/less file.
* An image url in a stylesheet (`url(...)`) or html (`<img src=...>`) file.

* [ES2015 `import`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import) ステートメント
* [CommonJS](http://www.commonjs.org/specs/modules/1.0/) の `require()` ステートメント
* [AMD](https://github.com/amdjs/amdjs-api/blob/master/AMD.md) の `define` と `require` ステートメント
* css/sass/less ファイル内部の [`@import` statement](https://developer.mozilla.org/en-US/docs/Web/CSS/@import)
* スタイルシート内 (`url(...)`) または html ファイル内の (`<img src=...>`) イメージ url

T> webpack 1 requires a specific loader to convert ES2015 `import`, however this is possible out of the box via webpack 2

T> webpack1 は ES2015 の `import` を変換するために特定のローダーを必要としてすが、これは webpack 2 経由で可能です。

## Supported Module Types

webpack supports modules written in a variety of languages and preprocessors, via _loaders_. _Loaders_ describe to webpack **how** to process non-JavaScript _modules_ and include these _dependencies_ into your _bundles_.
The webpack community has built _loaders_ for a wide variety of popular languages and language processors, including:

webpack はさまざまな言語やプリプロセッサで書かれたモジュールを  _loader_ 経由でサポートします。 _Loaders_ webpack に **どのように** JavaScriptではない _modules_　を処理しこれらの _dependecies_ を _bundles_ に含めるかを記述します。
webpack コミュニティは以下を含む幅広い種類の一般的な言語や言語プロセッサ用の _loaders_ を構築しました。

* [CoffeeScript](http://coffeescript.org)
* [TypeScript](https://www.typescriptlang.org)
* [ESNext (Babel)](https://babeljs.io)
* [Sass](http://sass-lang.com)
* [Less](http://lesscss.org)
* [Stylus](http://stylus-lang.com)

And many others! Overall, webpack provides a powerful and rich API for customization that allows one to use webpack for **any stack**, while staying **non-opinionated** about your development, testing, and production workflows.

他にもたくさん！全体的に webpack は開発、テストそして制作のワークフローについて **non-opinionated** を維持しながら **任意のスタック** に対して webpack に使用できるカスタム用の強力でリッチな API を提供します。

For a full list, see [**the list of loaders**](/loaders) or [**write your own**](/api/loaders).

すべてのリストは [**the list of loaders**](/loaders) または [**write your own**](/api/loaders) を参照してください。
