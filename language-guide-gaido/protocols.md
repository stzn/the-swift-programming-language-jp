# プロトコル\(Protocols\)

最終更新日: 2021/6/29

_プロトコル_は、特定のタスクまたは機能に準拠するメソッド、プロパティ、およびその他の要件の設計図を定義します。クラス、構造体、または列挙型がプロトコルに準拠でき、それらの要件の実装を提供します。プロトコルの要件を満たす全ての型は、そのプロトコルに_準拠している_といいます。

プロトコルは、準拠する型が実装する必要がある要件を指定することに加えて、プロトコルを拡張して、これらの要件の一部を実装したり、準拠する型が利用できる追加機能を実装できます。

## プロトコル構文\(Protocol Syntax\)

クラス、構造体、および列挙型と非常によく似た方法でプロトコルを定義します:

```swift
protocol SomeProtocol {
    // プロトコルの定義をここに
}
```

カスタム型は、定義の一部として、型名の後にプロトコル名をコロン\(`:`\)で区切って配置することにより、特定のプロトコルに準拠することを示します。複数のプロトコルにも準拠でき、カンマ\(`,`\)区切りで並べます:

```swift
struct SomeStructure: FirstProtocol, AnotherProtocol {
    // 構造体の定義をここに
}
```

クラスにスーパークラスがある場合は、準拠するプロトコルの前にスーパークラス名を記述し、その後にカンマを続けます:

```swift
class SomeClass: SomeSuperclass, FirstProtocol, AnotherProtocol {
    // クラスの定義をここに
}
```

## <a id="property-requirements">プロパティ要件\(Property Requirements\)</a>

プロトコルは、特定の名前と型のインスタンスプロパティまたは型プロパティを要件にできます。プロトコルでは、格納プロパティか計算プロパティかを指定しません。必要なプロパティの名前と型を指定するだけです。また、プロトコルは、各プロパティが get のみか、get/set どちらも必要かどうかも指定します。

プロトコルが get/set を要求する場合、そのプロパティ要件は、定数格納プロパティまたは読み取り専用の計算プロパティでは満たされません。プロトコルが get のみを要求する場合でも、その要件はあらゆる種類のプロパティによって満たされ、必要ならば実装の方でプロパティの set を追加することもできます。

プロパティ要件は常に変数プロパティとして宣言され、前に `var` キーワードが付きます。get/set は型宣言の後に `{ get set }` を記述することで示し、get は `{ get }` で示します。

```swift
protocol SomeProtocol {
    var mustBeSettable: Int { get set }
    var doesNotNeedToBeSettable: Int { get }
}
```

プロトコルで、型プロパティの要件の前には、必ず `static` キーワードを付けてください。クラスによって実装されるときは、`class` または `static` キーワードをプレフィックスを付けても要件を満たします:

```swift
protocol AnotherProtocol {
    static var someTypeProperty: Int { get set }
}
```

単一のインスタンスプロパティを要件に持つプロトコルの例を次に示します:

```swift
protocol FullyNamed {
    var fullName: String { get }
}
```

完全修飾名を提供するために、`FullyNamed` プロトコルに準拠する型が必要です。プロトコルは、準拠する型の性質について他に何も指定していません。型が完全修飾名を提供する必要があることを指定するだけです。プロトコルでは、`FullyNamed` 型には、`String` 型の `fullName` という get インスタンスプロパティが宣言されています。

`FullyNamed` プロトコルに準拠するシンプルな構造体の例を次に示します:

```swift
struct Person: FullyNamed {
    var fullName: String
}
let john = Person(fullName: "John Appleseed")
// john.fullName は "John Appleseed"
```

この例では、特定の名前を持つ人物を表す `Person` という構造体を定義しています。その定義の最初の行の一部として `FullyNamed` プロトコルに準拠することが記述されています。

`Person` の各インスタンスには、`String` 型の `fullName` という 1 つの格納プロパティがあります。これは、`FullyNamed` プロトコルの単一の要件と一致し、`Person` がプロトコルに正しく準拠していることを意味します。\(プロトコル要件が満たされていない場合、コンパイルエラーが発生します\)

下記も `FullyNamed` プロトコルに準拠する、より複雑なクラスです:

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

このクラスは、宇宙船の読み取り専用計算プロパティとして `fullName` プロパティを実装します。各 `Starship` クラスのインスタンスは、必須の `name` とオプショナルの `prefix` を格納します。`fullName` プロパティは、プレフィックス値が存在する場合はそれを使用し、名前の先頭に追加して宇宙船のフルネームを作成します。

## <a id="method-requirements">メソッド要件\(Method Requirements\)</a>

プロトコルはインスタンスメソッドと型メソッドを要件にすることもできます。これらのメソッドは、通常のインスタンスおよび型メソッドとまったく同じ方法でプロトコルの定義の一部として記述されますが、中括弧\(`{}`\)やメソッド本文はありません。通常のメソッドと同じ規則に従って、可変長パラメータを使用できます。ただし、プロトコルの定義内のメソッドのパラメータにデフォルト値を指定することはできません。

型プロパティの要件と同様に、型メソッドをプロトコルで定義するときは、常に要件の前に `static` キーワードを付けます。これは、クラスによって実装されるときに、型メソッドの要件に `class` または `static` キーワードが付いている場合にも当てはまります:

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

このプロトコル `RandomNumberGenerator` に準拠するには `random` と呼ばれるインスタンスメソッドが必要です。このインスタンスメソッドは、呼び出される度に `Double` 値を返します。プロトコルでは指定されていませんが、この値は `0.0` から `1.0`\(ただし、`1.0` を含まない\) までの数値が想定されています。

`RandomNumberGenerator` プロトコルは、各乱数がどのように生成されるかについて何も指定していません。単に、ジェネレータが新しい乱数を生成する標準的な方法を要求するだけです。

下記は、`RandomNumberGenerator` プロトコルに準拠するクラスの実装です。このクラスは、線形合同法発生器として知られる疑似乱数発生器アルゴリズムを実装しています:

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

## mutating メソッド要件\(Mutating Method Requirements\)

メソッドが属するインスタンスを変更する必要がある場合があります。値型\(つまり、構造体と列挙型\)のインスタンスメソッドの場合、メソッドの `func` キーワードの前に `mutating` キーワードを配置して、メソッドが属するインスタンスとそのインスタンスの全てのプロパティを変更できることを示します。このプロセスについては、[Modifying Value Types from Within Instance Methods\(インスタンスメソッド内からの値型の変更\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide-gaido/methods#modifying-value-types-from-within-instance-methods)で説明されています。

任意の型のインスタンスを変更することを目的としたプロトコルのインスタンスメソッドの要件を定義する場合は、`mutating` キーワードをメソッドにマークします。これにより、構造体と列挙型がそのメソッド要件を満たしてプロトコルに準拠することができます。

> NOTE  
> プロトコルのインスタンスメソッドの要件を `mutating` としてマークする場合、クラスがそのメソッドを実装するときに、`mutating` キーワードを記述する必要はありません。`mutating` キーワードは、構造体と列挙型のみ使用できます。

下記の例では、`Togglable` というプロトコルが定義されています。これは、`toggle` という単一のインスタンスメソッドの要件を定義しています。その名前が示すように、`toggle()` メソッドは、通常、その型のプロパティを変更することによって、準拠する型の状態をトグル\(反転\)することを目的としています。

`toggle()` メソッドは、`Togglable` プロトコル定義の一部として `mutating` キーワードがマークされており、メソッドが呼び出されたときに準拠するインスタンスの状態を変更する可能性を示しています。

構造体または列挙型に対して `Togglable` プロトコルを実装する場合、その構造体または列挙型は、`mutating` がマークされている `toggle()` メソッドの実装を提供することにより、プロトコルに準拠できます。

下記の例では、`OnOffSwitch` という列挙型を定義しています。この列挙型は、`on` と `off` の 2 つのケースによって示される 2 つの状態を切り替えます。列挙型の `toggle` 実装は、`Togglable` プロトコルの要件を満たすように、`mutating` としてマークされています:

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

## <a id="initializer-requirements">イニシャライザ要件\(Initializer Requirements\)</a>

プロトコルでは、型に準拠するために特定のイニシャライザが必要な場合があります。このイニシャライザは、通常のイニシャライザとまったく同じ方法でプロトコルの定義の一部として記述できますが、中括弧\(`{}`\)やイニシャライザ本文はありません:

```swift
protocol SomeProtocol {
    init(someParameter: Int)
}
```

### プロトコルイニシャライザ要件のクラス実装\(Class Implementations of Protocol Initializer Requirements\)

プロトコルのイニシャライザの要件は、準拠するクラスで指定イニシャライザまたは convenience イニシャライザとして実装できます。どちらの場合も、`required` 修飾子でイニシャライザの実装をマークする必要があります:

```swift
class SomeClass: SomeProtocol {
    required init(someParameter: Int) {
        // イニシャライザの実装をここに
    }
}
```

`required` 修飾子を使用すると、準拠するクラスの全てのサブクラスで、イニシャライザ要件を明示的に実装または継承して、プロトコルに準拠する必要があります。

`required` イニシャライザの詳細については、[Required Initializers\(必須イニシャライザ\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide-gaido/initialization#required-initializers)を参照ください。

> NOTE  
> `final` クラスはサブクラス化できないため、 `final` 修飾子でマークされているクラスでは、プロトコルのイニシャライザの実装を `required` 修飾子でマークする必要はありません。`final` 修飾子の詳細については、[Preventing Overrides\(オーバーライドを防ぐ\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide-gaido/inheritance#preventing-overrides)を参照ください。

サブクラスがスーパークラスからの指定イニシャライザをオーバーライドし、そのイニシャライザがプロトコル要件も満たす場合、`required` 修飾子と `override` 修飾子の両方でイニシャライザの実装をマークします:

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
        // イニシャライザの実装をここに
    }
}
```

### <a id="failable-initializer-requirements">失敗可能イニシャライザ要件\(Failable Initializer Requirements\)</a>

プロトコルは、[Failable Initializers\(失敗可能イニシャライザ\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/declarations#declarations-failable-initializers)で定義されているように、失敗可能イニシャライザ要件を定義できます。

失敗可能イニシャライザ要件は、準拠する型の失敗可能または失敗しないイニシャライザによって満たされます。失敗しないイニシャライザ要件は、失敗しないイニシャライザまたは暗黙的にアンラップされた失敗可能イニシャライザによって満たされます。

## <a id="protocols-as-types">型としてのプロトコル\(Protocols as Types\)</a>

プロトコルは、実際には機能を実装しません。それにもかかわらず、コード内で完全な型としてプロトコルを使用できます。プロトコルを型として使用することは、_存在型_と呼ばれることもあります。これは、「T がプロトコルに準拠するような型 T が存在する」というフレーズから来ています。

次のような他の型が許可されている多くの場所でプロトコルを使用できます。

* 関数、メソッド、またはイニシャライザのパラメータの型または戻り値の型として
* 定数、変数、またはプロパティの型として
* 配列、辞書、またはその他のコンテナ内のアイテムの型として

> NOTE  
> プロトコルは型であるため、他の型 \(`Int`、`String`、`Double` など\) と同じように名前を大文字で始めます。\(`FullyNamed` や `RandomNumberGenerator` など\)

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

この例では、ボードゲームで使用する n 面のサイコロを表す `Dice` という新しいクラスを定義しています。`Dice` のインスタンスには、側面の数を表す `sides` と呼ばれる整数のプロパティと、ダイスの出目を作成するための乱数ジェネレータを提供する `generator` と呼ばれるプロパティがあります。

`generator` プロパティの型は `RandomNumberGenerator` です。したがって、`RandomNumberGenerator` プロトコルに準拠する任意の型のインスタンスを設定できます。このプロパティに割り当てるインスタンスには、インスタンスが `RandomNumberGenerator` プロトコルに準拠する必要があることを除いて、他に何も必要ありません。型は `RandomNumberGenerator` なので、`Dice` クラス内のコードは、このプロトコルに準拠する `generator` のみを使用できます。つまり、その具体的なジェネレータの型で定義されているメソッドやプロパティを使用することはできません。ただし、[Downcasting\(ダウンキャスト\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide-gaido/type-casting#downcasting)で説明されているように、スーパークラスからサブクラスにダウンキャストできるのと同じ方法で、プロトコル型から準拠した具体的な型にダウンキャストできます。

`Dice` には、初期状態を設定するためのイニシャライザもあります。このイニシャライザには、`RandomNumberGenerator` 型の `generator` と呼ばれるパラメータがあります。新しい `Dice` インスタンスを初期化するときに、このパラメータに `RandomNumberGenerator` に準拠する型の値を渡すことができます。

`Dice` は、1 からサイコロの面の数までの整数値を返す 1 つのインスタンスメソッド、`roll` を提供しています。このメソッドは、ジェネレータの `random()` メソッドを呼び出して `0.0` から `1.0` の間の新しい乱数を作成し、この乱数を使用して正しい範囲内でサイコロの目を作成します。`generator` は `RandomNumberGenerator` に準拠することがわかっているため、呼び出すべき `random()` メソッドが存在していることは保証されています。

`Dice` クラスを使用して、`LinearCongruentialGenerator` インスタンスを乱数ジェネレータとして使用して、6 面体のサイコロを作成する方法を次に示します:

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

## <a id="delegation">委譲\(Delegation\)</a>

_委譲_は、クラスまたは構造体がその責任の一部を別の型のインスタンスに引き渡す\(または委譲する\)ことを可能にするデザインパターンです。このデザインパターンは、委譲される責任をカプセル化するプロトコルを定義することによって実装され、準拠する型 \(_デリゲート_と呼ばれる\)が委譲された機能を提供することを保証します。委譲を使用して、特定のアクションに応答したり、その準拠した具体的な型を知らなくても外部ソースからデータを取得したりできます。

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

`DiceGame` プロトコルは、サイコロを使用する全てのゲームに準拠できるプロトコルです。

`DiceGameDelegate` プロトコルは、`DiceGame` の進行状況を追跡するために準拠できます。_強参照_循環を防ぐために、デリゲートは_弱参照_として宣言されます。弱参照については、[Strong Reference Cycles Between Class Instances\(クラスインスタンス間の強参照循環\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide-gaido/automatic-reference-counting#strong-reference-cycles-between-class-instances)を参照ください。プロトコルをクラス専用としてマークすると、この章の後半で登場する `SnakesAndLadders` クラスがそのデリゲートを弱参照で使用しなければならないことを宣言できるようになります。クラス専用プロトコルは、[Class-Only Protocols\(クラス専用プロトコル\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide-gaido/protocols#class-only-protocols)で説明されているように、`AnyObject` を継承します。

これは、最初に [Control Flow\(制御フロー\)](control-flow.md) で紹介した「蛇とはしごゲーム」の別バージョンです。このバージョンは、ダイスロールに `Dice` インスタンスを使用しています。`SnakesAndLadders` は `DiceGame` プロトコルに準拠し、その進行状況を `DiceGameDelegate` に通知します:

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

蛇とはしごについては、[Break](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide-gaido/control-flow#break)を参照してください。

このバージョンでは、`DiceGame` プロトコルに準拠する `SnakesAndLadders` というクラスでラップされています。プロトコルに準拠するために、`dice` と呼ばれるプロパティの get と `play()` メソッドを提供しています。\(`dice` プロパティは、初期化後に変更する必要がないため、定数プロパティとして宣言されており、プロトコルでは get のみを要件にしています\)

蛇とはしごのゲームボードのセットアップは、クラスの `init()` イニシャライザ内で行われています。全てのゲームロジックは、プロトコルの `play` メソッドの中で行われています。このメソッドは、プロトコルで必須の `dice` プロパティを使用して、ダイスの出目を提供しています。

ゲームをプレイするためにデリゲートは必要ないため、`delegate` プロパティはオプショナルの `DiceGameDelegate` として定義されていることに注目してください。これはオプショナル型なので、`delegate` プロパティは自動的に `nil` で初期化されます。その後、ゲームのイニシャライザには、プロパティに適切なデリゲートを設定することができます。`DiceGameDelegate` プロトコルはクラス専用のため、デリゲートを `weak` として宣言し、循環参照を防ぐことができます。

`DiceGameDelegate` は、ゲームの進行状況を追跡するための 3 つのメソッドを提供しています。これらの 3 つのメソッドは、上記の `play()` メソッド内のゲームロジックに組み込まれており、新しいゲームの開始、新しいターンの開始、またはゲームの終了時に呼び出されます。

`delegate` プロパティはオプショナルの `DiceGameDelegate` のため、`play()` メソッドはデリゲートメソッドを呼び出す度にオプショナルチェーンを使用します。`delegate` プロパティが `nil` の場合、これらのデリゲートの呼び出しはエラーを出力せずに失敗します。`delegate` プロパティが `nil` ではない場合、デリゲートメソッドが呼び出され、パラメータとして `SnakesAndLadders` インスタンスが渡されます。

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

上記の `gameDidStart(_:)` の実装では、`game` パラメータを使用して、これからプレイするゲームに関するいくつかのイントロダクションを出力します。`game` パラメータの型は `SnakesAndLadders` ではなく `DiceGame` のため、`gameDidStart(_:)` は、`DiceGame` プロトコルの一部として実装されているメソッドとプロパティのみを使用できます。ただし、メソッドは引き続き型キャストを使用して、準拠したインスタンスの型を照会できます。この例では、`game` が実際に内部で `SnakesAndLadders` のインスタンスかどうかを確認し、その場合は適切なメッセージを出力します。

`gameDidStart(_:)` メソッドは、渡された `game` パラメータの `dice` プロパティにもアクセスしています。`game` は `DiceGame` プロトコルに準拠しているため、`dice` プロパティを持つことが保証されているため、`gameDidStart(_:)` メソッドは、プレイされているゲームの種類に関係なく、`dice` の `sides` プロパティにアクセスして出力できます。

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

## <a id="adding-protocol-conformance-with-an-extension">拡張機能を使ったプロトコル準拠の追加\(Adding Protocol Conformance with an Extension\)</a>

既存の型のソースコードにアクセスできない場合でも、既存の型を拡張して新しいプロトコルに準拠させることができます。extension は、新しいプロパティ、メソッド、および subscript を既存の型に追加できるため、プロトコルの要件を追加できます。拡張機能の詳細については、[Extensions\(拡張\)](extensions.md)を参照ください。

> NOTE  
> 型の既存のインスタンスは、extension でそのインスタンスの型がプロトコルに準拠すると、自動的にプロトコルに準拠します。

例えば、`TextRepresentable` と呼ばれるプロトコルは、テキストとして表現できる任意の型で実装できます。これは、それ自体の説明や、現在の状態の説明などの可能性があります:

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

この extension は、`Dice` の元の実装とまったく同じ方法で新しいプロトコルに準拠しています。プロトコル名を型名の後にコロン\(`:`\)で区切って指定し、プロトコルの全ての要件の実装は extension の中括弧内\(`{}`\)に指定します。

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

### <a id="conditionally-conforming-to-a-protocol">条件付きでのプロトコルへの準拠\(Conditionally Conforming to a Protocol\)</a>

ジェネリック型は、型のジェネリックパラメータがプロトコルに準拠している場合など、特定の条件下でのみプロトコルの要件を満たすことができるようにします。型を拡張するときに制約を並べることで、ジェネリック型を条件付きでプロトコルに準拠させることができます。`where` 句を記述して、準拠するプロトコルの名前の後にこれらの制約を記述します。ジェネリック `where` 句の詳細については、[Generic Where Clauses\(ジェネリック where 句\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide-gaido/generics#generic-where-clauses)を参照ください。

次の拡張により、`Array` インスタンスが `TextRepresentable` に準拠する型の要素を格納する場合は、常に `TextRepresentable` プロトコルに準拠するようになります:

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

### <a id="declaring-protocol-adoption-with-an-extension">拡張機能を使ったプロトコル準拠の宣言\(Declaring Protocol Adoption with an Extension\)</a>

型がすでにプロトコルの全ての要件を満たしているものの、そのプロトコルに準拠することを表明していない場合は、空の extension でプロトコルに準拠することができます:

```swift
struct Hamster {
    var name: String
    var textualDescription: String {
        return "A hamster named \(name)"
    }
}
extension Hamster: TextRepresentable {}
```

これで `TextRepresentable` が必要な型へ、`Hamster` のインスタンスを使用できるようになりました:

```swift
let simonTheHamster = Hamster(name: "Simon")
let somethingTextRepresentable: TextRepresentable = simonTheHamster
print(somethingTextRepresentable.textualDescription)
// "A hamster named Simon"
```

> NOTE  
> 要件を満たすだけで、型が自動的にプロトコルに準拠するわけではありません。プロトコルへの準拠を常に明示的に宣言する必要があります。

## <a id="adopting-a-protocol-using-a-synthesized-implementation">デフォルト実装を使用したプロトコル準拠\(Adopting a Protocol Using a Synthesized Implementation\)</a>

Swift は、多くのシンプルなケースで、`Equatable`、`Hashable`、および `Comparable` のプロトコルへの準拠を自動的に提供できます。このデフォルト実装を使用すると、プロトコル要件を自分で実装するために、繰り返しコードを記述する必要がなくなります。

Swift は、次の種類の独自の型に対して `Equatable` のデフォルト実装を提供します。

* `Equatable` プロトコルに準拠した型の格納プロパティのみで構成される構造体
* 関連値が `Equatable` プロトコルに準拠する型のみの列挙型
* 関連値のない列挙型

`==` のデフォルト実装を受け取るには、自分で `==` 演算子を実装せずに、元の宣言を含むファイルで `Equatable` への準拠を宣言します。`Equatable` プロトコルは、`!=` のデフォルトの実装を提供しています。

下記の例では、`Vector2D` 構造体と同様に、3 次元位置ベクトル `(x、y、z)` の `Vector3D` 構造体を定義しています。`x`、`y`、`z` プロパティは全て `Equatable` に準拠した型なので、`Vector3D` は等価演算子のデフォルト実装を受け取ります。

```swift
struct Vector3D: Equatable {
    var x = 0.0, y = 0.0, z = 0.0
}

let twoThreeFour = Vector3D(x: 2.0, y: 3.0, z: 4.0)
let anotherTwoThreeFour = Vector3D(x: 2.0, y: 3.0, z: 4.0)
if twoThreeFour == anotherTwoThreeFour {
    print("These two vectors are also equivalent.")
}
// "These two vectors are also equivalent."
```

Swift は、次の種類の独自の型に対して `Hashable` のデフォルト実装を提供します。

* `Hashable` プロトコルに準拠した型の格納プロパティのみで構成される構造体
* 関連値が `Hashable` プロトコルに準拠する型のみの列挙型
* 関連値のない列挙型

`hash(into:)` のデフォルト実装を受け取るには、`hash(into:)` メソッドを自分で実装せずに、元の宣言を含むファイルで `Hashable` への準拠を宣言します。

Swift は、Raw Value を持たない列挙型に `Comparable` のデフォルト実装を提供します。列挙型に関連値がある場合、それらは全て `Comparable` プロトコルに準拠している必要があります。`<` のデフォルト実装を受け取るには、自分で `<` 演算子を実装せずに、元の列挙宣言を含むファイルで `Comparable` への準拠を宣言します。残りの比較演算子\(`<=`、`>`、および `>=`\)は `Comparable` プロトコルがデフォルトで実装を提供してます。

下記の例では、初心者、中級者、および専門家向けのケースを含む `SkillLevel` 列挙型を定義しています。エキスパートは、持っている星の数によってさらにランク付けされます。

```swift
enum SkillLevel: Comparable {
    case beginner
    case intermediate
    case expert(stars: Int)
}
var levels = [SkillLevel.intermediate, SkillLevel.beginner,
              SkillLevel.expert(stars: 5), SkillLevel.expert(stars: 3)]
for level in levels.sorted() {
    print(level)
}
// "beginner"
// "intermediate"
// "expert(stars: 3)"
// "expert(stars: 5)"
```

## プロトコル型のコレクション\(Collections of Protocol Types\)

プロトコルは、[Protocols as Types\(型としてのプロトコル\)](protocols.md#protocols-as-types)で説明されているように、配列や辞書などのコレクションに格納される型として使用できます。この例では、`TextRepresentable` の配列を作成します。

```swift
let things: [TextRepresentable] = [game, d12, simonTheHamster]
```

配列内のアイテムを繰り返し処理し、各アイテムの説明をテキストで出力できるようになりました:

```swift
for thing in things {
    print(thing.textualDescription)
}
// A game of Snakes and Ladders with 25 squares
// A 12-sided dice
// A hamster named Simon
```

`Thing` 定数は `TextRepresentable` 型なことに注目してください。内部の実際のインスタンスがそれらの型の 1 つの場合でも、型は `Dice`、`DiceGame`、または `Hamster` ではありません。これは `TextRepresentable` 型で、`TextRepresentable` は全て `textualDescription` プロパティを持つことがわかっているため、ループ処理の中で安全に `thing.textualDescription` にアクセスできます。

## <a id="protocols-protocol-inheritance">プロトコル継承\(Protocol Inheritance\)</a>

プロトコルは 1 つ以上の他のプロトコルを継承でき、継承する要件の上にさらに要件を追加できます。プロトコル継承の構文はクラス継承の構文に似ていますが、継承された複数のプロトコルをカンマ\(`,`\)で区切って並べます:

```swift
protocol InheritingProtocol: SomeProtocol, AnotherProtocol {
    // プロトコルの実装をここに
}
```

上の `TextRepresentable` プロトコルを継承するプロトコルの例を次に示します:

```swift
protocol PrettyTextRepresentable: TextRepresentable {
    var prettyTextualDescription: String { get }
}
```

この例では、`TextRepresentable` から継承する新しいプロトコル `PrettyTextRepresentable` を定義しています。`PrettyTextRepresentable` に準拠するものは全て、`TextRepresentable` の全ての要件に加えて、`PrettyTextRepresentable` の追加の要件を満たす必要があります。この例では、`PrettyTextRepresentable` は、`String` を返す `prettyTextualDescription` というプロパティの get を提供するための 1 つの要件を追加します。

`SnakesAndLadders` クラスを拡張して、`PrettyTextRepresentable` に準拠させることができます:

```swift
extension SnakesAndLadders: PrettyTextRepresentable {
    var prettyTextualDescription: String {
        var output = textualDescription + ":\n"
        for index in 1...finalSquare {
            switch board[index] {
            case let ladder where ladder > 0:
                output += "▲ "
            case let snake where snake < 0:
                output += "▼ "
            default:
                output += "○ "
            }
        }
        return output
    }
}
```

この extension は、`SnakesAndLadders` 型が `PrettyTextRepresentable` プロトコルに準拠し、 `prettyTextualDescription` のプロパティの実装を提供していることを示しています。`PrettyTextRepresentable` に準拠するものは、必ず全て `TextRepresentable` なので、`prettyTextualDescription` の実装は、`TextRepresentable` プロトコルの `textualDescription` プロパティにアクセスして出力文字列を開始します。コロン\(`:`\)と改行\(`\n`\)を追加し、これをテキストの開始表現として使用します。次に、ボードの正方形の配列を繰り返し処理し、各正方形の内容を表す幾何学的形状を追加します:

* 正方形の値が `0` より大きい場合、それははしごの根本で、`▲` で表されます
* マスの値が `0` 未満の場合、それはヘビの頭で、`▼` で表されます
* それ以外の場合、正方形の値は `0` で、それは `○` で表される「自由な」正方形です

`prettyTextualDescription` プロパティを使用して、`SnakesAndLadders` インスタンスの説明をきれいに表示できるようになりました:

```swift
print(game.prettyTextualDescription)
// A game of Snakes and Ladders with 25 squares:
// ○ ○ ▲ ○ ○ ▲ ○ ○ ▲ ▲ ○ ○ ○ ▼ ○ ○ ○ ○ ▼ ○ ○ ▼ ○ ▼ ○
```

## <a id="class-only-protocols">クラス専用プロトコル\(Class-Only Protocols\)</a>

`AnyObject` プロトコルをプロトコル継承の一覧に追加することで、プロトコルへの準拠を\(構造体や列挙型ではなく\)クラス型に制限できます:

```swift
protocol SomeClassOnlyProtocol: AnyObject, SomeInheritedProtocol {
    // クラス専用プロトコルの実装をここに
}
```

上記の例では、`SomeClassOnlyProtocol` はクラス型でのみ準拠できます。`SomeClassOnlyProtocol` に準拠しようとする構造体または列挙型の定義を作成すると、コンパイルエラーになります。

> NOTE  
> そのプロトコルの要件によって定義された動作が、準拠する型が値型のセマンティクスではなく参照型のセマンティクスであることを想定\(要求\)する場合は、クラス専用プロトコルを使用してください。参照型と値型のセマンティクスの詳細については、[Structures and Enumerations Are Value Types\(構造体と列挙型は値型\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide-gaido/structures-and-classes#structures-and-enumerations-are-value-type)、[Classes Are Reference Types\(クラスは参照型\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide-gaido/structures-and-classes#classes-are-reference-types)を参照ください。

## <a id="protocol-composition">プロトコル合成\(Protocol Composition\)</a>

同時に複数のプロトコルに準拠すると便利な場合があります。プロトコル合成を使用して、複数のプロトコルを 1 つの要件に組み合わせることができます。プロトコル合成は、合成内の全てのプロトコルの要件を組み合わせた一時的なローカルプロトコルを定義したかのように動作します。プロトコル合成は、新しいプロトコル型を定義しません。

プロトコル合成は `SomeProtocol & AnotherProtocol` という形式で記述します。アンパサンド \(`&`\) で区切って、必要な数のプロトコルを並べることができます。プロトコルに加えて、プロトコル合成には、必要なスーパークラスを 1 つ含めることもできます。

`Named` と `Aged` という 2 つのプロトコルを組み合わせて、単一の関数パラメータに指定した例を次に示します:

```swift
protocol Named {
    var name: String { get }
}
protocol Aged {
    var age: Int { get }
}
struct Person: Named, Aged {
    var name: String
    var age: Int
}
func wishHappyBirthday(to celebrator: Named & Aged) {
    print("Happy birthday, \(celebrator.name), you're \(celebrator.age)!")
}
let birthdayPerson = Person(name: "Malcolm", age: 21)
wishHappyBirthday(to: birthdayPerson)
// "Happy birthday, Malcolm, you're 21!"
```

この例では、`Named` プロトコルには、`name` という `String` 型のプロパティの get 要件が 1 つのあります。`Aged` プロトコルには、`age` と呼ばれる `Int` 型のプロパティの get 要件が 1 つあります。どちらのプロトコルも、`Person` と呼ばれる構造体に準拠されています。

この例では、`wishHappyBirthday(to:)` 関数も定義されています。`celebrator` パラメータの型は `Named & Aged` です。これは、「`Named` プロトコルと `Aged` プロトコルの両方に準拠する任意の型」を意味します。必要なプロトコルの両方に準拠している限り、関数に渡される特定の型は問題ではありません。

この例では、`birthdayPerson` という名前の新しい `Person` インスタンスを作成し、この新しいインスタンスを `wishHappyBirthday(to:)` 関数に渡しています。`Person` は両方のプロトコルに準拠しているため、`wishHappyBirthday(to:)` 関数は誕生日の挨拶を出力できます。

先ほどの例の `Named` プロトコルと `Location` クラスを組み合わせた例を次に示します:

```swift
class Location {
    var latitude: Double
    var longitude: Double
    init(latitude: Double, longitude: Double) {
        self.latitude = latitude
        self.longitude = longitude
    }
}
class City: Location, Named {
    var name: String
    init(name: String, latitude: Double, longitude: Double) {
        self.name = name
        super.init(latitude: latitude, longitude: longitude)
    }
}
func beginConcert(in location: Location & Named) {
    print("Hello, \(location.name)!")
}

let seattle = City(name: "Seattle", latitude: 47.6, longitude: -122.3)
beginConcert(in: seattle)
// "Hello, Seattle!"
```

`beginConcert(in:)` 関数は、`Location & Named` 型のパラメータを受け取ります。これは、「`Location` のサブクラスで、`Named` プロトコルに準拠する任意の型」を意味します。この場合、`City` は両方の要件を満たしています。

`Person` は `Location` のサブクラスではないため、`beginConcert(in:)` 関数に `birthdayPerson` を渡すことは無効です。同様に、`Named` プロトコルに準拠していない `Location` のサブクラスを作成した場合、その型のインスタンスで `beginConcert(in:)` を呼び出すことも無効です。

## <a id="checking-for-protocol-conformance">プロトコル準拠チェック\(Checking for Protocol Conformance\)</a>

[Type Casting\(型キャスト\)](type-casting.md)で説明されている `is` および `as` 演算子を使用して、プロトコルの準拠を確認し、特定のプロトコルにキャストできます。プロトコルのチェックと型へのキャストは、型のチェックやキャストとまったく同じ構文を使用します。

* `is` 演算子は、インスタンスがプロトコルに準拠している場合は `true` を返し、準拠していない場合は `false` を返します
* ダウンキャスト演算子の `as?` は、プロトコルの型のオプショナル値を返します。インスタンスがそのプロトコルに準拠していない場合、この値は `nil` です
* ダウンキャスト演算子の `as!` は、強制的にダウンキャストし、ダウンキャストが成功しない場合は実行時エラーを引き起こします

この例では、`HasArea` というプロトコルを定義しています。このプロトコルには、`area` という `Double` 型の単一のプロパティの get 要件があります:

```swift
class Circle: HasArea {
    let pi = 3.1415927
    var radius: Double
    var area: Double { return pi * radius * radius }
    init(radius: Double) { self.radius = radius }
}
class Country: HasArea {
    var area: Double
    init(area: Double) { self.area = area }
}
```

`Circle` と `Country` の 2 つのクラスがあり、どちらも `HasArea` プロトコルに準拠しています:

```swift
class Circle: HasArea {
    let pi = 3.1415927
    var radius: Double
    var area: Double { return pi * radius * radius }
    init(radius: Double) { self.radius = radius }
}
class Country: HasArea {
    var area: Double
    init(area: Double) { self.area = area }
}
```

`Circle` クラスは、`radius` という格納プロパティに基づいて、計算プロパティ `area` プロパティの要件を実装します。`Country` クラスは、格納プロパティとして `area` プロパティを直接実装します。どちらのクラスも、`HasArea` プロトコルに正しく準拠しています。

下記は、`HasArea` プロトコルに準拠していない `Animal` というクラスです:

```swift
class Animal {
    var legs: Int
    init(legs: Int) { self.legs = legs }
}
```

`Circle`、`Country`、および `Animal` クラスには、共通の基本クラスがありません。とはいえ、それらは全てクラスなため、3 つ全ての型のインスタンスを使用して、`AnyObject` 型の値を格納する配列を初期化できます:

```swift
let objects: [AnyObject] = [
    Circle(radius: 2.0),
    Country(area: 243_610),
    Animal(legs: 4)
]
```

`objects` の配列は、半径 2 の `Circle` インスタンス、英国の表面積を平方キロメートル単位で初期化した `Country` インスタンス、そして 4 本の足を持つ Animal インスタンスを含む配列リテラルで初期化されています。

`objects` 配列を繰り返し処理して、配列内の各オブジェクトをチェックして、`HasArea` プロトコルに準拠しているかどうかを確認できます:

```swift
for object in objects {
    if let objectWithArea = object as? HasArea {
        print("Area is \(objectWithArea.area)")
    } else {
        print("Something that doesn't have an area")
    }
}
// Area is 12.5663708
// Area is 243610.0
// Something that doesn't have an area
```

配列内のオブジェクトが `HasArea` プロトコルに準拠している場合は、常に `as?` 演算子は、`objectWithArea` と呼ばれる定数へオプショナルバインドでアンラップされます。`objectWithArea` 定数は `HasArea` 型なことがわかっているため、その `area` プロパティは型安全な方法でアクセスおよび出力できます。

プロトコルに準拠したオブジェクトは、キャストで変更されないことに注目してください。それらはそれぞれ `Circle`、`Country`、`Animal` です。ただし、オブジェクトが `objectWithArea` 定数に格納されている時点では、`HasArea` 型とのみ認識されているため、アクセスできるのは `area` プロパティのみです。

## <a id="optional-protocol-requirements">オプショナルのプロトコル要件\(Optional Protocol Requirements\)</a>

プロトコルにオプショナルの要件を定義できます。これらの要件は、プロトコルに準拠する型によって実装される必要はありません。オプショナルの要件には、プロトコルの定義の一部として `optional` 修飾子が付けられます。Objective-C と相互運用するコードを作成できるように、オプショナルの要件が利用可能です。プロトコルとオプショナルの要件の両方が `@objc` 属性でマークされている必要があります。`@objc` プロトコルは、Objective-C クラスまたは他の `@objc` クラスから継承するクラスによってのみ準拠できることに注目してください。構造体や列挙型が準拠することはできません。

オプショナルの要件でメソッドまたはプロパティを使用すると、その型は自動的にオプショナルになります。たとえば、`(Int) -> String` 型のメソッドは `((Int) -> String)?` になります。メソッドの戻り値ではなく、関数型全体がオプショナルになっていることに注目してください。

プロトコルに準拠する型によって要件が実装されていない可能性を考慮して、オプショナルのプロトコル要件をオプショナルチェーンで呼び出すことができます。オプショナルメソッドの実装を確認するには、メソッドの呼び出し時にメソッド名の後に疑問符\(`?`\)を記述します\(`someOptionalMethod?(someArgument)` など\)。オプショナルチェーンについては、[Optional Chaining\(オプショナルチェーン\)](optional-chaining.md)を参照ください。

次の例では、`Counter` という整数をカウントするクラスを定義しています。これは、外部データソースを使用して増分量を提供します。このデータソースは、次の 2 つのオプショナルの要件がある `CounterDataSource` プロトコルによって定義されます:

```swift
@objc protocol CounterDataSource {
    @objc optional func increment(forCount count: Int) -> Int
    @objc optional var fixedIncrement: Int { get }
}
```

`CounterDataSource` プロトコルは、`increment(forCount:)` と呼ばれるオプショナルメソッドの要件と、`fixedIncrement` と呼ばれるオプショナルのプロパティの要件を定義しています。これらの要件は、データソースが `Counter` インスタンスに適切な増分量を提供するための 2 つの異なる方法を定義します。

> NOTE  
> 厳密に言えば、いずれのプロトコル要件を実装していなくても、`CounterDataSource` に準拠する独自クラスを作成できます。結局のところ、どちらもオプショナルです。技術的には許可されていますが、これはあまり有用なデータソースではありません。

以下で定義される `Counter` クラスには、`CounterDataSource?` 型のオプショナルの `dataSource` プロパティがあります:

```swift
class Counter {
    var count = 0
    var dataSource: CounterDataSource?
    func increment() {
        if let amount = dataSource?.increment?(forCount: count) {
            count += amount
        } else if let amount = dataSource?.fixedIncrement {
            count += amount
        }
    }
}
```

`Counter` クラスは、現在の値を `count` という変数プロパティに保存します。`Counter` クラスは、メソッドが呼び出されるたびに `count` プロパティを増加する `increment` というメソッドも定義します。

`increment()` メソッドは、最初に、データソースで `increment(forCount:)` メソッドの実装を呼び出して、増分量を取得しようとします。`increment()` メソッドは、オプショナルチェーンを使用して `increment(forCount:)` の呼び出しを試み、現在の `count` をメソッドの単一の引数として渡します。

ここでは、2 つの階層のオプショナルチェーンが行われていることに注目してください。まず、`dataSource` が `nil` の可能性があるため、`dataSource` にはその名前の後に疑問符\(`?`\)があり、`dataSource` が `nil` でない場合にのみ `increment(forCount:)` を呼び出す必要があることを示しています。次に、`dataSource` が存在する場合でも、`increment(forCount:)` がオプショナルのため、必ず実装されている保証はありません。ここで、`increment(forCount:)` が実装されない可能性も、オプショナルチェーンによって処理されます。`increment(forCount:)` の呼び出しは、`increment(forCount:)` が存在する場合、つまり `nil` でない場合にのみ発生します。これが `increment(forCount:)` にも名前の後に疑問符が付いている理由です。

`increment(forCount:)` の呼び出しはこれら 2 つの理由のいずれかで失敗する可能性があるため、呼び出しはオプショナルの `Int` 値を返します。これは、`CounterDataSource` の定義で `increment(forCount:)` がオプショナルではない `Int` 値を返すように定義されている場合でも当てはまります。オプショナルチェーンは 2 回行われていますが、結果は 1 つのオプショナルにラップされます。複数のオプショナルチェーンの詳細については、[Linking Multiple Levels of Chaining\(複数階層のチェーンへのリンク\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide-gaido/optional-chaining#linking-multiple-levels-of-chaining)を参照ください。

`increment(forCount:)` を呼び出した後、それが返すオプショナルの `Int` は、オプショナルバインディングを使用して、`amount` という定数にアンラップされます。オプショナルの `Int` に値が含まれている場合、つまり、デリゲートとメソッドの両方が存在し、メソッドが値を返した場合、`count` 格納プロパティに値が追加され、増加の動作が完了します。

`increment(forCount:)` メソッドから値を取得できない場合\(`dataSource` が `nil` か、データソースが `increment(forCount:)` を実装していないため\)、 `increment()` メソッドはデータソースの `fixedIncrement` プロパティから値を取得しようとします。`fixedIncrement` プロパティもオプショナルの要件のため、その値はオプショナルの `Int` 値です。ただし、`fixedIncrement` は、`CounterDataSource` プロトコル定義の一部として非オプショナルの `Int` プロパティとして定義されています。

下記は、`fixedIncrement` にアクセスされる度にデータソースが定数 `3` を返す簡単な `CounterDataSource` の実装です。これは、オプショナルの `fixedIncrement` プロパティ要件を実装することでこれを行います。

```swift
class ThreeSource: NSObject, CounterDataSource {
    let fixedIncrement = 3
}
```

`ThreeSource` のインスタンスを新しい `Counter` インスタンスのデータソースとして使用できます:

```swift
var counter = Counter()
counter.dataSource = ThreeSource()
for _ in 1...4 {
    counter.increment()
    print(counter.count)
}
// 3
// 6
// 9
// 12
```

上記のコードは、新しい `Counter` インスタンスを作成します。データソースを新しい `ThreeSource` インスタンスに設定します。そして、カウンタの `increment()` メソッドを 4 回呼び出します。予想どおり、カウンタの count プロパティは、`increment()` が呼び出されるたびに 3 ずつ増加します。

下記は `TowardsZeroSource` と呼ばれるより複雑なデータソースで、`Counter` インスタンスを現在の `count` からゼロに向かってカウントアップまたはカウントダウンします:

```swift
class TowardsZeroSource: NSObject, CounterDataSource {
    func increment(forCount count: Int) -> Int {
        if count == 0 {
            return 0
        } else if count < 0 {
            return 1
        } else {
            return -1
        }
    }
}
```

`TowardsZeroSource` クラスは、`CounterDataSource` プロトコルからオプショナルの `increment(forCount:)` メソッドを実装し、`count` 引数を使用して、カウントする方向を決定します。`count` が既にゼロの場合、メソッドは `0` を返し、それ以上のカウントが行われないことを示します。

`TowardsZeroSource` のインスタンスを既存の `Counter` インスタンスとともに使用して、-4 から 0 までカウントできます。カウンタがゼロに達すると、それ以上のカウントは行われません:

```swift
counter.count = -4
counter.dataSource = TowardsZeroSource()
for _ in 1...5 {
    counter.increment()
    print(counter.count)
}
// -3
// -2
// -1
// 0
// 0
```

## <a id="protocol-extensions">プロトコル Extension\(Protocol Extensions\)</a>

プロトコル Extension を使用して、準拠する型にメソッド、イニシャライザ、subscript、および計算プロパティの実装を提供できます。これにより、準拠する個々の型やグローバル関数ではなく、プロトコル自体に動作を定義できます。

例えば、`RandomNumberGenerator` プロトコルを拡張して、必須要件の `random()` メソッドを使用してランダムな `Bool` 値を返す `randomBool()` メソッドを提供できます:

```swift
extension RandomNumberGenerator {
    func randomBool() -> Bool {
        return random() > 0.5
    }
}
```

プロトコル Extension を作成することにより、全ての準拠する型は、追加の変更なしでこのメソッドの実装を自動的に使用することができます。

```swift
let generator = LinearCongruentialGenerator()
print("Here's a random number: \(generator.random())")
// "Here's a random number: 0.3746499199817101"
print("And here's a random Boolean: \(generator.randomBool())")
// "And here's a random Boolean: true"
```

プロトコル Extension は、準拠する型に実装を追加できますが、プロトコルにさらに定義を追加して拡張したり、別のプロトコルを継承することはできません。プロトコルの継承は、常にプロトコル宣言内で指定されます。

### デフォルト実装の提供\(Providing Default Implementations\)

プロトコル Extension を使用して、そのプロトコルのメソッドまたは計算プロパティ要件にデフォルトの実装を提供できます。準拠する型が要件のメソッドまたはプロパティの実装を提供している場合、プロトコル Extension によって提供されるものの代わりに、準拠する型の実装が使用されます。

> NOTE  
> extension によって提供されるプロトコル要件のデフォルト実装は、オプショナルのプロトコル要件とは異なります。準拠する型は、独自の実装を提供する必要はありませんが、デフォルトの実装を持つ要件は、オプショナルチェーンなしで呼び出すことができます。

例えば、`TextRepresentable` プロトコルを継承する `PrettyTextRepresentable` プロトコルは、要件の `prettyTextualDescription` プロパティのデフォルト実装を提供して、`textualDescription` プロパティにアクセスした結果を返すことができます:

```swift
extension PrettyTextRepresentable  {
    var prettyTextualDescription: String {
        return textualDescription
    }
}
```

### プロトコル Extensionに制約の追加\(Adding Constraints to Protocol Extensions\)

プロトコル Extension を定義するとき、準拠する型が拡張したメソッドとプロパティを使用できる前に、その型が満たす必要がある制約を指定できます。これらの制約は、拡張するプロトコルの名前の後のジェネリックの `where` 句によって記述されます。ジェネリック `where` 句の詳細については、[Generic Where Clauses\(ジェネリック where 句\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide-gaido/generics#generic-where-clauses)を参照ください。

例えば、要素が `Equatable` プロトコルに準拠しているコレクションに適用される `Collection` プロトコルの拡張を定義できます。コレクションの要素を標準ライブラリの `Equatable` プロトコルに制約することで、`==` および `!=` 演算子を使用して、2 つの要素間の等価性と不等価性をチェックできます:

```swift
extension Collection where Element: Equatable {
    func allEqual() -> Bool {
        for element in self {
            if element != self.first {
                return false
            }
        }
        return true
    }
}
```

`allEqual()` メソッドは、コレクション内の全ての要素が等しい場合にのみ `true` を返します。

1 つは全ての要素が同じで、もう 1 つは要素が同じでない、整数の 2 つの配列を考えます:

```swift
let equalNumbers = [100, 100, 100, 100, 100]
let differentNumbers = [100, 100, 200, 100, 200]
```

配列は `Collection` に準拠し、整数は `Equatable` に準拠しているため、 `equalNumbers` および `differentNumbers` は `allEqual()` メソッドを使用できます:

```swift
print(equalNumbers.allEqual())
// "true"
print(differentNumbers.allEqual())
// "false"
```

> NOTE  
> 準拠する型が、複数の制約が付いた拡張の要件を満たす同じメソッドまたはプロパティを実装している場合、Swift は最も厳しい制約がついている実装を選択して使用します。

