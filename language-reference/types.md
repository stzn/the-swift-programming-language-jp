# Types \(型\)

最終更新日:

Swift では、名前付き型(*named　type*)と複合型(*compound type*)の 2 種類があります。名前付き型は、定義されているときに特定の名前を指定できる型です。名前付き型には、クラス、構造体、列挙型、およびプロトコルが含まれます。例えば、`MyClass` という名前のユーザ定義クラスのインスタンスは `MyClass` 型です。ユーザ定義の型に加えて、Swift 標準ライブラリは、配列、辞書、およびオプショナルの値を含む多くの一般的に使用されている名前の型を定義します。

通常、数字、文字、文字列を表す型など、他の言語で基本的またはプリミティブと見なされるデータ型は、Swift 標準ライブラリでは構造体を使用して、定義され、実装されている名前付き型です。名前付き型のため、[Extensions](./expressions.md)、 [Extension Declaration](./declarations.md#extension-declaration拡張宣言)で説明されている拡張宣言を使用して、プログラムのニーズに合わせて振る舞いを拡張することができます。

複合型は、Swift 言語自体で定義されている名前のない型です。関数型とタプル型の 2 つの複合型があります。複合型の種類は、名前付き型および他の複合型を含んでいることがあります。例えば、タプル型の `(Int, (Int, Int))` には、名前付き型の `Int` と複合型の(Int, Int)2 つの要素が含まれています。

名前付き型または複合型の周囲に括弧(`()`)を付けることができます。ただし、型の周囲に括弧を必ずしも追加する必要はありません。例えば、`(Int)` は `Int` に相当します。

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

## Type Annotation(型アノテーション)

型アノテーションは、変数または式の型を明示的に指定します。次の例に示すように、型アノテーションはコロン(`:`)で始まり、型で終わります。

```swift
let someTuple: (Double, Double) = (3.14159, 2.71828)
func someFunction(a: Int) { /* ... */ }
```

最初の例では、表現がタプル型 `(Double, Double)` を持つように指定されています。2 番目の例では、`someFunction` 関数の引数 `a` が `Int` 型なことが指定されています。

型アノテーションは、型の前に型属性のオプショナルのリストを含めることができます。

> GRAMMAR OF A TYPE ANNOTATION  
> type-annotation → `:` [attributes](https://docs.swift.org/swift-book/ReferenceManual/Attributes.html#grammar_attributes)<sub>*opt*</sub> **inout**<sub>*opt*</sub> [type](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_type)

## Type Identifier(型識別子)

型識別子とは、名前付き型または名前付き型または複合型の型別名を参照します。

ほとんどの場合、型識別子は識別子と同じ名前の名前付き型を直接参照します。例えば、`Int` は、名前付き型 `Int` を直接参照する型識別子で、型識別子 `Dictionary<String, Int>` は直接指定された `Dictionary<String, Int>` を参照します。

型識別子が同じ名前の型を参照していない 2 つのケースがあります。最初のケースは、型識別子は、名前付きまたは複合型のエイリアスを参照する場合です。例えば、下記の例では、型アノテーション内の `Point` はタプル型 `(Int, Int)` を表します。

```swift
typealias Point = (Int, Int)
let origin: Point = (0, 0)
```

2 番目のケースは、型識別子が他のモジュールで宣言された名前付き型または他の型内にネストされた名前の型を参照するためにドット(`.`)構文を使用します。例えば、次のコードの型識別子は、`ExamPleModule` モジュールで宣言されている名前付き型 `MyType` を参照しています。

```swift
var someValue: ExampleModule.MyType
```

> GRAMMAR OF A TYPE IDENTIFIER  
> type-identifier → [type-name](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_type-name)  [generic-argument-clause](https://docs.swift.org/swift-book/ReferenceManual/GenericParametersAndArguments.html#grammar_generic-argument-clause) opt \|  [type-name](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_type-name)  [generic-argument-clause](https://docs.swift.org/swift-book/ReferenceManual/GenericParametersAndArguments.html#grammar_generic-argument-clause) opt `.`[type-identifier](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_type-identifier)  
> type-name → [identifier](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_identifier)

## Tuple Type(タプル型)

タプル型(*tuple type*)は、括弧に囲まれた、型のカンマ区切りのリストです。

関数が複数の値を含む単一のタプルを返すことを可能にするために、関数の戻り値の型としてタプル型を使用できます。タプル型の要素に名前を付けることもでき、それらの名前を使用して個々の要素の値を参照することもできます。要素名はコロン(`:`)の直前に識別子を指定します。これらの機能の両方を示す例については、[Functions with Multiple Return Values](./../language-guide/functions.md#functions-with-multiple-return-values複数の戻り値がある関数)を参照ください。

タプル型の要素に名前がある場合、その名前は型の一部です。

```swift
var someTuple = (top: 10, bottom: 12)  // someTuple の型は (top: Int, bottom: Int)
someTuple = (top: 4, bottom: 42) // OK: 名前が一致しています
someTuple = (9, 99)              // OK: 名前が推論されます
someTuple = (left: 5, right: 5)  // Error: 名前が一致していません
```

全てのタプル型には、空のタプル型 `()` の型エイリアスの `Void` を除いて、2 つ以上の型が含まれています。

> GRAMMAR OF A TUPLE TYPE  
> tuple-type → `(` `)` \|  `(` [tuple-type-element](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_tuple-type-element)  `,` [tuple-type-element-list](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_tuple-type-element-list)  `)`   
> tuple-type-element-list → [tuple-type-element](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_tuple-type-element) \|  [tuple-type-element](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_tuple-type-element)  `,` [tuple-type-element-list](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_tuple-type-element-list)  
> tuple-type-element → [element-name](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_element-name)  [type-annotation](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_type-annotation) \|  [type](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_type)  
> element-name → [identifier](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_identifier)

## Function Type

### Restrictions for Nonescaping Closures

## Array Type

## Dictionary Type

## Optional Type

## Implicitly Unwrapped Optional Type

## Protocol Composition Type

## Opaque Type

## Meta*type* Type

## Any Type

## Self Type

## Type Inheritance Clause

## Type Inference
