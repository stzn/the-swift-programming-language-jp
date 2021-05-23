# A Swift Tour

最終更新日: 2020/5/9

伝統的に、新しい言語の最初のプログラミングは "Hello world!" を画面に表示することを推めます。Swift ではたった 1 行でできます。

```swift
print("Hello, world!")
// Prints "Hello, world!"
```

もし C 言語や Objective-C の経験があるならばこのシンタックスに馴染みがあるかもしれませんが、Swift ではこの 1 行のコードは完成したプログラミングです。input/output や文字列を扱うために他のライブラリを import する必要もありません。グローバル領域に書かれたコードはプログラムのエントリポイントとして使用されます。`main()` 関数は必要ありません。全てのステートメントの末尾にセミコロンをつける必要もありません。

このツアーでは、様々なプログラム上のタスクを遂行する方法を示すことで、Swift でコードを書き始めるために必要十分な情報を提供します。もし理解できなくても心配しないでください。このツアーで紹介することは全て、この本で後々詳細に説明しています。

> NOTE  
> より理解を深めるために、Xcode でこのチャプターを開いて遊んでみましょう。Playgrounds を使うと、コードを編集して結果をすぐに確認することができます。
>
> [Download Playground](https://docs.swift.org/swift-book/GuidedTour/GuidedTour.playground.zip)

## Simple Values

定数を作成するのに `let`、変数を作成するのに `var` を使います。定数の値はコンパイル時に知る必要はありませんが、正確に一度だけ値を設定しなければなりません。つまり、値を指定をたった一度だけ行い、他のあらゆる場所から利用するために定数を活用できます。

```swift
var myVariable = 42
myVariable = 50
let myConstant = 42
```

定数や変数は設定したい値と同じ型でなければなりません。しかし、明示的に型を記載する必要はありません。定数や変数を作成する時に値を与えることで、コンパイラが型を推論します。例えば、コンパイラは `myVariable` の値が integer ということから、型を integer と推論します。

もし、最初に設定する値が十分な情報を提供しなかった場合\(もしくは最初に設定する値ではなかった場合\)、変数の後にコロンを付けて型を書くことで特定することができます。

```swift
let implicitInteger = 70
let implicitDouble = 70.0
let explicitDouble: Double = 70
```

> Experiment  
> 値が 4 で Float を明示的に指定した定数を作成してみましょう。

値は暗黙的に他の型に変換されることはありません。他の型に変換したい場合は、変換したい型のインスタンスを明示的に作成してください。

```swift
let label = "The width is "
let width = 94
let widthLabel = label + String(width)
```

> Experiment  
> 最後の行から String への変換を消してみてください。どんなエラーが起きるでしょうか？

もっと簡単な方法で文字列の中に値を含めることができます。値を括弧で囲み、括弧の前にバックスラッシュをつけます。

```swift
let apples = 3
let oranges = 5
let appleSummary = "I have \(apples) apples."
let fruitSummary = "I have \(apples + oranges) pieces of fruit."
```

> Experiment  
> 文字列に浮動小数点の計算を含めたり、誰かの名前を greeting に含めるために、\(\) を使用してみましょう。

複数行の文字列に対しては、3 つのダブルクォーテーション\(`"""`\)を使いましょう。末尾のクォーテーションに到達するまで、各行の最初のインデントは除外されます。

```swift
let quotation = """
I said "I have \(apples) apples."
And then I said "I have \(apples + oranges) pieces of fruit."
"""
```

Array や Dictionary には角括弧\(\[\]\)を使い、それらの要素には角括弧内に index や key を書きます。最後の要素の後ろにもカンマを付けることができます。

```swift
var shoppingList = ["catfish", "water", "tulips"]
shoppingList[1] = "bottle of water"

var occupations = [
    "Malcolm": "Captain",
    "Kaylee": "Mechanic",
]
occupations["Jayne"] = "Public Relations"
```

Array は要素を追加すると自動でサイズを増やします。

```swift
shoppingList.append("blue paint")
print(shoppingList)
```

空の Array や Dictionary を作成するためには、初期化のシンタックスを使います。

```swift
let emptyArray = [String]()
let emptyDictionary = [String: Float]()
```

型が推論できる場合は、空の Array は\[\]、Dictionary は\[:\]で書くことができます。\(例えば、変数に新しい値を設定する場合や関数に引数を渡す場合など\)

```swift
shoppingList = []
occupations = [:]
```

## Control Flow

条件を作成するには、 `if` と `switch` を使います。ループを作成するには `for-in` `while` `repeat-while` を使います。条件や optional に丸括弧\(\(\)\)を付けるかどうかは任意です。本文の周りの中括弧\({}\)は必須です。

```swift
let individualScores = [75, 43, 103, 87, 12]
var teamScore = 0
for score in individualScores {
    if score > 50 {
        teamScore += 3
    } else {
        teamScore += 1
    }
}
print(teamScore)
// Prints "11"
```

`if` ステートメントの中で、条件は Boolean でなければなりません。\(つまり、`if score { ... }` などのコードはエラーか暗黙的に 0 と推論されます\)

`if` と `let` を一緒に使用して、存在しないかもしれない値を扱うことができます。これらの値は optional で表現されます。optional は値が含まれているか、値が存在しないことを示す `nil` を含んでいます。値を optional にするには、値の型の後ろにクエスチョンマーク\(?\)を書きましょう。

```swift
var optionalString: String? = "Hello"
print(optionalString == nil)
// Prints "false"

var optionalName: String? = "John Appleseed"
var greeting = "Hello!"
if let name = optionalName {
    greeting = "Hello, \(name)"
}
```

> Experiment  
> optionalName を nil に変えてみましょう。 どんな greeting が出力されるでしょうか？ optionalName が nil の場合に別の greeting が出力されるように `else` を追加してみましょう。

もし optional の値が nil の場合、条件は false になり、中括弧内のコードはスキップされます。nil でなければ、optional の値はアンラップされて `let` の後の定数に値が設定され、中括弧内のブロックでその値を利用できます。

optional の値を扱うもう 1 つの方法として、?? を使用してデフォルトの値を提供します。もし optional の値が nil の場合、代わりにデフォルトの値が使われます。

```swift
let nickname: String? = nil
let fullName: String = "John Appleseed"
let informalGreeting = "Hi \(nickname ?? fullName)"
```

`switch` は、あらゆる種類のデータと比較のための operator を扱うことができます。\( integer と等価チェックだけに限定されません\)

```swift
let vegetable = "red pepper"
switch vegetable {
case "celery":
    print("Add some raisins and make ants on a log.")
case "cucumber", "watercress":
    print("That would make a good tea sandwich.")
case let x where x.hasSuffix("pepper"):
    print("Is it a spicy \(x)?")
default:
    print("Everything tastes good in soup.")
}
// Prints "Is it a spicy red pepper?"
```

> Experiment  
> default の case を削除してみましょう。どんなエラーが起きるでしょうか？

パターンに合った値を定数に設定するために `let` がどう使われるかに注目してください。

パターンに合った switch case 内のコードが実行された後、プログラムは switch ステートメントから抜け出します。次の case は実行されないので、各 case の最後に明示的に break を書く必要はありません。

`for-in` を使うことで Dictionary のそれぞれの要素の key-value ペアを受け取って、Dictionary 内のアイテムを反復処理をすることができます。Dictionary は順序のないコレクションなので、key-value ペアの受け取る順序は決まっていません。

```swift
let interestingNumbers = [
    "Prime": [2, 3, 5, 7, 11, 13],
    "Fibonacci": [1, 1, 2, 3, 5, 8],
    "Square": [1, 4, 9, 16, 25],
]
var largest = 0
for (_, numbers) in interestingNumbers {
    for number in numbers {
        if number > largest {
            largest = number
        }
    }
}
print(largest)
// Prints "25"
```

> Experiment  
> \_ を変数の名前に置き換えてみて、どの種類の値が最大だったかを追ってみましょう。

`while` を使うことで条件が変わるまで、ブロック内のコードを反復して実行できます。ループの条件を最後に置くことで、ループ内のブロックが少なくとも 1 回実行されるようにすることができます。

```swift
var n = 2
while n < 100 {
    n *= 2
}
print(n)
// Prints "128"

var m = 2
repeat {
    m *= 2
} while m < 100
print(m)
// Prints "128"
```

`..<` を使うと、index の range を生成でき、ループの index を追うことができます。

```swift
var total = 0
for i in 0..<4 {
    total += i
}
print(total)
// Prints "6
```

`..<` では後の値は除外され、`...` は両方の値を含みます。

## Functions and Closures

関数の定義には `func` を使います。\(\)の中に引数のリスト、その前に関数の名前を付けることで関数の呼び出します。 また、`->` の後ろに戻り値の型を指定して、関数名や引数と区別します。

```swift
func greet(person: String, day: String) -> String {
    return "Hello \(person), today is \(day)."
}
greet(person: "Bob", day: "Tuesday")
```

> Experiment  
> day 引数を削除して、greet に今日の lunch special を指定するための引数を追加してみましょう。

デフォルトで、関数は引数名をそのまま引数ラベルとして使用します。カスタムの引数ラベルを設定したい場合は、引数名の前に引数ラベルを記載しましょう。引数にラベルが不要な場合は、`_` を書きましょう。

```swift
func calculateStatistics(scores: [Int]) -> (min: Int, max: Int, sum: Int) {
    var min = scores[0]
    var max = scores[0]
    var sum = 0

    for score in scores {
        if score > max {
            max = score
        } else if score < min {
            min = score
        }
        sum += score
    }

    return (min, max, sum)
}
let statistics = calculateStatistics(scores: [5, 3, 100, 3, 9])
print(statistics.sum)
// Prints "120"
print(statistics.2)
// Prints "120"
```

関数はネストして使うことができます。ネストした関数は、外側で定義された変数にアクセスすることができます。ネスト関数を使うことで、長くて複雑な関数を整理することができます。

```swift
func returnFifteen() -> Int {
    var y = 10
    func add() {
        y += 5
    }
    add()
    return y
}
returnFifteen()
```

関数は第一級オブジェクトです。つまり、関数は値として他の関数を戻り値にすることができます。

```swift
func makeIncrementer() -> ((Int) -> Int) {
    func addOne(number: Int) -> Int {
        return 1 + number
    }
    return addOne
}
var increment = makeIncrementer()
increment(7)
```

引数に他の関数を受け取ることもできます。

```swift
func hasAnyMatches(list: [Int], condition: (Int) -> Bool) -> Bool {
    for item in list {
        if condition(item) {
            return true
        }
    }
    return false
}
func lessThanTen(number: Int) -> Bool {
    return number < 10
}
var numbers = [20, 19, 7, 12]
hasAnyMatches(list: numbers, condition: lessThanTen)
```

関数は実はクロージャの特殊なケースです。クロージャは、後で呼ばれる可能性があるコードブロックのことを指します。クロージャ内のコードは、そのクロージャが作成されたスコープで利用可能な変数や関数へアクセスすることができます。これは、実際に実行されるのが別のスコープ\(タイミング\)の場合でも当てはまります。\(上記のネストした関数の例でも同じことが見られます\)。中括弧\({}\)で囲むことで、名前なしのクロージャを作成することもできます。`in` を使用することで、コードの本文から引数と戻り値を分離することができます。

```swift
numbers.map({ (number: Int) -> Int in
    let result = 3 * number
    return result
})
```

> Experiment  
> 上記のクロージャを、全ての奇数で0を返すように書き換えてみましょう。

より簡潔にクロージャを書く複数の方法があります。クロージャの型が既にわかっている場合\(デリゲートのコールバックなど\)、引数、戻り値の型を省略できます。1 つのステートメントのみのクロージャの場合、暗黙的に、そのステートメントの型が戻り値の型になります。

```swift
let sortedNumbers = numbers.sorted { $0 > $1 }
print(sortedNumbers)
// Prints "[20, 19, 12, 7]"
```

## Objects and Classes

`class` を class 名の前に付けることで class を作成することができます。class 内のプロパティの宣言は、class 内にあるということを除いて、定数や変数の宣言方法と同じです。同様に、メソッドや関数の宣言も同じように書くことができます。

```swift
class Shape {
    var numberOfSides = 0
    func simpleDescription() -> String {
        return "A shape with \(numberOfSides) sides."
    }
}
```

> Experiment  
> let を使用して定数プロパティを追加しましょう。また、引数を受け取る別のメソッドも追加してみましょう。

class のインスタンスは、class 名の後に丸括弧\(\(\)\)を付けて生成します。そのインスタンスのプロパティやメソッドにアクセスするには、ドット\(.\)シンタックスを使います。

```swift
var shape = Shape()
shape.numberOfSides = 7
var shapeDescription = shape.simpleDescription()
```

このバージョンの `Shape` class は重要なことが抜けています。それは、インスタンスを生成するときに class を構築するためのイニシャライザです。生成するために `init` を使います。

```swift
class NamedShape {
    var numberOfSides: Int = 0
    var name: String

    init(name: String) {
        self.name = name
    }

    func simpleDescription() -> String {
        return "A shape with \(numberOfSides) sides."
    }
}
```

イニシャライザ内で `self` を使用して `name` プロパティと `name` 引数を区別していることに注目してください。インスタンスの作成時、イニシャライザの引数は関数の呼び出しと同じように渡されます。全てのプロパティは宣言時に値を設定するか\(`numberOfSides` 参照\)、イニシャライザ内で値を設定する必要があります。\(`name` 参照\)

インスタンスが開放される前に何かクリーンアップ作業が必要な場合、デイニシャライザを作成するために `deinit` を使います。

subclass は、その class 名の後に、superclass の名前をコロンで区切って指定します。標準で定義されているルート class の subclass にする必要は必ずしもありません。追加も省略もできます。

superclass のメソッドを subclass で override する場合は `override` を付けます。意図せずに override してしまった場合は、コンパイラがエラーにします。`override` を付けていても、`override` が正しく行われていない場合も、コンパイラはエラーにします。

```swift
class Square: NamedShape {
    var sideLength: Double

    init(sideLength: Double, name: String) {
        self.sideLength = sideLength
        super.init(name: name)
        numberOfSides = 4
    }

    func area() -> Double {
        return sideLength * sideLength
    }

    override func simpleDescription() -> String {
        return "A square with sides of length \(sideLength)."
    }
}
let test = Square(sideLength: 5.2, name: "my test square")
test.area()
test.simpleDescription()
```

> Experiment  
> radius と name をイニシャライザの引数に受けとる `Circle` という名前の `NameShape` の別の subclass を作ってみましょう。そして、 `Circle` class に area\(\), simpleDescription\(\) メソッドを実装してみましょう。

単純に値を保持するプロパティ以外に、プロパティが getter と setter を持つこともできます。

```swift
class EquilateralTriangle: NamedShape {
    var sideLength: Double = 0.0

    init(sideLength: Double, name: String) {
        self.sideLength = sideLength
        super.init(name: name)
        numberOfSides = 3
    }

    var perimeter: Double {
        get {
            return 3.0 * sideLength
        }
        set {
            sideLength = newValue / 3.0
        }
    }

    override func simpleDescription() -> String {
        return "An equilateral triangle with sides of length \(sideLength)."
    }
}
var triangle = EquilateralTriangle(sideLength: 3.1, name: "a triangle")
print(triangle.perimeter)
// Prints "9.3"
triangle.perimeter = 9.9
print(triangle.sideLength)
// Prints "3.3000000000000003"
```

`perimeter` の setter の中で、新しい値は暗黙的に `newValue` という名前になります。 `set` の後に丸括弧\(\(\)\)で囲んで明示的に指定することもできます。

`EquilateralTriangle` class のイニシャライザは 3 つの異なるステップがあります。

1. subclass で宣言されたプロパティに値を設定
2. superclass のイニシャライザを呼ぶ
3. superclass で定義されたプロパティの値を変更。この時点で getter setter メソッドを使用して他のセットアップ処理を実行

プロパティを計算する必要はないけれども、新しい値を設定する前後で何かコードを実行したい場合、`willSet`, didSet\` を使います。このコードは、イニシャライザ以外で値が変更された時に毎回実行されます。例えば、下の class は三角形の辺の長さが常に四角形の辺の長さと同じになります。

```swift
class TriangleAndSquare {
    var triangle: EquilateralTriangle {
        willSet {
            square.sideLength = newValue.sideLength
        }
    }
    var square: Square {
        willSet {
            triangle.sideLength = newValue.sideLength
        }
    }
    init(size: Double, name: String) {
        square = Square(sideLength: size, name: name)
        triangle = EquilateralTriangle(sideLength: size, name: name)
    }
}
var triangleAndSquare = TriangleAndSquare(size: 10, name: "another test shape")
print(triangleAndSquare.square.sideLength)
// Prints "10.0"
print(triangleAndSquare.triangle.sideLength)
// Prints "10.0"
triangleAndSquare.square = Square(sideLength: 50, name: "larger square")
print(triangleAndSquare.triangle.sideLength)
// Prints "50.0"
```

optional な値を扱う場合、 `?` をメソッド、プロパティ、subscript のような操作の前に書きます。もし `?` の前の値が `nil` の場合、`?` の後の処理は全て無視され、その式全体の値は `nil` になります。それ以外は、optional の値はアンラップされて、`?` の後は全てアンラップされた値として実行されます。どちらの場合も、式全体は optinal な値です。

```swift
let optionalSquare: Square? = Square(sideLength: 2.5, name: "optional square")
let sideLength = optionalSquare?.sideLength
```

## Enumerations and Structures

列挙型を作成するには `enum` を使います。class やその他の名前の付いた型と同様に、列挙型もメソッドを持つことができます。

```swift
enum Rank: Int {
    case ace = 1
    case two, three, four, five, six, seven, eight, nine, ten
    case jack, queen, king

    func simpleDescription() -> String {
        switch self {
        case .ace:
            return "ace"
        case .jack:
            return "jack"
        case .queen:
            return "queen"
        case .king:
            return "king"
        default:
            return String(self.rawValue)
        }
    }
}
let ace = Rank.ace
let aceRawValue = ace.rawValue
```

> Experiment  
> 2つの `Rank` の raw value を比較して `Rank` を比較するメソッドを追加してみましょう。

デフォルトで、Swift は 0 から 1 つずつ増加する値を raw value に設定しますが、明示的に値を指定してこの挙動を変更することもできます。上記の例では、`Ace` は明示的に 1 を raw value に指定しており、残りは、1 から順番に raw value が指定されます。文字列や浮動小数点も使うことができます。列挙型の個々の case の raw value には、 `rawValue` プロパティからアクセスすることができます。

raw value から列挙型のインスタンスを生成するためには `init?(rawValue:)` イニシャライザを使います。raw value に合致した case を返すか、合致するものがない場合は `nil` を返します。

```swift
if let convertedRank = Rank(rawValue: 3) {
    let threeDescription = convertedRank.simpleDescription()
}
```

列挙型の case の値は実在の値で、raw value を書くための別の方法ではありません。事実、意味のある raw value が存在しない場合は、raw value を提供する必要はありません。

```swift
enum Suit {
    case spades, hearts, diamonds, clubs

    func simpleDescription() -> String {
        switch self {
        case .spades:
            return "spades"
        case .hearts:
            return "hearts"
        case .diamonds:
            return "diamonds"
        case .clubs:
            return "clubs"
        }
    }
}
let hearts = Suit.hearts
let heartsDescription = hearts.simpleDescription()
```

> Experiment  
> `color()` メソッドを `Suit` に追加してみましょう。`spades` と `clubs` は "black" を返し、`hearts` と `diaminds` は "red" を返します。

`hearts` case を参照するために 2 つの方法があることに注目してください。`hearts` 定数に値を設定する時、列挙型 case の `Suits.hearts` は型を明示的に特定していないため、完全な名前で参照されています。一方、switch 文の中では、`self` が既に `Suit` だとわかっているため、列挙型 case は `.hearts` と省略方法で参照されています。型が既にわかっている場合はいつでも省略方法を使用することができます。

もし列挙型が raw value を持つ場合、これらの値は宣言の一部として決定されます。これはつまり、特定の列挙型 case の全てのインスタンスは、必ず同じ raw value を持つということです。列挙型 case が case に関連した値を持つもう 1 つの方法があります。これらはインスタンス生成時に値が決定され、列挙型 case のインスタンスごとに異なる値を持つ可能性があります。例えば、サーバから日の出時刻と日の入り時刻をリクエストする場合を考えてみてください。サーバはリクエスト情報に応じたレスポンスを返すか、リクエストが間違っている場合は間違っている内容を返します。

```swift
enum ServerResponse {
    case result(String, String)
    case failure(String)
}

let success = ServerResponse.result("6:00 am", "8:09 pm")
let failure = ServerResponse.failure("Out of cheese.")

switch success {
case let .result(sunrise, sunset):
    print("Sunrise is at \(sunrise) and sunset is at \(sunset).")
case let .failure(message):
    print("Failure...  \(message)")
}
// Prints "Sunrise is at 6:00 am and sunset is at 8:09 pm."
```

> Experiment  
> `ServerResponse` に3つ目の case を追加して、switch 文を変更しましょう。

日の出時刻と日の入り時刻が、switch に合致した case の一部として、`ServerResponse` から抽出されていることに注目してください。

struct を作成するには `struct` を使います。struct はメソッドやイニシャライザなど多くの class と同じ挙動をサポートしています。最も重要な違いは、struct はコードの中で渡される時に必ずコピーされる、ということです。class は参照を渡します。

```swift
struct Card {
    var rank: Rank
    var suit: Suit
    func simpleDescription() -> String {
        return "The \(rank.simpleDescription()) of \(suit.simpleDescription())"
    }
}
let threeOfSpades = Card(rank: .three, suit: .spades)
let threeOfSpadesDescription = threeOfSpades.simpleDescription()
```

> Experiment  
> rank と suit を持った全種類の card を含む array を返すメソッドを書いてみよう。

## Protocols and Extensions

protocol を宣言するために `protocol` を使います。

```text
protocol ExampleProtocol {
    var simpleDescription: String { get }
    mutating func adjust()
}
```

class, enum, struct は全て protocol に適合することができます。

```swift
class SimpleClass: ExampleProtocol {
    var simpleDescription: String = "A very simple class."
    var anotherProperty: Int = 69105
    func adjust() {
        simpleDescription += "  Now 100% adjusted."
    }
}
var a = SimpleClass()
a.adjust()
let aDescription = a.simpleDescription

struct SimpleStructure: ExampleProtocol {
    var simpleDescription: String = "A simple structure"
    mutating func adjust() {
        simpleDescription += " (adjusted)"
    }
}
var b = SimpleStructure()
b.adjust()
let bDescription = b.simpleDescription
```

> Experiment  
> `ExampleProtocolrank` にもう1つの要件を追加してみましょう。`SimpleClass` と `SimpleStructure` にどういう変更が必要になりますでしょうか？

struct `SimpleStructure` を変更するメソッドに `mutating` キーワードが付いていることに注目してください。 class のメソッドは class を変更することが許されているため、`mutating` を付ける必要はありません。

既存の型に新しい機能\(メソッドや計算プロパティ\)を追加する場合に、`extension` を使います。他のファイルやモジュールで宣言された型やライブラリやフレームワークからインポートした型に protocol を適合させる場合にも extension を使います。

```swift
extension Int: ExampleProtocol {
    var simpleDescription: String {
        return "The number \(self)"
    }
    mutating func adjust() {
        self += 42
    }
}
print(7.simpleDescription)
// Prints "The number 7"
```

> Experiment  
> `absoluteValue` プロパティを `Double` に追加する extension を書いてみましょう。

protocol の名前は他の名前が付いた型と同じように使うことができます。例えば、同じ 1 つの protocol に適合した異なる型のオブジェクトのコレクションを作成することができます。protocol 自体の型の値を扱っている場合、protocol 外側で定義されたメソッドを使うことはできません。

```swift
let protocolValue: ExampleProtocol = a
print(protocolValue.simpleDescription)
// Prints "A very simple class.  Now 100% adjusted."
// print(protocolValue.anotherProperty)  // Uncomment to see the error
```

変数 `protocolValue` が実行時には `SimpleClass` なことはわかるものの、コンパイラはこれをとある `ExampleProtocol` として扱います。つまり、protocol で定義されたメソッドやプロパティ以外へアクセスすることはできません。

## Error Handling

`Error` protocol に適合することでどんな型もエラーを表現することができます。

```swift
enum PrinterError: Error {
    case outOfPaper
    case noToner
    case onFire
}
```

関数に `throws` を付け、エラーを throw する箇所に `throw` を使うことでエラーを throw することができます。関数内でエラーを throw すると、関数がすぐにリターンして、その関数を呼んだ関数でエラーを処理します。

```swift
func send(job: Int, toPrinter printerName: String) throws -> String {
    if printerName == "Never Has Toner" {
        throw PrinterError.noToner
    }
    return "Job sent"
}
```

エラーを処理する方法は様々あります。1 つは `do-catch` を使うことです。`do` ブロックの中では、エラーを throw する可能性がある箇所の前に `try` を付けるます。`catch` ブロックの中では、明示的に異なる名前を設定しない限り、`error` という名前で自動的にエラー情報が与えられます。

```swift
do {
    let printerResponse = try send(job: 1040, toPrinter: "Bi Sheng")
    print(printerResponse)
} catch {
    print(error)
}
// Prints "Job sent"
```

> Experiment  
> printer の名前を"Never Has Toner"に変えてみましょう。`send(job:toPrinter:)` はエラーを throw します。

特定のエラーを処理するために、複数の `catch` ブロックを書くこともできます。switch 文の `case` のように `catch` の後ろにパターンを書きます。

```swift
do {
    let printerResponse = try send(job: 1440, toPrinter: "Gutenberg")
    print(printerResponse)
} catch PrinterError.onFire {
    print("I'll just put this over here, with the rest of the fire.")
} catch let printerError as PrinterError {
    print("Printer error: \(printerError).")
} catch {
    print(error)
}
// Prints "Job sent"
```

> Experiment  
> `do` ブロックの中でエラーを throw するコードを追加してみましょう。最初の `catch` ブロックでエラーを処理するためにはどの種類のエラーを throw する必要がありますでしょうか？ 2つ目、3つ目の場合はどうでしょうか？

エラーを処理するもう 1 つの方法は、`try?` を付けて結果を optional に変換することです。もしその関数がエラーを throw する場合、特定のエラーは破棄されて、結果が `nil` になります。そうでなければ、結果は、関数が返す値を内包した optional になります。

```swift
let printerSuccess = try? send(job: 1884, toPrinter: "Mergenthaler")
let printerFailure = try? send(job: 1885, toPrinter: "Never Has Toner")
```

関数内の全ての処理が実行されて、関数が結果返す直前にコードを実行したい場合、`defer` を使います。このブロックは関数がエラーを throw しても実行されます。違うタイミングで実行される必要はありますが、`defer` を使用してセットアップ用のコードの次にクリーンアップ用のコードを書くことができます。

```swift
var fridgeIsOpen = false
let fridgeContent = ["milk", "eggs", "leftovers"]

func fridgeContains(_ food: String) -> Bool {
    fridgeIsOpen = true
    defer {
        fridgeIsOpen = false
    }

    let result = fridgeContent.contains(food)
    return result
}
fridgeContains("banana")
print(fridgeIsOpen)
// Prints "false"
```

## Generics

generic な関数や型を作成するには、かぎ括弧\(&lt;&gt;\)の中に名前を書きます。

```swift
func makeArray<Item>(repeating item: Item, numberOfTimes: Int) -> [Item] {
    var result = [Item]()
    for _ in 0..<numberOfTimes {
        result.append(item)
    }
    return result
}
makeArray(repeating: "knock", numberOfTimes: 4)
```

class, enum, struct と同じように、generic な関数やメソッドも作成することができます。

```swift
// Reimplement the Swift standard library's optional type
enum OptionalValue<Wrapped> {
    case none
    case some(Wrapped)
}
var possibleInteger: OptionalValue<Int> = .none
possibleInteger = .some(100)
```

要件の一覧を特定するために、本文の直前に `where` を使います。例えば、protocol を実装するために型が必要な場合や、2 つの型が一致している必要がある場合、class が特定の superclass を継承している必要がある場合など。

```swift
func anyCommonElements<T: Sequence, U: Sequence>(_ lhs: T, _ rhs: U) -> Bool
    where T.Element: Equatable, T.Element == U.Element
{
    for lhsItem in lhs {
        for rhsItem in rhs {
            if lhsItem == rhsItem {
                return true
            }
        }
    }
    return false
}
anyCommonElements([1, 2, 3], [3])
```

> Experiment  
> `anyCommonElements(_:_:)` を2つの Sequence で共通の要素が含まれる配列を返すように修正してみよう。

`<T: Equatable>` は `<T> ... where T: Equatable` と同じです。

