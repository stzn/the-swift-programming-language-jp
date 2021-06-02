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

プロパティ要件は常に変数プロパティとして宣言され、前に `var` キーワードが付きます。取得可能+set プロパティは型宣言の後に `{ get set }` を記述することで示し、get プロパティは `{ get }` を記述することで示します。

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

プロトコルでは、型に準拠することで特定のイニシャライザを実装が必要な場合があります。これらのイニシャライザは、通常のイニシャライザとまったく同じ方法でプロトコルの定義の一部として記述しますが、中括弧(`{}`)やイニシャライザ本文はありません:

```swift
protocol SomeProtocol {
    init(someParameter: Int)
}
```

### Class Implementations of Protocol Initializer Requirements(プロトコルイニシャライザ要件のクラス実装)

プロトコルのイニシャライザ要件は、準拠するクラスで指定イニシャライザまたは convenience イニシャライザとして実装できます。どちらの場合も、`required` 修飾子でイニシャライザの実装をマークする必要があります:

```swift
class SomeClass: SomeProtocol {
    required init(someParameter: Int) {
        // イニシャライザの実装をここに
    }
}
```

`required` 修飾子を使用すると、準拠するクラスの全てのサブクラスでも、イニシャライザ要件を明示的または継承した実装で、プロトコルに準拠する必要があります。

`required` イニシャライザの詳細については、[Required Initializers](./initialization.md#required-initializers必須イニシャライザ)を参照ください。

> NOTE  
> `final` クラスはサブクラス化できないため、 `final` 修飾子でマークされているクラスでは、プロトコルのイニシャライザの実装を `required` 修飾子でマークする必要はありません。`final` 修飾子の詳細については、[Preventing Overrides](./initialization.md#preventing-overridesオーバーライドを防ぐ)を参照ください。

サブクラスがスーパークラスからの指定イニシャライザをオーバーライドし、そのイニシャライザがプロトコル要件にも一致する場合、`required` 修飾子と `override` 修飾子の両方でイニシャライザの実装をマークします:

```swift
protocol SomeProtocol {
    init()
}

class SomeSuperClass {
    init() {
        // イニシャライザの実装をここに
    }
}

class SomeSubClass: SomeSuperClass, SomeProtocol {
    // "required" は SomeProtocol から、 "override" は SomeSuperClass から
    required override init() {
        // initializer implementation goes here
    }
}
```

### Failable Initializer Requirements(失敗可能イニシャライザ要件)

プロトコルは、[Failable Initializers](./initialization.md#failable-initializers失敗可能イニシャライザ)で定義されているように、準拠する型の失敗可能イニシャライザの要件を定義できます。

失敗可能イニシャライザの要件は、準拠する型の失敗可能または失敗しないイニシャライザによって満たされます。失敗しないイニシャライザの要件は、失敗しないイニシャライザまたは暗黙的にアンラップされた失敗可能イニシャライザによって満たされます。

## Protocols as Types(型としてのプロトコル)

プロトコルは、実際には機能を実装しません。それにもかかわらず、コード内で完全な型としてプロトコルを使用できます。プロトコルを型として使用することは、存在型(*existential type*)と呼ばれることもあります。これは、「T がプロトコルに準拠するような型 T が存在する」というフレーズから来ています。

次のような他の型が許可されている多くの場所でプロトコルを使用できます。

* 関数、メソッド、またはイニシャライザの引数の型または戻り値の型として
* 定数、変数、またはプロパティの型として
* 配列、辞書、またはその他のコンテナ内のアイテムの型として

> NOTE  
> プロトコルは型であるため、他の型 (`Int`、`String`、`Double` など) と同じように名前を大文字で始めます。(`FullyNamed` や `RandomNumberGenerator` など)

型として使用されるプロトコルの例を次に示します:

```swift
class Dice {
    let sides: Int
    let generator: RandomNumberGenerator
    init(sides: Int, generator: RandomNumberGenerator) {
        self.sides = sides
        self.generator = generator
    }
    func roll() -> Int {
        return Int(generator.random() * Double(sides)) + 1
    }
}
```

この例では、ボード ゲームで使用する n 面のサイコロを表す `Dice` という新しいクラスを定義しています。`Dice` のインスタンスには、側面の数を表す `sides` と呼ばれる整数のプロパティと、ダイスのロール値を作成するための乱数ジェネレーターを提供する `generator` と呼ばれるプロパティがあります。

`generator` プロパティの型は `RandomNumberGenerator` です。したがって、`RandomNumberGenerator` プロトコルに準拠する任意の型のインスタンスに設定できます。このプロパティに割り当てるインスタンスには、インスタンスが `RandomNumberGenerator` プロトコルに準拠する必要があることを除いて、他に何も必要ありません。その型は `RandomNumberGenerator` なので、`Dice` クラス内のコードは、このプロトコルに準拠する全ての `generator` のみを使用できます。つまり、そのジェネレーターの具体的な型で定義されているメソッドやプロパティを使用することはできません。ただし、[Downcasting](./type-casting.md#downcastingダウンキャスト)で説明されているように、スーパークラスからサブクラスにダウンキャストできるのと同じ方法で、プロトコル型から準拠した具体的な型にダウンキャストできます。

`Dice` には、初期状態を設定するためのイニシャライザもあります。このイニシャライザには、型も `RandomNumberGenerator` の `generator` と呼ばれる引数があります。新しい `Dice` インスタンスを初期化するときに、この引数に準拠する型の値を渡すことができます。

`Dice` は、1 からサイコロの面の数までの整数値を返す 1 つのインスタンスメソッド、`roll` を提供しています。このメソッドは、ジェネレーターの `random()` メソッドを呼び出して `0.0` から `1.0` の間の新しい乱数を作成し、この乱数を使用して正しい範囲内でサイコロの目を作成します。`generator` は `RandomNumberGenerator` に準拠することがわかっているため、呼び出すべき `random()` メソッドがあることが保証されています。

`Dice` クラスを使用して、`LinearCongruentialGenerator` インスタンスを乱数ジェネレーターとして使用して、6 面体のサイコロを作成する方法を次に示します:

```swift

var d6 = Dice(sides: 6, generator: LinearCongruentialGenerator())
for _ in 1...5 {
    print("Random dice roll is \(d6.roll())")
}
// Random dice roll is 3
// Random dice roll is 5
// Random dice roll is 4
// Random dice roll is 5
// Random dice roll is 4
```

## Delegation(委譲)

委譲(*delegation*)は、クラスまたは構造体がその責任の一部を別の型のインスタンスに引き渡す(または委譲する)ことを可能にするデザインパターンです。このデザインパターンは、委譲された責任をカプセル化するプロトコルを定義することによって実装され、準拠する型 (デリゲート(*delegate*)と呼ばれる)が委譲された機能を提供することが保証されます。委譲を使用して、特定のアクションに応答したり、その準拠した具体的な型を知らなくても外部ソースからデータを取得したりできます。

下記の例では、サイコロベースのボードゲームで使用する 2 つのプロトコルを定義しています:

```swift
protocol DiceGame {
    var dice: Dice { get }
    func play()
}
protocol DiceGameDelegate: AnyObject {
    func gameDidStart(_ game: DiceGame)
    func game(_ game: DiceGame, didStartNewTurnWithDiceRoll diceRoll: Int)
    func gameDidEnd(_ game: DiceGame)
}
```

`DiceGame` プロトコルは、サイコロを使用する全てのゲームで準拠できるプロトコルです。

`DiceGameDelegate` プロトコルは、`DiceGame` の進行状況を追跡するために準拠できます。強参照(*strong reference*)循環を防ぐために、デリゲートは弱参照(*weak reference*)として宣言されます。弱参照については、[Strong Reference Cycles Between Class Instances](./automatic-reference-counting.md#strong-reference-cycles-between-class-instancesクラスインスタンス間の強参照循環)を参照ください。プロトコルをクラス専用としてマークすると、この章の後半で登場する `SnakesAndLadders` クラスがそのデリゲートが弱参照を使用しなければならないことを宣言できるようになります。クラス専用プロトコルは、[Class-Only Protocols](#class-only-protocolsクラス専用プロトコル)で説明されているように、`AnyObject` を継承します。

これは、最初に [Control Flow](./control-flow.md) で紹介した蛇とはしごゲームのバージョンです。このバージョンは、ダイスロールに `Dice` インスタンスを使用しています。`DiceGame` プロトコルに準拠すること。そして、その進行状況を `DiceGameDelegate` に通知します:

```swift
class SnakesAndLadders: DiceGame {
    let finalSquare = 25
    let dice = Dice(sides: 6, generator: LinearCongruentialGenerator())
    var square = 0
    var board: [Int]
    init() {
        board = Array(repeating: 0, count: finalSquare + 1)
        board[03] = +08; board[06] = +11; board[09] = +09; board[10] = +02
        board[14] = -10; board[19] = -11; board[22] = -02; board[24] = -08
    }
    weak var delegate: DiceGameDelegate?
    func play() {
        square = 0
        delegate?.gameDidStart(self)
        gameLoop: while square != finalSquare {
            let diceRoll = dice.roll()
            delegate?.game(self, didStartNewTurnWithDiceRoll: diceRoll)
            switch square + diceRoll {
            case finalSquare:
                break gameLoop
            case let newSquare where newSquare > finalSquare:
                continue gameLoop
            default:
                square += diceRoll
                square += board[square]
            }
        }
        delegate?.gameDidEnd(self)
    }
}
```

蛇とはしごについては、[Break](./control-flow.md#break)を参照してください。

このバージョンのゲームは、`DiceGame` プロトコルに準拠する `SnakesAndLadders` というクラスでラップされています。プロトコルに準拠するために、`dice` と呼ばれる get プロパティと `play()` メソッドを提供しています。(`dice` プロパティは、初期化後に変更する必要がないため、定数プロパティとして宣言されており、プロトコルでは取得可能なことのみを要件にしています)

蛇とはしごのゲームボードのセットアップは、クラスの `init()` イニシャライザ内で行われています。全てのゲームロジックは、プロトコルの `play` メソッドの中で行われています。このメソッドは、プロトコルで必須の `dice` プロパティを使用して、ダイスロール値を提供しています。

ゲームをプレイするためにデリゲートは必要ないため、`delegate` プロパティはオプショナルの `DiceGameDelegate` として定義されていることに注目してください。これはオプショナル型なので、`delegate` プロパティは自動的に `nil` で初期化されます。その後、ゲームのイニシャライザには、プロパティを適切なデリゲートに設定することができます。`DiceGameDelegate` プロトコルはクラス専用のため、デリゲートを `weak` として宣言し、循環参照を防ぐことができます。

`DiceGameDelegate` は、ゲームの進行状況を追跡するための 3 つのメソッドを提供しています。これらの 3 つのメソッドは、上記の `play()` メソッド内のゲームロジックに組み込まれており、新しいゲームの開始、新しいターンの開始、またはゲームの終了時に呼び出されます。

`delegate` プロパティはオプショナルの `DiceGameDelegate` のため、`play()` メソッドはデリゲートメソッドを呼び出す度にオプショナルチェーンを使用します。`delegate`  プロパティが `nil` の場合、これらのデリゲートの呼び出しはエラーを出力せずに失敗します。`delegate` プロパティが `nil` ではない場合、デリゲートメソッドが呼び出され、引数として `SnakesAndLadders` インスタンスが渡されます。

次の例は、`DiceGameDelegate` プロトコルに準拠する `DiceGameTracker` というクラスを示しています:

```swift
class DiceGameTracker: DiceGameDelegate {
    var numberOfTurns = 0
    func gameDidStart(_ game: DiceGame) {
        numberOfTurns = 0
        if game is SnakesAndLadders {
            print("Started a new game of Snakes and Ladders")
        }
        print("The game is using a \(game.dice.sides)-sided dice")
    }
    func game(_ game: DiceGame, didStartNewTurnWithDiceRoll diceRoll: Int) {
        numberOfTurns += 1
        print("Rolled a \(diceRoll)")
    }
    func gameDidEnd(_ game: DiceGame) {
        print("The game lasted for \(numberOfTurns) turns")
    }
}
```

`DiceGameTracker` は、`DiceGameDelegate` に必要な 3 つのメソッド全てを実装します。これらのメソッドを使用して、ゲームが経過したターン数を追跡します。これは、ゲームの開始時に `numberOfTurns` プロパティをゼロにリセットし、新しいターンが開始される度にそれを増加し、ゲームが終了すると合計ターン数を出力します。

上記の `gameDidStart(_:)` の実装では、`game` 引数を使用して、これからプレイするゲームに関するいくつかのイントロダクションを出力します。`game` 引数の型は `SnakesAndLadders` ではなく `DiceGame` のため、`gameDidStart(_:)` は、`DiceGame` プロトコルの一部として実装されているメソッドとプロパティのみ使用できます。ただし、メソッドは引き続き型キャストを使用して、準拠したインスタンスの型を照会できます。この例では、`game` が実際に内部で `SnakesAndLadders` のインスタンスかどうかを確認し、その場合は適切なメッセージを出力します。

`gameDidStart(_:)` メソッドは、渡された `game` 引数の `dice` プロパティにもアクセスしています。`game` は `DiceGame` プロトコルに準拠しているため、`dice` プロパティを持つことが保証されているため、`gameDidStart(_:)` メソッドは、プレイされているゲームの種類に関係なく、`dice` の `side` プロパティにアクセスして出力できます。

`DiceGameTracker` の実際の挙動は次のとおりです:

```swift
let tracker = DiceGameTracker()
let game = SnakesAndLadders()
game.delegate = tracker
game.play()
// 蛇とはしごの新しいゲームを開始します
// ゲームでは ６ 面体のサイコロを使います
// 3 が出ました
// 5 が出ました
// 4 が出ました
// 5 が出ました
```

## Adding Protocol Conformance with an Extension(拡張機能を使ったプロトコル準拠の追加)

既存の型のソースコードにアクセスできない場合でも、既存の型を拡張して新しいプロトコルに準拠させることができます。拡張機能(*extension*)は、新しいプロパティ、メソッド、および subscript を既存の型に追加できるため、プロトコルの要件を追加できます。拡張機能の詳細については、[Extensions](./extensions.md)を参照ください。

> NOTE  
> 型の既存のインスタンスは、extension でそのインスタンスの型がプロトコルに準拠すると、自動的にプロトコルに準拠します。

例えば、`TextRepresentable` と呼ばれるこのプロトコルは、テキストとして表現できる任意の型で実装できます。これは、それ自体の説明、または現在の状態のテキストバージョンの場合があります:

```swift
protocol TextRepresentable {
    var textualDescription: String { get }
}
```

上記の `Dice` クラスを拡張して、`TextRepresentable` に準拠させることができます:

```swift
extension Dice: TextRepresentable {
    var textualDescription: String {
        return "A \(sides)-sided dice"
    }
}
```

この extension は、`Dice` が元の実装とまったく同じ方法で新しいプロトコルに準拠しています。プロトコル名を型名の後にコロン(`:`)で区切って指定し、プロトコルの全ての要件の実装は extension の中括弧内(`{}`)に指定します。

これで、どの `Dice` インスタンスも `TextRepresentable` として扱えるようになりました:

```swift
let d12 = Dice(sides: 12, generator: LinearCongruentialGenerator())
print(d12.textualDescription)
// "A 12-sided dice"
```

同様に、`SnakesAndLadders` クラスを拡張して、`TextRepresentable` プロトコルに準拠させることができます:

```swift
extension SnakesAndLadders: TextRepresentable {
    var textualDescription: String {
        return "A game of Snakes and Ladders with \(finalSquare) squares"
    }
}
print(game.textualDescription)
// "A game of Snakes and Ladders with 25 squares"
```

### Conditionally Conforming to a Protocol(条件付きでのプロトコルへの準拠)

ジェネリック型は、型のジェネリック引数がプロトコルに準拠している場合など、特定の条件下でのみプロトコルの要件を満たすことができる場合があります。型を拡張するときに制約を並べることで、ジェネリック型を条件付きでプロトコルに準拠させることができます。`where` 句を記述して、準拠するプロトコルの名前の後にこれらの制約を記述します。ジェネリック `where` 句の詳細については、[Generic Where Clauses](./generics.md#generic-where-clauses)を参照ください。

次の拡張により、`Array` インスタンスが `TextRepresentable` に準拠する型の要素を格納する場合は常に、`TextRepresentable` プロトコルに準拠するようになります:

```swift
extension Array: TextRepresentable where Element: TextRepresentable {
    var textualDescription: String {
        let itemsAsText = self.map { $0.textualDescription }
        return "[" + itemsAsText.joined(separator: ", ") + "]"
    }
}
let myDice = [d6, d12]
print(myDice.textualDescription)
// "[A 6-sided dice, A 12-sided dice]"
```

### Declaring Protocol Adoption with an Extension(拡張機能を使ったプロトコル準拠の宣言)

## Adopting a Protocol Using a Synthesized Implementation(同期的な実装を使用したプロトコル準拠)

## Collections of Protocol Types(プロトコル型のコレクション)

## Protocol Inheritance(プロトコル継承)

## Class-Only Protocols(クラス専用プロトコル)

## Protocol Composition(プロトコルの組み合わせ)

## Checking for Protocol Conformance(プロトコル準拠チェック)

## Optional Protocol Requirements(オプショナルのプロトコル要件)
