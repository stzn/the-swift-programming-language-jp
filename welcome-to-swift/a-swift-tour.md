# A Swift Tour

伝統的に、新しい言語の最初のプログラミングは "Hello world!" を画面に表示することを推めます。Swift ではたった1行でできます。

```swift
print("Hello, world!")
// Prints "Hello, world!"
```

もし C 言語や Objective-C の経験があるならばこのシンタックスに馴染みがあるかもしれませんが、Swift ではこの1行のコードは完成したプログラミングです。input/output や文字列を扱うために他のライブラリを import する必要もありません。グローバル領域に書かれたコードはプログラムのエントリポイントとして使用されます。`main()` 関数は必要ありません。全てのステートメントの末尾にセミコロンをつける必要もありません。

このツアーでは、様々なプログラム上のタスクを遂行する方法を示すことで、Swift でコードを書き始めるために必要十分な情報を提供します。もし理解できなくても心配しないでください。このツアーで紹介することは全て、この本で後々詳細に説明しています。

> NOTE  
> より理解を深めるために、Xcodeでこのチャプターを開いて遊んでみましょう。Playgroundsを使うと、コードを編集して結果をすぐに確認することができます。
>
> [Download Playground](https://docs.swift.org/swift-book/GuidedTour/GuidedTour.playground.zip)

## Simple Values

定数を作成するのに `let`、変数を作成するのに `var` を使います。定数の値はコンパイル時に知る必要はありませんが、正確に一度だけ値を設定しなければなりません。つまり、値を指定をたった一度だけ行い、他のあらゆる場所から利用するために定数を活用できます。

```swift
var myVariable = 42
myVariable = 50
let myConstant = 42
```

定数や変数は設定したい値と同じ型でなければなりません。しかし、明示的に型を記載する必要はありません。定数や変数を作成する時に値を与えることで、コンパイラが型を推論します。例えば、コンパイラは `myVariable` を 値が integer であることから integer であると推論します。

もし、最初に設定する値が十分な情報を提供しなかった場合(もしくは最初に設定する値ではなかった場合)、変数の後にコロンを付けて型を書くことで特定することができます。

```swift
let implicitInteger = 70
let implicitDouble = 70.0
let explicitDouble: Double = 70
```

> Experiment  
> 値が 4 で Float を明示的に指定した定数を作成してみよう

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
> 文字列に浮動小数点の計算を含めたり、誰かの名前をgreetingに含めるために、\() を使ってみましょう。

複数行の文字列に対しては、3つのダブルクォーテーション(""")を使いましょう。末尾のクォーテーションに到達するまで、各行の最初のインデントは除外されます。

```swift
let quotation = """
I said "I have \(apples) apples."
And then I said "I have \(apples + oranges) pieces of fruit."
"""
```

Array や Dictionary には角括弧([])を使い、それらの要素には角括弧内に index や key を書きます。最後の要素の後ろにもカンマを付けることができます。

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

型が推論できる場合は、空の Array は[]、Dictionary は[:]で書くことができます。(例えば、変数に新しい値を設定する場合や関数に引数を渡す場合など)

```swift
shoppingList = []
occupations = [:]
```

## Control Flow

条件を作成するには、 `if` と `switch` を使います。ループを作成するには `for-in` `while` `repeat-while` を使います。条件やoptionalに丸括弧(())を付けるかどうかは任意です。body の周りの中括弧({})は必須です。

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

`if` ステートメントの中で、条件は Boolean でなければなりません。(つまり、`if score { ... }`などのコードはエラーか暗黙的に0と推論されます)

`if` と `let`を一緒に使って、存在しないかもしれない値を扱うことができます。これらの値は optional で表現されます。optional は値が含まれているか、値が存在しないことを示す `nil` を含んでいます。値を optional にするには、値の型の後ろにクエスチョンマーク(?)を書きましょう。

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

optional の値を扱うもう1つの方法として、??を使ってデフォルトの値を提供します。もし optional の値が nil の場合、代わりにデフォルトの値が使われます。

```swift
let nickname: String? = nil
let fullName: String = "John Appleseed"
let informalGreeting = "Hi \(nickname ?? fullName)"
```

 `switch` は、あらゆる種類のデータと比較のための operator を扱うことができます。( integer と等価チェックだけに限定されません)

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

`for-in` を使うことで Dictionary のそれぞれの要素の key-value ペアを受け取って、Dictionary 内のアイテムを反復処理をすることができます。Dictionary は順序のないコレクションなので、key-valueペアの受け取る順序は決まっていません。

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
> _ を変数の名前に置き換えてみて、どの種類の値が最大だったかを追ってみましょう・。

`while` を使うことで条件が変わるまで、ブロック内のコードを反復して実行できます。ループの条件を最後に置くことで、ループ内のブロックが少なくとも1回実行されるようにすることができます。

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

## Objects and Classes

## Enumerations and Structures

## Protocols and Extensions

## Error Handling

## Generics