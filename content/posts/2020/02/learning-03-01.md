---
author:
  name: "az"
date: 2020-02-03T00:00:00+09:00
linktitle: "Flutter: 静的解析を行う"
type:
- post 
- posts
title: "Flutter: 静的解析を行う"
draft: false
tags: ["学び", "Dart", "Flutter"]
categories: ["Flutter"]
description: "
今日学んだこと - 2020/02/03
Flutter: 静的解析を行う
"
---

# Flutter/Dart

## 静的解析を行う

analysis_options.yaml をプロジェクト直下に配置して静的解析を行う

```yaml
include: package:pedantic_mono/analysis_options.yaml

analyzer:
  strong-mode:
    implicit-casts: false
    implicit-dynamic: false

linter:
  rules:
    - avoid_types_on_closure_parameters 
    - avoid_void_async
    - await_only_futures
    - camel_case_types
    - cancel_subscriptions
    - close_sinks
    - constant_identifier_names
    - control_flow_in_finally
    - empty_statements
    - hash_and_equals
    - implementation_imports
    - non_constant_identifier_names
    - package_api_docs
    - package_names
    - package_prefixed_library_names
    - test_types_in_equals
    - throw_in_finally
    - unnecessary_brace_in_string_interps
    - unnecessary_getters_setters
    - unnecessary_new
    - unnecessary_statements
```

### ルールの説明
- avoid_types_on_closure_parameters 
  - 関数式パラメーターの型に注釈を付けることは、通常、パラメーター型がコンテキストからほとんど常に推論され、練習が冗長になるため、不要です。
  - https://dart-lang.github.io/linter/lints/avoid_types_on_closure_parameters.html
- avoid_void_async
  - 値を返さない非同期メソッドまたは関数を宣言するときは、単にvoidではなくFutureを返すことを宣言します。
  - https://dart-lang.github.io/linter/lints/avoid_void_async.html
- await_only_futures
  - Futureで返すときは、Future.value(value)を用いる。
  - https://dart-lang.github.io/linter/lints/await_only_futures.html
- camel_case_types
  - クラスとtypedefは、各単語（最初の単語を含む）の最初の文字を大文字にし、区切り文字を使用しないでください。
  - https://dart-lang.github.io/linter/lints/camel_case_types.html
- cancel_subscriptions
  - StreamSubscriptionのインスタンスをキャンセルすると、メモリリークと予期しない動作が防止されます。
  - https://dart-lang.github.io/linter/lints/cancel_subscriptions.html
- close_sinks
  - Sinkのインスタンスを閉じると、メモリリークと予期しない動作が防止されます。
  - https://dart-lang.github.io/linter/lints/close_sinks.html
- constant_identifier_names
  - 新しいコードでは、列挙値を含む定数変数にlowerCamelCaseを使用します。 定数にALL_CAPS_WITH_UNDERSCORESを使用する既存のコードでは、一貫性を保つために引き続きすべての大文字を使用できます。
  - https://dart-lang.github.io/linter/lints/constant_identifier_names.html
- control_flow_in_finally
  - finallyブロックで制御フローを使用すると、デバッグが困難な予期しない動作が必ず発生します。
  - https://dart-lang.github.io/linter/lints/control_flow_in_finally.html
- empty_statements
  - 空の文はほとんど常にバグを示します。
  - https://dart-lang.github.io/linter/lints/empty_statements.html
- hash_and_equals
  - DartのすべてのオブジェクトにはhashCodeがあります。 ==演算子とオブジェクトのhashCodeプロパティの両方は、一般的なハッシュマップの実装が適切に機能するために一貫している必要があります。したがって、==をオーバーライドする場合、一貫性を維持するためにhashCodeもオーバーライドする必要があります。同様に、hashCodeがオーバーライドされる場合は、==もオーバーライドする必要があります。
  - https://dart-lang.github.io/linter/lints/hash_and_equals.html
- implementation_imports
  - lib内のライブラリは公開されています。他のパッケージは自由にインポートできます。しかし、パッケージのコードの多くは、パッケージ自体によってのみインポートおよび使用される内部実装ライブラリです。それらはsrcと呼ばれるlibのサブディレクトリの中に入ります。整理に役立つ場合は、そこにサブディレクトリを作成できます。 同じパッケージ内の他のDartコード内からlib / srcにあるライブラリ（lib内の他のライブラリ、bin内のスクリプト、テストなど）を自由にインポートできますが、別のパッケージのlib / srcディレクトリからインポートしないでください。これらのファイルはパッケージのパブリックAPIの一部ではなく、コードを壊す可能性のある方法で変更される可能性があります。
  - https://dart-lang.github.io/linter/lints/implementation_imports.html
- non_constant_identifier_names
  - クラスメンバ、トップレベルの定義、変数、パラメータ、名前付きパラメータ、および名前付きコンストラクタは、最初の単語を除く各単語の最初の文字を大文字にし、セパレータを使用しないでください。
  - https://dart-lang.github.io/linter/lints/non_constant_identifier_names.html
- package_api_docs
  - pubパッケージレイアウトドキュメントで説明されているように、パブリックAPIはパッケージのlibフォルダー内のすべてから構成され、lib / srcの実装ファイルを除いて、エクスポートディレクティブで明示的にエクスポートされた要素を追加します。
  - https://dart-lang.github.io/linter/lints/package_api_docs.html
- package_names
  - パッケージ名はすべて小文字にする必要があり、単語を区切る下線はjust_like_thisです。基本的なラテン文字とアラビア数字のみを使用してください：[a-z0-9_]。また、名前が有効なDart識別子であることを確認してください-それは数字で始まっておらず、予約語ではありません。
  - https://dart-lang.github.io/linter/lints/package_names.html
- package_prefixed_library_names
  - このガイドラインは、2つのライブラリの名前が同じ場合に表示される警告を回避するのに役立ちます。推奨するルールは次のとおりです。
    - すべてのライブラリ名の前にパッケージ名を付けます。
    - エントリライブラリにパッケージと同じ名前を付けます。
    - パッケージ内の他のすべてのライブラリの場合、パッケージ名の後に、ドット区切りのパスをライブラリのDartファイルに追加します。
    - libの下のライブラリの場合、最上位のディレクトリ名を省略します。
  - https://dart-lang.github.io/linter/lints/package_prefixed_library_names.html
- test_types_in_equals
  - 型をテストしないと、クラスのコンシューマーにとって予期しないnullポインター例外が発生する場合があります。
  - https://dart-lang.github.io/linter/lints/test_types_in_equals.html
- throw_in_finally
  - inallyブロックで例外をスローすると、デバッグが困難な予期しない動作が必ず発生します。
  - https://dart-lang.github.io/linter/lints/throw_in_finally.html
- unnecessary_brace_in_string_interps
  - 単純な識別子を補間するだけで、直後にさらに多くの英数字テキストが続かない場合は、{}を省略できます。
  - https://dart-lang.github.io/linter/lints/unnecessary_brace_in_string_interps.html
- unnecessary_getters_setters
  - JavaとC＃では、実装がフィールドに転送するだけであっても、すべてのフィールドをゲッターとセッター（またはC＃のプロパティ）の後ろに隠すのが一般的です。そうすれば、これらのメンバーでさらに作業を行う必要が生じた場合、コールサイトに触れる必要なく実行できます。これは、ゲッターメソッドの呼び出しはJavaのフィールドへのアクセスとは異なり、プロパティへのアクセスはC＃のrawフィールドへのアクセスとバイナリ互換ではないためです。 Dartにはこの制限はありません。フィールドとゲッター/セッターは完全に区別できません。クラスのフィールドを公開し、後でそのフィールドを使用するコードに触れることなくゲッターとセッターでラップすることができます。
  - https://dart-lang.github.io/linter/lints/unnecessary_getters_setters.html
- unnecessary_new
  - インスタンスを作成するための新しいキーワード。
  - https://dart-lang.github.io/linter/lints/unnecessary_new.html
- unnecessary_statements
  - 明確な効果のないステートメントは通常不要であるか、分割する必要があります。
  - https://dart-lang.github.io/linter/lints/unnecessary_statements.html


### 参考
https://medium.com/flutter-jp/analysis-b8dbb19d3978