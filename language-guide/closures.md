# Closures

最終更新日:

クロージャ(*Closures*)は、コード内で受け渡して使用できるある機能の独立したブロックです。 Swift のクロージャは、C 言語や Objective-C のブロック、他のプログラミング言語のラムダに似ています。

クロージャは、定数と変数への参照を、それらを定義したコンテキストからキャプチャ(*capture*)して保存できます。 これは、これらの定数と変数をスコープに閉じ込める(*closing*する)と呼ばれます。 Swift は、キャプチャに関連した全てのメモリ管理を行います。

> NOTE  
> キャプチャの概念に慣れていなくても心配しないでください。 これについては、以下の[Capturing Values](#capturing-values)で詳しく説明します。

[Functions](./functions.md)で紹介したグローバル関数やネスト関数は、実際にはクロージャの特殊なケースです。 クロージャは、次の 3 つの形式のいづれかを取ります:

* グローバル関数は、名前があり、値をキャプチャしないクロージャです
* ネスト関数は、名前があり、囲んでいる関数から値を取得できるクロージャです
* クロージャ式は、周囲のコンテキストから値をキャプチャできる軽量の構文で書かれた名前のないクロージャです

Swift のクロージャは、一般的に、簡潔で、混乱のないシンタックスで書くことを促進すうように最適化された、すっきりとした明確なスタイルを備えています。最適化には次のものが含まれます。

* コンテキストから引数と戻り値の型を推測できる
* 単一の式のクロージャは `return` キーワードなしで暗黙のリターンできる
* 引数名の短縮ができる
* 末尾のクロージャ構文を使うことができる

## Closure Expressions

### The Sorted Method

### Closure Expression Syntax

### Inferring Type From Context

### Implicit Returns from Single-Expression Closures

### Shorthand Argument Names

### Operator Methods

## Trailing Closures

## Capturing Values

## Closures Are Reference Types

## Escaping Closures

## Autoclosures
