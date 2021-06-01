# Protocols

最終更新日:

プロトコル(*protocol*)は、特定のタスクまたは機能に適合するメソッド、プロパティ、およびその他の要件の設計図を定義します。プロトコルは、クラス、構造体、または列挙型で準拠でき、それらの要件の実装を提供します。プロトコルの要件を満たす全ての型は、そのプロトコルに準拠しているといいます。

準拠する型が実装する必要がある要件を指定することに加えて、プロトコルを拡張して、これらの要件の一部を実装したり、準拠する型が利用できる追加機能を実装したりできます。

## Protocol Syntax(プロトコル構文)

クラス、構造体、および列挙型と非常によく似た方法でプロトコルを定義します:

```swift
protocol SomeProtocol {
    // プロトコルの定義をここに
}
```

カスタム型は、定義の一部として、型名の後にプロトコル名をコロン(`:`)で区切って配置することにより、特定のプロトコルに準拠することを示します。 複数のプロトコルをリストでき、コンマで区切ることができます:

```swift
struct SomeStructure: FirstProtocol, AnotherProtocol {
    // 構造体の定義をここに
}
```

クラスにスーパークラスがある場合は、準拠するプロトコルの前にスーパークラス名をリストし、その後にコンマを続けます:

```swift
class SomeClass: SomeSuperclass, FirstProtocol, AnotherProtocol {
    // クラスの定義をここに
}
```

## Property Requirements(プロトコル要件)

## Method Requirements(メソッド要件)

## Mutating Method Requirements(mutatingメソッド要件)

## Initializer Requirements(イニシャライザ要件)

### Class Implementations of Protocol Initializer Requirements(プロトコルイニシャライザ要件のクラス実装)

### Failable Initializer Requirements(失敗可能イニシャライザ要件)

## Protocols as Types(型としてのプロトコル)

## Delegation(委譲)

## Adding Protocol Conformance with an Extension(拡張機能へのプロトコル準拠の追加)

### Conditionally Conforming to a Protocol(条件付きプロトコル準拠)

### Declaring Protocol Adoption with an Extension(拡張機能を使ったプロトコル準拠の宣言)

## Adopting a Protocol Using a Synthesized Implementation(同期的な実装を使用したプロトコル準拠)

## Collections of Protocol Types(プロトコル型のコレクション)

## Protocol Inheritance(プロトコル継承)

## Class-Only Protocols(クラスのみのプロトコル)

## Protocol Composition(プロトコルの組み合わせ)

## Checking for Protocol Conformance(プロトコル準拠チェック)

## Optional Protocol Requirements(オプショナルのプロトコル要件)
