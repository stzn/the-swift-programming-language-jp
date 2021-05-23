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

[Nested Functions](./functions.md#nested-functions)で紹介したネスト関数は、より大きな関数の一部として独立したコードブロックに名前を付けて定義するための便利な手段です。ただし、完全な宣言や名前を書かずに、関数のような構造のより短いバージョンを作成できれば便利な場合もあります。これは、1 つ以上の引数を受け取る関数またはメソッドで特に当てはまります。

クロージャ式(*Closure Expressions*)は、簡潔で明瞭にインラインのクロージャを記述する方法です。クロージャ式は、明確さや意図を失うことなく、短縮された形式でクロージャを記述するためのいくつかの最適化を行うことができます。下記のクロージャ式は、複数回繰り返し使用される `sorted(by:)` メソッドを、同じ機能をより簡潔なものに改良した、最適化の例です。

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
