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

プロトコルは、特定の名前と型のインスタンスプロパティまたは型プロパティに準拠することを要件にできます。プロトコルは、プロパティが格納プロパティか計算プロパティかを指定しません。必要なプロパティの名前と型を指定するだけです。プロトコルは、各プロパティが取得のみか、取得可能、設定可能どちらも必要かどうかも指定します。

プロトコルがプロパティに取得可能+設定可能を要求する場合、そのプロパティ要件は、定数格納プロパティまたは読み取り専用の計算プロパティでは満たされません。プロトコルが取得可能のみを要求する場合でも、その要件はあらゆる種類のプロパティによって満たされ、必要ならば実装の方でプロパティが設定可能でも要件を満たします。

プロパティ要件は常に変数プロパティとして宣言され、前に `var` キーワードが付きます。取得可能+設定可能プロパティは型宣言の後に `{ get set }` を記述することで示し、取得可能プロパティは `{ get }` を記述することで示します。

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

`FullyNamed` プロトコルでは、完全修飾名を提供するための準拠する型が必要です。プロトコルは、準拠する型の性質について他に何も指定していません。型がそれ自体の完全な名前を提供できる必要があることを指定するだけです。プロトコルでは、`FullyNamed` 型には、`String` 型の `fullName` という取得可能インスタンスプロパティが宣言されています。

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

プロトコルでは、特定のインスタンスメソッドと型に準拠することで型メソッドの実装が必要になる場合があります。これらのメソッドは、通常のインスタンスおよび型メソッドとまったく同じ方法でプロトコルの定義の一部として記述されますが、中括弧(`{}`)やメソッド本文はありません。通常のメソッドと同じ規則に従って、可変個引数を使用できます。ただし、プロトコルの定義内のメソッドの引数にデフォルト値を指定することはできません。

型プロパティの要件と同様に、プロトコルで定義するときは、常に型メソッドの要件の前に `static` キーワードを付けます。これは、クラスによって実装されるときに、型メソッドの要件に `class` または `static` キーワードが付いている場合にも当てはまります:

```swift
protocol SomeProtocol {
    static func someTypeMethod()
}
```

次の例では、単一のインスタンスメソッド要件を持つプロトコルを定義しています:

```swift
protocol RandomNumberGenerator {
    func random() -> Double
}
```

このプロトコル `RandomNumberGenerator` では、準拠するには `random` と呼ばれるインスタンスメソッドが必要です。このインスタンスメソッドは、呼び出されるたびに `Double` 値を返します。プロトコルの一部として指定されていませんが、この値は `0.0` から `1.0`(ただし、`1.0` を含まない) までの数値が想定されています。

`RandomNumberGenerator` プロトコルは、各乱数がどのように生成されるかについて何も指定していません。単に、ジェネレータが新しい乱数を生成する標準的な方法を提供することを要求するだけです。

下記は、`RandomNumberGenerator` プロトコルに準拠するクラスの実装です。このクラスは、線形合同法発生器として知られる疑似乱数発生器アルゴリズムを実装します:

```swift
class LinearCongruentialGenerator: RandomNumberGenerator {
    var lastRandom = 42.0
    let m = 139968.0
    let a = 3877.0
    let c = 29573.0
    func random() -> Double {
        lastRandom = ((lastRandom * a + c)
            .truncatingRemainder(dividingBy:m))
        return lastRandom / m
    }
}
let generator = LinearCongruentialGenerator()
print("Here's a random number: \(generator.random())")
// "Here's a random number: 0.3746499199817101"
print("And another one: \(generator.random())")
// "And another one: 0.729023776863283"
```

## Mutating Method Requirements(mutatingメソッド要件)

メソッドが属するインスタンスを変更する必要がある場合があります。値型 (つまり、構造体と列挙型) のインスタンスメソッドの場合、メソッドの `func` キーワードの前に `mutating` キーワードを配置して、メソッドが属するインスタンスとそのインスタンスの全てのプロパティを変更できることを示します。このプロセスについては、[Modifying Value Types from Within Instance Methods(インスタンスメソッド内からの値型の変更)](./methods.md#modifying-value-types-from-within-instance-methodsインスタンスメソッド内からの値型の変更)で説明されています。

任意の型のインスタンスを変更することを目的としたプロトコルのインスタンスメソッド要件を定義する場合は、プロトコルの定義の一部として `mutating` キーワードを使用してメソッドをマークします。これにより、構造体と列挙型がそのメソッド要件を満たしてプロトコルに準拠することができます。

> NOTE  
> プロトコルのインスタンスメソッドの要件を変更としてマークする場合、クラスがそのメソッドを実装するときに、`mutating` キーワードを記述する必要はありません。`mutating` キーワードは、構造体と列挙型によってのみ使用されます。

下記の例では、`Togglable` というプロトコルが定義されています。これは、`toggle` という単一のインスタンスメソッド要件を定義しています。その名前が示すように、`toggle()` メソッドは、通常、その型のプロパティを変更することによって、準拠する型の状態をトグルまたは反転することを目的としています。

`toggle()` メソッドは、`Togglable` プロトコル定義の一部として `mutating` キーワードがマークされており、メソッドが呼び出されたときに準拠するインスタンスの状態を変更される可能性を示しています。

構造体または列挙型に対して `Togglable` プロトコルを実装する場合、その構造体または列挙型は、`mutating` がマークされている `toggle()` メソッドの実装を提供することにより、プロトコルに準拠できます。

下記の例では、`OnOffSwitch` という列挙型を定義しています。この列挙型は、`on` と `off` の 2 つのケースによって示される 2 つの状態を切り替えます。列挙型の `toggle` 実装は、`Togglable` プロトコルの要件に一致するように、`mutating` としてマークされています:

```swift
enum OnOffSwitch: Togglable {
    case off, on
    mutating func toggle() {
        switch self {
        case .off:
            self = .on
        case .on:
            self = .off
        }
    }
}
var lightSwitch = OnOffSwitch.off
lightSwitch.toggle()
// lightSwitch は .on
```

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
