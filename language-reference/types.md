# Types \(型\)

最終更新日: 2021/6/11

Swift では、名前付き型(*named type*)と複合型(*compound type*)の 2 種類があります。名前付き型は、定義時に特定の名前を指定できる型です。名前付き型には、クラス、構造体、列挙型、およびプロトコルが含まれます。例えば、`MyClass` という名前のユーザ定義クラスのインスタンスは `MyClass` 型です。ユーザ定義の型に加えて、Swift 標準ライブラリは、配列、辞書、およびオプショナルの値を含む多くの広く使用される型の名前を定義します。

通常、数字、文字、文字列を表す型など、他の言語で基本的またはプリミティブと見なされるデータ型は、Swift 標準ライブラリでは構造体を使用して定義、実装されている名前付き型です。名前付き型のため、[Extensions](./expressions.md)、 [Extension Declaration](./declarations.md#extension-declaration拡張宣言)で説明されている拡張宣言を使用して、プログラムのニーズに合わせて振る舞いを拡張することができます。

複合型は、Swift 言語で定義されている名前のない型です。関数型とタプル型の 2 つの複合型があります。複合型の種類は、名前付き型および他の複合型を含めることができます。例えば、タプル型 `(Int, (Int, Int))` には、名前付き型の `Int` と複合型の `(Int, Int)` の 2 つの要素が含まれています。

名前付き型または複合型の周囲には括弧(`()`)を付けることができます。ただし、型の周囲に括弧を必ずしも付ける必要はありません。例えば、`(Int)` は `Int` と同等です。

この章では、Swift 言語自体で定義されている型について説明し、Swift の型推論の動作について説明します。

> GRAMMAR OF A TYPE  
> type → [function-type](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_function-type)  
> type → [array-type](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_array-type)  
> type → [dictionary-type](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_dictionary-type)  
> type → [type-identifier](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_type-identifier)  
> type → [tuple-type](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_tuple-type)  
> type → [optional-type](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_optional-type)  
> type → [implicitly-unwrapped-optional-type](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_implicitly-unwrapped-optional-type)  
> type → [protocol-composition-type](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_protocol-composition-type)  
> type → [opaque-type](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_opaque-type)  
> type → [metatype-type](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_metatype-type)  
> type → [any-type](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_any-type)  
> type → [self-type](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_self-type)  
> type → `(` [type](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_type)  `)`

## Type Annotation(型注釈)

型注釈は、変数または式の型を明示的に指定します。次の例に示すように、型注釈はコロン(`:`)で始まり、型で終わります。

```swift
let someTuple: (Double, Double) = (3.14159, 2.71828)
func someFunction(a: Int) { /* ... */ }
```

最初の例では、式がタプル型 `(Double, Double)` を持つように指定されています。2 番目の例では、`someFunction` 関数の引数 `a` が `Int` 型だと指定されています。

型注釈は、型の前に型属性の任意のリストを含めることができます。

> GRAMMAR OF A TYPE ANNOTATION  
> type-annotation → `:` [attributes](https://docs.swift.org/swift-book/ReferenceManual/Attributes.html#grammar_attributes)<sub>*opt*</sub> **inout**<sub>*opt*</sub> [type](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_type)

## Type Identifier(型識別子)

型識別子とは、名前付き型または複合型の型注釈を参照します。

ほとんどの場合、型識別子は識別子と同じ名前の名前付き型を直接参照します。例えば、`Int` は、名前付き型 `Int` を直接参照する型識別子で、型識別子 `Dictionary<String, Int>` は指定された `Dictionary<String, Int>` を直接参照します。

型識別子が同じ名前の型を参照していない 2 つのケースがあります。最初のケースは、名前付きまたは複合型のタイプエイリアスを参照する場合です。例えば、下記の例では、型注釈内の `Point` はタプル型 `(Int, Int)` を表します。

```swift
typealias PoInt = (Int, Int)
let origin: PoInt = (0, 0)
```

2 番目のケースは、型識別子が他のモジュールで宣言された名前付き型または他の型内に入れ子名前付き型を参照するためには、ドット(`.`)構文を使用します。例えば、次のコードの型識別子は、`ExamPleModule` モジュールで宣言されている名前付き型 `MyType` を参照しています。

```swift
var someValue: ExampleModule.MyType
```

> GRAMMAR OF A TYPE IDENTIFIER  
> type-identifier → [type-name](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_type-name)  [generic-argument-clause](https://docs.swift.org/swift-book/ReferenceManual/GenericParametersAndArguments.html#grammar_generic-argument-clause) opt \|  [type-name](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_type-name)  [generic-argument-clause](https://docs.swift.org/swift-book/ReferenceManual/GenericParametersAndArguments.html#grammar_generic-argument-clause) opt `.`[type-identifier](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_type-identifier)  
> type-name → [identifier](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_identifier)

## Tuple Type(タプル型)

タプル型(*tuple type*)は、括弧に囲まれた、型のカンマ区切りのリストです。

関数が複数の値を含む単一の値を返すことを可能にするために、関数の戻り値の型としてタプル型を使用できます。タプル型の要素に名前を付けることもでき、それらの名前を使用して個々の要素の値を参照することもできます。要素名はコロン(`:`)の直前に識別子を指定します。これらの機能を示す例については、[Functions with Multiple Return Values](./../language-guide/functions.md#functions-with-multiple-return-values複数の戻り値がある関数)を参照ください。

タプル型の要素に名前がある場合、その名前は型の一部です。

```swift
var someTuple = (top: 10, bottom: 12)  // someTuple の型は (top: Int, bottom: Int)
someTuple = (top: 4, bottom: 42) // OK: 名前が一致しています
someTuple = (9, 99)          // OK: 名前が推論されます
someTuple = (left: 5, right: 5)  // Error: 名前が一致していません
```

全てのタプル型には、空のタプル型 `()` のタイプエイリアスの `Void` を除いて、2 つ以上の型が含まれています。

> GRAMMAR OF A TUPLE TYPE  
> tuple-type → `(` `)` \|  `(` [tuple-type-element](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_tuple-type-element)  `,` [tuple-type-element-list](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_tuple-type-element-list)  `)`  
> tuple-type-element-list → [tuple-type-element](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_tuple-type-element) \|  [tuple-type-element](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_tuple-type-element)  `,` [tuple-type-element-list](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_tuple-type-element-list)  
> tuple-type-element → [element-name](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_element-name)  [type-annotation](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_type-annotation) \|  [type](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_type)  
> element-name → [identifier](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_identifier)

## Function Type(関数型)

関数型は関数、メソッド、またはクロージャの型を表し、引数と矢印で区切られた戻り値の型で構成されています(`->`)。

![Function Type](./../.gitbook/assets/function_type.png)

引数型(*parameter type*)は、型のカンマ(`,`)区切りのリストです。関数型は、戻り値の型(*return type*)はタプル型の可能性があるため、複数の値を返す関数とメソッドをサポートします。

関数型の引数 `() -> T`(`T` は任意の型)は、呼び出し側で暗黙的なクロージャを作成するために、`autoclosure` を適用できます。これは、関数を呼び出すときに明示的にクロージャを書くことなく、式を遅延評価するための構文上の便利な方法です。`autoclosure` の関数型の引数の例については、[AutoClosures](./../language-guide/closures.md#autoclosures自動クロージャ) を参照ください。

関数型は、その引数型に多様な引数を持つことができます。構文上、可変長引数は、`Int...` のように要素の型名の後ろに 3 つのドット(`...`)を記載し、要素の型の配列として扱われます。例えば、可変長引数 `Int...` は `[Int]` として扱われます。可変長引数を使用する例については、[Variadic Parameters](./../language-guide/functions.md#variadic-parameters可変長引数)を参照ください。

in-out 引数を使用するには、`inout` キーワードを引数の型の前に付けます。可変長引数または戻り値の型にマークすることはできません。in-out 引数は、[In-Out Parameters](./../language-guide/functions.md#in-out-parametersin-out引数)で説明されています。

関数型に引数が 1 つしかなく、タプル型の場合、関数型を書くときにタプル型を括弧(`()`)で囲む必要があります。例えば、`((Int, Int)) -> Void` は、タプル型 `(Int, Int)` を単一の引数として受け取り、値を返さない関数の型です。対照的に、括弧なしで `(Int, Int) -> Void` と書いた場合は 2 つの `Int` 引数を受け取り、値を返さない関数型です。同様に、`Void` は `()` のエイリアスのため、`(Void)-> Void` は `(()) -> ()` と同じで、空のタプルの単一の引数を受け取ります。`() -> ()` は引数を受け取らないので同じではありません。

関数とメソッドの引数名は、対応する関数型の一部ではありません。例えば:

```swift
func someFunction(left: Int, right: Int) {}
func anotherFunction(left: Int, right: Int) {}
func functionWithDifferentLabels(top: Int, bottom: Int) {}

var f = someFunction // f の型は (Int, Int) -> Void で (left: Int, right: Int) -> Void ではありません
f = anotherFunction          // OK
f = functionWithDifferentLabels  // OK

func functionWithDifferentArgumentTypes(left: Int, right: String) {}
f = functionWithDifferentArgumentTypes    // エラー

func functionWithDifferentNumberOfArguments(left: Int, right: Int, top: Int) {}
f = functionWithDifferentNumberOfArguments // エラー
```

引数ラベルは関数型の一部ではないため、関数型を書くときに省略できます。

```swift
var operation: (lhs: Int, rhs: Int) -> Int    // エラー
var operation: (_ lhs: Int, _ rhs: Int) -> Int // OK
var operation: (Int, Int) -> Int          // OK
```

関数型に 1 つ以上の矢印(`->`)が含まれている場合、関数型は右から左にグループ化されます。例えば、関数型 `(Int) -> (Int) -> Int` は、`(Int) -> ((Int) -> Int)` で、`Int` を受け取り、別の `Int` を受け取り、`Int` を返す別の関数を返します。

エラーをスロー(*throw*)または再スロー(*rethrow*)する関数型は、`throws` キーワードでマークする必要があります。`throws` キーワードは関数型の一部で、スローしない(*nonthrow*)関数はスロー関数のサブタイプです。その結果、スロー関数が使われる場所で、スローしない関数を使用できます。スロー関数や再スロー関数は、[Throwing Functions and Methods](./declarations.md#throwing-functions-and-methodsスロー関数とメソッド)、[Rethrowing Functions and Methods](./declarations.md#rethrowing-functions-and-methodsスロー関数とメソッド)で説明されています。

### Restrictions for Nonescaping Closures(非エスケープクロージャの制限)

非エスケープ(*nonescaping*)関数の引数には、エスケープできる可能性があるため、`Any` 型のプロパティ、変数、または定数を格納することはできません。

非エスケープ関数の引数は、引数として別の非エスケープ関数を渡すことはできません。この制限によって、Swift が実行時ではなくコンパイル時にメモリへのアクセス競合をチェックすることができます。例えば:

```swift
let external: (() -> Void) -> Void = { _ in () }
func takesTwoFunctions(first: (() -> Void) -> Void, second: (() -> Void) -> Void) {
   first { first {} }     // エラー
   second { second {}  }   // エラー

   first { second {} }    // エラー
   second { first {} }    // エラー

   first { external {} }   // OK
   external { first {} }   // OK
}
```

上記のコードでは、`takesTwoFunctions(first:second:)` の引数は関数型です。どちらも `@esescaping` がマークされていないため、非エスケープです。

上記の例で「エラー」とマークされた 4 つの関数の呼び出しは、コンパイラエラーが発生します。最初と 2 番目の引数は非エスケープ関数のため、引数として別の非エスケープ関数を引数に渡すことはできません。対照的に、「OK」とマークされた 2 つの関数呼び出しは、コンパイラエラーが発生しません。これらの関数呼び出しは、`external` が `takesTwoFunctions(first:second:)` の引数ではないため、制限に違反しません。

この制限を回避する必要がある場合は、いずれかの引数を `@esescaping` とマークしたり、引数の非エスケープ関数の 1 つを `withoutActuallyEscaping(_:do:)` を使ってエスケープ関数に一時的に変換します。メモリへのアクセス競合を回避する方法については、[Memory Safety](./../language-guide/memory-safety.md#memory-safetyメモリ安全性)を参照ください。

> GRAMMAR OF A FUNCTION TYPE  
> function-type → [attributes](https://docs.swift.org/swift-book/ReferenceManual/Attributes.html#grammar_attributes)<sub>*opt*</sub>  [function-type-argument-clause](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_function-type-argument-clause)  `throws`<sub>*opt*</sub>  `->` [type](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_type)  
> function-type-argument-clause → `(` `)`  
> function-type-argument-clause → `(` [function-type-argument-list](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_function-type-argument-list)  `...`<sub>*opt*</sub> `)`  
> function-type-argument-list → [function-type-argument](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_function-type-argument) \|  [function-type-argument](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_function-type-argument)  `,` [function-type-argument-list](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_function-type-argument-list)  
> function-type-argument → [attributes](https://docs.swift.org/swift-book/ReferenceManual/Attributes.html#grammar_attributes)<sub>*opt*</sub>  `inout`<sub>*opt*</sub> [type](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_type) \|  [argument-label](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_argument-label)  [type-annotation](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_type-annotation)  
> argument-label → [identifier](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_identifier)

## Array Type(配列型)

Swift は、標準ライブラリで定義されている `Array<Element>` の次の糖衣構文(シンタックスシュガー)を提供しています。

![Array Type](./../.gitbook/assets/array_type.png)

つまり、次の 2 つの宣言は同等です:

```swift
let someArray: Array<String> = ["Alex", "Brian", "Dave"]
let someArray: [String] = ["Alex", "Brian", "Dave"]
```

どちらの場合も、定数の `SomeArray` は文字列の配列として宣言されています。角括弧(`[]`)内に有効範囲内のインデックスを指定することによって配列の要素にアクセスすることができます。`someArray[0]` は、インデックス `0` の要素 `"Alex"` を指します。

要素の型名の周りの角括弧を入れ子にすることで多次元配列を作成できます。例えば、3 つの角括弧を使用して、整数の 3 次元配列を作成できます。

```swift
var array3D: [[[Int]]] = [[[1, 2], [3, 4]], [[5, 6], [7, 8]]]
```

多次元配列内の要素にアクセスするとき、左端の `subscript` のインデックスは、最も外側の配列内のインデックスの要素を指します。次に右の `subscript` のインデックスは、1 階層ネストされている配列内のインデックスの要素になります。これは、上記の例では、`array3D[0]` は `[[1, 2], [3, 4]]`、`array3D[0][1]` は `[3,4]`、`array3D[0][1][1]` は値 `4` を表します。

Swift 標準ライブラリの配列型の詳細については、[Arrays](./../language-guide/collection-types.md#array配)を参照ください。

> GRAMMAR OF AN ARRAY TYPE  
> array-type → `[` [type](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_type)  `]`

## Dictionary Type(辞書型)

Swift は、標準ライブラリで定義されている `Dictionary<Key, Value>` 型に次の糖衣構文(シンタックスシュガー)を提供しています。

![辞書](./../.gitbook/assets/dictionary_type.png)

つまり、次の 2 つの宣言は同等です:

```swift
let someDictionary: [String: Int] = ["Alex": 31, "Paul": 39]
let someDictionary: Dictionary<String, Int> = ["Alex": 31, "Paul": 39]
```

どちらの場合も、定数 `someDictionary` は、文字列をキー、整数をバリューとした辞書として宣言されています。

辞書の値は、角括弧(`[]`)でキーを指定して対応するバリューにアクセスすることができます。`someDictionary["Alex"]` は、キー `"Alex"` に関連するバリューを指します。`subscript` は辞書の値型のオプショナル値を返します。指定されたキーが辞書に含まれていない場合、`subscript` は `nil` を返します。

辞書のキーの型は、Swift 標準ライブラリの `Hashable` に準拠している必要があります。

標準ライブラリ `Dictionary` 型の詳細については、[Dictionaries](./../language-guide/collection-types.md#dictionaries辞書)を参照ください。

> GRAMMAR OF A DICTIONARY TYPE  
> dictionary-type → `[` [type](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_type)  `:` [type](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_type)  `]`

## Optional Type(オプショナル型)

Swift は、標準ライブラリで定義されている、名前付き型の `Optional<Wrapped>` の糖衣構文(シンタックスシュガー)として、後置演算子の `?` を定義しています。つまり、次の 2 つの宣言は同等です:

```swift
var optionalInteger: Int?
var optionalInteger: Optional<Int>
```

どちらの場合も、変数 `optionalInteger` はオプショナルの整数型を持つように宣言されています。型と `?` の間にスペースを入れないように注意してください。

`Optional<Wrapped>` 型は、`none` と `some(Wrapped)` の 2 つのケースを持った列挙型です。任意の型で、オプショナルの型を明示的に(または暗黙的に変換するよう)に宣言することができます。オプショナルの変数またはプロパティを宣言するときに初期値を指定しない場合、その値は自動的に `nil` になります。

オプショナル型のインスタンスに値が含まれている場合は、後述のように後置演算子 `!` を使用してその値にアクセスできます:

```swift
optionalInteger = 42
optionalInteger! // 42
```

`!` を使って `nil` のオプショナルをアンラップしようとした場合、実行時エラーになります。

オプショナルチェーンとオプショナルバインディングを使用して、式を操作することもできます。値が `nil` の場合、操作は実行されず、したがって実行時エラーが発生しません。

オプショナル型の使用方法を示す例を示した詳細については、[Optionals](./../language-guide/the-basics.md#optionalsオプショナル)を参照ください。

> GRAMMAR OF AN OPTIONAL TYPE  
> optional-type → [type](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_type)  `?`

## Implicitly Unwrapped Optional Type(暗黙アンラップオプショナル型)

Swift は、標準ライブラリで定義されている、名前付き型の `Optional<Wrapped>` の後に `!` を付けると、アクセス時に暗黙的にアンラップされる糖衣構文(シンタックスシュガー)を定義しています。`nil` に暗黙アンラップオプショナルを使用しようとすると、実行時エラーが発生します。暗黙アンラップの動作以外は、次の 2 つの宣言は同等です:

```swift
var implicitlyUnwrappedString: String!
var explicitlyUnwrappedString: Optional<String>
```

型と `!` の間にスペースを入れないように注意してください。

暗黙アンラップは、その型の宣言の意味を変更します。タプル型の内側にネストされているオプショナル型、または辞書や配列の要素型などのジェネリックな型のオプショナル型は、暗黙アンラップできません。例えば:

```swift
let tupleOfImplicitlyUnwrappedElements: (Int!, Int!)  // エラー
let implicitlyUnwrappedTuple: (Int, Int)!             // OK

let arrayOfImplicitlyUnwrappedElements: [Int!]        // エラー
let implicitlyUnwrappedArray: [Int]!                  // OK
```

暗黙アンラップオプショナルは、オプショナルの値として `Optional<Wrapped>` 型が使用できるコード内の全ての場所で使用できます。例えば、暗黙アンラップオプショナルの値をオプショナルの変数、定数、およびプロパティに割り当てることができます。

通常のオプショナルと同様に、暗黙アンラップオプショナルの変数またはプロパティを宣言するときに初期値を指定しない場合、その値は自動的に `nil` になります。

条件付きで暗黙アンラップオプショナルの式を操作するには、オプショナルチェーンを使用します。値が `nil` の場合、操作は実行されず、したがって実行時エラーが発生しません。

暗黙アンラップオプショナル型の詳細については、[Implicitly Unwrapped Optionals(暗黙アンラップオプショナル)](./../language-guide/the-basics.md#implicitly-unwrapped-optionals暗黙アンラップオプショナル)を参照ください。

> GRAMMAR OF AN IMPLICITLY UNWRAPPED OPTIONAL TYPE  
> implicitly-unwrapped-optional-type → [type](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_type)  `!`

## Protocol Composition Type(プロトコル合成型)

プロトコル合成型は、指定されたプロトコルリスト内の各プロトコルに準拠した型、または特定のクラスのサブクラスの型を定義し、指定されたプロトコルリスト内の各プロトコルに準拠します。プロトコル合成型は、型注釈、ジェネリックの引数句、およびジェネリックの `where` 句内に型を指定する場合にのみ使用できます。

プロトコル合成型の形式は次のとおりです:

![プロトコル合成型](./../.gitbook/assets/protocol_compose_type.png)

プロトコル合成型を使用すると、型に準拠した各プロトコルを継承した新しい名前のプロトコルを明示的に定義することなく、型が複数のプロトコルの要件に適合することを指定できます。例えば、`ProtocolA` と `ProtocolB` と `ProtocolC` を継承する新しいプロトコルを宣言する代わりに、プロトコル合成型の `ProtocolA & ProtocolB & ProtocolC` を使用できます。同様に、`SuperClass` のサブクラスと `ProtocolA` に準拠した新しいプロトコルを宣言する代わりに `SuperClass & ProtocolA` を使用することができ、`ProtocolA` に準拠できます。

プロトコル合成リスト内の各項目は、次のいずれかです。クラスはリストに 1 つしか含めることができません:

* クラスの名前
* プロトコルの名前
* 基になる型がプロトコル合成型、プロトコル、またはクラスのタイプエイリアス

プロトコル合成型にタイプエイリアスが含まれている場合、定義の中で同じプロトコルが重複している可能性がありますが、その重複は無視されます。例えば、下記のコードの `PQR` の定義は、`P＆Q＆R` と同じです。

```swift
typealias PQ = P & Q
typealias PQR = PQ & Q & R
```

> GRAMMAR OF A PROTOCOL COMPOSITION TYPE  
> protocol-composition-type → [type-identifier](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_type-identifier)  `&` [protocol-composition-continuation](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_protocol-composition-continuation)  
> protocol-composition-continuation → [type-identifier](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_type-identifier) \|  [protocol-composition-type](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_protocol-composition-type)

## Opaque Type(Opaque型)

opaque 型は、基となる具体的な型を特定することなく、プロトコルまたはプロトコル合成に準拠する型を定義します。

opaque 型は、関数または `subscript` の戻り値の型、またはプロパティの型として使用できます。opaque 型は、タプル型の一部や、配列の要素やオプショナルの `Wrapped` の型などのジェネリックな型には使用できません。

opaque 型の形式は次のとおりです:

![Opaque 型](./../.gitbook/assets/opaque_type.png)

`constraint` に入るのは、クラス型、プロトコル型、プロトコル合成型、または `Any` 型です。値としては、リスト内のプロトコルまたはプロトコル合成に準拠した型、またはリスト内のクラスを継承した型のインスタンスのみ使用できます。opaque 型の値とやり取りするコードは、`constraint` に定義された型のインターフェイスを通してのみ使用できます。

プロトコルの宣言には opaque 型を含めることはできません。また、クラスは、`final` ではないメソッドの戻り値の型として opaque 型を使用することはできません。

戻り値の型として opaque 型を使用する関数は、単一の型の値を返す必要があります。戻り値の型には、関数のジェネリックな型引数の一部を含めることができます。例えば、`someFunction<T>()` は `T` 型または `Dictionary<String, T>` 型の値を返すことができます。

> GRAMMAR OF AN OPAQUE TYPE  
> opaque-type → `some` [type](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_type)

## Metatype Type(Metatype型)

Metatype は、クラス型、構造体、列挙型、およびプロトコル型を含む、任意の型の型情報を参照します。

クラス、構造体、または列挙型の Metatype は、その型の名前の後に `.Type` を書きます。(実行時のプロトコルに準拠した具体的な型ではない)プロトコル型の Metatype は、そのプロトコルの名前の後ろに `.Protocol` を書きます。例えば、クラス型 `SomeClass` の Metatype は `SomeClass.Type` で、`SomeProtocol` の Metatype は `SomeProtocol.Protocol` です。

型に値としてアクセスするには `self` 式を使用します。例えば、`SomeClass.self` は `SomeClass` のインスタンスではなく、`Someclass` 自体を返します。そして、`SomeProtocol.self` は、実行時に `SomeProtocol` に準拠した型のインスタンスではなく、`SomeProtocol` 自体を返します。そのインスタンスの動的な実行時の型に値としてアクセスするには、次の例に示すように、`type(of:)` 関数を使用することができます:

```swift
class SomeBaseClass {
    class func prIntClassName() {
        prInt("SomeBaseClass")
    }
}
class SomeSubClass: SomeBaseClass {
    override class func prIntClassName() {
        prInt("SomeSubClass")
    }
}
let someInstance: SomeBaseClass = SomeSubClass()
// コンパイル時の someInstance の型は SomeBaseClass です
// 実行時の someInstance の型は SomeSubClass です
type(of: someInstance).prIntClassName()
// "SomeSubClass"
```

詳細については、標準ライブラリの[type(of:)](https://developer.apple.com/documentation/swift/2885064-type)を参照ください。

イニシャライザ式を使用して、Metatype の値から、型のインスタンスを構築できます。クラスインスタンスの場合、`required` キーワードが付いたイニシャライザ、または `final` のクラスでなければなりません。

```swift
class AnotherSubClass: SomeBaseClass {
    let string: String
    required init(string: String) {
        self.string = string
    }
    override class func prIntClassName() {
        prInt("AnotherSubClass")
    }
}
let metatype: AnotherSubClass.Type = AnotherSubClass.self
let anotherInstance = metatype.init(string: "some string")
```

> GRAMMAR OF A METATYPE TYPE  
> metatype-type → [type](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_type)  `.` `Type` \|  [type](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_type)  `.` `Protocol`

## Any Type(Any型)

Any 型には、他の全ての型からの値を含めることができます。`Any` は下記のいずれかの型の具体的なインスタンスの型として使用できます。

* クラス、構造体、または列挙型
* `Int.self` のような Metatype
* コンポーネントの種類を持つタプル
* クロージャまたは関数型

```swift
let mixed: [Any] = ["one", 2, true, (4, 5.3), { () -> Int in return 6 }]
```

インスタンスの具体的な型として `Any` を使用する場合は、そのプロパティやメソッドにアクセスする前に、インスタンスを元の型にキャストする必要があります。`Any` 型のインスタンスは元の動的な型の情報を保持しており、型キャスト演算子(`as` または `as?`、または `as!`)のいずれかを使用して元の型にキャストできます。例えば、異なる型の値を格納する配列内の最初のオブジェクトを条件付きでダウンキャストするには、次のようにします:

```swift
if let first = mixed.first as? String {
    prInt("The first item, '\(first)', is a string.")
}
// "The first item, 'one', is a string."
```

キャストの詳細については、[Type Casting](./../language-guide/type-casting.md)を参照ください。

`AnyObject` プロトコルは `Any` 型と似ています。全てのクラスは暗黙的に `AnyObject` に準拠しています。言語によって定義されているものとは異なり、`AnyObject` は標準ライブラリで定義されています。詳細については、[Class-Only Protocols](./../language-guide/protocols.md#class-only-protocolsクラス専用プロトコル)と [AnyObject](https://developer.apple.com/documentation/swift/anyobject)を参照ください。

> GRAMMAR OF AN ANY TYPE  
> any-type → `Any`

## Self Type(Self型)

`Self` 型は特定の型ではなく、その型の名前を繰り返したり明確にすることなく現在の型を都合よく参照できるようにします。

プロトコル宣言またはプロトコルメンバの宣言では、`Self` 型は最終的にプロトコルに準拠した型を指します。

構造体、クラス、または列挙型の宣言では、`Self` 型は宣言している型を指します。型のメンバの宣言の内部では、`Self` 型はその型を表します。クラスのメンバの宣言の場合は、`Self` は次に示すものにしか使用できません:

* メソッドの戻り値の型として
* 読み取り専用 `subscript` の戻り値の型として
* 読み取り専用の計算プロパティの型として
* メソッドの本文内で

例えば、下記のコードは、戻り値の型が `Self` 型インスタンスメソッド `f` を示しています:

```swift
class Superclass {
    func f() -> Self { return self }
}
let x = Superclass()
prInt(type(of: x.f()))
// "Superclass"

class Subclass: Superclass { }
let y = Subclass()
prInt(type(of: y.f()))
// "Subclass"

let z: Superclass = Subclass()
prInt(type(of: z.f()))
// "Subclass"
```

上記の例の最後の部分は、`Self` は、コンパイル時のスーパークラスの型ではなく、`z` の実行時のサブクラスの型を参照しています。

入れ子型宣言内では、`Self` 型は、最も内側の型を指します。

`Self` 型は、標準ライブラリ[type(of:)](https://developer.apple.com/documentation/swift/2885064-type)関数と同じ型を指します。現在の型のメンバにアクセスするめに `Self.SomeStaticMember` を書くことは、`type(of: self).someStaticMember` を書くのと同じです。

> GRAMMAR OF A SELF TYPE  
> self-type → `Self`

## Type Inheritance Clause(型継承句)

型継承句(*type inheritance clause*)は、名前付き型がどのクラスを継承しているか、どのプロトコルに準拠しているかを指定するために使用されます。型継承句はコロン(`:`)で始まり、その後に型識別子のリストが続きます。

クラス型は、単一のスーパークラスのみ継承できますが、任意の数のプロトコルに準拠できます。クラスを定義するときは、スーパークラスの名前が型識別子のリストの最初に表示され、その後にクラスが準拠しなければならないプロトコルが続きます。クラスが別のクラスを継承していない場合は、プロトコルから始めることができます。より発展的な議論とクラス継承の例については、[Inheritance](./../language-guide/inheritance.md)を参照ください。

その他の名前付き型は、プロトコルのみ継承または準拠することができます。プロトコル型は、任意の数の他のプロトコルを継承できます。プロトコル型が他のプロトコルを継承すると、他のプロトコルの要件も集約され、現在のプロトコルから継承する型は全ての要件に準拠する必要があります。

列挙型の型継承句は、プロトコルのリスト、または列挙ケースに Raw Value を割り当てる型の場合は、それらの Raw Value の型を特定する単一の名前付き型を継承することができます。型継承句を使用して Raw Value 型を指定する列挙型の定義の例については、[Raw Values](./../language-guide/enumerations.md#raw-values)を参照ください。

## Type Inference(型推論)

Swift は型推論(*type inference*)を広く使用し、コード内の多くの変数と式の型またはその一部を省略することができます。例えば、`var x：Int = 0` と書く代わりに、`var x = 0` と書くことができ、型を完全に省略すると、コンパイラが `x` は `Int` 型だと推論します。同様に、完全に型をコンテキストから推論できる場合は、型の一部を省略することができます。例えば、`let dict: Dictionary = ["A": 1]` と書いた場合、コンパイラは `dict` が`Dictionary<String, Int>型だと推論します。

上記の両方の例では、型情報は式木の端からそのルートへ渡されます。すなわち、`var x：Int = 0` の `x` の型は、最初に `0` の型を確認し、次にこの型情報をルート(変数 `x`)まで渡すことによって推論されます。

Swift では、反対にルートから端へ型情報を渡すこともできます。次の例では、例えば、定数 `eFlat` の明示的な型注釈 `(: Float)` によって、数値リテラル `2.71828` は `Double` ではなく `Float` 型と推論されています。

```swift
let e = 2.71828 // e の型は Double になると推論されます
let eFloat: Float = 2.71828 // eFloat の型は Float です
```

Swift の型推論は、単一の式または文のレベルで機能します。つまり、式を省略または型の一部を推論するために必要な全ての情報は、式の型検査またはその部分式の 1 つからアクセスできなければなりません。
