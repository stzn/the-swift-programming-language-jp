# Initialization(初期化)

最終更新日:

初期化(*Initialization*)は、使用するクラス、構造体、または列挙型のインスタンスを準備するプロセスです。このプロセスには、そのインスタンスに保存されている各プロパティの初期値の設定と、新しいインスタンスの使用準備が整う前に必要なその他の要素のセットアップや初期化の実行が含まれます。

この初期化プロセスを実装するには、イニシャライザ(*initializers*)を定義します。イニシャライザは、特定の型の新しいインスタンスを作成するために呼び出すことができる特別なメソッドのようなものです。Objective-C のイニシャライザとは異なり、Swift のイニシャライザは値を返しません。主な役割は、型の新しいインスタンスが初めて使用される前に正しく初期化されることを保証することです。

クラス型のインスタンスは、そのクラスのインスタンスが解放される直前にカスタムのクリーンアップを実行するデイニシャライザ(*deinitializer*) を実装することもできます。デイニシャライザの詳細については、[Deinitialization](./deinitialization.md)を参照ください。

## Setting Initial Values for Stored Properties(格納プロパティの初期値の設定)

クラスと構造は、そのクラスまたは構造体のインスタンスが作成されるまでに、格納されている全てのプロパティの適切な初期値を設定する必要があります。格納プロパティは、不確定な状態のままにすることはできません。

イニシャライザ内で、またはプロパティの定義の一部としてデフォルトのプロパティ値を割り当てることにより、格納プロパティの初期値を設定できます。これらのアクションについては、次のセクションで説明します。

> NOTE  
> 格納プロパティにデフォルト値を割り当てるか、イニシャライザ内でその初期値を設定すると、プロパティオブザーバを呼び出さずに、そのプロパティの値が直接設定されます。

### Initializers(イニシャライザ)

イニシャライザ(*initializers*)は、特定の型の新しいインスタンスを作成するために呼び出されます。最もシンプルな形式では、イニシャライザはパラメータのないインスタンスメソッドのようなもので、`init` キーワードを使用して記述されます:

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

この構造体は、パラメータのない単一のイニシャライザ `init` を定義します。これは、格納された温度を `32.0`(華氏での水の凝固点)の値で初期化します。

### Default Property Values(デフォルトのプロパティ値)

上記のように、イニシャライザ内から格納プロパティの初期値を設定できます。または、プロパティの宣言の一部としてデフォルトのプロパティ値(*default property value*)を指定します。これはプロパティの定義時に初期値を割り当てることにより、デフォルトのプロパティ値を指定します。

> NOTE  
> プロパティが常に同じ初期値を取る場合は、イニシャライザ内で値を設定するのではなく、デフォルト値を指定します。最終結果は同じですが、デフォルト値はプロパティの初期化を宣言とより密接に結び付けています。これにより、イニシャライザが短く明確になり、デフォルト値からプロパティの型を推論できるようになります。また、デフォルト値を使用すると、この章で後述するように、デフォルトのイニシャライザとイニシャライザの継承を利用しやすくなります。

プロパティが宣言された時点で `temperature` プロパティのデフォルト値を指定することにより、上記の `Fahrenheit` 構造体をよりシンプルな形式で書くことができます。

```swift
struct Fahrenheit {
    var temperature = 32.0
}
```

## Customizing Initialization(初期化のカスタマイズ)

次のセクションで説明するように、入力パラメータと optional プロパティ型を使用するか、初期化中に定数プロパティを割り当てることにより、初期化プロセスをカスタマイズできます。

### Initialization Parameters(イニシャライザの引数)

初期化パラメータをイニシャライザの定義の一部として提供して、初期化プロセスをカスタマイズする値の型と名前を定義できます。初期化パラメータには、関数およびメソッドパラメータと同じ機能と構文があります。

次の例では、摂氏で表された温度を格納する `Celsius` という構造体を定義しています。`Celsius` 構造体は、`init(fromFahrenheit:)` および `init(fromKelvin:)` という 2 つのカスタムイニシャライザを実装します。これらは、構造体の新しいインスタンスを異なる温度スケールの値で初期化しています。

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

最初のイニシャライザには、`fromFahrenheit` という引数ラベルと `fahrenheit` というパラメータ名を持つ単一の初期化パラメータがあります。2 番目のイニシャライザには、`fromKelvin` という引数ラベルと `kelvin` というパラメータ名を持つ単一の初期化パラメータがあります。どちらのイニシャライザも、単一の引数を対応する摂氏値に変換し、この値を `temperatureInCelsius` というプロパティに格納しています。

### Parameter Names and Argument Labels(パラメータ名と引数ラベル)

関数やメソッドのパラメータと同様に、初期化パラメータには、イニシャライザの本部内で使用するパラメータ名と、イニシャライザを呼び出すときに使用する引数ラベルの両方を含めることができます。

ただし、イニシャライザには、関数やメソッドのように括弧の前に識別関数名がありません。したがって、イニシャライザのパラメータの名前と型は、どのイニシャライザを呼び出す必要があるかを識別する上で特に重要な役割を果たします。このため、Swift は、イニシャライザが提供されていない場合、イニシャライザの全てのパラメータに自動で引数ラベルを提供します。

次の例では、`red`、`green`、`blue` という 3 つの定数プロパティを持つ `Color` という構造体を定義しています。これらのプロパティには、色の赤、緑、青の量を示す `0.0` ～ `1.0` の値が格納されます。

`Color` は、赤、緑、および青のコンポーネント用に `Double` 型の適切に名前が付けられた 3 つのパラメーターを含むイニシャライザを提供します。`Color` は、3 つすべての色コンポーネントに同じ値を提供するために使用される、単一の `white` パラメーターを持つ 2 番目のイニシャライザも提供します。

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

### Initializer Parameters Without Argument Labels(引数ラベルのないイニシャライザパラメータ)

初期化パラメータに引数ラベルを使用したくない場合は、そのパラメータの明示的な引数ラベルの代わりにアンダースコア (`_`) を記述して、デフォルトの挙動をオーバーライドします。

これは、上記の[Initialization Parameters](#initialization-parametersイニシャライザの引数)の `Celsius` の例の拡張バージョンで、すでに摂氏スケールにある `Double` 値から新しい `Celsius` インスタンスを作成する追加のイニシャライザを備えています。

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

イニシャライザの呼び出し `Celsius(37.0)` は、意図が明確なので引数ラベルを必要としません。したがって、このイニシャライザを `init(_ celsius: Double)` と記述して、名前のない `Double` 値を指定して呼び出すことができるのは適切です。

### Optional Property Types(optional のプロパティ型)

### Assigning Constant Properties During Initialization(初期化中の定数プロパティの割り当て)

## Default Initializers(デフォルトイニシャライザ)

### Memberwise Initializers for Structure Types(構造体の全メンバーを引数に取るイニシャライザ)

## Initializer Delegation for Value Types(値型のイニシャライザの委譲)

## Class Inheritance and Initialization(クラスの継承と初期化)

### Designated Initializers and Convenience Initializers(指定イニシャライザとcovenience イニシャライザ)

### Syntax for Designated and Convenience Initializers(指定イニシャライザとcovenience イニシャライザの構文)

### Initializer Delegation for Class Types(クラス型のイニシャライザの委譲)

### Two-Phase Initialization(2段階イニシャライザ)

### Initializer Inheritance and Overriding(イニシャライザの継承とオーバーライド)

### Automatic Initializer Inheritance(自動イニシャライザの継承)

### Designated and Convenience Initializers in Action(指定とcovenience イニシャライザの挙動)

## Failable Initializers(失敗可能イニシャライザ)

### Failable Initializers for Enumerations(列挙型の失敗可能イニシャライザ)

### Failable Initializers for Enumerations with Raw Values(Raw Valueを持つ列挙型の失敗可能イニシャライザ)

### Propagation of Initialization Failure(初期化の失敗の伝播)

### Overriding a Failable Initializer(失敗可能イニシャライザのオーバーライド)

### The init! Failable Initializer(init!を使った失敗可能イニシャライザ)

## Required Initializers(必須イニシャライザ)

## Setting a Default Property Value with a Closure or Function(クロージャまたは関数を使用したデフォルトのプロパティ値の設定)
