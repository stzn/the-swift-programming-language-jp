# Initialization \(初期化\)

最終更新日: 2021/6/29

初期化\(_Initialization_\)は、使用するクラス、構造体、または列挙型のインスタンスを準備するプロセスです。このプロセスには、そのインスタンスに保存されている各プロパティの初期値の設定と、新しいインスタンスの使用準備が整う前に必要なその他の要素のセットアップや初期化の実行が含まれます。

この初期化プロセスを実装するには、イニシャライザ\(_initializers_\)を定義します。イニシャライザは、特定の型の新しいインスタンスを作成するために呼び出すことができる特別なメソッドのようなものです。Objective-C のイニシャライザとは異なり、Swift のイニシャライザは値を返しません。主な役割は、型の新しいインスタンスが初めて使用される前に正しく初期化されていることを保証することです。

クラス型のインスタンスは、そのクラスのインスタンスが解放される直前にカスタムのクリーンアップを実行するデイニシャライザ\(_deinitializer_\)を実装することもできます。デイニシャライザの詳細については、[Deinitialization(デイニシャライザ)](deinitialization.md)を参照ください。

## Setting Initial Values for Stored Properties\(格納プロパティの初期値の設定\)

クラスと構造体は、そのクラスまたは構造体のインスタンスが作成されるまでに、全ての格納プロパティに適切な初期値を設定する必要があります。格納プロパティは、不確定な状態のままにすることはできません。

イニシャライザ内、またはプロパティの定義の一部としてデフォルトのプロパティ値を割り当てることにより、格納プロパティの初期値を設定できます。これらのアクションについては、次のセクションで説明します。

> NOTE  
> 格納プロパティにデフォルト値を割り当てるか、イニシャライザ内でその初期値を設定すると、プロパティオブザーバを呼び出さずに、そのプロパティに値が直接設定されます。

### Initializers\(イニシャライザ\)

*イニシャライザ*は、特定の型の新しいインスタンスを作成するために呼び出されます。最もシンプルな形式では、イニシャライザはパラメータのないインスタンスメソッドのようなもので、`init` キーワードを使用して記述されます:

```swift
init() {
    // ここで初期化を実行します
}
```

下記の例では、華氏で表された温度を保存するために `Fahrenheit` という新しい構造体を定義しています。`Fahrenheit` 構造体には、`Double` 型の `temperature` という 1 つの格納プロパティがあります。

```swift
struct Fahrenheit {
    var temperature: Double
    init() {
        temperature = 32.0
    }
}
var f = Fahrenheit()
print("The default temperature is \(f.temperature)° Fahrenheit")
// "The default temperature is 32.0° Fahrenheit"
```

この構造体はパラメータのない単一のイニシャライザ `init` を定義します。これは、格納された温度を `32.0`\(華氏での水の凝固点\)の値で初期化します。

### Default Property Values\(デフォルトのプロパティ値\)

上記のように、イニシャライザ内から格納プロパティの初期値を設定できます。または、プロパティの宣言の一部として、定義時に初期値を割り当てることで*デフォルトプロパティ値*を指定します。

> NOTE  
> プロパティが常に同じ初期値を取る場合は、イニシャライザ内で値を設定するのではなく、デフォルト値を指定します。最終結果は同じですが、デフォルト値の方がプロパティの初期化と宣言をより密接に結び付けています。これにより、イニシャライザが短く明確になり、デフォルト値からプロパティの型を推論できるようになります。また、デフォルト値を使用すると、この章で後述するように、デフォルトのイニシャライザとイニシャライザの継承を利用しやすくなります。

プロパティが宣言された時点で `temperature` プロパティのデフォルト値を指定することにより、上記の `Fahrenheit` 構造体をよりシンプルな形式で書くことができます。

```swift
struct Fahrenheit {
    var temperature = 32.0
}
```

## Customizing Initialization\(初期化のカスタマイズ\)

次のセクションで説明するように、入力パラメータやオプショナル型のプロパティを使用したり、初期化中に定数プロパティを割り当てることにより、初期化プロセスをカスタマイズできます。

### Initialization Parameters\(イニシャライザのパラメータ\)

初期化パラメータをイニシャライザの定義の一部として提供することで、初期化プロセスをカスタマイズするための値の型と名前を定義できます。初期化パラメータには、関数およびメソッドパラメータと同じ機能と構文があります。

次の例では、摂氏で表された温度を格納する `Celsius` という構造体を定義しています。`Celsius` 構造体は、`init(fromFahrenheit:)` および `init(fromKelvin:)` という 2 つのカスタムイニシャライザを実装しています。これらは、構造体の新しいインスタンスを異なる温度スケールの値で初期化しています。

```swift
struct Celsius {
    var temperatureInCelsius: Double
    init(fromFahrenheit fahrenheit: Double) {
        temperatureInCelsius = (fahrenheit - 32.0) / 1.8
    }
    init(fromKelvin kelvin: Double) {
        temperatureInCelsius = kelvin - 273.15
    }
}
let boilingPointOfWater = Celsius(fromFahrenheit: 212.0)
// boilingPointOfWater.temperatureInCelsius は 100.0
let freezingPointOfWater = Celsius(fromKelvin: 273.15)
// freezingPointOfWater.temperatureInCelsius は 0.0
```

最初のイニシャライザには、`fromFahrenheit` という引数ラベルと `fahrenheit` というパラメータ名を持つ単一のパラメータがあります。2 番目のイニシャライザには、`fromKelvin` という引数ラベルと `kelvin` というパラメータ名を持つ単一のパラメータがあります。どちらのイニシャライザも、単一のパラメータを対応する摂氏の値に変換し、この値を `temperatureInCelsius` というプロパティに格納しています。

### Parameter Names and Argument Labels\(パラメータ名と引数ラベル\)

関数やメソッドのパラメータと同様に、初期化パラメータには、イニシャライザの本文内で使用するパラメータ名と、イニシャライザを呼び出すときに使用する引数ラベルの両方を含めることができます。

ただし、イニシャライザには、関数やメソッドのように括弧の前に識別関数名がありません。したがって、イニシャライザのパラメータの名前と型は、どのイニシャライザを呼び出す必要があるかを識別する上で特に重要な役割を果たします。このため、Swift は、イニシャライザが提供されていない場合、イニシャライザの全てのパラメータに自動で引数ラベルを提供します。

次の例では、`red`、`green`、`blue` という 3 つの定数プロパティを持つ `Color` という構造体を定義しています。これらのプロパティには、色の赤、緑、青の量を示す `0.0` ～ `1.0` の値が格納されます。

`Color` は、赤、緑、および青のコンポーネント用に `Double` 型の適切に名前が付けられた 3 つのパラメータを含むイニシャライザを提供します。`Color` は、3 つ全ての色コンポーネントに同じ値を提供するために使用される、単一の `white` パラメータを持つ 2 番目のイニシャライザも提供します。

```swift
struct Color {
    let red, green, blue: Double
    init(red: Double, green: Double, blue: Double) {
        self.red   = red
        self.green = green
        self.blue  = blue
    }
    init(white: Double) {
        red   = white
        green = white
        blue  = white
    }
}
```

両方のイニシャライザを使用して、各イニシャライザのパラメータに名前付きの値を指定することで、新しい `Color` インスタンスを作成できます。

```swift
let magenta = Color(red: 1.0, green: 0.0, blue: 1.0)
let halfGray = Color(white: 0.5)
```

引数ラベルを使用せずにこれらのイニシャライザを呼び出すことはできないことに注意してください。引数ラベルが定義されている場合は、常にイニシャライザで使用する必要があり、それらを省略するとコンパイルエラーになります。

```swift
let veryGreen = Color(0.0, 1.0, 0.0)
// 引数ラベルが必要になるため、コンパイルエラーになります
```

### Initializer Parameters Without Argument Labels\(引数ラベルのないイニシャライザパラメータ\)

初期化パラメータに引数ラベルを使用したくない場合は、そのパラメータの明示的な引数ラベルの代わりにアンダースコア \(`_`\) を記述して、デフォルトの挙動をオーバーライドします。

これは、上記の[Initialization Parameters(イニシャライザのパラメータ)](#initialization-parametersイニシャライザのパラメータ)の `Celsius` の例の拡張バージョンで、すでに摂氏スケールにある `Double` 値から新しい `Celsius` インスタンスを作成する追加のイニシャライザを備えています。

```swift
struct Celsius {
    var temperatureInCelsius: Double
    init(fromFahrenheit fahrenheit: Double) {
        temperatureInCelsius = (fahrenheit - 32.0) / 1.8
    }
    init(fromKelvin kelvin: Double) {
        temperatureInCelsius = kelvin - 273.15
    }
    init(_ celsius: Double) {
        temperatureInCelsius = celsius
    }
}
let bodyTemperature = Celsius(37.0)
// bodyTemperature.temperatureInCelsius は 37.0
```

イニシャライザの呼び出し `Celsius(37.0)` は、意図が明確なので引数ラベルを必要としません。したがって、このイニシャライザを `init(_ celsius: Double)` と記述して、名前のない `Double` 値を指定して呼び出すことができるのは妥当です。

### Optional Property Types\(オプショナルのプロパティ型\)

独自に定義した型に、論理的に「値なし」が許される格納プロパティがある場合\(おそらく、初期化中にその値を設定できないか、後で「値なし」になる可能性があるため\)、プロパティをオプショナル型を使って宣言できます。オプショナル型のプロパティは、値 `nil` で自動的に初期化されます。こうすることで、プロパティが初期化中に意図的に「まだ値がない」ことを示すことができます。

次の例では、`SurveyQuestion` というクラスを定義し、オプショナルの `String` プロパティとして `response` を定義しています。

```swift
class SurveyQuestion {
    var text: String
    var response: String?
    init(text: String) {
        self.text = text
    }
    func ask() {
        print(text)
    }
}
let cheeseQuestion = SurveyQuestion(text: "Do you like cheese?")
cheeseQuestion.ask()
// "Do you like cheese?"
cheeseQuestion.response = "Yes, I do like cheese."
```

アンケートの質問への回答は質問されるまでわからないため、回答プロパティは `String?` または `Optional<String>` で宣言されています。`SurveyQuestion` の新しいインスタンスが初期化されると、デフォルト値の `nil`、つまり「文字列はまだありません」が自動的に割り当てられます。

### Assigning Constant Properties During Initialization\(初期化中の定数プロパティの割り当て\)

初期化が完了するまでに明確な値が設定されている限り、初期化中の任意の時点で定数プロパティに値を割り当てることができます。定数プロパティに値が割り当てられると、それ以上変更することはできません。

> NOTE  
> クラスインスタンスの場合、初期化中に定数プロパティを変更できるのは、それを導入したクラスだけです。サブクラスでは変更できません。

上記の `SurveyQuestion` の例を修正して、質問の `text` プロパティに変数プロパティではなく定数プロパティを使用して、`SurveyQuestion` のインスタンスが作成されたら質問が変更されないことを示すことができます。`text` プロパティは定数ですが、クラスのイニシャライザ内で設定できます。

```swift
class SurveyQuestion {
    let text: String
    var response: String?
    init(text: String) {
        self.text = text
    }
    func ask() {
        print(text)
    }
}
let beetsQuestion = SurveyQuestion(text: "How about beets?")
beetsQuestion.ask()
// "How about beets?"
beetsQuestion.response = "I also like beets. (But not with cheese.)"
```

## Default Initializers\(デフォルトイニシャライザ\)

Swift は、全てのプロパティにデフォルト値を設定し、1 つもイニシャライザを提供しない構造体またはクラスに対して、デフォルトのイニシャライザを提供します。デフォルトのイニシャライザは、全てのプロパティがデフォルト値に設定された新しいインスタンスを作成するだけです。

この例では、ショッピングリスト内のアイテムの名前、数量、購入状態をカプセル化する `ShoppingListItem` というクラスを定義しています。

```swift
class ShoppingListItem {
    var name: String?
    var quantity = 1
    var purchased = false
}
var item = ShoppingListItem()
```

`ShoppingListItem` クラスの全てのプロパティにはデフォルト値があり、スーパークラスを持たない基本クラスのため、`ShoppingListItem` は、全てのプロパティにデフォルト値が設定された新しいインスタンスを作成するイニシャライザの実装を自動で獲得します\(`name` プロパティはオプショナルの `String` プロパティのため、この値がコードに記述されていなくても、デフォルト値の `nil` が自動的に設定されます\)。上記の例では、`ShoppingListItem` クラスのデフォルトイニシャライザを使用して `ShoppingListItem()` と記述し、新しいインスタンスを作成して `item` という変数に割り当てています。

### Memberwise Initializers for Structure Types\(構造体のメンバワイズイニシャライザ\)

構造体は、独自のカスタムイニシャライザを定義していない場合、*メンバワイズイニシャライザ*を自動的に定義されます。デフォルトイニシャライザとは異なり、構造体は、デフォルト値を持たないプロパティが格納されている場合でも、メンバワイズイニシャライザが定義されます。

メンバワイズイニシャライザは、新しい構造体インスタンスのプロパティを初期化する簡単な方法です。新しいインスタンスのプロパティの初期値は、プロパティ名によってメンバワイズイニシャライザに渡すことができます。

下記の例では、`width` と `height` という 2 つのプロパティを持つ `Size` という構造体を定義しています。両方のプロパティは、デフォルト値 `0.0` を割り当てることにより、`Double` 型だと推論されます。

`Size` 構造体は、新しい `Size` インスタンスを初期化するために使用できるメンバワイズイニシャライザ `init(width:height:)` が自動的に定義されています。

```swift
struct Size {
    var width = 0.0, height = 0.0
}
let twoByTwo = Size(width: 2.0, height: 2.0)
```

メンバワイズイニシャライザを呼び出すときは、デフォルト値を持つ全てのプロパティの値を省略できます。上記の例では、`Size` 構造体の `height` と `width` の両方のプロパティにデフォルト値があります。プロパティのいずれかまたは両方を省略できます。イニシャライザは、省略した全てのプロパティにデフォルト値を使用します。例えば、次のようになります:

```swift
let zeroByTwo = Size(height: 2.0)
print(zeroByTwo.width, zeroByTwo.height)
// "0.0 2.0"

let zeroByZero = Size()
print(zeroByZero.width, zeroByZero.height)
// "0.0 0.0"
```

## Initializer Delegation for Value Types\(値型のイニシャライザの委譲\)

イニシャライザは、他のイニシャライザを呼び出して、インスタンスの初期化の一部を実行できます。*イニシャライザの委譲*と呼ばれるこのプロセスは、複数のイニシャライザ間でコードが重複するのを防ぎます。

イニシャライザの委譲がどのように機能するか、および許可される委譲の形式に関する規則は、値型とクラス型で異なります。値型\(構造体と列挙型\)は継承をサポートしていないため、イニシャライザの委譲プロセスは比較的簡単です。ただし、[Inheritance(継承)](inheritance.md)で説明されているように、クラスは他のクラスを継承できます。これは、クラスが継承した全ての格納プロパティに、初期化中に適切な値が割り当てられることを保証する追加の責任があることを意味します。これらの責任は、下記の[Class Inheritance and Initialization(クラスの継承と初期化)](initialization.md#class-inheritance-and-initializationクラスの継承と初期化)で説明されています。

値型の場合、独自のカスタムイニシャライザを作成するときに、`self.init` を使用して同じ値型から他のイニシャライザを参照します。`self.init` は、イニシャライザ内からのみ呼び出すことができます。

値型のカスタムイニシャライザを定義すると、その型のデフォルトイニシャライザ\(または構造体の場合はメンバワイズイニシャライザ\)にアクセスできなくなることに注意してください。この制約は、より複雑なイニシャライザで初期化に必須の設定をしている場合に、誤って自動イニシャライザを使用することでイニシャライザが呼ばれない\(=初期化が完全にできない\)状況を防ぎます。

> NOTE  
> カスタムの値型をデフォルトイニシャライザとメンバワイズイニシャライザ、および独自のカスタムイニシャライザで初期化できるようにする場合は、値型の元の実装の一部としてではなく、extension にカスタムイニシャライザを記述します。詳細については、[Extension(拡張)](extensions.md)を参照ください。

次の例では、幾何学的な四角形を表すカスタム `Rect` 構造体を定義しています。この例では、`Size` と `Point` という 2 つの補助となる構造体が必要です。どちらも、全てのプロパティにデフォルト値 `0.0` を提供します:

```swift
struct Size {
    var width = 0.0, height = 0.0
}
struct Point {
    var x = 0.0, y = 0.0
}
```

下記の 3 つの方法のいずれかで、`Rect` 構造体を初期化できます。つまり、`0` で初期化されたデフォルトの `origin` と `size` のプロパティ値を使用するか、特定の原点とサイズを指定するか、特定の中心点とサイズを指定します。これらの初期化の選択肢は、`Rect` 構造体の定義の一部として 3 つのカスタムイニシャライザで表されます：

```swift
struct Rect {
    var origin = Point()
    var size = Size()
    init() {}
    init(origin: Point, size: Size) {
        self.origin = origin
        self.size = size
    }
    init(center: Point, size: Size) {
        let originX = center.x - (size.width / 2)
        let originY = center.y - (size.height / 2)
        self.init(origin: Point(x: originX, y: originY), size: size)
    }
}
```

最初の `Rect` イニシャライザ `init()` は、独自のカスタムイニシャライザがない場合に受け取るデフォルトのイニシャライザと機能的に同じです。このイニシャライザには、空の中括弧 `{}` のペアで表される空の本文があります。このイニシャライザを呼び出すと、原点とサイズのプロパティが両方ともプロパティ定義の `Point(x: 0.0, y: 0.0)` と `Size(width: 0.0, height: 0.0)` のデフォルト値で初期化された `Rect` インスタンスが返されます:

```swift
let basicRect = Rect()
// basicRect の origin は (0.0, 0.0) で size は (0.0, 0.0)
```

2 番目の `Rect` イニシャライザ `init(origin:size:)` は、独自のカスタムイニシャライザがない場合に構造体に自動で定義されるメンバワイズイニシャライザと機能的に同じです。このイニシャライザは、単に `origin` および `size` 引数の値を適切な格納されたプロパティに割り当てます:

```swift
let originRect = Rect(origin: Point(x: 2.0, y: 2.0),
                      size: Size(width: 5.0, height: 5.0))
// originRect の origin は (2.0, 2.0) で size は (5.0, 5.0)
```

3 番目の `Rect` イニシャライザ `init(center:size:)` は、もう少し複雑です。`center` と `size` 値に基づいて適切な原点を計算することから始めます。次に、`init(origin:size:)` イニシャライザを呼び出し\(委譲\)します。このイニシャライザは、適切なプロパティに新しい原点とサイズの値を保存します。

```swift
let centerRect = Rect(center: Point(x: 4.0, y: 4.0),
                      size: Size(width: 3.0, height: 3.0))
// centerRect の origin は (2.5, 2.5) で size は (3.0, 3.0)
```

`init(center:size:)` イニシャライザを使っても、`origin` と `size` の新しい値を適切なプロパティに割り当てることができます。ただし、`init(center:size:)` イニシャライザは、適切な機能をすでに提供している既存のイニシャライザを利用する方が便利\(そして意図がより明確\)です。

> NOTE  
> `init()` および `init(origin:size:)` イニシャライザを、定義の外で記述する別の方法については、[Extension(拡張)](extensions.md)を参照ください。

## Class Inheritance and Initialization\(クラスの継承と初期化\)

サブクラスがスーパークラスから継承する全てのプロパティを含む、クラスの全ての格納プロパティには、初期化中に初期値を割り当てる必要があります。

Swift は、クラス型に対して 2 種類のイニシャライザを定義して、全ての格納プロパティが確実に初期値を受け取るようにします。これらは、*指定イニシャライザ*および *covenience イニシャライザ*と呼ばれます。

### Designated Initializers and Convenience Initializers\(指定イニシャライザとcovenience イニシャライザ\)

*指定イニシャライザ*は、クラスの主要なイニシャライザです。指定イニシャライザは、そのクラスで導入された全てのプロパティを完全に初期化し、適切なスーパークラスのイニシャライザを呼び出して、スーパークラスへチェーンして初期化プロセスを続行します。

複数の指定イニシャライザを持つクラスはあまりなく、1 つだけ持つのが一般的です。指定イニシャライザは、初期化が行われる「漏斗」のような地点で、初期化プロセスはスーパークラスへチェーンして上に向かって続行します。

全てのクラスには、少なくとも 1 つの指定イニシャライザが必要です。場合によっては、この要件は、以下の[Automatic Initializer Inheritance(自動イニシャライザの継承)](initialization.md#automatic-initializer-inheritance自動イニシャライザの継承)で説明されているように、スーパークラスから 1 つ以上の指定イニシャライザを継承することで満たされます。

convenience イニシャライザは二次的なもので、クラスのイニシャライザをサポートします。convenience イニシャライザを定義して、指定イニシャライザのパラメータの一部をデフォルト値に設定し、convenience イニシャライザから指定イニシャライザを呼び出すことができます。また、特定のユースケースまたは入力値の型用にそのクラスのインスタンスを作成するために、convenience イニシャライザを定義することもできます。

クラスで必要ない場合は、convenience イニシャライザを提供する必要はありません。共通の初期化パターンへのショートカットとして時間の節約になったり、初期化の意図をより明確にできる場合は、常に convenience イニシャライザを作成してください。

### Syntax for Designated and Convenience Initializers\(指定イニシャライザとcovenience イニシャライザの構文\)

クラスの指定イニシャライザは、値型のシンプルなイニシャライザと同じ方法で記述します:

![指定イニシャライザ](../.gitbook/assets/14_designatedInitializer.png)

covenience イニシャライザは同じスタイルで記述できますが、`init` キーワードの前にスペース区切りで `covenience` 修飾子を配置します:

![covenience イニシャライザ](../.gitbook/assets/14_convenienceInitializer.png)

### Initializer Delegation for Class Types\(クラス型のイニシャライザの委譲\)

指定イニシャライザと convenience イニシャライザ間の関係を簡潔にするために、Swift はイニシャライザ間の呼び出しの委譲に次の 3 つのルールを適用します:

* **ルール 1** 指定イニシャライザは、直接スーパークラスの指定イニシャライザを呼び出す必要があります
* **ルール 2** convenience イニシャライザは、同じクラスの別のイニシャライザを呼び出す必要があります
* **ルール 3** convenience イニシャライザは、最終的に指定イニシャライザを呼び出す必要があります

これを覚える簡単な方法は次のとおりです。

* 指定イニシャライザは常に上に委譲する必要があります
* convenience イニシャライザは常に委譲する必要があります

これらのルールを次の図に示します:

![イニシャライザの委譲](../.gitbook/assets/initializerDelegation01_2x.png)

ここでは、スーパークラスには、1 つの指定イニシャライザと 2 つの convenience イニシャライザがあります。ある convenience イニシャライザが別の convenience イニシャライザを呼び出し、それが次に単一の指定イニシャライザを呼び出します。これは、上記のルール 2 と 3 を満たします。スーパークラス自体にはそれ以上のスーパークラスがないため、ルール 1 は適用されません。

この図のサブクラスには、2 つの指定イニシャライザと 1 つの convenience イニシャライザがあります。convenience イニシャライザは、同じクラスから別のイニシャライザしか呼び出せないため、2 つの指定イニシャライザのいずれかを呼び出す必要があります。これは、上記のルール 2 と 3 を満たします。上記のルール 1 を満たすには、両方の指定イニシャライザがスーパークラスの単一の指定イニシャライザを呼び出す必要があります。

> NOTE  
> これらのルールは、クラスのユーザが各クラスのインスタンスを作成する方法には影響しません。上の図の全てのイニシャライザを使用して、それらが属するクラスの完全に初期化されたインスタンスを作成できます。ルールは、クラスのイニシャライザの実装の記述方法にのみ影響します。

次の図は、4 つのクラスのより複雑なクラス階層を示しています。これは、この階層の指定イニシャライザがクラスイニシャライザ初期化の「漏斗」のように機能し、チェーン内のクラス間の相互関係をシンプルにする方法を示しています:

![より複雑なイニシャライザの委譲](../.gitbook/assets/initializerDelegation02_2x.png)

### Two-Phase Initialization\(2段階の初期化\)

Swift でのクラスの初期化は 2 段階のプロセスです。最初の段階では、各格納プロパティに、それを導入したクラスが初期値を割り当てます。全ての格納プロパティの初期状態が決定されると、第 2 段階が開始され、新しいインスタンスを使用する準備が整ったと見なされる前に、各クラスの格納プロパティをさらにカスタマイズする機会が与えられます。

2 段階の初期化プロセスを使用すると、初期化が安全になり、クラス階層内の各クラスに完全な柔軟性がもたらされます。2 段階の初期化は、プロパティ値が初期化される前にアクセスされるのを防ぎ、プロパティ値が別のイニシャライザによって予期せず別の値に設定されるのを防ぎます。

> NOTE  
> Swift の 2 段階の初期化プロセスは、Objective-C の初期化に似ています。主な違いは、第 1 段階では、Objective-C がゼロまたはヌル値 \(`0` や `nil` など\) を全てのプロパティに割り当てることです。Swift の初期化フローは、カスタムの初期値を設定できるという点でより柔軟であり、`0` または `nil` が有効なデフォルト値ではない型に対処できます。

Swift のコンパイラは、2 段階の初期化がエラーなしで完了したことを確認するために、4 つの有用な安全チェックを実行します。

#### **安全チェック 1**  

  指定イニシャライザは、スーパークラスのイニシャライザに委譲する前に、そのクラスによって導入された全てのプロパティが初期化されていることを確認する必要があります

前述のように、オブジェクトのメモリは、全ての格納プロパティの初期状態が判明すると、完全に初期化されたと見なされます。このルールが満たされるためには、指定イニシャライザは、チェーンを引き継ぐ前に、それ自体の全てのプロパティが初期化されていることを確認する必要があります。

#### **安全チェック 2**  

  指定イニシャライザは、継承したプロパティに値を割り当てる前に、スーパークラスのイニシャライザに委譲する必要があります。そうしないと、指定イニシャライザが割り当てる新しい値は、スーパークラスの初期化によって上書きされてしまいます

#### **安全チェック 3**  

  convenience イニシャライザは、値をプロパティ\(同じクラスで定義されたプロパティを含む\)に割り当てる前に、別のイニシャライザに委譲する必要があります。そうしないと、convenience イニシャライザが割り当てる新しい値は、独自クラスの指定イニシャライザによって上書きされます

#### **安全チェック 4**  

  イニシャライザは、初期化の最初の段階が完了するまで、インスタンスメソッドを呼び出したり、インスタンスプロパティの値を読み取ったり、値として自分自身を参照したりすることはできません

クラスインスタンスは、最初の段階が終了するまで完全には有効ではありません。プロパティへのアクセスとメソッドの呼び出しは、最初の段階の終了時にクラスインスタンスが有効なことがわかった場合にのみ可能です。

上記の 4 つの安全チェックに基づいて、2 段階の初期化がどのように実行されるかを次に示します:

#### **第 1 段階**  

* 指定イニシャライザまたは convenience イニシャライザがクラスで呼び出されます
* そのクラスの新しいインスタンスがメモリに割り当てられます。メモリはまだ初期化されていません
* そのクラスの指定イニシャライザは、そのクラスによって導入された全ての格納プロパティに値があることを確認します。この時点でこれらの格納プロパティのメモリが初期化されました
* 指定イニシャライザは、スーパークラスがそれ自身の格納プロパティに対して同じタスクを実行するために、スーパークラスのイニシャライザに委譲します
* これは、初期化チェーンの最上位に到達するまで、クラスの継承チェーンが継続します
* チェーンのトップに到達し、チェーン内の最後のクラスが、全ての格納プロパティに値があることを確認すると、インスタンスのメモリは完全に初期化されたと見なされ、第 1 段階が完了します

#### **第 2 段階**  

* チェーンの最上位から下に戻ると、チェーン内の指定された各イニシャライザは、インスタンスをさらにカスタマイズすることができます。イニシャライザは、`self` にアクセスして、そのプロパティを変更したり、インスタンスメソッドを呼び出したりできるようになりました
* 最後に、チェーン内の convenience イニシャライザは、インスタンスをカスタマイズして `self` を操作することができるようになります

仮のサブクラスとスーパークラスを使った第 1 段階の初期化呼び出し方法は次のとおりです:

![2段階の初期化 第 1 段階](../.gitbook/assets/twoPhaseInitialization01_2x.png)

この例では、初期化はサブクラスの convenience イニシャライザの呼び出しから始まります。この convenience イニシャライザは、まだプロパティを変更できません。まず convenience イニシャライザから同じクラスの指定イニシャライザに委譲します。

指定イニシャライザは、安全チェック 1 に従って、サブクラスの全てのプロパティに値があることを確認します。次に、指定イニシャライザからそのスーパークラスを呼び出して、チェーンの上に向かって初期化を続行します。

スーパークラスの指定イニシャライザは、スーパークラスの全てのプロパティに値があることを確認します。初期化するスーパークラスはこれ以上ないため、これ以上の委譲は必要ありません。

スーパークラスの全てのプロパティに初期値が設定されるとすぐに、そのメモリは完全に初期化されたと見なされ、第 1 段階が完了します。

第 2 段階での同じ初期化呼び出し方法は次のとおりです:

![2段階の初期化 第 2 段階](../.gitbook/assets/twoPhaseInitialization02_2x.png)

スーパークラスの指定イニシャライザは、インスタンスをさらにカスタマイズする機会を得ます\(ただし、必ず必要ではありません\)。

スーパークラスの指定イニシャライザが完了すると、サブクラスの指定イニシャライザで追加のカスタマイズを実行できます\(ただし、これも必ずしも行う必要はありません\)。

最後に、サブクラスの指定イニシャライザが終了すると、最初に呼び出された convenience イニシャライザは追加のカスタマイズを実行できます。

### Initializer Inheritance and Overriding\(イニシャライザの継承とオーバーライド\)

Objective-C のサブクラスとは異なり、Swift のサブクラスはデフォルトでスーパークラスのイニシャライザを継承しません。Swift のアプローチは、スーパークラスからのシンプルなイニシャライザが、より具体的なサブクラスによって継承され、完全または正しく初期化されていないサブクラスの新しいインスタンスを作成すること防ぎます。

> NOTE  
> スーパークラスのイニシャライザは特定の状況で継承できますが、安全で適切な場合に限ります。詳細については、以下の[Automatic Initializer Inheritance(自動イニシャライザの継承)](initialization.md#automatic-initializer-inheritance自動イニシャライザの継承)を参照してください。

サブクラスでそのスーパークラスの 1 つ以上の同じイニシャライザを使用したい場合、サブクラス内でそれらのイニシャライザをカスタムした実装を提供できます。

スーパークラスの指定イニシャライザに一致するサブクラスのイニシャライザを作成すると、その指定イニシャライザのオーバーライドを効率的に提供することができます。これを行うためには、サブクラスのイニシャライザ定義の前に `override` 修飾子を記述する必要があります。[Default Initializers(デフォルトイニシャライザ)](initialization.md#default-initializersデフォルトイニシャライザ)で説明されているように、これは、自動的に提供されるデフォルトのイニシャライザをオーバーライドする場合にも当てはまります。

オーバーライドされたプロパティ、メソッド、または subscript の場合と同様に、`override` 修飾子が存在すると、Swift は、スーパークラスにオーバーライドされる指定イニシャライザがあるかどうかを確認し、オーバーライドするイニシャライザのパラメータが意図したとおりの形になっていることを検証します。

> NOTE  
> サブクラスのイニシャライザの実装が convenience イニシャライザであっても、スーパークラスで指定イニシャライザをオーバーライドするときは、常に `override` 修飾子を記述します。

逆に、スーパークラスの convenience イニシャライザと一致するサブクラスのイニシャライザを作成する場合、上記の[Initializer Delegation for Class Types(クラス型のイニシャライザの委譲)](initialization.md#initializer-delegation-for-class-typesクラス型のイニシャライザの委譲)で説明したルールに従って、そのスーパークラスの convenience イニシャライザがサブクラスから直接呼び出されることはありません。したがって、サブクラスは\(厳密に言えば\)スーパークラスのイニシャライザをオーバーライドしていません。その結果、スーパークラスの convenience イニシャライザに一致する実装を提供する場合は、`override` 修飾子を記述しません。

下記の例では、`Vehicle` という基本クラスを定義しています。この基本クラスは、デフォルトの `Int` 値が 0 の `numberOfWheels` という格納プロパティを宣言しています。`numberOfWheels` プロパティは、`description` という計算プロパティで使用され、乗り物の特性を説明しています:

```swift
class Vehicle {
    var numberOfWheels = 0
    var description: String {
        return "\(numberOfWheels) wheel(s)"
    }
}
```

`Vehicle` クラスは、その唯一の格納プロパティにデフォルト値を提供し、カスタムのイニシャライザを提供していません。その結果、[Default Initializers(デフォルトイニシャライザ)](initialization.md#default-initializersデフォルトイニシャライザ)で説明されているように、デフォルトのイニシャライザを自動的に受け取ります。デフォルトイニシャライザは、\(使用可能な場合\)常にクラスの指定イニシャライザで、`numberOfWheels` が `0` の新しい `Vehicle` インスタンスを作成するために使用できます:

```swift
let vehicle = Vehicle()
print("Vehicle: \(vehicle.description)")
// Vehicle: 0 wheel(s)
```

次の例では、`Bicycle` という `Vehicle` のサブクラスを定義しています:

```swift
class Bicycle: Vehicle {
    override init() {
        super.init()
        numberOfWheels = 2
    }
}
```

`Bicycle` サブクラスは、カスタムの指定イニシャライザ `init()` を定義します。この指定イニシャライザは `Bicycle` のスーパークラスの指定イニシャライザと一致するため、`Bicycle` では `override` 修飾子でマークされています。

`Bicycle` の `init()` イニシャライザは、最初に `super.init()` を呼び出します。これは、`Bicycle` クラスのスーパークラスの `Vehicle` のデフォルトのイニシャライザを呼び出します。これにより、`Bicycle` がプロパティを変更する前に、継承したプロパティ `numberOfWheels` が `Vehicle` によって初期化されます。`super.init()` を呼び出した後、`numberOfWheels` の値は新しい値 `2` に置き換えられます。

`Bicycle` のインスタンスを作成する場合、継承した `description` 計算プロパティを呼び出して、`numberOfWheels` プロパティがどのように更新されたかを確認できます:

```swift
let bicycle = Bicycle()
print("Bicycle: \(bicycle.description)")
// Bicycle: 2 wheel(s)
```

サブクラスのイニシャライザが第 2 段階の初期化プロセスで何も行わず、スーパークラスに引数のない指定イニシャライザがある場合、サブクラスの全ての格納プロパティに値を割り当てた後、`super.init()` の呼び出しを省略できます。

この例では、`Hoverboard` と呼ばれる `Vehicle` の別のサブクラスを定義しています。イニシャライザでは、`Hoverboard` クラスはその `color` プロパティのみを設定します。このイニシャライザは、`super.init()` を明示的に呼び出す代わりに、スーパークラスのイニシャライザを暗黙的に呼び出してプロセスを完了しています。

```swift
class Hoverboard: Vehicle {
    var color: String
    init(color: String) {
        self.color = color
        // super.init() は暗黙的に呼ばれています
    }
    override var description: String {
        return "\(super.description) in a beautiful \(color)"
    }
}
```

`Hoverboard` のインスタンスは、`Vehicle` イニシャライザによって提供されるデフォルトのホイール数を使用します。

```swift
let hoverboard = Hoverboard(color: "silver")
print("Hoverboard: \(hoverboard.description)")
// Hoverboard: 0 wheel(s) in a beautiful silver
```

> NOTE  
> サブクラスは、初期化中に継承した変数プロパティを変更できますが、継承した定数プロパティを変更することはできません。

### Automatic Initializer Inheritance\(自動イニシャライザの継承\)

上記のように、サブクラスはデフォルトでスーパークラスのイニシャライザを継承しません。ただし、特定の条件が満たされた場合、スーパークラスのイニシャライザは自動的に継承されます。つまり、実際には、多くの場合でイニシャライザのオーバーライドを記述する必要がなく、安全にオーバーライドできる場合はいつでも最小限の手間で、スーパークラスのイニシャライザを継承できます。

サブクラスに導入する新しいプロパティにデフォルト値を提供する場合、次の 2 つのルールが適用されます:

#### **ルール 1**

* サブクラスが指定イニシャライザを定義していない場合、サブクラスはそのスーパークラスの指定イニシャライザを全て自動的に継承します

#### **ルール 2**

* サブクラスが、ルール 1 に従ってスーパークラスの指定イニシャライザを自動で継承するか、またはカスタムのイニシャライザを実装することによってスーパークラスの全ての指定イニシャライザを実装する場合、全てのスーパークラスの convenience イニシャライザを自動的に継承します

これらのルールは、サブクラスがさらに convenience イニシャライザを追加した場合にも適用されます。

> NOTE  
> サブクラスは、ルール 2 を満たす方法の 1 つとして、スーパークラスの指定イニシャライザをサブクラスの convenience イニシャライザとして実装できます。

### Designated and Convenience Initializers in Action\(指定とcovenience イニシャライザの挙動\)

次の例は、指定イニシャライザ、convenience イニシャライザ、および自動イニシャライザの継承の挙動を示しています。この例では、`Food`、`RecipeIngredient`、および `ShoppingListItem` という 3 つのクラスの階層を定義し、それらのイニシャライザがどのように相互作用するかを示しています。

階層の基本クラスは `Food` と呼ばれ、食品の名前をカプセル化するシンプルなクラスです。`Food` クラスは、`name` という単一の `String` プロパティを宣言し、`Food` インスタンスを作成するために 2 つのイニシャライザを提供します。

```swift
class Food {
    var name: String
    init(name: String) {
        self.name = name
    }
    convenience init() {
        self.init(name: "[Unnamed]")
    }
}
```

次の図は、`Food` クラスのイニシャライザのチェーンを示しています。

![Foodクラスのイニシャライザのチェーン](../.gitbook/assets/initializersExample01_2x.png)

クラスにはデフォルトで*メンバワイズイニシャライザ*を生成しないため、`Food` クラスは、`name` という単一の引数を受け取る指定イニシャライザを提供します。このイニシャライザを使用して、特定の名前で新しい `Food` インスタンスを作成できます。

```swift
let namedMeat = Food(name: "Bacon")
// namedMeat の name は "Bacon"
```

`Food` クラスの `init(name: String)` イニシャライザは、指定イニシャライザとして提供されています。これは、新しい `Food` インスタンスの全ての格納プロパティが完全に初期化されていることを保証するためです。`Food` クラスにはスーパークラスがないため、`init(name: String)` イニシャライザは、初期化を完了するために `super.init()` を呼び出す必要はありません。

`Food` クラスは、引数のない convenience イニシャライザ `init()` も提供します。`init()` イニシャライザは、`[Unnamed]` のという `name` の値を使って `Food` クラスの `init(name: String)` に委譲することで、新しい食品にデフォルトのプレースホルダ名を提供します。

```swift
let mysteryMeat = Food()
// mysteryMeat の name は "[Unnamed]"
```

階層の 2 番目のクラスは、`RecipeIngredient` と呼ばれる `Food` のサブクラスです。`RecipeIngredient` クラスは、料理レシピの材料をモデル化しています。これは、\(`Food` から継承する `name` プロパティに加えて\) `amount` という `Int` プロパティを宣言し、`RecipeIngredient` インスタンスを作成するための 2 つのイニシャライザを提供します。

```swift
class RecipeIngredient: Food {
    var quantity: Int
    init(name: String, quantity: Int) {
        self.quantity = quantity
        super.init(name: name)
    }
    override convenience init(name: String) {
        self.init(name: name, quantity: 1)
    }
}
```

次の図は、`RecipeIngredient` クラスのイニシャライザのチェーンを示しています。

![RecipeIngredientクラスのイニシャライザのチェーン](../.gitbook/assets/initializersExample02_2x.png)

`RecipeIngredient` クラスには、単一の指定イニシャライザ `init(name: String, amount: Int)` があり、これを使用して新しい `RecipeIngredient` インスタンスの全てのプロパティを設定できます。このイニシャライザは、渡された `quantity` 引数を `quantity` プロパティに割り当てることから開始します。これは、`RecipeIngredient` で定義された唯一の新しいプロパティです。その後、イニシャライザは `Food` クラスの `init(name: String)` イニシャライザに委譲します。このプロセスは、上記の[Two-Phase Initialization(2 段階の初期化)](initialization.md#two-phase-initialization2段階の初期化)の初期化の安全チェック 1 を満たしています。

`RecipeIngredient` は、名前だけで `RecipeIngredient` インスタンスを作成する convenience イニシャライザ `init(name: String)` も定義します。この convenience イニシャライザは、数量を `1` と想定して、明示的に数量を指定せずに `RecipeIngredient` インスタンスを作成します。この convenience イニシャライザの定義により、`RecipeIngredient` インスタンスの作成がより迅速かつ便利になり、単一量の `RecipeIngredient` インスタンスを複数作成する際にコードの重複が回避できます。この convenience イニシャライザは、単にクラスの指定イニシャライザに委譲し、`quantity` に `1` を渡します。

`RecipeIngredient` によって提供される `init(name: String)` convenience イニシャライザは、`Food` の `init(name: String)` 指定イニシャライザと同じパラメータを受け取ります。つまり、この convenience イニシャライザはスーパークラスから指定イニシャライザをオーバーライドするため、`override` 修飾子を付ける必要があります\([Initializer Inheritance and Overriding(イニシャライザの継承とオーバーライド)](initialization.md#initializer-inheritance-and-overridingイニシャライザの継承とオーバーライド)を参照\)。

`RecipeIngredient` は、convenience イニシャライザとして `init(name: String)` イニシャライザを提供していますが、さらにそのスーパークラスの指定イニシャライザの全ての実装を提供しています。したがって、`RecipeIngredient` は、そのスーパークラスの convenience イニシャライザも全て自動的に継承します。

この例では、`RecipeIngredient` のスーパークラスは `Food` で、`init()` という名前の convenience イニシャライザが 1 つあります。したがって、このイニシャライザは `RecipeIngredient` に継承されます。継承されたバージョンの `init()` は、`Food` バージョンではなく `RecipeIngredient` バージョンの `init(name：String)` に委譲することを除いて、`Food` バージョンとまったく同じように機能します。

これらの 3 つの全てのイニシャライザを使用して、新しい `RecipeIngredient` インスタンスを作成できます:

```swift
let oneMysteryItem = RecipeIngredient()
let oneBacon = RecipeIngredient(name: "Bacon")
let sixEggs = RecipeIngredient(name: "Eggs", quantity: 6)
```

階層の 3 番目の最後のクラスは、`ShoppingListItem` と呼ばれる `RecipeIngredient` のサブクラスです。`ShoppingListItem` クラスは、ショッピングリストに表示されるレシピの材料をモデル化しています。

ショッピングリストの全てのアイテムは、「未購入」として始まります。これを表すために、`ShoppingListItem` には、`purchased` という `Bool` 型プロパティが導入されており、デフォルト値は `false` です。`ShoppingListItem` は、インスタンスの説明をする `description` という計算プロパティも追加しています。

```swift
class ShoppingListItem: RecipeIngredient {
    var purchased = false
    var description: String {
        var output = "\(quantity) x \(name)"
        output += purchased ? " ✔" : " ✘"
        return output
    }
}
```

> NOTE  
> `ShoppingListItem` は、`purchased` の初期値を提供するイニシャライザを定義しません。これは、\(ここでモデル化されている\)ショッピングリスト内のアイテムは常に未購入から始まるためです。

導入する全てのプロパティにデフォルト値を提供し、それ自体はイニシャライザを定義しないため、`ShoppingListItem` は全ての指定イニシャライザと convenience イニシャライザをスーパークラスから自動的に継承します。

次の図は、3 つのクラス全てのイニシャライザのチェーンの全体を示しています。

![3 つのクラスのイニシャライザのチェーン](../.gitbook/assets/initializersExample03_2x.png)

継承した 3 つのイニシャライザ全てを使用して、新しい `ShoppingListItem` インスタンスを作成できます。

```swift
var breakfastList = [
    ShoppingListItem(),
    ShoppingListItem(name: "Bacon"),
    ShoppingListItem(name: "Eggs", quantity: 6),
]
breakfastList[0].name = "Orange juice"
breakfastList[0].purchased = true
for item in breakfastList {
    print(item.description)
}
// 1 x Orange juice ✔
// 1 x Bacon ✘
// 6 x Eggs ✘
```

ここでは、3 つの新しい `ShoppingListItem` インスタンスを含む配列リテラルから、`breakfastList` という新しい配列が作成されます。配列の型は `[ShoppingListItem]` だと推論されます。配列が作成された後、配列の先頭にある `ShoppingListItem` の名前が `"[Unnamed]"` から `"Orange juice"` に変更され、購入済みとしてマークされます。配列内の各アイテムの説明を出力すると、デフォルトの状態が期待どおりに設定されていることがわかります。

## Failable Initializers\(失敗可能イニシャライザ\)

初期化が失敗する可能性があるクラス、構造体、または列挙型を定義すると便利な場合があります。この失敗は、無効な初期化パラメータ値、必要な外部リソースの欠如、または初期化の成功を妨げるその他の条件によって引き起こされる可能性があります。

失敗する可能性のある初期化条件に対処するには、クラス、構造体、または列挙型定義の一部として、1 つ以上の*失敗可能イニシャライザ*を定義します。`init` キーワードの後に疑問符を置いて、失敗するイニシャライザを記述します\(`init?`\)。

> NOTE  
> 同じパラメータの型と名前で失敗可能イニシャライザと失敗しないイニシャライザを定義することはできません。

失敗可能なイニシャライザは、初期化する型のオプショナルの値を作成します。初期化に失敗する場所を示すために、失敗可能イニシャライザ内に `return nil` を記述します。

> NOTE  
> 厳密に言えば、イニシャライザは値を返しません。むしろ、その役割は、初期化が終了するまでに自分自身が完全かつ正しく初期化されることを保証することです。初期化に失敗した場合 `return nil` を記述しますが、初期化の成功を示すために `return` キーワードを使用することはありません。

例えば、失敗可能イニシャライザは、数値型変換のために実装されます。数値型間の変換で値が正確に維持されるようにするには、`init(exactly:)` イニシャライザを使用します。型変換で値を維持できない場合、イニシャライザは失敗します:

```swift
let wholeNumber: Double = 12345.0
let pi = 3.14159

if let valueMaintained = Int(exactly: wholeNumber) {
    print("\(wholeNumber) conversion to Int maintains value of \(valueMaintained)")
}
// "12345.0 conversion to Int maintains value of 12345"

let valueChanged = Int(exactly: pi)
// valueChanged は Int? で Int ではありません

if valueChanged == nil {
    print("\(pi) conversion to Int doesn't maintain value")
}
// "3.14159 conversion to Int doesn't maintain value"
```

下記の例では、`Animal` と呼ばれる構造体を定義し、`species` と呼ばれる `String` の定数プロパティを使用しています。`Animal` 構造体は、`species` と呼ばれる単一のパラメータを持つ失敗可能イニシャライザも定義します。このイニシャライザは、イニシャライザに渡された `species` の値が空の文字列かどうかをチェックします。空の文字列が見つかった場合、初期化に失敗します。それ以外の場合、`species` のプロパティの値が設定され、初期化が成功します:

```swift
struct Animal {
    let species: String
    init?(species: String) {
        if species.isEmpty { return nil }
        self.species = species
    }
}
```

この失敗可能イニシャライザを使用して、新しい `Animal` インスタンスの初期化を試み、初期化が成功したかどうかを確認できます:

```swift
let someCreature = Animal(species: "Giraffe")
// someCreature は Animal? で Animal ではありません

if let giraffe = someCreature {
    print("An animal was initialized with a species of \(giraffe.species)")
}
// "An animal was initialized with a species of Giraffe"
```

空の文字列を失敗可能イニシャライザの `species` パラメータに渡すと、イニシャライザは初期化に失敗します:

```swift
let anonymousCreature = Animal(species: "")
// anonymousCreature は Animal? で Animal ではありません

if anonymousCreature == nil {
    print("The anonymous creature couldn't be initialized")
}
// "The anonymous creature couldn't be initialized"
```

> NOTE  
> 空の文字列 \("Giraffe" ではなく "" など\) のチェックは、オプショナルの String 値がないことを示す `nil` のチェックとは異なります。上記の例では、空の文字列 \(""\) は妥当な非 オプショナルの文字列です。ただし、`Animal` がその `species` プロパティの値として空の文字列を持つことは適切ではありません。この制限をモデル化するために、空の文字列が見つかった場合、失敗可能イニシャライザは初期化に失敗します。

### Failable Initializers for Enumerations\(列挙型の失敗可能イニシャライザ\)

失敗可能イニシャライザを使用して、1 つ以上のパラメータに基づいて適切な列挙ケースを選択できます。提供されたパラメータが適切な列挙ケースと一致しない場合、イニシャライザは失敗する可能性があります。

以下の例では、3 つの可能な状態\(`kelvin`、`celsius`、`fahrenheit`\)を使用して、`TemperatureUnit` という列挙型を定義しています。温度記号を表す `Character` 値の適切な列挙ケースを見つけるために、失敗可能イニシャライザが使用されます:

```swift
enum TemperatureUnit {
    case kelvin, celsius, fahrenheit
    init?(symbol: Character) {
        switch symbol {
        case "K":
            self = .kelvin
        case "C":
            self = .celsius
        case "F":
            self = .fahrenheit
        default:
            return nil
        }
    }
}
```

この失敗可能イニシャライザを使用して、3 つの可能な状態に対して適切な列挙ケースを選択し、パラメータがこれらの状態のいずれかに一致しない場合に初期化を失敗する可能性があります。

```swift
let fahrenheitUnit = TemperatureUnit(symbol: "F")
if fahrenheitUnit != nil {
    print("This is a defined temperature unit, so initialization succeeded.")
}
// "This is a defined temperature unit, so initialization succeeded."

let unknownUnit = TemperatureUnit(symbol: "X")
if unknownUnit == nil {
    print("This isn't a defined temperature unit, so initialization failed.")
}
// "This isn't a defined temperature unit, so initialization failed."
```

### Failable Initializers for Enumerations with Raw Values\(Raw Valueを持つ列挙型の失敗可能イニシャライザ\)

Raw Value を持つ列挙型は、適切な Raw Value 型の `rawValue` と呼ばれるパラメータを受け取り、一致する列挙ケースが見つかった場合はそれを選択するか、一致する値がない場合は初期化に失敗する失敗可能イニシャライザ `init?(rawValue:)` を自動的に生成します。

上記の `TemperatureUnit` の例を書き直して、`Character` 型の Raw Value を使用し、`init?(rawValue:)` イニシャライザを利用できます:

```swift
enum TemperatureUnit: Character {
    case kelvin = "K", celsius = "C", fahrenheit = "F"
}

let fahrenheitUnit = TemperatureUnit(rawValue: "F")
if fahrenheitUnit != nil {
    print("This is a defined temperature unit, so initialization succeeded.")
}
// "This is a defined temperature unit, so initialization succeeded."

let unknownUnit = TemperatureUnit(rawValue: "X")
if unknownUnit == nil {
    print("This isn't a defined temperature unit, so initialization failed.")
}
// "This isn't a defined temperature unit, so initialization failed."
```

### Propagation of Initialization Failure\(初期化の失敗の伝播\)

クラス、構造体、または列挙型の失敗可能イニシャライザは、同じクラス、構造体、または列挙型から別の失敗可能イニシャライザに委譲できます。同様に、サブクラスの失敗可能イニシャライザは、スーパークラスの失敗可能イニシャライザに委譲できます。

いずれの場合も、委譲した別のイニシャライザで初期化に失敗すると、初期化プロセス全体がすぐに失敗し、それ以上の初期化コードは実行されません。

> NOTE  
> 失敗可能イニシャライザから失敗しないイニシャライザに委譲することもできます。他の方法では失敗しない既存の初期化プロセスに失敗する可能性を追加する必要がある場合は、このアプローチを使用します。

下記の例では、`CartItem` という `Product` のサブクラスを定義しています。`CartItem` クラスは、オンラインショッピングカート内のアイテムをモデル化しています。`CartItem` は、`quantity` と呼ばれる定数格納プロパティを導入し、このプロパティが常に少なくとも `1` の値を持つようにします:

```swift
class Product {
    let name: String
    init?(name: String) {
        if name.isEmpty { return nil }
        self.name = name
    }
}

class CartItem: Product {
    let quantity: Int
    init?(name: String, quantity: Int) {
        if quantity < 1 { return nil }
        self.quantity = quantity
        super.init(name: name)
    }
}
```

`CartItem` の失敗可能イニシャライザは、`1` 以上の `quantity` の値を受け取ったかどうかを検証することから始まります。`quantity` が無効な場合、初期化プロセス全体はすぐに失敗し、それ以上の初期化コードは実行されません。同様に、`Product` の失敗可能イニシャライザは `name` の値をチェックし、`name` が空の文字列の場合、初期化プロセスはすぐに失敗します。

`name` が空でなく、`quantity` が `1` 以上の `CartItem` インスタンスを作成すると、初期化は成功します:

```swift
if let twoSocks = CartItem(name: "sock", quantity: 2) {
    print("Item: \(twoSocks.name), quantity: \(twoSocks.quantity)")
}
// "Item: sock, quantity: 2"
```

`quantity` の値が `0` の `CartItem` インスタンスを作成しようとすると、`CartItem` のイニシャライザによって初期化が失敗します:

```swift
if let zeroShirts = CartItem(name: "shirt", quantity: 0) {
    print("Item: \(zeroShirts.name), quantity: \(zeroShirts.quantity)")
} else {
    print("Unable to initialize zero shirts")
}
// "Unable to initialize zero shirts"
```

同様に、`name` の値が空の `CartItem` インスタンスを作成しようとすると、スーパークラスの `Product` イニシャライザによって初期化が失敗します。

```swift
if let oneUnnamed = CartItem(name: "", quantity: 1) {
    print("Item: \(oneUnnamed.name), quantity: \(oneUnnamed.quantity)")
} else {
    print("Unable to initialize one unnamed product")
}
// "Unable to initialize one unnamed product"
```

### Overriding a Failable Initializer\(失敗可能イニシャライザのオーバーライド\)

他のイニシャライザと同様に、サブクラスでスーパークラスの失敗可能イニシャライザをオーバーライドできます。または、スーパークラスの失敗可能イニシャライザをサブクラスの失敗しないイニシャライザでオーバーライドできます。これにより、スーパークラスの初期化が失敗しても、初期化が失敗しないサブクラスを定義できます。

スーパークラスの失敗可能イニシャライザをサブクラスの失敗しないイニシャライザでオーバーライドする場合、スーパークラスのイニシャライザに委譲する唯一の方法は、スーパークラスの失敗可能イニシャライザの結果を強制アンラップすることです。

> NOTE  
> 失敗可能イニシャライザを失敗しないイニシャライザでオーバーライドすることはできますが、その逆はできません。

下記の例では、`Document` というクラスを定義しています。このクラスは、空ではない文字列値または `nil` のいずれかが可能ですが、空文字列にすることはできない `name` プロパティで初期化できるドキュメントをモデル化しています。

```swift
class Document {
    var name: String?
    // このイニシャライザは name が nil の Document を作成します
    init() {}
    // このイニシャライザは name が 空文字ではない Document を作成します
    init?(name: String) {
        if name.isEmpty { return nil }
        self.name = name
    }
}
```

次の例では、`AutomaticallyNamedDocument` という `Document` のサブクラスを定義しています。`AutomaticNamedDocument` サブクラスは、`Document` の指定イニシャライザの両方をオーバーライドします。これらのオーバーライドにより、インスタンスが名前なしで初期化された場合、または空の文字列が `init(name:)` イニシャライザに渡された場合に、`AutomaticallyNamedDocument` インスタンスは `"[Untitled]"` を`name` の初期値として持つことが保証されます。

```swift
class AutomaticallyNamedDocument: Document {
    override init() {
        super.init()
        self.name = "[Untitled]"
    }
    override init(name: String) {
        super.init()
        if name.isEmpty {
            self.name = "[Untitled]"
        } else {
            self.name = name
        }
    }
}
```

`AutomaticNamedDocument` は、スーパークラスの失敗可能イニシャライザ\(`init?(name:)`\)を、失敗しないイニシャライザ\(`init(name:)`\)でオーバーライドします。`AutomaticNamedDocument` は、スーパークラスとは異なる方法で空の文字列に対処するため、イニシャライザは失敗することがなく、代わりに失敗しないバージョンのイニシャライザを提供します。

イニシャライザで強制アンラップを使用して、サブクラスの失敗しないイニシャライザの実装の一部として、スーパークラスから失敗可能イニシャライザを呼び出すことができます。例えば、以下の `UntitledDocument` サブクラスは常に `"[Untitled]"` という名前で、初期化中にスーパークラスの失敗可能イニシャライザ\(`init(name:)`\)を使用します:

```swift
class UntitledDocument: Document {
    override init() {
        super.init(name: "[Untitled]")!
    }
}
```

この場合、スーパークラスの `init(name:)` イニシャライザが名前に空の文字列を指定して呼び出された場合、強制アンラップにより実行時エラーが発生しますが、文字列定数で呼び出されているため、イニシャライザが失敗しないことがわかり、この場合は実行時エラーは発生しません。

### The init! Failable Initializer\(init!を使った失敗可能イニシャライザ\)

通常、`init` キーワードの後に疑問符\(`?`\)を配置して\(`init?`\)、適切な型のオプショナルのインスタンスを作成する失敗可能イニシャライザを定義します。または、適切な型へ暗黙的にアンラップしてオプショナルのインスタンスを作成する失敗可能イニシャライザを定義することもできます。これを行うには、疑問符の代わりに `init` キーワードの後に感嘆符\(`!`\)を置きます\(`init!`\)。

`init?` から `init!` へ委譲でき、その逆も可能です。また、`init!` で `init?` をオーバーライドでき、その逆も可能です。`init` から `init!` に委譲することもできますが、`init!` の初期化が失敗した場合は、アサーションを引き起こします。

## Required Initializers\(必須イニシャライザ\)

クラスの全てのサブクラスがそのイニシャライザを実装する必要があることを示すために、クラスのイニシャライザの定義の前に `required` 修飾子を記述します。

```swift
class SomeClass {
    required init() {
        // ここにイニシャライザの実装を書きます
    }
}
```

また、全てのサブクラスの必須イニシャライザの実装の前に、`required` 修飾子を記述して、イニシャライザの必須要件が初期化チェーン内の他のサブクラスに適用されることを示す必要があります。必須指定イニシャライザをオーバーライドするときは、`override` 修飾子を記述しません:

```swift
class SomeSubclass: SomeClass {
    required init() {
        // ここにサブクラスの必須イニシャライザを実装します
    }
}
```

> NOTE  
> 継承したイニシャライザで要件を満たすことができる場合、必須イニシャライザの明示的な実装を提供する必要はありません。

## Setting a Default Property Value with a Closure or Function\(クロージャまたは関数を使用したデフォルトのプロパティ値の設定\)

格納プロパティのデフォルト値にカスタマイズまたは設定が必要な場合は、クロージャまたはグローバル関数を使用して、そのプロパティにカスタマイズされたデフォルト値を提供できます。プロパティが属する型の新しいインスタンスが初期化されるたびに、クロージャまたは関数が呼び出され、その戻り値がプロパティのデフォルト値として割り当てられます。

これらの種類のクロージャまたは関数は、通常、プロパティと同じ型の一時的な値を作成し、その値を調整して目的の初期状態にしてからその値を返し、プロパティのデフォルト値として使用します。

クロージャを使用してデフォルトのプロパティ値を提供する方法の概要を次に示します:

```swift
class SomeClass {
    let someProperty: SomeType = {
        // このクロージャ内に someProperty のデフォルト値を作成します
        // someValue は SomeType と同じ型である必要があります
        return someValue
    }()
}
```

クロージャの終了中括弧\(`}`\)の後に空の括弧\(`()`\)が続いていることに注目してください。これは Swift にクロージャをすぐに実行するように指示します。これらの括弧を省略すると、クロージャの戻り値ではなく、クロージャ自体をプロパティに割り当てようとします。

> NOTE  
> クロージャを使用してプロパティを初期化する場合、残りのインスタンスはクロージャが実行された時点ではまだ初期化されていないことに注意してください。つまり、たとえプロパティにデフォルト値があっても、クロージャ内から他のプロパティ値にアクセスできないことを意味します。また、暗黙的な `self` プロパティを使用したり、インスタンスのメソッドを呼び出すこともできません。

下記の例では、チェスゲームのボードをモデル化する `Chessboard` という構造体を定義しています。チェスは `8 x 8` の盤上で、白と黒の正方形が交互に並んでいます。

![チェスゲームのボード](../.gitbook/assets/chessBoard_2x.png)

このゲームボードを表すために、`Chessboard` 構造体には、`boardColors` という 1 つのプロパティがあり、これは 64 の `Bool` 値の配列です。配列内の `true` の値は黒い正方形を表し、`false` の値は白い正方形を表します。配列の最初のアイテムはボードの左上の正方形を表し、配列の最後のアイテムはボードの右下の正方形を表します。

`boardColors` 配列は、色の値を設定するためのクロージャで初期化されています。

```swift
struct Chessboard {
    let boardColors: [Bool] = {
        var temporaryBoard = [Bool]()
        var isBlack = false
        for i in 1...8 {
            for j in 1...8 {
                temporaryBoard.append(isBlack)
                isBlack = !isBlack
            }
            isBlack = !isBlack
        }
        return temporaryBoard
    }()
    func squareIsBlackAt(row: Int, column: Int) -> Bool {
        return boardColors[(row * 8) + column]
    }
}
```

新しい `Chessboard` インスタンスが作成されるたびに、クロージャが実行され、`boardColors` のデフォルト値を計算して返します。上記の例のクロージャは、ボード上の各マスの適切な色を計算して、`temporaryBoard` と呼ばれる一時配列に設定し、セットアップが完了すると、この一時配列をクロージャの戻り値として返します。返された配列値は `boardColors` に保存され、`squareIsBlackAt(row:column:)` というユーティリティ関数から検索できます。

```swift
let board = Chessboard()
print(board.squareIsBlackAt(row: 0, column: 1))
// "true"
print(board.squareIsBlackAt(row: 7, column: 7))
// "false"
```
