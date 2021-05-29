# Properties(プロパティ)

最終更新日:

プロパティは、値を特定のクラス、構造体、または列挙型に関連付けます。格納プロパティ(*stored property*)は定数と変数の値をインスタンスの一部として保存しますが、計算プロパティ(*computed property*)は値を(保存するのではなく)計算します。計算プロパティは、クラス、構造体、および列挙型から使用できます。格納プロパティは、クラスと構造体のみで使用できます。

格納および計算プロパティは、通常、特定の型のインスタンスに関連付けられています。ただし、プロパティを型自体に関連付けることもできます。このようなプロパティは、型プロパティ(*type property*)と呼ばれます。

さらに、プロパティオブザーバ(*property observer*)を定義して、プロパティ値の変更を監視して、それに応じたアクションを起こすことができます。プロパティオブザーバは、自分で定義した格納プロパティ、およびサブクラスがそのスーパークラスから継承するプロパティに追加できます。

プロパティラッパ(*property wrapper*)を使用して、複数のプロパティのゲッタとセッタでコードを再利用することもできます。

## Stored Properties(格納プロパティ)

最もシンプルな形式だと、格納プロパティ(*Stored Property*)は、特定のクラスまたは構造体のインスタンスの一部として保存される定数または変数です。格納プロパティは、変数格納プロパティ(`var`)または定数格納プロパティ(`let`) のいずれかです。

[Default Property Values](./initialization.md#default-property-valuesデフォルトプロパティ値)で説明されているように、格納プロパティのデフォルト値をその定義の一部として設定できます。また、初期化中に格納プロパティの初期値を設定および変更することもできます。これは、[Assigning Constant Properties During Initialization](./initialization.md#assigning-constant-properties-during-initialization初期化中の定数プロパティへの値の設定)で説明されているように、定数の格納にも当てはまります。

以下の例では、`FixedLengthRange` という構造体を定義しています。これは、作成後に範囲の長さを変更できない整数の範囲を記述します。

```swift
struct FixedLengthRange {
    var firstValue: Int
    let length: Int
}
var rangeOfThreeItems = FixedLengthRange(firstValue: 0, length: 3)
// 0, 1, 2 の整数の範囲を表しています
rangeOfThreeItems.firstValue = 6
// 6, 7, 8 の整数の範囲を表しています
```

`FixedLengthRange` のインスタンスには、`firstValue` と呼ばれる変数格納プロパティと `length` という定数格納プロパティがあります。上記の例では、定数のため、長さは新しい範囲が作成されたときに初期化され、それ以降は変更できません。

### Stored Properties of Constant Structure Instances(定数に保存されたインスタンスのプロパティ)

構造体のインスタンスを作成し、そのインスタンスを定数に割り当てる場合、インスタンスのプロパティは、変数で宣言されていても変更できません。

```swift
let rangeOfFourItems = FixedLengthRange(firstValue: 0, length: 4)
// 0, 1, 2 の整数の範囲を表しています
rangeOfFourItems.firstValue = 6
// firstValue は変数ですがエラーになります
```

`rangeOfFourItems` は定数として(`let` キーワードを使用して)宣言されているため、`firstValue` が変数でも、その `firstValue` プロパティを変更することはできません。

この挙動は、構造型が値型のためです。値型のインスタンスが定数としてマークされている場合、その全てのプロパティも同様です。

参照型のクラスには同じことが当てはまりません。参照型のインスタンスを定数に割り当てた場合でも、そのインスタンスの変数を変更できます。

### Lazy Stored Properties(遅延格納プロパティ)

遅延格納プロパティ(*lazy stored property*)は、最初に使用されるまで初期値が計算されないプロパティです。宣言の前に `lazy` 修飾子を記述して、遅延格納プロパティを示します。

> NOTE  
> インスタンスの初期化が完了するまで初期値が取得できない可能性があるため、遅延プロパティは常に変数として(`var` キーワードを使用して)宣言する必要があります。定数プロパティは、初期化が完了する前に常に値を持っている必要があるため、`lazy` で宣言することはできません。

遅延格納プロパティは、プロパティの初期値が、インスタンスの初期化が完了後でないと値がわからない外部要因に依存している場合に役立ちます。プロパティの初期値が必要になるまで実行すべきではない、複雑または計算コストの高いセットアップが必要な場合にも役立ちます。

下記の例では、遅延格納プロパティを使用して、複雑なクラスの不必要な初期化を回避しています。この例では、`DataImporter` と `DataManager` という 2 つのクラスを定義していますが、どちらも全体像は見せていません:

```swift
class DataImporter {
    /*
    DataImporter は、外部ファイルからデータをインポートするためのクラスです
    このクラスは、初期化にかなりの時間がかかると想定します
    */
    var filename = "data.txt"
    // DataImporter クラスは、ここでデータのインポート機能を提供します
}

class DataManager {
    lazy var importer = DataImporter()
    var data = [String]()
    // DataManager クラスはここでデータの管理機能を提供します
}

let manager = DataManager()
manager.data.append("Some data")
manager.data.append("Some more data")
// importer プロパティの DataImporter インスタンスはまだ作成されていません
```

`DataManager` クラスには、`data` と呼ばれる格納プロパティがあり、新しい空の `String` 型の配列で初期化されています。残りの機能は示されていませんが、この `DataManager` クラスの目的は、この `String` の配列を管理し、アクセスする機能を提供することです。

`DataManager` クラスの機能の一部は、ファイルからデータをインポートする機能です。この機能は、`DataImporter` クラスによって提供されています。このクラスは、初期化にかなりの時間がかかると想定されています。これは、`DataImporter` インスタンスが初期化されるときに、`DataImporter` インスタンスがファイルを開いてその内容をメモリに読み込む必要があるからかもしれません。

`DataManager` インスタンスはファイルからデータをインポートしなくてもデータを管理できるため、`DataManager` 自体が作成されるときに、`DataManager` は新しい `DataImporter` インスタンスを作成しません。代わりに、`DataImporter` インスタンスを最初に使用するときに作成する方が理にかなっています。

`lazy` 修飾子でマークされているため、`importer` プロパティの `DataImporter` インスタンスは、`importer` プロパティが最初にアクセスされたとき(`filename` プロパティが照会されたときなど)にのみ作成されます。

```swift
print(manager.importer.filename)
// importer プロパティの DataImporter インスタンスが作成されました
// "data.txt"
```

> NOTE  
> `lazy` 修飾子でマークされたプロパティが複数のスレッドから同時にアクセスされ、そのプロパティがまだ初期化されていない場合、そのプロパティが 1 回だけ初期化されるという保証はありません。

### Stored Properties and Instance Variables(格納プロパティはインスタンス変数)

`Objective-C` の経験がある場合は、クラスインスタンスの一部として値と参照を格納する 2 つの方法が提供されていることを知っているかもしれません。プロパティに加えて、インスタンス変数をプロパティに格納されている値のバッキングストアとして使用できます。

Swift は、これらの概念を単一のプロパティ宣言に統合しました。Swift のプロパティには対応するインスタンス変数がなく、プロパティのバッキングストアには直接アクセスしません。このアプローチにより、様々なコンテキストで値にアクセスできてしまうことから起きる混乱が回避され、ただ唯一のプロパティの宣言へと単純化されています。名前、型、メモリ管理特性など、プロパティに関する全ての情報は、型の定義の一部として 1 つの場所に集約されます。

## Computed Properties(計算プロパティ)

格納プロパティに加えて、クラス、構造体、および列挙型は、値を保存しない計算プロパティを定義できます。代わりに、間接的に他のプロパティの値の取得するゲッタと、必要ならば、値の設定を行うセッタを提供します。

```swift
struct Point {
    var x = 0.0, y = 0.0
}
struct Size {
    var width = 0.0, height = 0.0
}
struct Rect {
    var origin = Point()
    var size = Size()
    var center: Point {
        get {
            let centerX = origin.x + (size.width / 2)
            let centerY = origin.y + (size.height / 2)
            return Point(x: centerX, y: centerY)
        }
        set(newCenter) {
            origin.x = newCenter.x - (size.width / 2)
            origin.y = newCenter.y - (size.height / 2)
        }
    }
}
var square = Rect(origin: Point(x: 0.0, y: 0.0),
                  size: Size(width: 10.0, height: 10.0))
let initialSquareCenter = square.center
square.center = Point(x: 15.0, y: 15.0)
print("square.origin is now at (\(square.origin.x), \(square.origin.y))")
// "square.origin is now at (10.0, 10.0)"
```

この例では、幾何学的形状を操作するための 3 つの構造体を定義しています。

* `Point` は、x 座標と y 座標をカプセル化しています
* `Size` は `width` と `height` をカプセル化しています
* `Rect` は、原点とサイズで四角形を定義しています

`Rect` 構造体は、`center` と呼ばれる計算プロパティも提供します。`Rect` の現在の中心位置は常にその `origin` と `size` から判断できるため、中心点を明示的に `Point` として保存する必要はありません。代わりに、`Rect` は `center` と呼ばれる計算プロパティ変数のカスタム ゲッタとセッタを定義して、あたかも格納プロパティかのように四角形の中心を操作できるようにします。

上記の例では、`square` という新しい `Rect` 変数を作成しています。`square` 変数は、原点 `(0, 0)`、幅と高さ `10` で初期化されます。この正方形は、下の図では青い正方形で表されています。

次に、ドット構文(`square.center`)を介して `Square` 変数の `center` プロパティにアクセスします。これにより、`center` のゲッタが呼び出され、現在のプロパティ値が取得できます。ゲッタは、既存の値を返すのではなく、正方形の中心を表す新しい `Point` を計算して返します。上記のように、ゲッタは `(5, 5)` の中心点を正しく返します。

次に、`center` プロパティに新しく `(15, 15)` が設定されます。これにより、下の図のオレンジ色の正方形で示されている新しい位置に正方形が上下に移動します。`center` プロパティを設定すると、保存されている `origin` プロパティの `x` と `y` の値を変更する `center` のセッタが呼び出され、正方形が新しい位置に移動します。

![計算プロパティ](./../.gitbook/assets/computedProperties_2x.png)

### Shorthand Setter Declaration(短縮セッタ宣言)

計算プロパティのセッタが、設定される新しい値の名前を定義しない場合、デフォルト名の `newValue` が使用されます。この省略表記を利用した `Rect` 構造体の代替バージョンを次に示します:

```swift
struct AlternativeRect {
    var origin = Point()
    var size = Size()
    var center: Point {
        get {
            let centerX = origin.x + (size.width / 2)
            let centerY = origin.y + (size.height / 2)
            return Point(x: centerX, y: centerY)
        }
        set {
            origin.x = newValue.x - (size.width / 2)
            origin.y = newValue.y - (size.height / 2)
        }
    }
}
```

### Shorthand Getter Declaration(短縮ゲッタ宣言)

ゲッタの本文全体が単一の式の場合、ゲッタは暗黙的にその式を返します。この省略表記とセッタの省略表記を利用した別のバージョンの `Rect` 構造体を次に示します:

```swift
struct CompactRect {
    var origin = Point()
    var size = Size()
    var center: Point {
        get {
            Point(x: origin.x + (size.width / 2),
                  y: origin.y + (size.height / 2))
        }
        set {
            origin.x = newValue.x - (size.width / 2)
            origin.y = newValue.y - (size.height / 2)
        }
    }
}
```

ゲッタからの戻り値の省略は、[Functions With an Implicit Return](./functions.md#functions-with-an-implicit-return暗黙的な戻り値がある関数)で説明されているように、関数からの戻り値を省略した場合と同じ規則に従います。

### Read-Only Computed Properties(読み取り専用計算プロパティ)

ゲッタはあるがセッタは持たない計算プロパティは、読み取り専用の計算プロパティ(*read-only computed property*)と呼ばれます。読み取り専用計算プロパティは常に値を返し、ドット構文を介してアクセスできますが、別の値を設定することはできません。

> NOTE  
> 計算プロパティ(読み取り専用を含む)は、その値が固定されていないため、`var` キーワードを使用して変数プロパティとして宣言する必要があります。`let` キーワードは定数プロパティにのみ使用され、インスタンスの初期化の一部として設定された後は値を変更できないことを示します。

`get` キーワードとその中括弧(`{}`)を削除することで、読み取り専用計算プロパティの宣言を簡素化できます。

```swift
struct Cuboid {
    var width = 0.0, height = 0.0, depth = 0.0
    var volume: Double {
        return width * height * depth
    }
}
let fourByFiveByTwo = Cuboid(width: 4.0, height: 5.0, depth: 2.0)
print("the volume of fourByFiveByTwo is \(fourByFiveByTwo.volume)")
// "the volume of fourByFiveByTwo is 40.0"
```

この例では、`Cuboid` という新しい構造体を定義します。これは、`width`、`height`、`depth` プロパティを持つ 3D の長方形のボックスを表します。この構造体には、`volume` と呼ばれる読み取り専用の計算プロパティもあります。これは、直方体の現在のボリュームを計算して返します。特定のボリューム値に `width`、`height`、`depth` のどの値を使用するかについてあいまいになるため、`volume` を設定可能にすることは意味がありません。それでも、`Cuboid` が読み取り専用の計算プロパティを提供して、外部ユーザーが現在の計算済みボリュームを見られようにすると便利です。

## Property Observers(プロパティオブザーバ)

プロパティオブザーバ(*property observer*)は、プロパティの値の変化を監視し、それに対応します。プロパティオブザーバは、新しい値がプロパティの現在の値と同じ場合でも、プロパティの値が設定されるたびに呼び出されます。

次の場所にプロパティオブザーバを追加できます。

* 自身で定義した格納プロパティ
* 継承した格納プロパティ
* 継承した計算済みプロパティ

継承されたプロパティの場合、サブクラスでそのプロパティをオーバーライドすることにより、プロパティオブザーバを追加します。自身で定義した計算プロパティの場合、オブザーバーを作成しようとする代わりに、プロパティのセッタを使用して値の変更を監視し、応答します。プロパティのオーバーライドについては、[Overriding](./inheritance.md#overridingオーバーライド)オーバーライドで説明されています。

プロパティにこれらのオブザーバのいずれかまたは両方を定義するオプションがあります。

* `willSet` は、値が格納される直前に呼び出されます
* `didSet` は、新しい値が格納された直後に呼び出されます

`willSet` オブザーバを実装すると、新しいプロパティ値が定数パラメータとして渡されます。 `willSet` 実装の一部として、このパラメータの名前を指定できます。 実装内にパラメータ名と括弧を記述しない場合、パラメータは `newValue` というのデフォルトのパラメータ名で使用可能になります。

同様に、 `didSet` オブザーバを実装する場合、古いプロパティ値を含む定数パラメータが渡されます。パラメータに名前を付けるか、`oldValue` というのデフォルト パラメータ名を使用できます。独自の `didSet` オブザーバ内のプロパティに値を割り当てると、新しい値によって、設定されたばかりの値が置き換えられます。

> NOTE  
> スーパークラス プロパティの `willSet` および `didSet` オブザーバは、プロパティがサブクラスのイニシャライザで設定されると、スーパークラスのイニシャライザが呼び出された後に呼び出されます。 スーパークラスのイニシャライザが呼び出される前に、サブクラスが独自のプロパティを設定している間は呼び出されません。  
> イニシャライザの委譲については、[Initializer Delegation for Value Types](./initialization.md#initializer-delegation-for-value-types値型のイニシャライザの委譲)、[Initializer Delegation for Class Types](./initialization.md#initializer-delegation-for-class-typesクラス型のイニシャライザの委譲)を参照ください。

`willSet` と `didSet` の使用例を次に示します。下記の例では、`StepCounter` という名前の新しいクラスを定義しています。これは、人の合計歩数を追跡します。このクラスは、歩数計やその他の歩数計からの入力データや日常生活での運動を追跡する歩数計に使われます。

```swift
class StepCounter {
    var totalSteps: Int = 0 {
        willSet(newTotalSteps) {
            print("About to set totalSteps to \(newTotalSteps)")
        }
        didSet {
            if totalSteps > oldValue  {
                print("Added \(totalSteps - oldValue) steps")
            }
        }
    }
}
let stepCounter = StepCounter()
stepCounter.totalSteps = 200
// About to set totalSteps to 200
// Added 200 steps
stepCounter.totalSteps = 360
// About to set totalSteps to 360
// Added 160 steps
stepCounter.totalSteps = 896
// About to set totalSteps to 896
// Added 536 steps
```

`StepCounter` クラスは、`Int` 型の `totalSteps` プロパティを宣言します。これは、`willSet` および `didSet` オブザーバを持つ格納プロパティです。

プロパティに新しい値が割り当てられるたびに、`totalSteps` の `willSet` および `didSet` オブザーバーが呼び出されます。これは、新しい値が現在の値と同じでも当てはまります。

この例の `willSet` オブザーバは、次の新しい値に `newTotalSteps` のカスタムパラメータ名を使用します。この例では、設定しようとしている値を出力するだけです。

`didSet` オブザーバは、`totalSteps` の値が更新された後に呼び出されます。 これは、`totalSteps` の新しい値を古い値と比較します。合計ステップ数が増えると、新しく何ステップ増えたかを示すメッセージが出力されます。`didSet` オブザーバは古い値のカスタム パラメータ名を提供せず、代わりに `oldValue` のデフォルト名が使用されます。

> NOTE  
> オブザーバを持つプロパティを関数に入出力パラメータとして渡すと、`willSet` および `didSet` オブザーバが常に呼び出されます。これは、入力パラメータのコピーインコピーアウト(copy-in copy-out)メモリモデルによるものです。値は常に関数の最後でプロパティに書き戻されます。入出力パラメータの動作の詳細については、[In-Out Parameters](./../language-reference/declarations.md#in-Out-parameters入出力パラメータ)を参照ください。

## Property Wrappers(プロパティラッパ)

プロパティラッパ(*Property Wrapper*)は、プロパティの格納方法を管理するコードと、プロパティを定義するコードとの間に境界レイヤを追加します。例えば、スレッドセーフチェックを提供するプロパティ、またはその基になるデータをデータベースに格納するプロパティがある場合、全てのプロパティにそのコードを記述する必要があります。プロパティラッパを使用すると、ラッパを定義するときに管理コードを 1 回記述し、その管理コードを複数のプロパティに適用して再利用できます。

プロパティラッパを定義するには、`wrappedValue` プロパティを定義する構造体、列挙型、またはクラスを作成します。下記のコードでは、`TwelveOrLess` 構造体により、ラップする値に常に `12` 以下の数値が含まれることが保証されます。より大きな数値を格納するように要求すると、代わりに `12` が格納されます。

```swift
@propertyWrapper
struct TwelveOrLess {
    private var number = 0
    var wrappedValue: Int {
        get { return number }
        set { number = min(newValue, 12) }
    }
}
```

セッタは新しい値が `12` 未満だと確認し、ゲッタは格納された値を返します。

> NOTE  
> 上記の例の `number` の宣言は、変数を `private` としてマークします。これにより、`number` は `TwelveOrLess` の実装でのみ使用されます。それ以外の場所に記述されたコードは、`wrappedValue` のゲッタとセッタを使用して値にアクセスし、数値を直接使用することはできません。`private` については、[Access Control](./access-control.md)を参照ください。

プロパティにラッパを適用するには、属性としてプロパティの前にラッパの名前を記述します。 `TwelveOrLess` プロパティラッパを使用して、大きさが常に `12` 以下になるようにする四角形を格納する構造体を次に示します。

```swift
struct SmallRectangle {
    @TwelveOrLess var height: Int
    @TwelveOrLess var width: Int
}

var rectangle = SmallRectangle()
print(rectangle.height)
// "0"

rectangle.height = 10
print(rectangle.height)
// "10"

rectangle.height = 24
print(rectangle.height)
// "12"
```

`height` と `width` のプロパティは、`TwelveOrLess.number` を 0 に設定する `TwelveOrLess` の定義から初期値を取得します。`TwelveOrLess` のセッタは、`10` を有効な値として扱うため、数値 `10` を `rectangle.height` に格納すると、記述どおりに進みます。ただし、`24` は `TwelveOrLess` が許容する値よりも大きいため、`24` を格納しようとすると、代わりに `rectangle.height` を最大許容値の `12` に設定することになります。

プロパティにラッパを適用すると、コンパイラは、ラッパにストレージを提供するコードと、ラッパを介したプロパティへのアクセスするためのコードの 2 つを合成します。(プロパティラッパ自体にはラップされた値を格納する責務があるため、合成されるコードはありません) 特別な属性構文を利用せずに、プロパティラッパの動作を使用するコードを作成することもできます。例えば、前のコードリストの `SmallRectangle` に、属性として `@TwelveOrLess` を記述する代わりに、`TwelveOrLess` 構造体でそのプロパティを明示的にラップしています。

```swift
struct SmallRectangle {
    private var _height = TwelveOrLess()
    private var _width = TwelveOrLess()
    var height: Int {
        get { return _height.wrappedValue }
        set { _height.wrappedValue = newValue }
    }
    var width: Int {
        get { return _width.wrappedValue }
        set { _width.wrappedValue = newValue }
    }
}
```

`_height` および `_width` プロパティは、プロパティラッパ `TwelveOrLess` のインスタンスを格納します。`height` と `width` のゲッタとセッタは、`wrappedValue` プロパティへのアクセスをラップします。

### Setting Initial Values for Wrapped Properties(ラップされたプロパティの初期値の設定)

上記の例のコードは、`TwelveOrLess` の定義で `number` に初期値を与えることにより、ラップされたプロパティの初期値を設定します。このプロパティラッパを使用するコードでは、`TwelveOrLess` でラップされたプロパティに異なる初期値を指定できません。例えば、`SmallRectangle` の定義では `height` や `width` の初期値を指定できません。初期値の設定やその他のカスタマイズをサポートするには、プロパティラッパでイニシャライザを追加する必要があります。これは、ラップされた値と最大値を設定するイニシャライザを定義する `SmallNumber` と呼ばれる `TwelveOrLess` の拡張バージョンです。

```swift
@propertyWrapper
struct SmallNumber {
    private var maximum: Int
    private var number: Int

    var wrappedValue: Int {
        get { return number }
        set { number = min(newValue, maximum) }
    }

    init() {
        maximum = 12
        number = 0
    }
    init(wrappedValue: Int) {
        maximum = 12
        number = min(wrappedValue, maximum)
    }
    init(wrappedValue: Int, maximum: Int) {
        self.maximum = maximum
        number = min(wrappedValue, maximum)
    }
}
```

`SmallNumber` の定義には、`init()`、`init(wrappedValue:)`、および `init(wrappedValue:maximum:)` の 3 つのイニシャライザが含まれています。下記の例では、これらのイニシャライザを使用して、ラップされた値と最大値を設定します。イニシャライザとその構文については、[Initialization](./initialization.md)を参照ください。

プロパティにラッパを適用し、初期値を指定しない場合、Swift は `init()` を使用してラッパを設定します。例えば:

```swift
truct ZeroRectangle {
    @SmallNumber var height: Int
    @SmallNumber var width: Int
}

var zeroRectangle = ZeroRectangle()
print(zeroRectangle.height, zeroRectangle.width)
// "0 0"
```

`height` と `width` をラップする `SmallNumber` のインスタンスは、`SmallNumber()` を呼び出すことによって作成されます。そのイニシャライザ内のコードは、0 と 12 のデフォルト値を使用して、ラップされた初期値と最大値の初期値を設定します。プロパティラッパは、`SmallRectangle` で `TwelveOrLess` を使用した以前の例のように、引き続き全ての初期値を設定します。その例とは異なり、`SmallNumber` は、プロパティの宣言の一部としてこれらの初期値を書き込むこともサポートしています。

プロパティの初期値を指定すると、Swift は `init(wrappedValue:)` イニシャライザを使用してラッパを設定します。例えば:

```swift
struct UnitRectangle {
    @SmallNumber var height: Int = 1
    @SmallNumber var width: Int = 1
}

var unitRectangle = UnitRectangle()
print(unitRectangle.height, unitRectangle.width)
// "1 1"
```

ラッパを使用してプロパティに `= 1` を書き込むと、それは `init(wrappedValue:)` イニシャライザの呼び出しに変換されます。`height` と `width` をラップする `SmallNumber` のインスタンスは、`SmallNumber(wrappedValue: 1)` を呼び出すことによって作成されます。イニシャライザは、ここで指定されたラップされた値を使用し、デフォルトの最大値の `12` を使用します。

カスタム属性の後の括弧内に引数を記述すると、Swift はそれらの引数を受け入れるイニシャライザ初期化子を使用してラッパを設定します。 たとえば、初期値と最大値を指定すると、Swift は `init(wrappedValue:maximum:)` イニシャライザを使用します:

```swift
struct NarrowRectangle {
    @SmallNumber(wrappedValue: 2, maximum: 5) var height: Int
    @SmallNumber(wrappedValue: 3, maximum: 4) var width: Int
}

var narrowRectangle = NarrowRectangle()
print(narrowRectangle.height, narrowRectangle.width)
// "2 3"

narrowRectangle.height = 100
narrowRectangle.width = 100
print(narrowRectangle.height, narrowRectangle.width)
// "5 4"
```

`height` をラップする `SmallNumber` のインスタンスは `SmallNumber(wrappedValue: 2, maximum: 5)` を呼び出すことで作成され、`width` 幅をラップするインスタンスは `SmallNumber(wrappedValue: 3, maximum: 4)` を呼び出すことで作成されます。

プロパティラッパに引数を含めることで、ラッパの初期状態を設定したり、ラッパの作成時に他のオプションをラッパに渡すことができます。この構文は、プロパティラッパを使用する最も一般的な方法です。必要な引数を属性に指定でき、それらはイニシャライザに渡されます。

プロパティラッパに引数を含める場合、値を設定して初期値を指定することもできます。Swift は、この値の代入を `wrappedValue` 引数のように扱い、含まれる引数を受け入れるイニシャライザを使用します。例えば:

```swift
struct MixedRectangle {
    @SmallNumber var height: Int = 1
    @SmallNumber(maximum: 9) var width: Int = 2
}

var mixedRectangle = MixedRectangle()
print(mixedRectangle.height)
// "1"

mixedRectangle.height = 20
print(mixedRectangle.height)
// "12"
```

`height` をラップする `SmallNumber` のインスタンスは、デフォルトの最大値 12 を使用する `SmallNumber(wrappedValue: 1)` を呼び出すことによって作成されます。`width` をラップするインスタンスは、`SmallNumber(wrappedValue: 2, maximum: 9)` を呼び出すことによって作成されます。

### Projecting a Value From a Property Wrapper(Property Wrapperからの値の投影)

プロパティラッパは、ラップされた値に加えて、投影値(*projected value*)を定義することで追加機能を公開できます。例えば、データベースへのアクセスを管理するプロパティラッパは、その投影値で `flushDatabaseConnection()` メソッドを提供できます。投影値の名前は、ドル記号 (`$`) で始まることを除いて、ラップされた値と同じです。コードでは `$` で始まるプロパティを定義できないため、投影値が定義したプロパティに干渉することはありません。

上記の `SmallNumber` の例では、プロパティを大きすぎる数値に設定しようとすると、プロパティラッパは数値を格納する前に調整します。下記のコードは、`projectedValue` プロパティを `SmallNumber` 構造体に追加して、プロパティラッパが新しい値を格納する前にプロパティの新しい値を調整したかどうかを追跡します。

```swift
@propertyWrapper
struct SmallNumber {
    private var number = 0
    var projectedValue = false
    var wrappedValue: Int {
        get { return number }
        set {
            if newValue > 12 {
                number = 12
                projectedValue = true
            } else {
                number = newValue
                projectedValue = false
            }
        }
    }
}
struct SomeStructure {
    @SmallNumber var someNumber: Int
}
var someStructure = SomeStructure()

someStructure.someNumber = 4
print(someStructure.$someNumber)
// "false"

someStructure.someNumber = 55
print(someStructure.$someNumber)
// "true"
```

`someStructure.$someNumber` を書き込むと、ラッパの投影値にアクセスします。 4 などの小さな数を格納した後、`someStructure.$someNumber` の値は `false` です。ただし、55 などの大きすぎる数値を格納しようとすると、投影値は `true` になります。

プロパティ ラッパーは、投影された値として任意の型の値を返すことができます。この例では、プロパティラッパは、数値が調整されたかどうかに関係なく、1 つの情報のみを公開するため、そのブール値をその投影値として公開します。より多くの情報を公開する必要があるラッパは、他のデータ型のインスタンスを返すことも、`self` を返してラッパーのインスタンスをその投影値として公開することもできます。

プロパティのゲッタやインスタンスメソッドなど、型の一部から投影された値にアクセスする場合、他のプロパティにアクセスするのと同じように、プロパティ名の前の `self.` を省略できます。次の例のコードは、`height` と `width` のラッパの投影値を `$height` および `$width` として参照します。

```swift
enum Size {
    case small, large
}

struct SizedRectangle {
    @SmallNumber var height: Int
    @SmallNumber var width: Int

    mutating func resize(to size: Size) -> Bool {
        switch size {
        case .small:
            height = 10
            width = 20
        case .large:
            height = 100
            width = 100
        }
        return $height || $width
    }
}
```

プロパティラッパの構文は、ゲッタとセッタを持つプロパティの単なるシュガー構文なので、`height` と `width` へのアクセスは、他のプロパティへのアクセスと同じように働きます。例えば、`resize(to:)` のコードは、プロパティラッパを使用して `height` と `width` にアクセスします。`resize(to: .large)` を呼び出すと、`.large` の switch の case により、長方形の高さと幅が 100 に設定されます。ラッパはそれぞれのプロパティが 12 を超えないようにするため、値を調整したことを記録し、投影値に `true` が設定されます。`resize(to:)` の最後で、`return` ステートメントは `$height` と `$width` をチェックして、プロパティラッパが `height` または `width` のいずれかを調整したかどうかを判断します。

## Global and Local Variables(グローバル変数とローカル変数)

プロパティを計算および監視するための上記の機能は、グローバル変数とローカル変数でも使用できます。グローバル変数は、関数、メソッド、クロージャ、または特定の型の外部で定義される変数です。ローカル変数は、関数、メソッド、またはクロージャ内で定義される変数です。

前の章で登場したグローバル変数とローカル変数は全て格納変数です。格納プロパティと同様に、格納変数は、特定の型の値を保存し、その値を設定および取得できるようにします。

ただし、グローバルスコープまたはローカルスコープのいずれかでも、計算変数を定義したり、格納変数のオブザーバを定義することもできます。計算変数は、値を保存するのではなく計算し、計算プロパティと同じ方法で記述されます。

> NOTE  
> グローバル定数と変数は、遅延格納プロパティと同様の方法で、常に必要なったら計算されます。遅延格納プロパティとは異なり、グローバル定数と変数は `lazy` 修飾子でマークする必要はありません。  
>  
> ローカル定数と変数は決して遅延して計算されません。

プロパティラッパは、ローカルに保存された変数に適用できますが、グローバル変数または計算変数には適用できません。例えば、下記のコードでは、`myNumber` は `SmallNumber` をプロパティラッパとして使用します。

```swift
func someFunction() {
    @SmallNumber var myNumber: Int = 0

    myNumber = 10
    // myNumber は 10

    myNumber = 24
    // myNumber は 12
}
```

プロパティに小数を適用する場合と同様に、`myNumber` を 10 に設定することは有効です。プロパティラッパは 12 を超える値を許可しないため、`myNumber` を 24 ではなく 12 に設定します。

## Type Properties

### Type Property Syntax

### Querying and Setting Type Properties