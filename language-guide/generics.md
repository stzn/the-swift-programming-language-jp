# Generics

最終更新日:

ジェネリックなコードを使用すると、定義した要件に従って、任意の型で機能する柔軟で再利用可能な関数と型を作成できます。重複を避け、その意図を明確で抽象的な方法で表現するコードを書くことができます。

ジェネリクス(*Generics*)は Swift の最も強力な機能の 1 つで、Swift 標準ライブラリの多くはジェネリック コードで構築されています。気づかないかもしれませんが、この言語ガイド全体でもジェネリクスを使用しています。例えば、Swift の `Array` 型と `Dictionary` 型はどちらもジェネリックコレクションです。`Int` 値を保持する配列、`String` 値を保持する配列、または Swift で作成できる他のどんな型の配列を作成できます。同様に、指定された型の値を格納する辞書を作成でき、その型に制限はありません。

## The Problem That Generics Solve(ジェネリックが解決する問題)

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
