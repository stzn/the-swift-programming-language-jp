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

オプショナル try 式は `try?` 演算子の後にエラーをスローできる式が続く形で構成されます。演算子の後にエラーをスローできる式が続きます。次の形式です:

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
> 構文解析時には、式はフラットなリストとして表されたバイナリ演算子で構成されていました。このリストは、演算子の優先順位を適用することによってツリーに変換されます。例えば、式 `2 + 3 * 5` は、最初は5つの項目、`2`、`+`、`3`、`*`、および `5` として解釈され、その後 `(2 + (3 * 5))` のツリーに変換します  

> GRAMMAR OF A BINARY EXPRESSION  
> binary-expression → [binary-operator](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_binary-operator)  [prefix-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_prefix-expression)  
> binary-expression → [assignment-operator](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_assignment-operator)  [try-operator](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_try-operator)<sub>*opt*</sub> [prefix-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_prefix-expression)  
> binary-expression → [conditional-operator](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_conditional-operator)  [try-operator](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_try-operator)<sub>*opt*</sub> [prefix-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_prefix-expression)  
> binary-expression → [type-casting-operator](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_type-casting-operator)  
> binary-expressions → [binary-expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_binary-expression)  [binary-expressions](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_binary-expressions)<sub>*opt*</sub>

### Assignment Operator(代入演算子)

代入演算子は特定の式の新しい値を設定します。次の形式です:

![Assignment Operator](./../.gitbook/assets/assignment_operator.png)

値を評価した結果得られた値を expression に設定されます。式がタプルの場合、値は同じ数の要素を持つタプルでなければなりません。(タプルはネストすることもできます)。代入は、値の各部分から expression の中の対応する部分に実行されます。例えば:

```swift
(a, _, (b, c)) = ("test", 9.45, (12, 3))
// a は「テスト」、 b は 12、 c は 3、 9.45 は無視されます
```

代入演算子は任意の値を返しません。

> GRAMMAR OF AN asSIGNMENT OPERATOR  
> assignment-operator → `=`

### Ternary Conditional Operator(三項条件演算子)

三項条件演算子は、条件の値に基づいて、2 つの値のうちの 1 つに評価されます。次の形式です:

![Ternary Conditional Operator](./../.gitbook/assets/ternary_conditional_operator.png)

条件が `true` と評価された場合、条件演算子は最初の式を評価し、その値を返します。それ以外の場合は、2 番目の式を評価してその値を返します。未使用の式は評価されません。

三項条件演算子を使用する例については、[Ternary Conditional Operator](./../language-guide/basic-operators.md#ternary-conditional-operator三項条件演算子)を参照ください。

> GRAMMAR OF A CONDITIONAL OPERATOR  
> conditional-operator → `?` [expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_expression)  `:`

### Type-Casting Operators(型キャスト演算子)

4 つの型のキャスト演算子があります: `is` 演算子、`as` 演算子、`as?` 演算子、そして `as!` 演算子。

それらは次の形式を持っています:

![Type-Casting Operators](./../.gitbook/assets/type_casting_operator.png)

`is` 演算子は実行時に式が指定された型にキャストできるかどうかを確認します。式が指定された型にキャストできる場合は `true` を返します。それ以外の場合は、`false` を返します。

`as` 演算子は、コンパイル時にキャストが常に成功するとわかっている場合にキャストを実行します。アップキャストは、中間変数を使用せずに型のスーパー型のインスタンスとして式を使用できます。下記のアプローチは同等です。

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

ブリッジングを使用すると、新しいインスタンスを作成せずに、`String` などの Swift 標準ライブラリ型の式を、それに相応する `NSString` などの Foudation 型で使用できるようにしています。ブリッジングの詳細については、[Working with Foundation Types](https://developer.apple.com/documentation/swift/imported_c_and_objective_c_apis/working_with_foundation_types)を参照ください。

`as?` 演算子は、指定型へ条件付きキャストを実行します。`as?` 演算子は指定された型のオプショナルを返します。実行時に、キャストが成功した場合、式の値がオプショナルで返されます。それ以外の場合、返される値は `nil` です。指定された型へのキャストが失敗するか、成功することが明らかな場合は、コンパイルエラーが発生します。

`as!` 演算子は、指定された型に強制キャストを実行します。`as!` 演算子は、オプショナル型ではなく、指定された型の値を返します。キャストが失敗した場合は、実行時エラーが発生します。`x as! T` は `(x as? T)!` の挙動と同じです。

型キャストの詳細や型キャスト演算子を使用する例については、[Type Casting](./../language-guide/type-casting.md)を参照ください。

> GRAMMAR OF A TYPE-CASTING OPERATOR  
> type-casting-operator → `is` [type](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_type)  
> type-casting-operator → `as` [type](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_type)  
> type-casting-operator → `as` `?` [type](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_type)  
> type-casting-operator → `as` `!` [type](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_type)

## Primary Expressions(基本式)

基本式は最も基本的な種類の式です。それらは自身を式として使用することができたり、他のトークンと組み合わせたり、前置式、バイナリ式、および後置式を作成することができます。

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

リテラル式は、通常のリテラル(文字列や数など)、配列または辞書リテラル、playground リテラル、または下記の特別なリテラルのいずれかで構成されています。

| Literal | Type | Value |
| :---: | :---: | :---: |
| `#file` | `String` | 使用されているファイルへのパス |
| `#fileID` | `String` | 使用されているファイルとモジュールの名前 |
| `#filePath` | `String` | 表示されているファイルへのパス |
| `#line` | `Int` | 使用されている行番号 |
| `#column` | `Int` | 開始列番号 |
| `#function` | `String` | 使用されている宣言の名前 |
| `#dsohandle` | `UnsafeRawPointer` | 使用中の動的共有オブジェクト(*dynamic shared object*(DSO))ハンドル |

`#file` の文字列値は、古い `#filePath` から新しい `#fileID` への移行を有効にするために、言語のバージョンによって異なります。現在、`#file` は `#filePath` と同じ値を持ちます。将来の Swift のバージョンでは、`#file` は代わりに `#fileID` と同じ値を持ちます。将来のバージョンの挙動を適用するには、`#file` を `#fileID` または `#filepath` に置き換える必要があります。

`#fileID` 式の文字列値にはモジュール/ファイル形式があります。ここで言う、「ファイル」は式が使用されているファイルの名前で、「モジュール」は、がこのファイルが属しているモジュールの名前です。`#filePath` 式の文字列値は、式が使用されているファイルへのフルパスです。[Line Control Statement](./statements.md#line-control-statement行制御文)で説明されているように、これらの値はどちらも `#sourceLocation` に変わる可能性があります。`#fileID` は `#filePath` とは異なり、ソースファイルへのフルパスをソースファイルに埋め込むことはできませんので、より良いプライバシーを提供し、コンパイルされたバイナリのサイズを減させることができます。テスト、ビルドスクリプト、また配布プログラムの一部にはならないコードの外側で `#filePath` を使用しないでください。

> NOTE  
> `#fileID` 式は、最初のスラッシュ(`/`）の前のテキストをモジュール名、最後のスラッシュ(`/`）の後のテキストをファイル名と読んでください。将来的には、`MyModule/some/disambigation/myfile.swift` などのように、複数のスラッシュが含まれている可能性があります。

`#function` の値は、関数内ではその関数の名前です。メソッド内では、そのメソッドの名前、プロパティ get または set 内ではプロパティ名、`init` や `subscript` のような特別なメンバ内では、そのキーワード名、およびファイルのトップレベルでは、現在のモジュールの名です。

関数またはメソッドの引数のデフォルト値として使用すると、呼び出し側でデフォルト値の式が評価されると、特別なリテラル値が決定します。

```swift
func logFunctionName(string: String = #function) {
   print(string)
}
func myFunction() {
   logFunctionName() // "myFunction()".
}
```

配列リテラルは、値の順序付けられた集合です。次の形式です:

![配列リテラル](./../.gitbook/assets/array_expression.png)

配列内の最後の式の後にカンマ(`,`)を続けることもできます。配列リテラルの値は `[T]` 型で、`T` はその内部の式の型です。複数の型の式がある場合、`T` はそれらに最も近い共通スーパー型になります。空の配列リテラルは、空の角括弧(`[]`)を使用し、指定された型の空の配列を作成するためにも使用できます。

```swift
var emptyArray: [Double] = []
```

辞書リテラルは、順序のないキーバリューペアのコレクションです。次の形式です:

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

### Self Expression(Self式)

Self 式は、それが出現する現在の型またはインスタンスへの明示的な参照です。次の形式です:

![Self式](./../.gitbook/assets/self_expression.png)

イニシャライザ、subscript、またはインスタンスメソッドでは、`self` は、それが出現する現在の型のインスタンスを表します。型メソッドでは、`self` はそれが出現する現在の型を表します。

`self` 式は、関数引数などのスコープ内に同じ名前の別の変数があり、何を指すのかが曖昧な場合に、メンバへアクセスするときに指定します。例えば:

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
