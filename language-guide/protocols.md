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

カスタム型は、定義の一部として、型名の後にプロトコル名をコロン(`:`)で区切って配置することにより、特定のプロトコルに準拠することを示します。複数のプロトコルをリストでき、コンマで区切ることができます:

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

## Property Requirements(プロパティ要件)

プロトコルは、特定の名前と型のインスタンスプロパティまたは型プロパティに準拠することを要件にできます。プロトコルは、プロパティが格納プロパティか計算プロパティかを指定しません。必要なプロパティの名前と型を指定するだけです。プロトコルは、各プロパティがゲッタのみか、ゲッタセッタどちらも必要かどうかも指定します。

プロトコルがプロパティにゲッタセッタを要求する場合、そのプロパティ要件は、定数格納プロパティまたは読み取り専用の計算プロパティでは満たされません。プロトコルがゲッタのみを要求する場合でも、その要件はあらゆる種類のプロパティによって満たされ、必要ならば実装の方でプロパティのセッタを定義していても要件を満たします。

プロパティ要件は常に変数プロパティとして宣言され、前に `var` キーワードが付きます。ゲッタセッタプロパティは型宣言の後に `{ get set }` を記述することで示し、ゲッタプロパティは `{ get }` を記述することで示します。

```swift
protocol SomeProtocol {
    var mustBeSettable: Int { get set }
    var doesNotNeedToBeSettable: Int { get }
}
```

プロトコルで、型プロパティの要件の前には、必ず `static` キーワードを付けてください。このルールは、クラスによって実装されるときに、型プロパティに `class` または `static` キーワードをプレフィックスを付けても要件を満たします:

```swift
protocol AnotherProtocol {
    static var someTypeProperty: Int { get set }
}
```

単一のインスタンスプロパティ要件を持つプロトコルの例を次に示します:

```swift
protocol FullyNamed {
    var fullName: String { get }
}
```

`FullyNamed` プロトコルでは、完全修飾名を提供するための準拠する型が必要です。プロトコルは、準拠する型の性質について他に何も指定していません。型がそれ自体の完全な名前を提供できる必要があることを指定するだけです。プロトコルでは、`FullyNamed` 型には、`String` 型の `fullName` というゲッタインスタンスプロパティが宣言されています。

`FullyNamed` プロトコルに準拠するシンプルな構造体の例を次に示します:

```swift
struct Person: FullyNamed {
    var fullName: String
}
let john = Person(fullName: "John Appleseed")
// john.fullName は "John Appleseed"
```

この例では、特定の名前付き人物を表す `Person` という構造体を定義しています。その定義の最初の行の一部として `FullyNamed` プロトコルに準拠すると述べています。

`Person` の各インスタンスには、`String` 型の `fullName` という 1 つの格納プロパティがあります。これは、`FullyNamed` プロトコルの単一の要件と一致し、`Person` がプロトコルに正しく準拠していることを意味します。(プロトコル要件が満たされていない場合、Swift はコンパイルエラーが発生します)

これも `FullyNamed` プロトコルに準拠する、より複雑なクラスです:

```swift
class Starship: FullyNamed {
    var prefix: String?
    var name: String
    init(name: String, prefix: String? = nil) {
        self.name = name
        self.prefix = prefix
    }
    var fullName: String {
        return (prefix != nil ? prefix! + " " : "") + name
    }
}
var ncc1701 = Starship(name: "Enterprise", prefix: "USS")
// ncc1701.fullName は "USS Enterprise"
```

このクラスは、宇宙船の計算された読み取り専用プロパティとして `fullName` プロパティを実装します。各 `Starship` クラスのインスタンスは、必須の `name` とオプショナルの `prefix` を格納します。`fullName` プロパティは、プレフィックス値が存在する場合はそれを使用し、名前の先頭に追加して宇宙船のフルネームを作成します。

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
