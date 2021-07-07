# Swiftツアー\(A Swift Tour\)

最終更新日: 2021/7/7

伝統的に、新しい言語の最初のプログラミングは "Hello world!" を画面に表示することを勧めます。これを Swift ではたった 1 行で達成できます。

```swift
print("Hello, world!")
// "Hello, world!"
```

もし C 言語や Objective-C の経験があるならばこの構文に馴染みがあるかもしれませんが、Swift ではこの 1 行のコードでプログラミングは完成しています。input/output や文字列を扱うために他のライブラリを import する必要もありません。グローバル領域に書かれたコードはプログラムのエントリポイントとして使用されます。`main()` 関数は必要ありません。全てのステートメントの末尾にセミコロンをつける必要もありません。

このツアーでは、様々なプログラム上のタスクを遂行する方法を示すことで、Swift でコードを書き始めるために必要十分な情報を提供します。もし理解できなくても心配しないでください。このツアーで紹介することは全て、この本で後々詳細に説明しています。

> NOTE  
> より理解を深めるために、Xcode でこのチャプターを開いて遊んでみましょう。Playgrounds を使用すると、コードを編集して結果をすぐに確認することができます。
>
> [Download Playground](https://docs.swift.org/swift-book/GuidedTour/GuidedTour.playground.zip)

## Simple Values\(シンプルな値\)

定数を作成するのに `let`、変数を作成するのに `var` を使います。定数の値はコンパイル時に知る必要はありませんが、正確に一度だけ値を設定しなければなりません。つまり、値の指定をたった一度だけ行い、他のあらゆる場所からその定数を利用できます。

```swift
var myVariable = 42
myVariable = 50
let myConstant = 42
```

定数や変数は設定したい値と同じ型でなければなりません。しかし、明示的に型を記載する必要はありません。定数や変数を作成する時に値を与えることで、コンパイラが型を推論します。例えば、コンパイラは `myVariable` の値が整数ということから、型を整数と推論します。

もし、最初に設定する値が十分な情報を提供しなかった場合\(もしくは最初に設定する値ではなかった場合\)、変数の後にコロン\(`:`\)を付けて型を書くことで特定することができます。

```swift
let implicitInteger = 70
let implicitDouble = 70.0
let explicitDouble: Double = 70
```

> Experiment  
> 値が `4` で `Float` を明示的に指定した定数を作成してみましょう。

値は暗黙的に他の型に変換されることはありません。他の型に変換したい場合は、変換したい型のインスタンスを明示的に作成してください。

```swift
let label = "The width is "
let width = 94
let widthLabel = label + String(width)
```

> Experiment  
> 最後の行から `String` への変換を消してみてください。どんなエラーが起きるでしょうか？

もっと簡単な方法で文字列の中に値を含めることができます。値を括弧で囲み、括弧の前にバックスラッシュをつけます。

```swift
let apples = 3
let oranges = 5
let appleSummary = "I have \(apples) apples."
let fruitSummary = "I have \(apples + oranges) pieces of fruit."
```

> Experiment  
> 文字列に浮動小数点の計算を含めたり、誰かの名前を greeting に含めるために、`\()` を使用してみましょう。

複数行の文字列に対しては、3 つのダブルクォーテーション\(`"""`\)を使いましょう。末尾のクォーテーションに到達するまで、各行の最初のインデントは除外されます。

```swift
let quotation = """
I said "I have \(apples) apples."
And then I said "I have \(apples + oranges) pieces of fruit."
"""
```

配列や辞書には角括弧\(`[]`\)を使い、それらの要素には角括弧内にインデックスやキーを書きます。最後の要素の後ろにもカンマ\(`,`\)を付けることができます。

```swift
var shoppingList = ["catfish", "water", "tulips"]
shoppingList[1] = "bottle of water"

var occupations = [
    "Malcolm": "Captain",
    "Kaylee": "Mechanic",
]
occupations["Jayne"] = "Public Relations"
```

配列は要素を追加すると自動でサイズを増やします。

```swift
shoppingList.append("blue paint")
print(shoppingList)
```

空の配列や辞書を作成するためには、初期化の構文を使います。

```swift
let emptyArray = [String]()
let emptyDictionary = [String: Float]()
```

型が推論できる場合は、空の配列は `[]`、辞書は `[:]` で書くことができます。\(例えば、変数に新しい値を設定する場合や関数に引数を渡す場合など\)

```swift
shoppingList = []
occupations = [:]
```

## Control Flow\(制御フロー\)

条件を作成するには、 `if` と `switch` を使います。ループを作成するには `for-in` `while` `repeat-while` を使います。条件やオプショナルに丸括弧\(`()`\)を付けるかどうかは任意です。本文の周りの中括弧\(`{}`\)は必須です。

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
// "11"
```

`if` 文の中で、条件はブール式でなければなりません。つまり、`if score { ... }` などのコードはエラーで、暗黙的に 0 にはなりません。

`if` と `let` を一緒に使用して、存在しないかもしれない値を扱うことができます。これらの値はオプショナルで表現されます。オプショナルは値が含まれているか、値が存在しないことを示す `nil` を含んでいます。値をオプショナルにするには、値の型の後ろにクエスチョンマーク\(`?`\)を書きましょう。

```swift
var optionalString: String? = "Hello"
print(optionalString == nil)
// "false"

var optionalName: String? = "John Appleseed"
var greeting = "Hello!"
if let name = optionalName {
    greeting = "Hello, \(name)"
}
```

> Experiment  
> `optionalName` を `nil` に変えてみましょう。どんな greeting が出力されるでしょうか？ `optionalName` が `nil` の場合に別の greeting が出力されるように `else` を追加してみましょう。

もしオプショナルの値が `nil` の場合、条件は `false` になり、中括弧内のコードはスキップされます。`nil` でなければ、オプショナルの値はアンラップされて `let` の後の定数に代入され、中括弧内のブロック内でその値を利用できます。

オプショナルの値を扱うもう 1 つの方法として、`??` 演算子を使用してデフォルトの値を提供します。もしオプショナルの値が `nil` の場合、代わりにデフォルトの値が使われます。

```swift
let nickname: String? = nil
let fullName: String = "John Appleseed"
let informalGreeting = "Hi \(nickname ?? fullName)"
```

`switch` は、あらゆる種類のデータと比較のための演算子を扱うことができます。\(整数や等価チェックだけに限定されません\)

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
// "Is it a spicy red pepper?"
```

> Experiment  
> `default` の `case` を削除してみましょう。どんなエラーが起きるでしょうか？

パターンに合った値を定数に設定するために `let` がどう使われるかに注目してください。

パターンに合った switch case 内のコードが実行された後、プログラムは switch 文から抜け出します。次の case は実行されないので、各 case の最後に明示的に break を書く必要はありません。

`for-in` を使用することで辞書のそれぞれの要素のキーバリューペアを受け取って、辞書内のアイテムに反復処理をすることができます。辞書は順序のないコレクションなので、受け取るキーバリューペアの順序は決まっていません。

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
// "25"
```

> Experiment  
> `_` を変数の名前に置き換えてみて、どの種類の値が最大だったかを追ってみましょう。

`while` を使用することで条件が変わるまで、ブロック内のコードを反復して実行できます。ループの条件を最後に置くことで、ループ内のブロックが少なくとも 1 回実行されるようにすることができます。

```swift
var n = 2
while n < 100 {
    n *= 2
}
print(n)
// "128"

var m = 2
repeat {
    m *= 2
} while m < 100
print(m)
// "128"
```

`..<` を使用すると、インデックスの範囲を生成でき、ループのインデックスを追うことができます。

```swift
var total = 0
for i in 0..<4 {
    total += i
}
print(total)
// "6
```

`..<` では後ろの値は除外され、`...` は両方の値を含みます。

## Functions and Closures\(関数とクロージャ\)

関数の定義には `func` を使います。括弧\(`()`\)の中に引数のリスト、その前に関数の名前を付けることで関数を呼び出します。また、`->` の後ろに戻り値の型を指定して、パラメータ名や型と区別します。

```swift
func greet(person: String, day: String) -> String {
    return "Hello \(person), today is \(day)."
}
greet(person: "Bob", day: "Tuesday")
```

> Experiment  
> `day` パラメータを削除して、greet に今日の lunch special を指定するためのパラメータを追加してみましょう。

デフォルトで、関数はパラメータ名をそのままラベルとして使用します。独自の引数ラベルを設定したい場合は、パラメータ名の前に引数ラベルを記載してください。引数にラベルが不要な場合は、`_` を書きましょう。

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
// "120"
print(statistics.2)
// "120"
```

関数はネストして使用することができます。ネストした関数は、外側で定義された変数にアクセスすることができます。ネストした関数を使用することで、長くて複雑な関数を整理することができます。

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

関数は実はクロージャの特殊なケースです。クロージャは、後で呼ばれる可能性があるコードブロックのことを指します。クロージャ内のコードは、そのクロージャが作成されたスコープで利用可能な変数や関数へアクセスすることができます。これは、実際に実行されるのが別のスコープ\(タイミング\)の場合でも当てはまります。上記のネストした関数の例でも同じことが見られます。中括弧\(`{}`\)で囲むことで、名前なしのクロージャを作成することもできます。`in` を使用することで、コードの本文から引数と戻り値を分離することができます。

```swift
numbers.map({ (number: Int) -> Int in
    let result = 3 * number
    return result
})
```

> Experiment  
> 上記のクロージャを、全ての奇数で 0 を返すように書き換えてみましょう。

より簡潔にクロージャを書く複数の方法があります。クロージャの型が既にわかっている場合\(デリゲートのコールバックなど\)、パラメータと戻り値の型を省略できます。単一の文のみのクロージャの場合、暗黙的に、その文の値が戻り値になります。

```swift
let mappedNumbers = numbers.map({ number in 3 * number })
print(mappedNumbers)
// "[60, 57, 21, 36]"
```

名前ではなく番号でパラメータを参照できます。このアプローチは、非常に短いクロージャで特に役立ちます。関数の最後の引数として渡されたクロージャは、括弧の直後に表示できます。 クロージャが関数の唯一の引数の場合は、括弧を完全に省略できます。

```swift
let sortedNumbers = numbers.sorted { $0 > $1 }
print(sortedNumbers)
// "[20, 19, 12, 7]"
```

## Objects and Classes\(オブジェクトとクラス\)

`class` をクラス名の前に付けることでクラスを作成することができます。クラス内のプロパティの宣言は、クラス内にあるということを除いて、定数や変数の宣言方法と同じです。同様に、メソッドや関数の宣言も同じように書くことができます。

```swift
class Shape {
    var numberOfSides = 0
    func simpleDescription() -> String {
        return "A shape with \(numberOfSides) sides."
    }
}
```

> Experiment  
> `let` を使用して定数プロパティを追加してください。また、引数を受け取る別のメソッドも追加してみましょう。

クラスのインスタンスは、クラス名の後に丸括弧\(`()`\)を付けて生成します。そのインスタンスのプロパティやメソッドにアクセスするには、ドット\(`.`\)構文を使います。

```swift
var shape = Shape()
shape.numberOfSides = 7
var shapeDescription = shape.simpleDescription()
```

このバージョンの `Shape` クラスは重要なことが抜けています。それは、インスタンスを生成するときにクラスを構築するためのイニシャライザです。生成するために `init` を使います。

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

サブクラスは、そのクラス名の後に、スーパークラスの名前をコロン\(`:`\)で区切って指定します。標準で定義されているルートクラスのサブクラスにする必要は必ずしもありません。追加も省略もできます。

スーパークラスのメソッドをサブクラスでオーバーライドする場合は `override` を付けます。意図せずにオーバーライドしてしまった場合は、コンパイラがエラーにします。`override` を付けていても、`override` が正しく行われていない場合も、コンパイラはエラーにします。

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
> radius と name をイニシャライザの引数に受けとる `Circle` という名前の `NameShape` の別のサブクラスを作ってみましょう。そして、 `Circle` クラスに `area()`、`simpleDescription()` メソッドを実装してみましょう。

単純に値を保持するプロパティ以外に、プロパティが get と set を持つこともできます。

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
// "9.3"
triangle.perimeter = 9.9
print(triangle.sideLength)
// "3.3000000000000003"
```

`perimeter` の sett の中で、新しい値は暗黙的に `newValue` という名前になります。`set` の後に丸括弧\(`()`\)で囲んで明示的に指定することもできます。

`EquilateralTriangle` クラスのイニシャライザは 3 つの異なるステップがあります。

1. サブクラスで宣言されたプロパティに値を設定します
2. スーパークラスのイニシャライザを呼びます
3. スーパークラスで定義されたプロパティの値を変更。この時点で get や set やメソッドを使用して他のセットアップ処理を実行できます

プロパティを計算する必要はないけれども、新しい値を設定する前後で何かコードを実行したい場合、`willSet`、`didSet` を使います。このコードは、イニシャライザ以外で値が変更された時に毎回実行されます。例えば、下のクラスは三角形の辺の長さが常に四角形の辺の長さと同じになります。

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
// "10.0"
print(triangleAndSquare.triangle.sideLength)
// "10.0"
triangleAndSquare.square = Square(sideLength: 50, name: "larger square")
print(triangleAndSquare.triangle.sideLength)
// "50.0"
```

オプショナルな値を扱う場合、`?` をメソッド、プロパティ、subscript のような操作の前に書きます。もし `?` の前の値が `nil` の場合、`?` の後の処理は全て無視され、その式全体の値は `nil` になります。それ以外、オプショナルの値はアンラップされて、`?` の後は全てアンラップされた値として実行されます。どちらの場合も、式全体はオプショナルな値です。

```swift
let optionalSquare: Square? = Square(sideLength: 2.5, name: "optional square")
let sideLength = optionalSquare?.sideLength
```

## Enumerations and Structures\(列挙型と構造体\)

列挙型を作成するには `enum` を使います。クラスやその他の名前の付いた型と同様に、列挙型もメソッドを持つことができます。

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
> 2つの `Rank` の Raw Value を比較して `Rank` を比較するメソッドを追加してみましょう。

デフォルトで、Swift は 0 から 1 つずつ増加する値を Raw Value に代入しますが、明示的に値を指定してこの挙動を変更することもできます。上記の例では、`Ace` は明示的に `1` を Raw Value に指定しており、残りは、`1` から順番に Raw Value が指定されます。文字列や浮動小数点も使用することができます。列挙型の個々のケースの Raw Value には、`rawValue` プロパティからアクセスすることができます。

Raw Value から列挙型のインスタンスを生成するためには `init?(rawValue:)` イニシャライザを使います。Raw Value に合致したケースを返すか、合致するものがない場合は `nil` を返します。

```swift
if let convertedRank = Rank(rawValue: 3) {
    let threeDescription = convertedRank.simpleDescription()
}
```

列挙型のケースの値は実在の値で、Raw Value を書くための別の方法ではありません。事実、意味のある Raw Value が存在しない場合は、Raw Value を提供する必要はありません。

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

`hearts` ケースを参照するために 2 つの方法があることに注目してください。`hearts` 定数に値を設定する時、列挙ケースの `Suits.hearts` は型を明示的に特定していないため、完全な名前で参照されています。一方、switch 文の中では、`self` が既に `Suit` だとわかっているため、列挙ケースは `.hearts` と省略方法で参照されています。型が既にわかっている場合はいつでも省略方法を使用することができます。

もし列挙型が Raw Value を持つ場合、これらの値は宣言の一部として決定されます。これはつまり、特定の列挙ケースの全てのインスタンスは、必ず同じ Raw Value を持つということです。列挙ケースが関連値を持つもう 1 つの方法があります。これらはインスタンス生成時に値が決定され、列挙ケースのインスタンスごとに異なる値を持つ可能性があります。例えば、サーバから日の出時刻と日の入り時刻をリクエストする場合を考えてみてください。サーバはリクエスト情報に応じたレスポンスを返すか、リクエストが間違っている場合は間違っている内容を返します。

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
// "Sunrise is at 6:00 am and sunset is at 8:09 pm."
```

> Experiment  
> `ServerResponse` に3つ目のケースを追加して、switch 文を変更してください。

日の出時刻と日の入り時刻が、switch に合致したケースの一部として、`ServerResponse` から抽出されていることに注目してください。

構造体を作成するには `struct` を使います。構造体はメソッドやイニシャライザなど多くのクラスと同じ挙動をサポートしています。最も重要な違いは、構造体はコードの中で渡される時に必ずコピーされる、ということです。クラスは参照を渡します。

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

## Protocols and Extensions\(プロトコルと拡張\)

プロトコルを宣言するために `protocol` を使います。

```text
protocol ExampleProtocol {
    var simpleDescription: String { get }
    mutating func adjust()
}
```

クラス、列挙型、構造体は全てプロトコルに準拠することができます。

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
> `ExampleProtocolrank` にもう1つの要件を追加してみましょう。`SimpleClass` と `SimpleStructure` にどういう変更が必要になるでしょうか？

構造体 `SimpleStructure` を変更するメソッドに `mutating` キーワードが付いていることに注目してください。クラスのメソッドはクラスを変更することが許されているため、`mutating` を付ける必要はありません。

既存の型に新しい機能\(メソッドや計算プロパティ\)を追加する場合に、`extension` を使います。他のファイルやモジュールで宣言された型やライブラリやフレームワークから import した型にプロトコルを準拠させる場合にも `extension` を使います。

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
// "The number 7"
```

> Experiment  
> `absoluteValue` プロパティを `Double` に追加する `extension` を書いてみましょう。

プロトコルの名前は、他の名前が付いた型と同じように使用することができます。例えば、同じ 1 つのプロトコルに準拠した異なる型のオブジェクトのコレクションを作成することができます。プロトコル型の値をそのまま扱っている場合、プロトコルの外側で定義されたメソッドを使用することはできません。

```swift
let protocolValue: ExampleProtocol = a
print(protocolValue.simpleDescription)
// "A very simple class.  Now 100% adjusted."
// print(protocolValue.anotherProperty)  // Uncomment to see the error
```

変数 `protocolValue` が実行時に `SimpleClass` になることはわかるものの、コンパイラはこれを、ある `ExampleProtocol` として扱います。つまり、プロトコルで定義されたメソッドやプロパティ以外へアクセスすることはできません。

## Error Handling\(エラーハンドリング\)

`Error` プロトコルに準拠することでどんな型もエラーを表現することができます。

```swift
enum PrinterError: Error {
    case outOfPaper
    case noToner
    case onFire
}
```

関数に `throws` を付け、エラーをスローする箇所に `throw` を使用することでエラーをスローすることができます。関数内でエラーをスローすると、関数がすぐにリターンして、その関数を呼んだ関数でエラーを処理します。

```swift
func send(job: Int, toPrinter printerName: String) throws -> String {
    if printerName == "Never Has Toner" {
        throw PrinterError.noToner
    }
    return "Job sent"
}
```

エラーを処理する方法は様々あります。1 つは `do-catch` を使用することです。`do` ブロックの中では、エラーをスローする可能性がある箇所の前に `try` を付けます。`catch` ブロックの中では、明示的に異なる名前を設定しない限り、`error` という名前で自動的にエラー情報が与えられます。

```swift
do {
    let printerResponse = try send(job: 1040, toPrinter: "Bi Sheng")
    print(printerResponse)
} catch {
    print(error)
}
// "Job sent"
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
// "Job sent"
```

> Experiment  
> `do` ブロックの中でエラーを throw するコードを追加してみましょう。最初の `catch` ブロックでエラーを処理するためにはどの種類のエラーを throw する必要がありますでしょうか？ 2つ目、3つ目の場合はどうでしょうか？

エラーを処理するもう 1 つの方法は、`try?` を付けて結果をオプショナルに変換することです。もしその関数がエラーをスローする場合、特定のエラーは破棄されて、結果が `nil` になります。そうでなければ、結果は、関数が返す値を内包したオプショナルになります。

```swift
let printerSuccess = try? send(job: 1884, toPrinter: "Mergenthaler")
let printerFailure = try? send(job: 1885, toPrinter: "Never Has Toner")
```

関数内の全ての処理が実行されて、関数が結果を返す直前にコードを実行したい場合、`defer` を使います。このブロックは関数がエラーをスローしても実行されます。違うタイミングで実行される必要はありますが、`defer` を使用してセットアップ用のコードの次にクリーンアップ用のコードを書くことができます。

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
// "false"
```

## Generics\(ジェネリクス\)

ジェネリックな関数や型を作成するには、山括弧\(`<>`\)の中に名前を書きます。

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

クラス、列挙型、構造体と同じように、ジェネリックな関数やメソッドも作成することができます。

```swift
// Swift 標準ライブラリのオプショナル型の再実装
enum OptionalValue<Wrapped> {
    case none
    case some(Wrapped)
}
var possibleInteger: OptionalValue<Int> = .none
possibleInteger = .some(100)
```

要件の一覧を指定するために、本文の直前に `where` を使います。例えば、プロトコルを実装するために型が必要な場合や、2 つの型が一致している必要がある場合、クラスが特定のスーパークラスを継承している必要がある場合など。

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
> `anyCommonElements(_:_:)` を2つのシーケンスの共通の要素が含まれる配列を返すように修正してみましょう。

`<T: Equatable>` は `<T> ... where T: Equatable` と同じです。

