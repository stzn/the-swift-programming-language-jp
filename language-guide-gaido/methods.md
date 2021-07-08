# メソッド \(Methods\)

最終更新日: 2021/6/28

メソッドは、特定の型に紐づいた関数です。クラス、構造体、および列挙型は全て、インスタンスメソッドを定義できます。インスタンスメソッドは、特定の型のインスタンスを操作するための特定のタスクと機能をカプセル化します。クラス、構造体、および列挙型は、型自体に関連付けられている型メソッドを定義することもできます。型メソッドは、Objective-C のクラスメソッドに似ています。

Swift で構造体と列挙型がメソッドを定義できることは、C 言語および Objective-C との大きな違いです。Objective-C では、クラスはメソッドを定義できる唯一の型です。Swift では、クラス、構造体、または列挙型の間で選択でき、作成した型のメソッドを柔軟に定義できます。

## インスタンスメソッド\(Instance Methods\)

インスタンスメソッドは、特定のクラス、構造体、または列挙型のインスタンスに属する関数です。それらは、インスタンスプロパティにアクセスしたり変更する方法を提供したり、インスタンスに関連する機能を提供することによって、そのインスタンスの機能をサポートします。[Functions\(関数\)](functions.md)で説明されているように、インスタンスメソッドの構文は関数とまったく同じです。

インスタンスメソッドは、それが属する型の開き括弧\(`{`\)と閉じ括弧\(`}`\)内に記述します。インスタンスメソッドは、その型の他の全てのインスタンスメソッドおよびプロパティに暗黙的にアクセスできます。インスタンスメソッドは、それが属する型の特定のインスタンスでのみ呼び出すことができます。既存のインスタンスがなければ、単独で呼び出すことはできません。

アクションが発生した回数をカウントするために使用できる、シンプルな `Counter` クラスを定義する例を次に示します:

```swift
class Counter {
    var count = 0
    func increment() {
        count += 1
    }
    func increment(by amount: Int) {
        count += amount
    }
    func reset() {
        count = 0
    }
}
```

`Counter` クラスは 3 つのインスタンスメソッドを定義します。

* `increment()` は、カウンタを 1 ずつインクリメントします
* `increment(by: Int)` は、指定された整数分だけカウンタをインクリメントします
* `reset()` は、カウンタを 0 にリセットします

`Counter` クラスは、現在のカウンタ値を追跡するために変数プロパティ `count` も宣言しています

プロパティと同じドット構文でインスタンスメソッドを呼び出します。

```swift
let counter = Counter()
// counter の初期値は 0
counter.increment()
// counter の値は 1
counter.increment(by: 5)
// counter の値は 6
counter.reset()
// counter の値は 0
```

[Function Argument Labels and Parameter Names\(引数ラベルとパラメータ名\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide-gaido/functions#function-argument-labels-and-parameter-names)で説明されているように、関数パラメータは、\(関数の本文内で使用する\)名前と\(関数を呼び出すときに使用する\)引数ラベルの両方を持つことができます。メソッドは型に紐づいた関数にすぎないため、メソッドパラメータについても同じことが言えます。

### self プロパティ\(The self Property\)

型の全てのインスタンスには、インスタンスそれ自体を表す `self` と呼ばれる暗黙のプロパティがあります。自身のインスタンスメソッド内で現在のインスタンスを参照するには、`self` プロパティを使用します。

上記の例の `increment()` メソッドは、次のように記述できます:

```swift
func increment() {
    self.count += 1
}
```

実際には、コードに `self` を頻繁に記述する必要はありません。`self` を明示的に記述しない場合、メソッド内のプロパティまたはメソッド名を使用するとき、Swift は常に現在のインスタンスのプロパティまたはメソッドを参照していると見なします。この想定は、`Counter` の 3 つのインスタンスメソッド内で\(`self.count` ではなく\)`count` を使用していることからもわかります。

この規則の例外として、インスタンスメソッドのパラメータ名がそのインスタンスのプロパティと同じ名前の場合に発生します。この場合、パラメータ名が優先され、より厳密な方法でプロパティを参照する必要が生じます。パラメータ名とプロパティ名を区別するには、`self` プロパティを使用します。

ここで、`self` は、`x` と呼ばれるメソッドパラメータと `x` と呼ばれるインスタンスプロパティとの間のあいまいさを解消します:

```swift
struct Point {
    var x = 0.0, y = 0.0
    func isToTheRightOf(x: Double) -> Bool {
        return self.x > x
    }
}
let somePoint = Point(x: 4.0, y: 5.0)
if somePoint.isToTheRightOf(x: 1.0) {
    print("This point is to the right of the line where x == 1.0")
}
// "This point is to the right of the line where x == 1.0"
```

`self` プレフィックスがない場合、Swift は両方の `x` をメソッドパラメータを参照していると見なします。

### <a id="modifying-value-types-from-within-instance-methods">インスタンスメソッド内からの値型の変更\(Modifying Value Types from Within Instance Methods\)</a>

構造体と列挙型は値型です。デフォルトでは、値型のプロパティはそのインスタンスメソッド内から変更できません。

ただし、特定のメソッド内で構造体または列挙型のプロパティを変更する必要がある場合に、そのメソッドに _mutating_ な挙動を認めることができます。こうすることでメソッドはプロパティを変更できるようになり、メソッドが行った変更は、メソッドの終了時に元の構造体に書き戻されます。このメソッドは、新しいインスタンスを暗黙的な `self` プロパティに割り当てることもでき、メソッドが終了すると、既存のインスタンスはこの新しいインスタンスに置き換えられます。

この挙動を可能にするには、そのメソッドの `func` キーワードの前に `mutating` キーワードを配置します:

```swift
struct Point {
    var x = 0.0, y = 0.0
    mutating func moveBy(x deltaX: Double, y deltaY: Double) {
        x += deltaX
        y += deltaY
    }
}
var somePoint = Point(x: 1.0, y: 1.0)
somePoint.moveBy(x: 2.0, y: 3.0)
print("The point is now at (\(somePoint.x), \(somePoint.y))")
// "The point is now at (3.0, 4.0)"
```

上記の `Point` 構造体は、自身に変更を加える `moveBy(x:y:)` メソッドを定義します。これは、`Point` インスタンスを一定量移動します。新しいポイントを返す代わりに、呼び出されたポイントを実際に変更します。プロパティを変更できるようにするために、`mutating` キーワードがその定義に追加されてます。

[Stored Properties of Constant Structure Instances\(定数の格納インスタンスのプロパティ\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide-gaido/properties#stored-properties-of-constant-structure-instances)で説明されているように、構造体の定数内の変数プロパティを変更できないため、構造体の定数で変更メソッドを呼び出すことはできないことに注意してください。

```swift
let fixedPoint = Point(x: 3.0, y: 3.0)
fixedPoint.moveBy(x: 2.0, y: 3.0)
// エラーが出力されます
```

### mutating メソッド内からselfへの値の割り当て\(Assigning to self Within a Mutating Method\)

メソッドを変更すると、まったく新しいインスタンスを暗黙的な `self` プロパティに割り当てることができます。上記の `Point` の例は、代わりに次のように記述できます:

```swift
struct Point {
    var x = 0.0, y = 0.0
    mutating func moveBy(x deltaX: Double, y deltaY: Double) {
        self = Point(x: x + deltaX, y: y + deltaY)
    }
}
```

このバージョンの `mutating` `moveBy(x:y:)` メソッドは、`x` と `y` がターゲットの位置に設定された新しい構造体を作成します。この代替バージョンのメソッドを呼び出した場合の最終結果は、先ほどのバージョンを呼び出した場合とまったく同じになります。

列挙型の変更メソッドは、暗黙的な `self` パラメータを同じ列挙型とは異なるケースに設定できます。

```swift
enum TriStateSwitch {
    case off, low, high
    mutating func next() {
        switch self {
        case .off:
            self = .low
        case .low:
            self = .high
        case .high:
            self = .off
        }
    }
}
var ovenLight = TriStateSwitch.low
ovenLight.next()
// ovenLight は .high
ovenLight.next()
// ovenLight は .off
```

この例では、スイッチの 3 つの状態を列挙型で定義しています。スイッチは、`next()` メソッドが呼び出されるたびに、3 つの異なる電源状態\(`off`、`low` と `high`\)を切り替えます。

### 型メソッド\(Type Methods\)

上で説明したように、インスタンスメソッドは、特定の型のインスタンスで呼び出すメソッドです。型自体で呼び出されるメソッドを定義することもできます。このようなメソッドは_型メソッド_と呼ばれます。メソッドの `func` キーワードの前に `static` キーワードを記述して、型メソッドを示します。クラスは代わりに `class` キーワードを使用して、サブクラスがそのメソッドのスーパークラスの実装をオーバーライドできるようにすることができます。

> NOTE  
> Objective-C では、クラスに対してのみ型レベルのメソッドを定義できます。Swift では、全てのクラス、構造体、および列挙型に対して型レベルのメソッドを定義できます。各型メソッドは、サポートする型に明示的にスコープされます。

型メソッドは、インスタンスメソッドと同様にドット構文で呼び出せます。ただし、その型のインスタンスではなく、その型に対して型メソッドを呼び出します。`SomeClass` というクラスで型メソッドを呼び出す方法は次のとおりです:

```swift
class SomeClass {
    class func someTypeMethod() {
        // 型メソッドの実装はここに
    }
}
SomeClass.someTypeMethod()
```

型メソッドの本文内で、暗黙的な `self` プロパティは、その型のインスタンスではなく、型自体を参照します。これは、インスタンスプロパティとインスタンスメソッドパラメータの場合と同様に、`self` を使用して、型プロパティと型メソッドパラメータのあいまいさを解消できることを意味します。

より一般的には、型メソッドの本文内で使用する修飾されていないメソッド名とプロパティ名は、他の型レベルのメソッドとプロパティを参照します。型メソッドは、型名のプレフィックスを必要とせずに、他のメソッドの名前で別の型メソッドを呼び出すことができます。同様に、構造体と列挙型の型メソッドは、型名のプレフィックスなしで型プロパティの名前を使用して、型プロパティにアクセスできます。

下記では、ゲームの様々なレベルまたはステージを通じてプレーヤーの進行状況を追跡する `LevelTracker` と呼ばれる構造体を定義しています。シングルプレイヤー用のゲームですが、1 つのデバイスに複数のプレイヤーの情報を保存できます。

ゲームの全てのレベル\(レベル 1 を除く\)は、ゲームを最初にプレイしたときに決定されます。プレーヤーがそのレベルをクリアするたびに、そのレベルはデバイス上の全てのプレーヤーに対して解放されます。`LevelTracker` 構造体は、型プロパティとメソッドを使用して、ゲームのどのレベルが解放されたのかを追跡します。また、個々のプレーヤーの現在のレベルも追跡します。

```swift
struct LevelTracker {
    static var highestUnlockedLevel = 1
    var currentLevel = 1

    static func unlock(_ level: Int) {
        if level > highestUnlockedLevel { highestUnlockedLevel = level }
    }

    static func isUnlocked(_ level: Int) -> Bool {
        return level <= highestUnlockedLevel
    }

    @discardableResult
    mutating func advance(to level: Int) -> Bool {
        if LevelTracker.isUnlocked(level) {
            currentLevel = level
            return true
        } else {
            return false
        }
    }
}
```

`LevelTracker` 構造体は、任意のプレーヤーが解放した最高レベルを追跡します。この値は、`highestUnlockedLevel` という型プロパティに格納されます。

`LevelTracker` は、`highestUnlockedLevel` プロパティを操作する 2 つの型メソッドも定義します。1 つ目は、`unlock(_:)` と呼ばれる型メソッドで、新しいレベルが解放されるたびに最高の `highestUnlockedLevel` の値を更新します。2 つ目は、`isUnlocked(_ :)` と呼ばれる便利な型メソッドで、特定のレベル番号がすでに解放されている場合に `true` を返します。\(これらの型メソッドは、`LevelTracker.highestUnlockedLevel` として記述しなくても、`highestUnlockedLevel` 型プロパティにアクセスできることに注目してください\)

型プロパティと型メソッドに加えて、`LevelTracker` は、ゲーム全体での個々のプレーヤーの進行状況を追跡します。これは、`currentLevel` というインスタンスプロパティを使用して、プレーヤーが現在再生しているレベルを追跡します。

`currentLevel` プロパティの管理を容易にするために、`LevelTracker` は `advance(to:)` というインスタンスメソッドを定義します。`currentLevel` を更新する前に、このメソッドは、要求された新しいレベルが既に解放されているかどうかを確認します。`advance(to:)` メソッドは、`currentLevel` を実際に設定できたかどうかを示すブール値を返します。`advance(to:)` の戻り値を無視することもあるため、このメソッドは `@discardableResult` 属性でマークされています。この属性の詳細については、[Attributes\(属性\)](../language-reference/attributes.md)を参照ください。

`LevelTracker` 構造体は、下記に示す `Player` クラスで使用され、個々のプレーヤーの進行状況を追跡および更新します:

```swift
class Player {
    var tracker = LevelTracker()
    let playerName: String
    func complete(level: Int) {
        LevelTracker.unlock(level + 1)
        tracker.advance(to: level + 1)
    }
    init(name: String) {
        playerName = name
    }
}
```

`Player` クラスは、`LevelTracker` の新しいインスタンスを作成して、そのプレーヤーの進行状況を追跡します。また、`complete(level:)` というメソッドも提供します。これは、プレーヤーが特定のレベルをクリアするたびに呼び出されます。このメソッドは、全てのプレーヤーの次のレベルを解放し、プレーヤーの進行状況を更新して次のレベルに移動します。\(レベルは前の行の `LevelTracker.unlock(_:)` の呼び出しによって解放されていることがわかっているため、`advance(to:)` のブール値の戻り値は無視されています\)

新しいプレーヤーの `Player` クラスのインスタンスを作成し、プレーヤーがレベル 1 をクリアしたときに何が起こるかを確認できます。

```swift
var player = Player(name: "Argyrios")
player.complete(level: 1)
print("highest unlocked level is now \(LevelTracker.highestUnlockedLevel)")
// "highest unlocked level is now 2"
```

2 番目のプレーヤーを作成し、そのプレーヤーをゲーム内のどのプレーヤーもまだ解放していないレベルに移動しようとすると、プレーヤーの現在のレベルを設定する際に失敗します:

```swift
player = Player(name: "Beto")
if player.tracker.advance(to: 6) {
    print("player is now on level 6")
} else {
    print("level 6 hasn't yet been unlocked")
}
// "level 6 hasn't yet been unlocked"
```

