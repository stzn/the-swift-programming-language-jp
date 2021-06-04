# Generics

最終更新日:

ジェネリックなコードを使用すると、定義した要件に従って、任意の型で機能する柔軟で再利用可能な関数と型を作成できます。重複を避け、その意図を明確で抽象的な方法で表現するコードを書くことができます。

ジェネリクス(*Generics*)は Swift の最も強力な機能の 1 つで、Swift 標準ライブラリの多くはジェネリック コードで構築されています。気づかないかもしれませんが、この言語ガイド全体でもジェネリクスを使用しています。例えば、Swift の `Array` 型と `Dictionary` 型はどちらもジェネリックコレクションです。`Int` 値を保持する配列、`String` 値を保持する配列、または Swift で作成できる他のどんな型の配列を作成できます。同様に、指定された型の値を格納する辞書を作成でき、その型に制限はありません。

## The Problem That Generics Solve(ジェネリクスが解決する問題)

これは `swapTwoInts(_:_:)` と呼ばれる標準の非ジェネリック関数で、2 つの `Int` 値を交換します:

```swift
func swapTwoInts(_ a: inout Int, _ b: inout Int) {
    let temporaryA = a
    a = b
    b = temporaryA
}
```

この関数は、[In-Out Parameters](./functions.md#in-Out-ParametersIn-Out引数)で説明されているように、inout 引数を使用して `a` と `b` の値を交換します。

`swapTwoInts(_:_:)` 関数は、`b` の元の値を `a` に、`a` の元の値を `b` に入れ替えます。この関数を呼び出して、2 つの `Int` 変数の値を交換できます:

```swift
var someInt = 3
var anotherInt = 107
swapTwoInts(&someInt, &anotherInt)
print("someInt is now \(someInt), and anotherInt is now \(anotherInt)")
// "someInt is now 107, and anotherInt is now 3"
```

`swapTwoInts(_:_:)` 関数は便利ですが、`Int` 値でのみ使用できます。2 つの `String` 値または 2 つの `Double` 値を交換する場合は、以下に示す `swapTwoStrings(_:_:)` および `swapTwoDoubles(_:_:)` 関数などの関数をさらに作成する必要があります:

```swift
func swapTwoStrings(_ a: inout String, _ b: inout String) {
    let temporaryA = a
    a = b
    b = temporaryA
}

func swapTwoDoubles(_ a: inout Double, _ b: inout Double) {
    let temporaryA = a
    a = b
    b = temporaryA
}
```

`swapTwoInts(_:_:)`、`swapTwoStrings(_:_:)`、および `swapTwoDoubles(_:_:)` 関数の本体が同一なことに気付いたかもしれません。唯一の違いは、受け入れられる値の型(`Int`、`String`、および `Double`)です。

任意の型の 2 つの値を交換する 1 つの関数を作成する方が、より便利で、はるかに柔軟です。ジェネリックコードを使用すると、そのような関数を作成できます。(これらの関数の一般的なバージョンは、下記で定義されています)

> NOTE  
> 3 つの関数すべてで、`a` と `b` の型は同じでなければなりません。`a` と `b` が同じ型でない場合、それらの値を交換することはできません。Swift は型安全な言語であり、(例えば)`String` 型の変数と `Double` 型の変数が互いに値を交換することを許可しません。そうしようとすると、コンパイルエラーが発生します。

## Generic Functions(ジェネリック関数)

## Type Parameters(型引数)

## Naming Type Parameters(型引数の命名)

## Generic Types(ジェネリック型)

## Extending a Generic Type(ジェネリック型の拡張)

## Type Constraints(型制約)

### Type Constraint Syntax(型制約構文)

### Type Constraints in Action(型制約の挙動)

## Associated Types(関連型)

### Associated Types in Action(関連型の挙動)

### Extending an Existing Type to Specify an Associated Type(関連型を特定するための既存の型の拡張)

### Adding Constraints to an Associated Type(関連型への制約の追加)

### Using a Protocol in Its Associated Type’s Constraints(関連型に制約へのプロトコルの使用)

## Generic Where Clauses(ジェネリックWhere句)

## Extensions with a Generic Where Clause(ジェネリックWhere句を使った拡張)

## Contextual Where Clauses(コンテキストのWhere句)

## Associated Types with a Generic Where Clause(ジェネリックWhere句を使用した関連型)

## Generic Subscripts(ジェネリックsubscript)
