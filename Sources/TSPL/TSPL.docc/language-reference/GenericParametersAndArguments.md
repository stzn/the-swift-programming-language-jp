# ジェネリックパラメータと引数\(Generic Parameters and Arguments\)

<!--最終更新日: 2022/12/3  -->
<!--原文: https://docs.swift.org/swift-book/ReferenceManual/GenericParametersAndArguments.html -->

宣言を一般化して具体的な型を抽象化する。

この章では、ジェネリック型、関数、およびイニシャライザのパラメータと引数について説明します。ジェネリック型、関数、サブスクリプト、またはイニシャライザを宣言すると、ジェネリック型、関数、またはイニシャライザで使用できる型パラメータを指定します。これらの型パラメータは、ジェネリック型のインスタンスが作成されたとき、またはジェネリック関数またはイニシャライザが呼び出されたときに、実際の具象型の引数に置き換えられるプレースホルダとして機能します。

Swift のジェネリクスの概要については、[Generics\(ジェネリクス\)](../language-guide/generics)を参照ください。

## ジェネリックパラメータ句\(Generic Parameter Clause\)

ジェネリックパラメータ句は、ジェネリック型または関数の型パラメータを指定し、それらのパラメータに関連する制約や要件とともに指定します。ジェネリックパラメータ句は、山括弧\(`<>`\)で囲まれており、形式は次のとおりです。

```swift
<<#generic parameter list#>>
```

_generic parameter list_ は、ジェネリックパラメータのカンマ区切りのリストで、その各形式は次のとおりです。

```swift
<#type parameter#>: <#constraint#>
```

_type parameter_ の後に任意の _constraint_ を付けて構成します。_type parameter_ は、単にプレースホルダ型の名前です\(例えば、`T`、`U`、`V`、`Key`、`Value` など\)。関数またはイニシャライザのシグネチャを含む型や関数の残りの部分から型パラメータ\(およびその関連型の全ての型\)にアクセスできます。

_constraint_ は、型パラメータが特定のクラスを継承するか、プロトコルまたはプロトコル合成に準拠することを指定します。例えば、下記のジェネリック関数では、ジェネリックパラメータ `T: Comparable` は、型パラメータ `T` に代入された任意の型パラメータが `Comparable` プロトコルに準拠しなければならないことを示しています。

```swift
func simpleMax<T: Comparable>(_ x: T, _ y: T) -> T {
    if x < y {
        return y
    }
    return x
}
```

例えば、`Int` と `Double` は、どちらも `Comparable` プロトコルに準拠しているため、この関数はどちらの型の引数も受け入れます。ジェネリック型とは対照的に、ジェネリック関数やイニシャライザを使用するときはジェネリックパラメータ句を指定しません。型引数は、関数またはイニシャライザに渡された引数の型から推論されます。

```swift
simpleMax(17, 42) // T は Int に推論されます
simpleMax(3.14159, 2.71828) // T は Double に推論されます
```

### ジェネリック where 句\(Generic Where Clauses\)

型または関数の本文の開始中括弧\(`{`\)の直前にジェネリック `where` 句を含むことによって、型パラメータとその関連型に追加要件を指定できます。ジェネリック `where` 句は、`where` キーワードで構成され、その後に 1 つ以上の要件のカンマ区切りのリストが続きます。

```swift
where <#requirements#>
```

ジェネリック `where` 句の _requirements_ は、型パラメータがクラスを継承するか、プロトコルまたはプロトコル合成に準拠することを指定します。ジェネリック `where` 句は型パラメータのシンプルな制約を表現するための糖衣構文\(シンタックスシュガー\)を提供しますが\(例えば、`<T: Comparable>` は `<T> where T: Comparable` などと同等\)、それを使用して複雑な制約を型パラメータや関連型に提供することもできます。例えば、プロトコルに準拠するために、関連型の型パラメータを制限することができます。`<S: Sequence> where S.Iterator.Element: Equatable` は、`S` が `Sequence` プロトコルに準拠し、関連型 `S.Iterator.Element` が `Equatable` プロトコルに準拠することを指定します。この制約は、シーケンスの各要素が `Equatable` だということを保証します。

`==` 演算子を使用して、2 つの型が同じという要件を指定することもできます。例えば、`<S1: Sequence, S2: Sequence> where S1.Iterator.Element == S2.Iterator.Element` は `S1` と `S2` が `Sequence` プロトコルに準拠し、両方のシーケンスの要素が同じ型でなければならない制約を表現します。

型パラメータに置き換えられる任意の型引数は、型パラメータの全て制約と要件を満たす必要があります。

ジェネリック `where` 句は、型パラメータを含む宣言の一部として、または型パラメータを含む宣言の中にネストされている宣言の一部として使用することができます。ネストされた宣言のジェネリック `where` 句は、その宣言をラップしている宣言の型パラメータを参照できます。ただし、その要件は、`where` 句が書かれている宣言にのみ適用されます。

ラップされている宣言にも `where` 句がある場合、両方の句の要件が組み合わされます。下記の例では、`startSwithZero()` は、`Element` が `SomeProtocol` と `Numeric` の両方に準拠している場合にのみ使用できます。

```swift
extension Collection where Element: SomeProtocol {
    func startsWithZero() -> Bool where Element: Numeric {
        return first == .zero
    }
}
```

異なる制約、要件、またはその両方を提供することによって、ジェネリック関数またはイニシャライザをオーバーロードすることができます。オーバーロードされたジェネリック関数またはイニシャライザを呼び出すと、コンパイラはこれらの制約を使用して、どのオーバーロードされた関数またはイニシャライザを呼び出すかを決めます。

ジェネリック `where` 句の詳しい情報とジェネリック関数宣言の例は、[Generic Where Clauses\(ジェネリック where 句\)](../language-guide/generics.md#generic-where-clausesジェネリックwhere句)を参照ください。

> GRAMMAR OF A GENERIC PARAMETER CLAUSE  
> generic-parameter-clause → `<` [generic-parameter-list](https://docs.swift.org/swift-book/ReferenceManual/GenericParametersAndArguments.html#grammar_generic-parameter-list) `>`  
> generic-parameter-list → [generic-parameter](https://docs.swift.org/swift-book/ReferenceManual/GenericParametersAndArguments.html#grammar_generic-parameter) \| [generic-parameter](https://docs.swift.org/swift-book/ReferenceManual/GenericParametersAndArguments.html#grammar_generic-parameter) `,` [generic-parameter-list](https://docs.swift.org/swift-book/ReferenceManual/GenericParametersAndArguments.html#grammar_generic-parameter-list)  
> generic-parameter → [type-name](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_type-name)  
> generic-parameter → [type-name](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_type-name) `:` [type-identifier](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_type-identifier)  
> generic-parameter → [type-name](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_type-name) `:` [protocol-composition-type](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_protocol-composition-type)  
> generic-where-clause → `where` [requirement-list](https://docs.swift.org/swift-book/ReferenceManual/GenericParametersAndArguments.html#grammar_requirement-list)  
> requirement-list → [requirement](https://docs.swift.org/swift-book/ReferenceManual/GenericParametersAndArguments.html#grammar_requirement) \| [requirement](https://docs.swift.org/swift-book/ReferenceManual/GenericParametersAndArguments.html#grammar_requirement) `,` [requirement-list](https://docs.swift.org/swift-book/ReferenceManual/GenericParametersAndArguments.html#grammar_requirement-list)  
> requirement → [conformance-requirement](https://docs.swift.org/swift-book/ReferenceManual/GenericParametersAndArguments.html#grammar_conformance-requirement) \| [same-type-requirement](https://docs.swift.org/swift-book/ReferenceManual/GenericParametersAndArguments.html#grammar_same-type-requirement)  
> conformance-requirement → [type-identifier](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_type-identifier) `:` [type-identifier](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_type-identifier)  
> conformance-requirement → [type-identifier](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_type-identifier) `:` [protocol-composition-type](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_protocol-composition-type)  
> same-type-requirement → [type-identifier](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_type-identifier) `==` [type](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_type)

## ジェネリック引数句\(Generic Argument Clause\)

_ジェネリック引数句_は、ジェネリック型の型引数を指定します。ジェネリック引数句は山括弧\(`<>`\)で囲まれており、形式は次のとおりです。

```swift
<<#generic argument list#>>
```

_generic argument list_ は、型パラメータのカンマ区切りのリストです。_型引数_は、ジェネリック型のジェネリックパラメータ句に対応する型パラメータを置き換えた実際の具象型の名前です。つまり、そのジェネリック型の具象型バージョンです。下記の例は、Swift 標準ライブラリのジェネリックな辞書型のシンプルなバージョンを示しています。

```swift
struct Dictionary<Key: Hashable, Value>: Collection, ExpressibleByDictionaryLiteral {
    /* ... */
}
```

ジェネリック `Dictionary` 型の具象型バージョン、`Dictionary<String, Int>` は、ジェネリックパラメータ `Key: Hashable` と `Value` を具象型の `String` と `Int` に置き換えることで形成されます。各型引数は、それが置き換えるジェネリックパラメータの全ての制約を満たす必要があります。上記の例では、`Key` 型パラメータは `Hashable` プロトコルに準拠するように制約され、したがって `String` も `Hashable` プロトコルに準拠していなければなりません。

\(適切な制約と要件を満たす場合\)ジェネリック型の具象型バージョンで、型パラメータを型引数で置き換えることもできます。例えば、`Array<Element>` の型パラメータ `Element` を配列 `Array<Int>` に置き換えることができ、要素が整数の配列を形成することができます。

```swift
let arrayOfArrays: Array<Array<Int>> = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
```

[Generic Parameter Clause\(ジェネリックパラメータ句\)](generic-parameters-and-arguments.md#generic-parameter-clauseジェネリックパラメータ句)で述べたように、ジェネリック関数またはイニシャライザの型引数を指定する場合には、ジェネリック引数句を使用しません。

> GRAMMAR OF A GENERIC ARGUMENT CLAUSE  
> generic-argument-clause → `<` [generic-argument-list](https://docs.swift.org/swift-book/ReferenceManual/GenericParametersAndArguments.html#grammar_generic-argument-list) `>`  
> generic-argument-list → [generic-argument](https://docs.swift.org/swift-book/ReferenceManual/GenericParametersAndArguments.html#grammar_generic-argument) \| [generic-argument](https://docs.swift.org/swift-book/ReferenceManual/GenericParametersAndArguments.html#grammar_generic-argument) `,` [generic-argument-list](https://docs.swift.org/swift-book/ReferenceManual/GenericParametersAndArguments.html#grammar_generic-argument-list)  
> generic-argument → [type](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_type)

