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

### In-Out Expression(inout式)

### Try Operator(try演算子)

### Await Operator(await演算子)

## Binary Expressions(バイナリ式)

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
