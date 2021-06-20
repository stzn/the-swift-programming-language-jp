# Expressions \(式\)

最終更新日: 2021/6/12

Swift では、前置式、バイナリ式、基本式、後置式の 4 種類の式があります。式が評価されると、値を返すか、副作用を起こすか、またはその両方を引き起こします。

前置式とバイナリ式を使用すると、演算子をより小さな式に適用できます。基本式は概念的には最もシンプルな種類の式で、値にアクセスする方法を提供します。後置式は、前置式やバイナリ式と同様に、関数呼び出しやメンバアクセスなど、後置式を使用してより複雑な式を構築できます。各式は、下記のセクションで詳しく説明されています。

> GRAMMAR OF AN EXPRESSION  
> expression → [try-operator](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_try-operator)<sub>*opt*</sub> [prefix-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_prefix-expression)  [binary-expressions](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_binary-expressions)<sub>*opt*</sub>  
> expression-list → [expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_expression) \|  [expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_expression)  `,` [expression-list](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_expression-list)

## Prefix Expressions(前置式)

前置式は、式と任意の前置演算子を組み合わせます。前置演算子は 1 つの引数を受け取り、その後に式が続きます。

これらの演算子の動作については、[Basic Operators](./../language-guide/basic-operators.md)と[Advanced Operators](./../language-guide/advanced-operators.md)を参照ください。

Swift 標準ライブラリによって提供されている演算子については、[Operator Declarations](https://developer.apple.com/documentation/swift/swift_standard_library/operator_declarations)を参照ください。

> GRAMMAR OF A PREFIX EXPRESSION  
> prefix-expression → [prefix-operator](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_prefix-operator)<sub>*opt*</sub> [postfix-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_postfix-expression)  
> prefix-expression → [in-out-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_in-out-expression)

### In-Out Expression(In-Out式)

in-out 式は、関数呼び出し式に in-out 引数として渡された変数にマークをします。

![In-Out式](./../.gitbook/assets/inout_expression.png)

In-Out 引数の詳細については、[In-Out Parameters](./../language-guide/functions.md#in-Out-ParametersIn-Out引数)を参照ください。

In-Out 式は、[Implicit Conversion to a Pointer Type](#implicit-conversion-to-a-pointer-typeポインタ型への暗黙変換)で説明されているように、ポインタが必要なコンテキストに非ポインタ引数を指定するときにも使用されます。

> GRAMMAR OF AN IN-OUT EXPRESSION  
> in-out-expression → `&` [identifier](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_identifier)

### Try Operator(Try演算子)

Try 演算子は、`try` 演算子の後にエラーをスローできる式が続く形で構成されます。形式は次のとおりです:

![try演算子](./../.gitbook/assets/try_operator.png)

`try` 式の値は expression の値です。

オプショナル try 式は `try?` 演算子の後にエラーをスローできる式が続く形で構成されます。形式は次のとおりです:

![try?演算子](./../.gitbook/assets/try?_operator.png)

式がエラーをスローしない場合、`try?` の値は式の値を含むオプショナルです。それ以外の場合、`try?` の値は `nil` です。

強制 try 式は `try!` 演算子の後にエラーをスローできる式が続く形で構成されます。形式は次のとおりです:

![try!演算子](./../.gitbook/assets/try!_operator.png)

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

バイナリ演算子が代入演算子の場合、または `try` 式が括弧で囲まれていない限り、`try` 式はバイナリ演算子の右側には使用できません。

`try` と `await` 演算子の両方を含む場合は、最初に `try` が来なければなりません。

`try`、`try?` と `try!` の使用方法についての詳細は[Error Handling](./../language-guide/error-handling.md)を参照ください。

> GRAMMAR OF A TRY EXPRESSION  
> try-operator → `try` \|  `try` `?` \|  `try` `!`

### Await Operator(Await演算子)

await 式は、`await` 演算子の後に非同期関数の結果を返す式が続けて構成されます。形式は次のとおりです:

![await演算子](./../.gitbook/assets/await_operator.png)

`await` 式の値は experssion の値です。

`await` でマークされた式を潜在的な中断ポイントと呼びます。非同期関数の実行は、`await` でマークされている箇所で中断することができます。さらに、同時並行コードは他の点で中断して実行されることはありません。つまり、もし次の潜在的な中断ポイントに行く前に状態の更新が完了した場合、一時的に不変性を破壊する可能性がある状態を、潜在的な中断ポイント間で安全に更新することができます。

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

バイナリ式は、左右の引数を受け取る式と中置(*infix*)バイナリ演算子を組み合わせます。形式は次のとおりです:

![Binary Expressions](./../.gitbook/assets/binary_expression.png)

これらの演算子の動作については、[Basic Operators](./../language-guide/basic-operators.md) と [Advanced Operators](./../language-guide/advanced-operators.md)を参照ください。

標準ライブラリによって提供されている演算子については、[Operator Declarations](https://developer.apple.com/documentation/swift/operator_declarations)を参照ください。

> NOTE  
> 構文解析時には、式はバイナリ演算子のフラットなリストを構成します。このリストは、演算子の優先順位を適用することによってツリーに変換されます。例えば、式 `2 + 3 * 5` は、最初は5つの項目、`2`、`+`、`3`、`*`、および `5` として解釈され、その後 `(2 + (3 * 5))` のツリーに変換します  

> GRAMMAR OF A BINARY EXPRESSION  
> binary-expression → [binary-operator](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_binary-operator)  [prefix-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_prefix-expression)  
> binary-expression → [assignment-operator](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_assignment-operator)  [try-operator](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_try-operator)<sub>*opt*</sub> [prefix-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_prefix-expression)  
> binary-expression → [conditional-operator](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_conditional-operator)  [try-operator](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_try-operator)<sub>*opt*</sub> [prefix-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_prefix-expression)  
> binary-expression → [type-casting-operator](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_type-casting-operator)  
> binary-expressions → [binary-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_binary-expression)  [binary-expressions](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_binary-expressions)<sub>*opt*</sub>

### Assignment Operator(代入演算子)

代入演算子は特定の式に新しい値を設定します。形式は次のとおりです:

![Assignment Operator](./../.gitbook/assets/assignment_operator.png)

value を評価した結果得られた値が expression に設定されます。式がタプルの場合、値は同じ数の要素を持つタプルでなければなりません。(タプルはネストすることもできます)。代入は、値の各部分から expression の中の対応する部分に対して行われます。例えば:

```swift
(a, _, (b, c)) = ("test", 9.45, (12, 3))
// a は"test"、 b は 12、 c は 3、 9.45 は無視されます
```

代入演算子は任意の値を返しません。

> GRAMMAR OF AN asSIGNMENT OPERATOR  
> assignment-operator → `=`

### Ternary Conditional Operator(三項条件演算子)

三項条件演算子は、条件の値に基づいて、2 つの値のうちの 1 つに評価されます。形式は次のとおりです:

![Ternary Conditional Operator](./../.gitbook/assets/ternary_conditional_operator.png)

条件が `true` と評価された場合、条件演算子は最初の式を評価し、その値を返します。それ以外の場合は、2 番目の式を評価してその値を返します。未使用の式は評価されません。

三項条件演算子を使用する例については、[Ternary Conditional Operator](./../language-guide/basic-operators.md#ternary-conditional-operator三項条件演算子)を参照ください。

> GRAMMAR OF A CONDITIONAL OPERATOR  
> conditional-operator → `?` [expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_expression)  `:`

### Type-Casting Operators(型キャスト演算子)

4 つの型のキャスト演算子があります: `is` 演算子、`as` 演算子、`as?` 演算子、そして `as!` 演算子。

それらは次の形式を持っています:

![Type-Casting Operators](./../.gitbook/assets/type_casting_operator.png)

`is` 演算子は実行時に式が指定された型にキャストできるかどうかを確認します。キャストできる場合は `true` を返します。それ以外の場合は、`false` を返します。

`as` 演算子は、コンパイル時にキャストが常に成功するとわかっている場合にキャストを実行します。アップキャストは、中間変数を使用せずに型のスーパー型のインスタンスとして式を使用できます。下記のアプローチはどれも同等です。

```swift
func f(_ any: Any) { print("Function for Any") }
func f(_ int: Int) { print("Function for Int") }
let x = 10
f(x)
// "Function for Int"

let y: Any = x
f(y)
// "Function for Any"

f(x as Any)
// "Function for Any"
```

ブリッジングを使用して、新しいインスタンスを作成せずに、`String` などの Swift 標準ライブラリ型の式を、それに相応する `NSString` などの Foudation 型で使用できるようにしています。ブリッジングの詳細については、[Working with Foundation Types](https://developer.apple.com/documentation/swift/imported_c_and_objective_c_apis/working_with_foundation_types)を参照ください。

`as?` 演算子は、指定型へ条件付きキャストを実行します。`as?` 演算子は指定された型のオプショナルを返します。実行時に、キャストが成功した場合、式の値がオプショナルで返されます。それ以外の場合、返される値は `nil` です。指定された型へのキャストが失敗するか、成功することが明らかな場合は、コンパイルエラーが発生します。

`as!` 演算子は、指定された型に強制キャストを実行します。`as!` 演算子は、オプショナル型ではなく、指定された型の値を返します。キャストが失敗した場合は、実行時エラーが発生します。`x as! T` は `(x as? T)!` の挙動と同じです。

型キャストの詳細や型キャスト演算子を使用する例については、[Type Casting](./../language-guide/type-casting.md)を参照ください。

> GRAMMAR OF A TYPE-CASTING OPERATOR  
> type-casting-operator → `is` [type](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_type)  
> type-casting-operator → `as` [type](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_type)  
> type-casting-operator → `as` `?` [type](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_type)  
> type-casting-operator → `as` `!` [type](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_type)

## Primary Expressions(基本式)

基本式は最も基本的な種類の式です。それらは自身を式として使用したり、他のトークンと組み合わせたり、前置式、バイナリ式、および後置式を作成することができます。

> GRAMMAR OF A PRIMARY EXPRESSION  
> primary-expression → [identifier](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_identifier)  [generic-argument-clause](https://docs.swift.org/swift-book/ReferenceManual/GenericParametersAndArguments.html#grammar_generic-argument-clause)<sub>*opt*</sub>  
> primary-expression → [literal-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_literal-expression)  
> primary-expression → [self-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_self-expression)  
> primary-expression → [superclass-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_superclass-expression)  
> primary-expression → [closure-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_closure-expression)  
> primary-expression → [parenthesized-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_parenthesized-expression)  
> primary-expression → [tuple-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_tuple-expression)  
> primary-expression → [implicit-member-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_implicit-member-expression)  
> primary-expression → [wildcard-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_wildcard-expression)  
> primary-expression → [key-path-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_key-path-expression)  
> primary-expression → [selector-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_selector-expression)  
> primary-expression → [key-path-string-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_key-path-string-expression)

### Literal Expression(リテラル式)

リテラル式は、通常のリテラル(文字列や数など)、配列または辞書リテラル、playground リテラル、または下記の特別なリテラルのいずれかで構成されます。

| Literal | Type | Value |
| :---: | :---: | :---: |
| `#file` | `String` | 使用されているファイルへのパス |
| `#fileID` | `String` | 使用されているファイルとモジュールの名前 |
| `#filePath` | `String` | 使用されているファイルへのパス |
| `#line` | `Int` | 使用されている行番号 |
| `#column` | `Int` | 開始列番号 |
| `#function` | `String` | 使用されている宣言の名前 |
| `#dsohandle` | `UnsafeRawPointer` | 使用中の動的共有オブジェクト(*dynamic shared object*(DSO))ハンドル |

`#file` の文字列値は、古い `#filePath` から新しい `#fileID` への移行を有効にするために、言語のバージョンによって異なります。現在、`#file` は `#filePath` と同じ値を持ちます。将来の Swift のバージョンでは、`#file` は代わりに `#fileID` と同じ値を持ちます。将来のバージョンの挙動を適用するには、`#file` を `#fileID` または `#filePath` に置き換える必要があります。

`#fileID` 式の文字列値はモジュール/ファイル形式です。ここで言う、「ファイル」は式が使用されているファイルの名前で、「モジュール」は、がこのファイルが属しているモジュールの名前です。`#filePath` 式の文字列値は、式が使用されているファイルへのフルパスです。[Line Control Statement](./statements.md#line-control-statement行制御文)で説明されているように、これらの値はどちらも `#sourceLocation` に変わる可能性があります。`#fileID` は `#filePath` とは異なり、ソースファイルへのフルパスをソースファイルに埋め込むことはできないため、より良いプライバシーを提供し、コンパイルされたバイナリのサイズを減させることができます。テスト、ビルドスクリプト、また配布されるプログラムの一部にはならないその他のコードの外側で `#filePath` を使用しないでください。

> NOTE  
> `#fileID` 式は、最初のスラッシュ(`/`)の前のテキストをモジュール名、最後のスラッシュ(`/`)の後のテキストをファイル名と読んでください。将来的には、`MyModule/some/disambigation/myfile.swift` などのように、複数のスラッシュが含まれている可能性があります。

`#function` の値は、関数内ではその関数の名前です。メソッド内では、そのメソッドの名前、プロパティ get または set 内ではプロパティ名、`init` や `subscript` のような特別なメンバ内では、そのキーワード名、およびファイルのトップレベルでは、現在のモジュール名です。

関数またはメソッドの引数のデフォルト値として使用すると、呼び出し側でデフォルト値の式が評価され、特別なリテラル値が決定します。

```swift
func logFunctionName(string: String = #function) {
   print(string)
}
func myFunction() {
   logFunctionName() // "myFunction()".
}
```

配列リテラルは、順序付けられた値の集合です。形式は次のとおりです:

![配列リテラル](./../.gitbook/assets/array_expression.png)

配列内の最後の式の後にカンマ(`,`)を続けることもできます。配列リテラルの値は `[T]` 型で、`T` はその内部の式の型です。複数の型の式がある場合、`T` はそれらに最も近い共通のスーパー型になります。空の配列リテラルは、空の角括弧(`[]`)を使用し、指定された型の空の配列を作成するためにも使用できます。

```swift
var emptyArray: [Double] = []
```

辞書リテラルは、順序のないキーバリューペアのコレクションです。形式は次のとおりです:

![辞書リテラル](./../.gitbook/assets/dictionary_expression.png)

辞書内の最後の式の後にカンマ(`,`)を続けることができます。辞書リテラルの値は `[Key：Value]` 型で、`Key` はそのキー式の型、`Value` はその値の式の型です。複数の型の式がある場合、キーとバリューはそれぞれの値に最も近い共通のスーパー型になります。空の辞書リテラルは、空の配列リテラルと区別するために、一対の括弧内にコロンを書きます(`[:]`)。空の辞書リテラルを使用して、指定されたキーとバリュー型の空の辞書リテラルを作成できます。

```swift
var emptyDictionary: [String: Double] = [:]
```

`playground` リテラルは、プログラムエディタ内の色、ファイル、または画像の対話型な表現を作成するために Xcode によって使用されます。Xcode の外側のプレーンテキストの `playground` リテラルには、特別なリテラル構文を使用します。

Xcode の playground リテラルの使用方法については、Xcode ヘルプ内の[Add a color, file, or image literal](https://help.apple.com/xcode/mac/current/#/dev4c60242fc)を参照ください。

> GRAMMAR OF A LITERAL EXPRESSION  
> literal-expression → [literal](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_literal)  
> literal-expression → [array-literal](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_array-literal) \|  [dictionary-literal](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_dictionary-literal) \|  [playground-literal](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_playground-literal)  
> literal-expression → `#file` \|  `#fileID` \|  `#filePath`  
> literal-expression → `#line` \|  `#column` \|  `#function` \|  `#dsohandle`  
> array-literal → `[` [array-literal-items](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_array-literal-items)<sub>*opt*</sub> `]`  
> array-literal-items → [array-literal-item](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_array-literal-item)  `,`<sub>*opt*</sub> \|  [array-literal-item](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_array-literal-item)  `,` [array-literal-items](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_array-literal-items)  
> array-literal-item → [expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_expression)  
> dictionary-literal → `[` [dictionary-literal-items](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_dictionary-literal-items)  `]` \|  `[` `:` `]`  
> dictionary-literal-items → [dictionary-literal-item](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_dictionary-literal-item)  `,`<sub>*opt*</sub> \|  [dictionary-literal-item](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_dictionary-literal-item)  `,` [dictionary-literal-items](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_dictionary-literal-items)  
> dictionary-literal-item → [expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_expression)  `:` [expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_expression)  
> playground-literal → `#colorLiteral` `(` `red` `:` [expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_expression)  `,` `green` `:` [expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_expression)  `,` `blue` `:`[expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_expression) `,` `alpha` `:` [expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_expression)  `)`  
> playground-literal → `#fileLiteral` `(` `resourceName` `:` [expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_expression)  `)`  
> playground-literal → `#imageLiteral` `(` `resourceName` `:` [expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_expression)  `)`

### Self Expression(self式)

`self` 式は、それが使用さえている現在の型またはインスタンスへの明示的な参照です。形式は次のとおりです:

![Self式](./../.gitbook/assets/self_expression.png)

イニシャライザ、subscript、またはインスタンスメソッドでは、`self` は、それが出現する現在の型のインスタンスを表します。型メソッドでは、`self` はそれが出現する現在の型を表します。

`self` 式は、関数引数などスコープ内に同じ名前の別の変数があり、何を指すのかが曖昧な場合に、メンバへアクセスするときに指定します。例えば:

```swift
class SomeClass {
    var greeting: String
    init(greeting: String) {
        self.greeting = greeting
    }
}
```

値型の mutating メソッドでは、その値型の新しいインスタンスを `self` に代入できます。例えば:

```swift
struct Point {
    var x = 0.0, y = 0.0
    mutating func moveBy(x deltaX: Double, y deltaY: Double) {
        self = Point(x: x + deltaX, y: y + deltaY)
    }
}
```

> GRAMMAR OF A SELF EXPRESSION  
> self-expression → `self` \|  [self-method-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_self-method-expression) \|  [self-subscript-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_self-subscript-expression) \|  [self-initializer-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_self-initializer-expression)  
> self-method-expression → `self` `.` [identifier](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_identifier)  
> self-subscript-expression → `self` `[` [function-call-argument-list](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_function-call-argument-list)  `]`  
> self-initializer-expression → `self` `.` `init`

### Superclass Expression(スーパークラス式)

スーパークラス式は、クラスがスーパークラスとやり取りすることを可能にします。次のいずれかの形式があります:

![スーパークラス式](./../.gitbook/assets/superclass_expression.png)

最初の形式はスーパークラスのメンバにアクセスするために使用されます。2 番目の形式は、スーパークラスの subscript の実装にアクセスするために使用されます。3 番目の形式は、スーパークラスのイニシャライザにアクセスするために使用されます。

サブクラスは、スーパークラスの実装を利用するために、メンバ、subscript、およびイニシャライザの実装でスーパークラス式を使用できます。

> GRAMMAR OF A SUPERCLASS EXPRESSION  
> superclass-expression → [superclass-method-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_superclass-method-expression) \|  [superclass-subscript-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_superclass-subscript-expression) \|  [superclass-initializer-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_superclass-initializer-expression)  
> superclass-method-expression → `super` `.` [identifier](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_identifier)  
> superclass-subscript-expression → `super` `[` [function-call-argument-list](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_function-call-argument-list)  `]`  
> superclass-initializer-expression → `super` `.` `init`

### Closure Expression(クロージャ式)

クロージャ式は、他のプログラミング言語では、ラムダまたは匿名関数とも呼ばれているクロージャを作成します。関数宣言のように、クロージャには文が含まれており、その囲まれている範囲から定数と変数をキャプチャします。形式は次のとおりです:

![クロージャ式](./../.gitbook/assets/closure_expression.png)

[Function Declaration](./declarations.md#function-declaration関数宣言)で説明されているように、引数は関数宣言内の引数と同じ形式です。

クロージャをより簡潔に書くことができるいくつかの特別な形式があります:

* クロージャは、その引数、戻り値の型、またはその両方の型を省略できます。引数名と型の両方を省略する場合は、文の前の `in` キーワードを省略してください。省略された型を推論できない場合は、コンパイルエラーが発生します
* クロージャはその引数名を省略することができます。その際は暗黙的に `$0`、`$1`、 `$2` などのように `$` の後ろに引数の位置を続けた名前が与えられます
* 単一の式からなるクロージャは、その式の値を返すことが明らかです。この式の内容は、囲まれている式の型を推論するときにも使用されます

次のクロージャ式は同等です:

```swift
myFunction { (x: Int, y: Int) -> Int in
    return x + y
}

myFunction { x, y in
    return x + y
}

myFunction { return $0 + $1 }

myFunction { $0 + $1 }
```

関数の引数としてクロージャを渡す方法については、[Function Call Expression](#function-call-expression関数呼び出し式)を参照ください。

クロージャ式は、関数呼び出しの一部としてすぐにクロージャを使用するときなど、可変または定数に格納されることなく使用できます。上記のコードの `myFunction` に渡されたクロージャ式は、即時に使用される例です。その結果、クロージャ式がエスケープか非エスケープかは、式の周囲のコンテキストによって異なります。クロージャ式は、即時に呼ばれるか、または非エスケープ関数の引数として渡されると、非エスケープです。それ以外の場合、クロージャ式はエスケープです。

クロージャのエスケープの詳細については、[Escaping Closures](./../language-guide/closures.md#escaping-closuresエスケープクロージャ)を参照ください。

#### Capture Lists(キャプチャリスト)

---

デフォルトでは、クロージャ式は、、周囲のスコープの定数と変数を強い参照を持ってキャプチャします。キャプチャリストを使用して、クロージャ内で値をキャプチャする方法を明示的に制御できます。

キャプチャリストは、引数のリストの前に、角括弧(`[]`)で囲まれた式のカンマ(`,`)区切りのリストとして書かれています。キャプチャリストを使用する場合は、引数名、引数型、および戻り値の型を省略しても、`in` キーワードを使用する必要があります。

キャプチャリストへの各エントリは、クロージャが作成されたときに初期化されます。キャプチャリスト内の各エントリに対して、定数は周囲のスコープの同じ名前を持つ定数または変数の値で初期化できます。例えば、下記のコードでは、`a` はキャプチャリストに含まれていますが、`b` は含まれていません。

```swift
var a = 0
var b = 0
let closure = { [a] in
    print(a, b)
}

a = 10
b = 10
closure()

// "0 10"
```

クロージャの範囲内の定数と周囲の範囲内の変数に 2 つ `a` という同じ名前の異なるものがありますが、`b` という名前の変数は 1 つだけです。内部スコープ内の `a` は、クロージャが作成されたときに外側の `a` 値で初期化されますが、それらの値は繋がっていません。つまり、これは、外側の範囲内の `a` の値の変化が内側の範囲内の `a` の値に影響を与えないことも、クロージャの内側の値の変化が外側の `a` 影響を与えるません。対照的に、`b` という名前の変数は外側の範囲内に 1 つしかなく、クロージャの内側または外側からの変化は両方に影響を与えます。

キャプチャされた変数の型に参照セマンティクスがある場合、この区別はありません。例えば、下のコードに `x` という 2 つのものがありますが、外部スコープの変数と内部スコープの定数は、両方とも参照セマンティクスのために同じオブジェクトを参照します。

```swift
class SimpleClass {
    var value: Int = 0
}
var x = SimpleClass()
var y = SimpleClass()
let closure = { [x] in
    print(x.value, y.value)
}

x.value = 10
y.value = 10
closure()
// "10 10"
```

式の値の型がクラスの場合は、式の値へ弱参照または非所有参照で取り込むために、キャプチャリスト内の式に `weak` または `unowned` をマークすることができます。

```swift
myFunction { print(self.title) }                    // 暗黙的な強参照
myFunction { [self] in print(self.title) }          // 明示的な強参照
myFunction { [weak self] in print(self!.title) }    // 弱参照
myFunction { [unowned self] in print(self.title) }  // 非所有参照
```

任意の式をキャプチャリスト内の名前付きの値にバインドすることもできます。クロージャが作成されたときに式が評価され、値は指定された強度でキャプチャされます。例えば:

```swift
// parent として self.parent を弱参照する
myFunction { [weak parent = self.parent] in print(parent!.title) }
```

クロージャ式の詳細と例については、[Closure Expressions](./../language-guide/closures.md#closure-expressionsクロージャ式)を参照ください。キャプチャリストの詳細および例については、[Resolving Strong Reference Cycles for Closures](./../language-guide/automatic-reference-counting.md#resolving-strong-reference-cycles-for-closuresクロージャの強参照循環の解消)を参照ください。

> GRAMMAR OF A CLOSURE EXPRESSION  
> closure-expression → `{` [closure-signature](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_closure-signature)<sub>*opt*</sub> [statements](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar_statements)<sub>*opt*</sub> `}`  
> closure-signature → [capture-list](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_capture-list) opt [closure-parameter-clause](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_closure-parameter-clause)  `throws`<sub>*opt*</sub> [function-result](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_function-result)<sub>*opt*</sub> `in`  
> closure-signature → [capture-list](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_capture-list)  `in`  
> closure-parameter-clause → `(` `)` \|  `(` [closure-parameter-list](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_closure-parameter-list)  `)` \|  [identifier-list](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_identifier-list)  
> closure-parameter-list → [closure-parameter](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_closure-parameter) \|  [closure-parameter](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_closure-parameter)  `,` [closure-parameter-list](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_closure-parameter-list)  
> closure-parameter → [closure-parameter-name](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_closure-parameter-name)  [type-annotation](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_type-annotation)<sub>*opt*</sub>  
> closure-parameter → [closure-parameter-name](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_closure-parameter-name)  [type-annotation](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_type-annotation)  `...`  
> closure-parameter-name → [identifier](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_identifier)  
> capture-list → `[` [capture-list-items](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_capture-list-items)  `]`  
> capture-list-items → [capture-list-item](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_capture-list-item) \|  [capture-list-item](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_capture-list-item)  `,` [capture-list-items](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_capture-list-items)  
> capture-list-item → [capture-specifier](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_capture-specifier)<sub>*opt*</sub> [identifier](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_identifier)  
> capture-list-item → [capture-specifier](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_capture-specifier)<sub>*opt*</sub> [identifier](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_identifier)  `=` [expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_expression)  
> capture-list-item → [capture-specifier](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_capture-specifier)<sub>*opt*</sub> [self-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_self-expression)  
> capture-specifier → `weak` \|  `unowned` \|  `unowned(safe)` \|  `unowned(unsafe)`

### Implicit Member Expression(暗黙メンバ式)

暗黙メンバ式は、型推論によって暗黙的に型を決定できるコンテキストにおいて、列挙ケースや型メソッドなどの型のメンバにアクセスするための省略記法です。形式は次のとおりです:

![暗黙メンバ式](./../.gitbook/assets/implicit_member_expression.png)

例えば:

```swift
var x = MyEnumeration.someValue
x = .anotherValue
```

推論された型がオプショナルの場合は、暗黙メンバ式にオプショナルでない型のメンバを使用することもできます。

```swift
var someOptional: MyEnumeration? = .someValue
```

暗黙メンバ式の後に[Postfix Expressions](#postfix-expressions後置式)でリストされている後置演算子またはその他の後置構文を続けることができます。これは暗黙メンバ式チェーン(*chained implicit member expression*)と呼ばれます。同じ型を持つことは全ての後置式チェーンで一般的ですが、最低限の要件として、暗黙メンバ式チェーン全体がそのコンテキストで暗黙的に推論される型と互換性がある必要があります。具体的には、暗黙的に推論される型がオプショナルの場合は、オプショナル以外の型の値を使用でき、クラス型の場合、そのサブクラスを使用できます。例えば:

```swift
class SomeClass {
    static var shared = SomeClass()
    static var sharedSubclass = SomeSubclass()
    var a = AnotherClass()
}
class SomeSubclass: SomeClass { }
class AnotherClass {
    static var s = SomeClass()
    func f() -> SomeClass { return AnotherClass.s }
}
let x: SomeClass = .shared.a.f()
let y: SomeClass? = .shared
let z: SomeClass = .sharedSubclass
```

上記のコードでは、`x` の型はそのコンテキストから暗黙的に推論された型と正確に一致し、`y` の型は `Someclass` から `SomeClass?` に変換され、`z` の型は `SomeSubclass` から `SomeClass` に変換されます。

> GRAMMAR OF A IMPLICIT MEMBER EXPRESSION  
> implicit-member-expression → `.` [identifier](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_identifier)

### Parenthesized Expression(括弧内の式)

括弧内の式は、括弧で囲まれた式で構成されます。式を明示的にグループ化することで、括弧を使用して操作の優先順位を指定できます。括弧のグループ化は式の型を変更しません(例：`(1)` はただの `Int` です。

> GRAMMAR OF A PARENTHESIZED EXPRESSION  
> parenthesized-expression → `(` [expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_expression)  `)`

### Tuple Expression(タプル式)

タプル式は、括弧で囲まれた式のカンマ区切りのリストで構成されています。各式は、コロン(`:`)で区切られ、その前に識別子を指定することもできます。形式は次のとおりです:

![タプル式](./../.gitbook/assets/tuple_expression.png)

タプル式の各識別子は、タプル式の範囲内で一意な必要があります。入れ子タプル式では、同じレベルで入れ子識別子を一意にする必要があります。例えば、`(a: 10, a: 20)` はラベル `a` が同じレベルで 2 回使用されているため無効です。ただし、`(a: 10, b: (a: 1, x: 2))` は有効です。`a` は 2 回使用されていますが、外側のタプルに 1 回、内側のタプルに 1 回使用されています。

タプル式には、式を含めなくても、2 つ以上の式を含めることもできます。括弧内の単一の式は括弧内の式です。

> NOTE  
> 空のタプル式と空のタプル型はいずれもSwiftでは `()` で書きます。`Void` は `()` のタイプエイリアスのため、空のタプル型を書くために使用できます。ただし、全てのタイプエイリアスと同様に、`Void` は常に型で、空のタプル式を書くためには使用できません。

> GRAMMAR OF A TUPLE EXPRESSION  
> tuple-expression → `(` `)` \|  `(` [tuple-element](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_tuple-element)  `,` [tuple-element-list](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_tuple-element-list)  `)`   
> tuple-element-list → [tuple-element](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_tuple-element) \|  [tuple-element](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_tuple-element)  `,` [tuple-element-list](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_tuple-element-list)  
> tuple-element → [expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_expression) \|  [identifier](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_identifier)  `:` [expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_expression)

### Wildcard Expression(ワイルドカード式)

ワイルドカード式は、代入中に値を明示的に無視するために使用されます。例えば、次の代入式 `10` は `x` に代入されますが、`20` は無視されています:

```swift
(x, _) = (10, 20)
// x は 10 で 20 は 無視されます
```

> GRAMMAR OF A WILDCARD EXPRESSION  
> wildcard-expression → `_`

### Key-Path Expression(Key-Path式)

Key-Path 式は、型のプロパティまたは subscript を参照します。Key-Value Observing などのような、動的プログラミングのタスクで Key-Path 式を使用します。次の形式があります:

![Key-Path式](./../.gitbook/assets/key-path_expression.png)

type name には、`String`、`[Int]`、や `Set<Int>` などのジェネリックな引数を含めた、具体的な型の名前です。

path は、プロパティ名、subscript、オプショナルチェーン式、および強制アンラップ式で構成されます。これらの Key-Path コンポーネントのそれぞれは、必要に応じて任意の順序で繰り返すことができます。

コンパイル時には、Key-Path 式は [KeyPath](https://developer.apple.com/documentation/swift/keypath)クラスのインスタンスに置き換えられます。

Key-Path を使用して値にアクセスするには、KeyPath を `subscript(keyPath:)` subscript に渡します。これは全ての型で利用可能です。例えば:

```swift
struct SomeStructure {
    var someValue: Int
}

let s = SomeStructure(someValue: 12)
let pathToProperty = \SomeStructure.someValue

let value = s[keyPath: pathToProperty]
// value は 12
```

型名は、型推論で暗黙的に型を決定できるコンテキストでは省略できます。次のコードは、`\ someClass.someProperty` の代わりに `\.someProperty` を使用しています:

```swift
class SomeClass: NSObject {
    @objc dynamic var someProperty: Int
    init(someProperty: Int) {
        self.someProperty = someProperty
    }
}

let c = SomeClass(someProperty: 10)
c.observe(\.someProperty) { object, change in
    // ...
}
```

path は、識別 Key-Path`(\.self)` を作成するために `self` を参照できます。識別 Key-Path は、インスタンス全体を参照しているので、それを使用して、変数に格納されている全てのデータを単一のステップでアクセスして変更できます。例えば:

```swift
var compoundValue = (a: 1, b: 2)
// compoundValue = (a: 10, b: 20) と同じ
compoundValue[keyPath: \.self] = (a: 10, b: 20)
```

path には、プロパティのプロパティを参照するために、ピリオドで区切って複数のプロパティ名を含めることができます。このコードは、Key-Path 式 `\OuterStructure.outer.someValue` を使用して、`OuterStructure` 型の `outer` プロパティの `someValue` プロパティにアクセスしています:

```swift
struct OuterStructure {
    var outer: SomeStructure
    init(someValue: Int) {
        self.outer = SomeStructure(someValue: someValue)
    }
}

let nested = OuterStructure(someValue: 24)
let nestedKeyPath = \OuterStructure.outer.someValue

let nestedValue = nested[keyPath: nestedKeyPath]
// nestedValue は 24
```

path は、subscript の引数型が `Hashable` プロトコルに準拠している限り角括弧(`[]`)を使用して subscript を含めることができます。この例では、Key-Path の subscript を使用して、配列の 2 番目の要素にアクセスしています。

```swift
let greetings = ["hello", "hola", "bonjour", "안녕"]
let myGreeting = greetings[keyPath: \[String].[1]]
// myGreeting は 'hola'
```

subscript で使用される値は、名前付きの値またはリテラルです。値は Value セマンティクスを使用して Key-Path にキャプチャされます。次のコードは、Key-Path 式と `greetings` 配列の 3 番目の要素の両方にアクセスするために、可変の `index` を使用しています。`index` が変更されると、Key-Path 式は依然として 3 番目の要素を参照する一方、クロージャは新しいインデックスを使用しています。

```swift
var index = 2
let path = \[String].[index]
let fn: ([String]) -> String = { strings in strings[index] }

print(greetings[keyPath: path])
// "bonjour"
print(fn(greetings))
// "bonjour"

// index に新しい値を設定しても、path には影響しません
index += 1
print(greetings[keyPath: path])
// "bonjour"

// fn が index を参照するので、新しい値を使用しています
print(fn(greetings))
// "안녕"
```

path はオプショナルチェーンと強制アンラップを使用できます。このコードは、オプショナルの文字列のプロパティにアクセスするための Key-Path でオプショナルチェーンを使用しています。

```swift
let firstGreeting: String? = greetings.first
print(firstGreeting?.count as Any)
// "Optional(5)"

// Key-Path を使用して同じことをしています
let count = greetings[keyPath: \[String].first?.count]
print(count as Any)
// "Optional(5)"
```

Key-Path のコンポーネントを、型内に深くネストされている値にアクセスために組み合わせることができます。次のコードは、これらのコンポーネントを組み合わせた Key-Path 式を使用して、配列内の辞書のプロパティの様々な値にアクセスしています:

```swift
let interestingNumbers = ["prime": [2, 3, 5, 7, 11, 13, 17],
                          "triangular": [1, 3, 6, 10, 15, 21, 28],
                          "hexagonal": [1, 6, 15, 28, 45, 66, 91]]
print(interestingNumbers[keyPath: \[String: [Int]].["prime"]] as Any)
// "Optional([2, 3, 5, 7, 11, 13, 17])"
print(interestingNumbers[keyPath: \[String: [Int]].["prime"]![0]])
// "2"
print(interestingNumbers[keyPath: \[String: [Int]].["hexagonal"]!.count])
// "7"
print(interestingNumbers[keyPath: \[String: [Int]].["hexagonal"]!.count.bitWidth])
// "64"
```

関数またはクロージャを使用できるコンテキストでは、Key-Path 式を使用できます。具体的には、`(SomeType) -> Value` 型の関数やクロージャの代わりに、基の型が `SomeType` で、その path が `Value` 型の値を生成することができます。

```swift
struct Task {
    var description: String
    var completed: Bool
}
var toDoList = [
    Task(description: "Practice ping-pong.", completed: false),
    Task(description: "Buy a pirate costume.", completed: true),
    Task(description: "Visit Boston in the Fall.", completed: false),
]

// 以下の両方のアプローチは同等です
let descriptions = toDoList.filter(\.completed).map(\.description)
let descriptions2 = toDoList.filter { $0.completed }.map { $0.description }
```

Key-Path 式の副作用は、式が評価される時点でのみ評価されます。例えば、Key-Path 式で subscript の内側の関数呼び出しを行うと、関数は、Key-Path が使用される度にではなく、式を評価する際に 1 回だけ呼び出されます。

```swift
func makeIndex() -> Int {
    print("Made an index")
    return 0
}
// 下の行は makeIndex() を呼び出します
let taskKeyPath = \[Task][makeIndex()]
// "Made an index"

// taskKeyPath を使用すると makeIndex() は再び呼び出されません。
let someTask = toDoList[keyPath: taskKeyPath]
```

Objective-C API とやり取りするコード内の Key-Path の使用方法の詳細については、[Using Objective-C Runtime Features in Swift](https://developer.apple.com/documentation/swift/using_objective_c_runtime_features_in_swift)を参照ください。Key-Value Coding や Key-Value Observing については、[Key-Value Coding Programming Guide](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueCoding/index.html#//apple_ref/doc/uid/10000107i)と[Key-Value Observing Programming Guide](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueObserving/KeyValueObserving.html#//apple_ref/doc/uid/10000177i)を参照ください。

> GRAMMAR OF A KEY-PATH EXPRESSION  
> key-path-expression → `\` [type](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_type)<sub>*opt*</sub> `.` [key-path-components](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_key-path-components)  
> key-path-components → [key-path-component](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_key-path-component) \|  [key-path-component](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_key-path-component)  `.` [key-path-components](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_key-path-components)  
> key-path-component → [identifier](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_identifier)  [key-path-postfixes](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_key-path-postfixes)<sub>*opt*</sub> \|  [key-path-postfixes](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_key-path-postfixes)  
> key-path-postfixes → [key-path-postfix](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_key-path-postfix)  [key-path-postfixes](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_key-path-postfixes)<sub>*opt*</sub>  
> key-path-postfix → `?` \|  `!` \|  `self` \|  `[` [function-call-argument-list](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_function-call-argument-list)  `]`

### Selector Expression(Selector式)

セレクタ式を使用すると、Objective-C のメソッドまたはプロパティの get や set を参照するために使用されるセレクタにアクセスできます。形式は次のとおりです:

![Selector式](./../.gitbook/assets/selector_expression.png)

メソッド名とプロパティ名は、Objective-C ランタイムで使用可能なメソッドまたはプロパティへの参照にする必要があります。セレクタ式の値は `Selector` 型のインスタンスです。例えば:

```swift
class SomeClass: NSObject {
    @objc let property: String

    @objc(doSomethingWithInt:)
    func doSomething(_ x: Int) { }

    init(property: String) {
        self.property = property
    }
}
let selectorForMethod = #selector(SomeClass.doSomething(_:))
let selectorForPropertyGetter = #selector(getter: SomeClass.property)
```

プロパティの get のセレクタを作成すると、プロパティ名は変数または定数プロパティを参照できます。対照的に、プロパティの set のセレクタを作成すると、プロパティ名は変数プロパティのみ参照しなければなりません。

同じ名前でシグネチャが異なるメソッド間の曖昧さを軽減するために `as` 演算子と一緒にグループ化するための括弧を含めることができます。例えば:

```swift
extension SomeClass {
    @objc(doSomethingWithString:)
    func doSomething(_ x: String) { }
}
let anotherSelector = #selector(SomeClass.doSomething(_:) as (SomeClass) -> (String) -> Void)
```

セレクタが実行時ではなくコンパイル時に作成されるため、コンパイラはメソッドまたはプロパティが存在すること、およびそれらが Objective-C ランタイムに公開されていることを確認できます。

> NOTE  
> メソッド名とプロパティ名は式ですが、それらは決して評価されません。

Objective-C API とやり取りする Swift コードでセレクタを使用する方法の詳細については、[Using Objective-C Runtime Features in Swift](https://developer.apple.com/documentation/swift/using_objective_c_runtime_features_in_swift)を参照ください。

> GRAMMAR OF A SELECTOR EXPRESSION  
> selector-expression → `#selector` `(` [expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_expression)  `)`  
> selector-expression → `#selector` `(` `getter:` [expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_expression)  `)`  
> selector-expression → `#selector` `(` `setter:` [expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_expression)  `)`

### Key-Path String Expression(Key-Path文字列式)

Key-Path 文字列式を使用すると、Key-Value Coding や Key-Value Observing API で使用するために、Objective-C のプロパティを参照するための文字列にアクセスできます。形式は次のとおりです:

![Key-Path文字列式](./../.gitbook/assets/key-path_string_expression.png)

プロパティ名は、Objective-C ランタイムで使用可能なプロパティを参照する必要があります。コンパイル時には、Key-Path 文字列式は文字列リテラルに置き換えられます。例えば:

```swift
class SomeClass: NSObject {
    @objc var someProperty: Int
    init(someProperty: Int) {
        self.someProperty = someProperty
    }
}

let c = SomeClass(someProperty: 12)
let keyPath = #keyPath(SomeClass.someProperty)

if let value = c.value(forKey: keyPath) {
    print(value)
}
// "12"
```

クラス内で Key-Path 文字列式を使用すると、クラス名なしでプロパティ名だけを書くことでそのクラスのプロパティを参照できます。

```swift
extension SomeClass {
    func getSomeKeyPath() -> String {
        return #keyPath(someProperty)
    }
}
print(keyPath == c.getSomeKeyPath())
// "true"
```

Key-Path 文字列は実行時ではなく、コンパイル時に作成されているため、コンパイラはプロパティが存在すること、およびそのプロパティが Objective-C ランタイムに公開されていることを確認できます。

Objective-C API とやり取りする Swift コードで Key-Path を使用する方法の詳細については、[Using Objective-C Runtime Features in Swift](https://developer.apple.com/documentation/swift/using_objective_c_runtime_features_in_swift)を参照ください。Key-Value Coding と Key-Value Observing については、[Key-Value Coding Programming Guide](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueCoding/index.html#//apple_ref/doc/uid/10000107i)と[Key-Value Observing Programming Guide](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueObserving/KeyValueObserving.html#//apple_ref/doc/uid/10000177i)を参照ください。

> NOTE  
> プロパティ名は式ですが、それらは決して評価されません。

> GRAMMAR OF A KEY-PATH STRING EXPRESSION  
> key-path-string-expression → `#keyPath` `(` [expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_expression)  `)`

## Postfix Expressions(後置式)

後置式は、後置演算子またはその他の後置構文を式に適用することによって形成されます。構文的には、全ての基本式も後置式です。

これらの演算子の動作については、[Basic Operators](./../language-guide/basic-operators.md)と[Advanced Operators](./../language-guide/advanced-operators.md)を参照ください。

Swift 標準ライブラリによって提供されている演算子については、[Operator Declarations](https://developer.apple.com/documentation/swift/swift_standard_library/operator_declarations)を参照ください。

> GRAMMAR OF A POSTFIX EXPRESSION  
> postfix-expression → [primary-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_primary-expression)  
> postfix-expression → [postfix-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_postfix-expression)  [postfix-operator](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_postfix-operator)  
> postfix-expression → [function-call-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_function-call-expression)  
> postfix-expression → [initializer-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_initializer-expression)  
> postfix-expression → [explicit-member-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_explicit-member-expression)  
> postfix-expression → [postfix-self-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_postfix-self-expression)  
> postfix-expression → [subscript-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_subscript-expression)  
> postfix-expression → [forced-value-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_forced-value-expression)  
> postfix-expression → [optional-chaining-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_optional-chaining-expression)

### Function Call Expression(関数呼び出し式)

関数呼び出し式は、関数名とそれに続く関数の引数のカンマ区切りリストからなる関数名で構成されています。関数呼び出し式は形式は次のとおりです:

![関数呼び出し式1](./../.gitbook/assets/function_call_expression1.png)

function name は、関数型の任意の式です。

関数定義に引数名が含まれている場合、関数呼び出しは、コロン(`:`)で区切られた引数値の前に名前を含める必要があります。この種の関数呼び出し式は形式は次のとおりです:

![引数名を含んだ関数呼び出し式](./../.gitbook/assets/function_call_expression2.png)

関数呼び出し式は、閉じ括弧(`}`)の直後にクロージャ式の形で末尾クロージャを含めることができます。末尾クロージャは、最後の括弧内の引数の後の関数型の引数と解釈されます。最初のクロージャ式に引数ラベルは付けません。次のクロージャ式の前には引数ラベルを付けます。下記の例は、末尾クロージャの構文を使用して、 末尾クロージャを使用しない関数呼び出しバージョンと同等なことをしています:

```swift
// someFunction 関数は引数として整数とクロージャを受け取ります
someFunction(x: x, f: { $0 == 13 })
someFunction(x: x) { $0 == 13 }

// anotherFunction 関数は引数として整数と 2 つのクロージャを受け取ります
anotherFunction(x: x, f: { $0 == 13 }, g: { print(99) })
anotherFunction(x: x) { $0 == 13 } g: { print(99) }
```

末尾クロージャが関数の唯一の引数の場合は、括弧を省略できます。

```swift
// someMethod は唯一の引数としてクロージャを受け取ります
myData.someMethod() { $0 == 13 }
myData.someMethod { $0 == 13 }
```

引数に末尾クロージャを含めるために、コンパイラは次のように左から右へ関数の引数を調べます:

| 末尾クロージャ | 引数 | アクション |
| :---: | :---: | :---: |
| ラベルあり | ラベルあり | ラベルが同じ場合、クロージャは引数と一致します。それ以外の場合は、スキップされます |
| ラベルあり | ラベルなし | 引数はスキップされます |
| ラベルなし | ラベルあり/なし | 下記に定義されているように、引数が関数型と見なされる場合、クロージャは引数と一致します。それ以外の場合は、スキップされます。|

末尾クロージャは、それが一致する関数の引数に渡されます。スキャンプロセス中にスキップされた引数は、値が渡されません。例えば、デフォルトの引数を使用できます。一致する引数を見つけた後、スキャンは次の末尾クロージャと次の引数に続きます。マッチングプロセスの最後に、全ての末尾クロージャが一致している必要があります。

引数が In-Out 引数ではなく、次のいずれかの場合、引数は関数型と見なされます:

* `(Bool) -> Int` のように引数の型が関数型
* `@autoclosure () -> ((Bool) -> Int)` のように、ラップされた式の型が関数型の自動クロージャ引数
* `((Bool) -> Int)...` のように、配列要素の型が関数型の可変長引数
* `Optional<(Bool) -> Int>` のように、型がオプショナルの 1 つ以上の層にラップされている引数
* `(Optional<(Bool) -> Int>)...` のように、上記の許可された型を組み合わせた引数

末尾クロージャが機能的には関数型のように見えるが関数ではない引数と一致する場合、クロージャは必要に応じてラップされます。例えば、引数の型がオプショナルの型の場合、クロージャは自動的に `Optional` でラップされます

これは右から左にマッチングを実行していた Swift 5.3 以前のコードから移行を簡単にするために、スキャン方向で異なる結果を生成する場合は、古い右から左へ順序付けされ、コンパイラは警告を生成します。それ以降の Swift のバージョンでは常に左から右へ正しく順序付けします。

```swift
typealias Callback = (Int) -> Int
func someFunction(firstClosure: Callback? = nil,
                  secondClosure: Callback? = nil) {
    let first = firstClosure?(10)
    let second = secondClosure?(20)
    print(first ?? "-", second ?? "-")
}

someFunction()  // "- -"
someFunction { return $0 + 100 }  // "Ambiguous"
someFunction { return $0 } secondClosure: { return $0 }  // "10 20"
```

上記の例では、"Ambiguous"とマークされている関数の呼び出しは"- 120"が出力され、Swift 5.3 ではコンパイラが警告を生成します。それ以降の Swift のバージョンでは "110 -"が出力されます。

クラス、構造体、または列挙型は、[Methods with Special Names](./declarations.md#methods-with-special-names特別な名前を持つメソッド)で説明されているような、いくつかのメソッドの 1 つを宣言することで、関数呼び出しの糖衣構文(シンタックスシュガー)を使うことができます。

#### Implicit Conversion to a Pointer Type(ポインタ型への暗黙変換)

---

関数呼び出し式で、引数と引数に渡された値が異なる場合、コンパイラは次のリストの暗黙的な変換の 1 つを適用することによって、その型が一致するようにします。

* `inout SomeType` は、`UnsafePointer<SomeType>` または `UnsafeMutablePointer<SomeType>` になる可能性があります
* `inout Array<SomeType>` は、`UnsafePointer<SomeType>` または `UnsafeMutablePointer<SomeType>` になる可能性があります
* `Array<SomeType>` は、`UnsafePointer<SomeType>` になる可能性があります
* `String` は `UnsafePointer<CChar>` になる可能性があります

次の 2 つの関数呼び出しは同等です:

```swift
func unsafeFunction(pointer: UnsafePointer<Int>) {
    // ...
}
var myNumber = 1234

unsafeFunction(pointer: &myNumber)
withUnsafePointer(to: myNumber) { unsafeFunction(pointer: $0) }
```

これらの暗黙の変換によって作成されたポインタは、関数呼び出しの間だけ有効です。未定義の動作を避けるために、関数呼び出しが終了した後までポインタを保持しないようにしてください。

> NOTE  
> 配列を暗黙的に安全でないポインタに変換すると、Swift は、配列のストレージが必要に応じて配列を変換またはコピーすることによって連続していることを保証します。例えば、この構文は、そのストレージに関する API の契約がない(動作が定義されているか定かではない) `NSArray` のサブクラスから `Array` にブリッジされた配列でこの構文を使用できます。配列のストレージがすでに連続していることを保証する必要がある場合、暗黙の変換を行わないようにするために、`Array` の代わりに `ContigureArray` を使用します

`withUnsafePointer(to:)` のような明示的な機能の代わりに、`&` を使うことで、低レベルの C 言語の関数の呼び出しやすくするのに役立ちます。ただし、他の Swift コードから関数を呼び出すときは、安全でない API を明示的に使用する代わりとして `&` を使用しないでください。

> GRAMMAR OF A FUNCTION CALL EXPRESSION  
> function-call-expression → [postfix-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_postfix-expression)  [function-call-argument-clause](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_function-call-argument-clause)  
> function-call-expression → [postfix-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_postfix-expression)  [function-call-argument-clause](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_function-call-argument-clause) opt [trailing-closures](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_trailing-closures)  
> function-call-argument-clause → `(` `)` \|  `(` [function-call-argument-list](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_function-call-argument-list)  `)`  
> function-call-argument-list → [function-call-argument](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_function-call-argument) \|  [function-call-argument](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_function-call-argument)  `,` [function-call-argument-list](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_function-call-argument-list)  
> function-call-argument → [expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_expression) \|  [identifier](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_identifier)  `:` [expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_expression)  
> function-call-argument → [operator](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_operator) \|  [identifier](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_identifier)  `:` [operator](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_operator)  
> trailing-closures → [closure-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_closure-expression)  [labeled-trailing-closures](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_labeled-trailing-closures)<sub>*opt*</sub>  
> labeled-trailing-closures → [labeled-trailing-closure](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_labeled-trailing-closure)  [labeled-trailing-closures](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_labeled-trailing-closures)<sub>*opt*</sub>  
> labeled-trailing-closure → [identifier](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_identifier)  `:` [closure-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_closure-expression)

### Initializer Expression(イニシャライザ式)

イニシャライザ式は型のイニシャライザへアクセスします。形式は次のとおりです:

![イニシャライザ式](./../.gitbook/assets/initializer_expression.png)

イニシャライザ式を使用して、型の新しいインスタンスを初期化します。スーパークラスのイニシャライザに委譲するイニシャライザ式を使用することもできます。

```swift
class SomeSubClass: SomeSuperClass {
    override init() {
        // subclass の初期化処理をここに
        super.init()
    }
}
```

関数のように、イニシャライザを値として使用することができます。例えば:

```swift
// 型注釈は、String に複数のイニシャイザがあるため必要です
let initializer: (Int) -> String = String.init
let oneTwoThree = [1, 2, 3].map(initializer).reduce("", +)
print(oneTwoThree)
// "123"
```

名前で型を指定した場合は、イニシャライザ式を使用せずに型のイニシャライザにアクセスできます。他の場合では、イニシャライザ式を使用する必要があります。

```swift
let s1 = SomeType.init(data: 3)  // 有効
let s2 = SomeType(data: 1)       // これも有効

let s3 = type(of: someValue).init(data: 7)  // 有効
let s4 = type(of: someValue)(data: 5)       // エラー
```

> GRAMMAR OF AN INITIALIZER EXPRESSION  
> initializer-expression → [postfix-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_postfix-expression)  `.` `init`  
> initializer-expression → [postfix-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_postfix-expression)  `.` `init` `(` [argument-names](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_argument-names)  `)`

### Explicit Member Expression(明示的メンバ式)

明示的メンバ式では、名前付き型、タプル、またはモジュールのメンバへアクセスできます。アイテムとそのメンバの識別子の間のピリオド(`.`)で構成されています。

![明示的メンバ式](./../.gitbook/assets/explicit_member_expression.png)

名前付き型のメンバは、型の宣言または拡張子の一部として指定されます。例えば:

```swift
class SomeClass {
    var someProperty = 42
}
let c = SomeClass()
let y = c.someProperty  // メンバへのアクセス
```

タプルのメンバは、0 から始まる整数が順番に暗黙的に指定されており、使用することができます。例えば:

```swift
var t = (10, 20, 30)
t.0 = t.1
// t は今 (20, 20, 30)
```

モジュールのメンバはそのモジュールの最上位の宣言にアクセスします。

`dynamicMemberLookup` 属性で宣言された型には、[Attributes](./attributes.md)で説明されているように、実行時に検索できるメンバが含まれています。

引数名だけが異なるメソッドまたはイニシャライザを区別するには、引数名を括弧内に入れ、引数名の後にコロン(`:`)を書きます。名前のない引数にはアンダースコア(`_`)を書きます。オーバーロードされたメソッドを区別するには、型注釈を使用してください。例えば:

```swift
class SomeClass {
    func someMethod(x: Int, y: Int) {}
    func someMethod(x: Int, z: Int) {}
    func overloadedMethod(x: Int, y: Int) {}
    func overloadedMethod(x: Int, y: Bool) {}
}
let instance = SomeClass()

let a = instance.someMethod              // あいまい
let b = instance.someMethod(x:y:)        // 明確

let d = instance.overloadedMethod        // あいまい
let d = instance.overloadedMethod(x:y:)  // まだあいまい
let d: (Int, Bool) -> Void  = instance.overloadedMethod(x:y:)  // 明確
```

ピリオドが行の先頭に示されている場合は、暗黙メンバ式としてではなく、明示的メンバ式の一部として解釈されます。例えば、次のリストはメソッドチェーンで呼び出しが複数行にわたって分割された呼び出しを示しています:

```swift
let x = [10, 3, 20, 15, 4]
    .sorted()
    .filter { $0 > 5 }
    .map { $0 * 100 }
```

> GRAMMAR OF AN EXPLICIT MEMBER EXPRESSION  
> explicit-member-expression → [postfix-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_postfix-expression)  `.` [decimal-digits](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_decimal-digits)  
> explicit-member-expression → [postfix-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_postfix-expression)  `.` [identifier](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_identifier)  [generic-argument-clause](https://docs.swift.org/swift-book/ReferenceManual/GenericParametersAndArguments.html#grammar_generic-argument-clause)<sub>*opt*</sub>  
> explicit-member-expression → [postfix-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_postfix-expression)  `.` [identifier](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_identifier)  `(` [argument-names](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_argument-names)  `)`  
> argument-names → [argument-name](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_argument-name)  [argument-names](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_argument-names)<sub>*opt*</sub>  
> argument-name → [identifier](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_identifier)  `:`

### Postfix Self Expression(後置self式)

後置 `self` 式は、型や式の直後に `.self` を付けて構成します。次の形式があります:

![後置self式](./../.gitbook/assets/postfix_self_expression.png)

最初の形式は expression の値に評価されます。例えば、`x.self` は `x` と評価されます。

2 番目の形式は type の値に評価されます。この形式を使用して、型に値としてアクセスできます。例えば、`SomeClass.self` は `SomeClass` 型自体に評価されるため、型レベルの引数を受け取る関数またはメソッドに渡すことができます。

> GRAMMAR OF A POSTFIX SELF EXPRESSION  
> postfix-self-expression → [postfix-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_postfix-expression)  `.` `self`

### Subscript Expression(subscript式)

subscript 式は、対応する subscript 宣言の get と set を使用して subscript へのアクセスすることができます。形式は次のとおりです:

![subscript式](./../.gitbook/assets/subscript_expression.png)

subscript 式の値を評価するには、expression 型の subscript の get を subscript の引数として index expressions を渡して呼び出します。値を設定するために、subscript の set を同じ方法で呼び出します。

subscript 宣言については、[Protocol Subscript Declaration](./declarations.md#protocol-subscript-declarationプロトコルのsubscript宣言)を参照ください。

> GRAMMAR OF A SUBSCRIPT EXPRESSION  
> subscript-expression → [postfix-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_postfix-expression)  `[` [function-call-argument-list](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_function-call-argument-list)  `]`

### Forced-Value Expression(強制アンラップ式)

強制アンラップ式は、特定の値が `nil` ではないオプショナルの値を表示します。形式は次のとおりです:

![強制アンラップ式](./../.gitbook/assets/forced-Value_expression.png)

式の値が `nil` でない場合、オプショナルの値はアンラップされ、対応するオプショナルの非オプショナルの型で返されます。それ以外の場合は、実行時エラーが発生します。

強制的にアンラップされた値は、値自体を変化させる、またはその値のメンバの 1 つに割り当てることによって、変更できます。例えば:

```swift
var x: Int? = 0
x! += 1
// x は 1

var someDictionary = ["a": [1, 2, 3], "b": [10, 20]]
someDictionary["a"]![0] = 100
// someDictionary は ["a": [100, 2, 3], "b": [10, 20]]
```

> GRAMMAR OF A FORCED-VALUE EXPRESSION  
> forced-value-expression → [postfix-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_postfix-expression)  `!`

### Optional-Chaining Expression(オプショナルチェーン式)

オプショナルチェーン式は後置式で、オプショナルの値を使用するための簡単な構文を提供します。形式は次のとおりです:

![オプショナルチェーン式](./../.gitbook/assets/optional-chaining_expression.png)

後置 `?` 演算子は式の値を変更せずに式からオプショナルチェーン式を作成します。

オプショナルチェーン式は、後置式で使用しなければならず、後置式を特別な方法で評価します。オプショナルチェーン式の値が `nil` の場合、後置式の他の全ての操作は無視され、後置式全体が `nil` に評価されます。`nil` ではない場合、値はアンラップされ、後置式の残りの部分を評価するために使用されます。どちらの場合も、後置式の値は依然としてオプショナル型です。

オプショナルチェーン式を含む後置式が他の後置式の内側にネストされている場合は、最も外側の式だけがオプショナル型を返します。下記の例では、`c` が `nil` ではない場合、その値はアンラップされ、その値は `.performAction()` を評価するために使用される `.property` を評価するために使用されます。全体の式 `c？.property.performAction()` はオプショナルの型の値を持ちます。

```swift
var c: SomeClass?
var result: Bool? = c?.property.performAction()
```

次の例は、オプショナルチェーンを使用せずに上記の例の動作を表現しています。

```swift
var result: Bool?
if let unwrappedC = c {
    result = unwrappedC.property.performAction()
}
```

オプショナルチェーン式のアンラップ値は、値自体を変える、またはその値のメンバに代入することで変更できます。オプショナルチェーン式の値が `nil` の場合、代入演算子の右側の式は評価されません。例えば:

```swift
func someFunctionWithSideEffects() -> Int {
    return 42  // 実際の副作用はありません
}
var someDictionary = ["a": [1, 2, 3], "b": [10, 20]]

someDictionary["not here"]?[0] = someFunctionWithSideEffects()
// someFunctionWithSideEffects は評価されません
// someDictionary はまだ ["a": [1, 2, 3], "b": [10, 20]]

someDictionary["a"]?[0] = someFunctionWithSideEffects()
// someFunctionWithSideEffects は評価され、42 を返します
// someDictionary は今 ["a": [42, 2, 3], "b": [10, 20]]
```

> GRAMMAR OF AN OPTIONAL-CHAINING EXPRESSION  
> optional-chaining-expression → [postfix-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_postfix-expression)  `?`
