# Expressions \(式\)

最終更新日:

Swift では、前置式、バイナリ式、基本式、後置式の 4 種類の式があります。式が評価されると、値を返すか、副作用を起こすか、またはその両方を引き起こします。

前置式とバイナリ式を使用すると、演算子をより小さな式に適用できます。基本式は概念的には最も簡単な種類の式で、値にアクセスする方法を提供します。後置式は、前置式やバイナリ式と同様に、関数呼び出しやメンバアクセスなどの後置式を使用してより複雑な式を構築できます。各式は、下記のセクションで詳しく説明されています。

> GRAMMAR OF AN EXPRESSION  
> expression → [try-operator](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_try-operator)<sub>*opt*</sub> [prefix-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_prefix-expression)  [binary-expressions](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_binary-expressions)<sub>*opt*</sub>   
> expression-list → [expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_expression) \|  [expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_expression)  `,` [expression-list](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_expression-list)

## Prefix Expressions(前置式)

前置式は、オプショナルの前置演算子を式と組み合わせます。前置演算子は 1 つの引数を受け取り、その後に式が続きます。

これらの演算子の動作については、[Basic Operators](./../language-guide/basic-operators.md)と[Advanced Operators](./../language-guide/advanced-operators.md)を参照ください。

Swift 標準ライブラリによって提供されている演算子については、[Operator Declarations](https://developer.apple.com/documentation/swift/swift_standard_library/operator_declarations)を参照ください。

> GRAMMAR OF A PREFIX EXPRESSION  
> prefix-expression → [prefix-operator](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_prefix-operator)<sub>*opt*</sub> [postfix-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_postfix-expression)  
> prefix-expression → [in-out-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_in-out-expression)

### In-Out Expression(In-Out式)

in-out 式は、関数呼び出し式に in-out 引数として渡される変数をマークします。

![In-Out式](./../.gitbook/assets/inout_expression.png)

In-Out 引数の詳細については、[In-Out Parameters](./../language-guide/functions.md#in-Out-ParametersIn-Out引数)を参照ください。

In-Out 式は、[Implicit Conversion to a Pointer Type](#implicit-conversion-to-a-pointer-typeポインタ型への暗黙変換)で説明されているように、ポインタが必要なコンテキスト内の非ポインタ引数を指定するときにも使用されます。

> GRAMMAR OF AN IN-OUT EXPRESSION  
> in-out-expression → `&` [identifier](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_identifier)

### Try Operator(Try演算子)

Try 演算子は、`try` 演算子の後にエラーをスローできる式が続く形で構成されます。次の形式です:

![try式](./../.gitbook/assets/try_expression.png)

`try` 式の値は expression の値です。

オプショナル try 式は `try?` 演算子の後にエラーをスローできる式が続く形で構成されます。オペレータの後にエラーをスローできる式が続きます。次の形式です:

![try?式](./../.gitbook/assets/try?_expression.png)

式がエラーをスローしない場合、`try?` の値は式の値を含むオプショナルです。それ以外の場合は、`try?` の値は `nil` です。

強制 try 式は `try!` 演算子の後にエラーをスローできる式が続く形で構成されます。次の形式です:

![try!式](./../.gitbook/assets/try!_expression.png)

`try!` の値は experssion の値です。式がエラーをスローすると、実行時エラーが発生します。

バイナリ演算子の左側の式に `try`、`try?` または `try!`、がマークされている場合、その演算子はバイナリ式全体に適用されます。一方で、括弧(`()`)を使用して、演算子の適用範囲を明示することもできます。

```swift
//両方の関数呼び出しに適用されます
sum = try someThrowingFunction() + anotherThrowingFunction()

//両方の関数呼び出しに適用されます
sum = try (someThrowingFunction() + anotherThrowingFunction())

//エラー：最初の関数呼び出しにのみ適用されます
sum = (try someThrowingFunction()) + anotherThrowingFunction()
```

バイナリ演算子が代入演算子の場合、または `try` 式が括弧に囲まれていない限り、`try` 式はバイナリ演算子の右側には使用できません。

`try` と `await` 演算子の両方を含む場合は、最初に `try` が来なければなりません。

`try`、`try?` と `try!` の使用方法についての詳細は[Error Handling](./../language-guide/error-handling.md)を参照ください。

> GRAMMAR OF A TRY EXPRESSION  
> try-operator → `try` \|  `try` `?` \|  `try` `!`

### Await Operator(Await演算子)

await 式は、`await` 演算子の後に非同期関数の結果を使用する式が続いて構成されます。次の形式です:

![await演算子](./../.gitbook/assets/await_operator.png)

`await` 式の値は experssion の値です。

`await` でマークされた式を潜在的な中断ポイントと呼びます。非同期関数の実行は、`await` でマークされている箇所で中断することができます。さらに、同時並行コードの実行は他の場所で中断されません。つまり、仮に次の潜在的な中断ポイントの前に状態の更新を完了できるのならば一時的に状態の不変性を破壊してしまいますが、これが起きないため潜在的な中断ポイント間のコードで安全に状態を更新することができます。

`await` 式は、`async(priority:operation:)` 関数の引数に渡される末尾クロージャのように、非同期コンテキスト内でのみ使用することができます。`defer` 文の本文、または同期関数型の自動クロージャ(*autoclosure*) では使用できません。

バイナリ演算子の左側の式に `await` 演算子がマークされている場合、その演算子はバイナリ式全体に適用されます。ただし、括弧を使用して、演算子の適用範囲について明示することができます。

```swift
//両方の関数呼び出しに適用されます
sum = await someAsyncFunction() + anotherAsyncFunction()

//両方の関数呼び出しに適用されます
sum = await (someAsyncFunction() + anotherAsyncFunction())

//エラー：最初の関数呼び出しにのみ適用されます
sum = (await someAsyncFunction()) + anotherAsyncFunction()
```

バイナリ演算子が代入演算子の場合、または `await` 式が括弧内に囲まれていない限り、`await` 式はバイナリ演算子の右側に使用できません。

式が `await` と `try` 演算子の両方を含む場合、最初に `try` 演算子が来なければなりません。

> GRAMMAR OF AN AWAIT EXPRESSION  
> *await-operator* → **await**

## Binary Expressions(バイナリ式)

バイナリ式は、左右の引数を受け取る式と中置(*infix*)バイナリ演算子を組み合わせます。次の形式です:

![Binary Expressions](./../.gitbook/assets/binary_expression.png)

これらの演算子の動作については、[Basic Operators](./../language-guide/basic-operators.md) と [Advanced Operators](./../language-guide/advanced-operators.md)を参照ください。

標準ライブラリによって提供されている演算子については、[Operator Declarations](https://developer.apple.com/documentation/swift/operator_declarations)を参照ください。

> NOTE  
> 構文解析時には、式はフラットなリストとして表されたバイナリ演算子で構成されていました。 このリストは、演算子の優先順位を適用することによってツリーに変換されます。 例えば、式 `2 + 3 * 5` は、最初は5つの項目、`2`、`+`、`3`、`*`、および `5` として解釈され、その後 `(2 + (3 * 5))` のツリーに変換します  

> GRAMMAR OF A BINARY EXPRESSION  
> binary-expression → [binary-operator](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_binary-operator)  [prefix-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_prefix-expression)  
> binary-expression → [assignment-operator](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_assignment-operator)  [try-operator](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_try-operator)<sub>*opt*</sub> [prefix-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_prefix-expression)  
> binary-expression → [conditional-operator](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_conditional-operator)  [try-operator](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_try-operator)<sub>*opt*</sub> [prefix-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_prefix-expression)  
> binary-expression → [type-casting-operator](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_type-casting-operator)  
> binary-expressions → [binary-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_binary-expression)  [binary-expressions](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_binary-expressions)<sub>*opt*</sub>

### Assignment Operator(代入演算子)

### Ternary Conditional Operator(三項条件演算子)

### Type-Casting Operators(型キャスト演算子)

## Primary Expressions(基本式)

### Literal Expression(リテラル式)

### Self Expression(Self式)

### Superclass Expression(スーパークラス式)

### Closure Expression(クロージャ式)

#### Capture Lists(キャプチャリスト)

---

### Implicit Member Expression(暗黙メンバ式)

### Parenthesized Expression(括弧で囲まれた式)

### Tuple Expression(タプル式)

### Wildcard Expression(ワイルドカード式)

### Key-Path Expression(KeyPath式)

### Selector Expression(セレクタ式)

### Key-Path String Expression(KeyPath文字列式)

## Postfix Expressions(後置式)

### Function Call Expression(関数呼び出し式)

#### Implicit Conversion to a Pointer Type(ポインタ型への暗黙変換)

---

### Initializer Expression(イニシャライザ式)

### Explicit Member Expression(明示的メンバ式)

### Postfix Self Expression(後置Self式)

### Subscript Expression(subscript式)

### Forced-Value Expression(強制アンラップ式)

### Optional-Chaining Expression(オプショナルチェーン式)
