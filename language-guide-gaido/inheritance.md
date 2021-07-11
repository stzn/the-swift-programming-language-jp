# 継承\(Inheritance\)

最終更新日: 2021/6/28

クラスは、メソッド、プロパティ、およびその他の特性を別のクラスから継承できます。あるクラスが別のクラスを継承する場合、継承するクラスは_サブクラス_と呼ばれ、継承されるクラスは_スーパークラス_と呼ばれます。継承は、Swift においてクラスと他の型を区別する重要な挙動です。

Swift のクラスは、スーパークラスに属するメソッド、プロパティ、および subscript を呼び出してアクセスしたり、それらのメソッド、プロパティ、および subscript の独自のオーバーライドバージョンを提供して挙動を変更できたりします。Swift は、オーバーライドしている定義がスーパークラスの定義に一致していることを確認することで、オーバーライドが正しいことを確認します。

クラスは、プロパティの値が変更されたときに通知を受けるために、継承したプロパティに_プロパティオブザーバ_を追加することもできます。プロパティオブザーバは、元々格納プロパティとして定義されているか計算プロパティとして定義されているかに関係なく、任意のプロパティに追加できます。

## 基本クラスの定義\(Defining a Base Class\)

別のクラスから継承しないクラスは、_基本クラス_と呼ばれます。

> NOTE  
> Swift は、全てのクラスで共通する基本クラスを継承しません。スーパークラスを指定せずに定義したクラスは、自動的に基本クラスになります。

以下の例では、`Vehicle` という基本クラスを定義しています。この基本クラスは、`currentSpeed` と呼ばれる格納プロパティを定義します。デフォルト値は `0.0` です\(プロパティの型は `Double` と推論されます\)。`currentSpeed` プロパティの値は、`description` と呼ばれる読み取り専用の `String` 型の計算プロパティに使用され、乗り物の説明をします。

`Vehicle` 基本クラスは、`makeNoise` と呼ばれるメソッドも定義します。このメソッドは、何もしませんが、後で `Vehicle` のサブクラスによってカスタマイズされます。

```swift
class Vehicle {
    var currentSpeed = 0.0
    var description: String {
        return "traveling at \(currentSpeed) miles per hour"
    }
    func makeNoise() {
        // 何もしない - 乗り物は必ずしも騒音を出しません
    }
}
```

イニシャライザ構文を使用して `Vehicle` の新しいインスタンスを作成します。これは、型名とそれに続く空の括弧\(`()`\)として記述されます。

```swift
let someVehicle = Vehicle()
```

新しい `Vehicle` インスタンスを作成したら、その `description` プロパティにアクセスして、乗り物の現在の速度の人間が読める形式の説明を出力します。

```swift
print("Vehicle: \(someVehicle.description)")
// Vehicle: traveling at 0.0 miles per hour
```

`Vehicle` クラスは、任意の乗り物に共通の特性を定義しますが、そのまま使用されることはあまりありません。より有用にするには、より具体的な種類の乗り物を記述する必要があります。

## サブクラス化\(Subclassing\)

サブクラス化は、既存のクラスに基づいて新しいクラスを作成することを指します。サブクラスは既存のクラスの特性を継承し、それを変更することができます。サブクラスに新しい特性を追加することもできます。

サブクラスにスーパークラスがあることを示すには、サブクラス名の後にコロン\(`:`\)で区切ってスーパークラス名を記述します。

```swift
class SomeSubclass: SomeSuperclass {
    // サブクラスの定義をここに
}
```

次の例では、`Vehicle` のスーパークラスを持つ `Bicycle` というサブクラスを定義しています。

```swift
class Bicycle: Vehicle {
    var hasBasket = false
}
```

新しい `Bicycle` クラスは、`currentSpeed` プロパティと `description` プロパティ、`makeNoise()` メソッドなど、`Vehicle` の全ての特性を自動的に継承します。

加えて、`Bicycle` クラスは新しい格納プロパティ `hasBasket` を定義し、デフォルト値は `false` です\(プロパティは `Bool` 型と推論されます\)。

デフォルトでは、作成する新しい `Bicycle` インスタンスにはカゴがありません。特定の `Bicycle` インスタンスが作成された後、そのインスタンスに対して `hasBasket` プロパティを `true` に設定できます。

```swift
let bicycle = Bicycle()
bicycle.hasBasket = true
```

継承した `currentSpeed` プロパティを変更し、インスタンスで継承した `description` プロパティにその変更を反映することもできます。

```swift
bicycle.currentSpeed = 15.0
print("Bicycle: \(bicycle.description)")
// Bicycle: traveling at 15.0 miles per hour
```

サブクラス自体をサブクラス化できます。次の例では、「タンデム」と呼ばれる 2 人乗り自転車のサブクラスを作成します。

```swift
class Tandem: Bicycle {
    var currentNumberOfPassengers = 0
}
```

`Tandem` は `Bicycle` から全てのプロパティとメソッドを継承し、`Bicycle` は `Vehicle` から全てのプロパティとメソッドを継承します。`Tandem` サブクラスは、`currentNumberOfPassengers` と呼ばれる新しい格納プロパティも追加します。デフォルト値は `0` です。

`Tandem` インスタンスを作成する場合、その新しいプロパティと継承したプロパティのいずれかを操作して、`Vehicle` から継承した読み取り専用の `description` プロパティの変更を反映できます。

```swift
let tandem = Tandem()
tandem.hasBasket = true
tandem.currentNumberOfPassengers = 2
tandem.currentSpeed = 22.0
print("Tandem: \(tandem.description)")
// Tandem: traveling at 22.0 miles per hour
```

## <a id="overriding">オーバーライド\(Overriding\)</a>

サブクラスは、インスタンスメソッド、型メソッド、インスタンスプロパティ、型プロパティ、またはスーパークラスから継承する subscript の独自のカスタム実装を提供できます。これは_オーバーライド_と呼ばれます。

継承した特性をオーバーライドするには、オーバーライドする定義の前に `override` キーワードを付けます。そうすることで、間違った定義を継承していないことが明確になります。誤ってオーバーライドすると予期しない挙動が発生する可能性があり、`override` キーワードを使用しないオーバーライドは、コンパイル時にエラーになります。

また、Swift コンパイラは、`override` キーワードを見て、オーバーライドするクラスのスーパークラス\(またはその親の 1 つ\)に、指定した宣言と一致する宣言があることを確認します。このチェックにより、オーバーライドする定義が正しいことが確認されます。

### 親クラスのメソッド、プロパティ、subscriptへのアクセス\(Accessing Superclass Methods, Properties, and Subscripts\)

サブクラスにメソッド、プロパティ、または subscript のオーバーライドを指定する場合、オーバーライドの一部として既存のスーパークラスの実装を使用すると便利な場合があります。例えば、その既存の実装の挙動を変更したり、変更された値を既存の継承した変数に格納したりできます。

正しく実装されている場合、`super` プレフィックスを使用して、メソッド、プロパティ、または subscript のスーパークラスバージョンにアクセスします:

* `someMethod()` という名前のオーバーライドされたメソッドは、オーバーライドしたメソッドの実装内で `super.someMethod()` を呼び出すことにより、`someMethod()` のスーパークラスのバージョンを呼び出すことができます
* `someProperty` と呼ばれるオーバーライドされたプロパティは、オーバーライドした get または set 実装内で `super.someProperty` として `someProperty` のスーパークラスのバージョンにアクセスできます
* オーバーライドされた `someIndex` に対する subscript は、オーバーライドした subscript 実装内から、`super[someIndex]` で同じ `someIndex` に対する subscript のスーパークラスのバージョンにアクセスできます

### メソッドのオーバーライド\(Overriding Methods\)

継承したインスタンスまたは型メソッドをオーバーライドして、サブクラス内のメソッドの特定の目的に適合した、または代わりとなる実装を提供できます。

次の例では、`Train` と呼ばれる `Vehicle` の新しいサブクラスを定義します。これは、`Train` が `Vehicle` から継承した `makeNoise()` メソッドをオーバーライドしています。

```swift
class Train: Vehicle {
    override func makeNoise() {
        print("Choo Choo")
    }
}
```

`Train` の新しいインスタンスを作成し、その `makeNoise()` メソッドを呼び出すと、`Train` サブクラスのバージョンのメソッドが呼び出されていることがわかります。

```swift
let train = Train()
train.makeNoise()
// "Choo Choo"
```

### プロパティのオーバーライド\(Overriding Properties\)

継承したインスタンスまたは型プロパティをオーバーライドして、そのプロパティに独自の get/set を提供したり、プロパティオブザーバを追加して、基になるプロパティ値が変更されたときにオーバーライドしたプロパティを通してその値を監視できます。

#### <a id="overriding-property-getters-and-setters">get と set のオーバーライド\(Overriding Property Getters and Setters\)</a>

継承したプロパティが格納プロパティか計算プロパティかに関係なく、継承したプロパティをオーバーライドする独自の get \(および必要ならば set \)を提供できます。継承したプロパティの格納または計算プロパティの実装は、サブクラスには認識されません。継承したプロパティは、特定の名前と型を持っていることだけを認識します。オーバーライドしたプロパティの名前と型の両方を常に指定して、そのオーバーライドが同じ名前と型のスーパークラスのプロパティと一致することをコンパイラがチェックできるようにする必要があります。

サブクラスで get/set の両方をオーバーライドすることにより、継承した読み取り専用プロパティを読み取り/書き込みプロパティにすることができます。ただし、継承した読み取り/書き込みプロパティを読み取り専用プロパティにすることはできません。

> NOTE  
> プロパティのオーバーライドの一部として set を提供する場合は、get も提供する必要があります。オーバーライドした get 内で継承した値を変更したくない場合は、get から `super.someProperty` を返すことで、継承した値を渡すことができます。`someProperty` は、オーバーライドするプロパティの名前です。

次の例では、`Vehicle` のサブクラスの `Car` という新しいクラスを定義します。`Car` クラスは、デフォルトの整数値 `1` で、`gear` と呼ばれる新しい格納プロパティを導入します。`Car` クラスは、`Vehicle` から継承する `description` プロパティもオーバーライドして、現在のギアを含むカスタムの説明を提供します:

```swift
class Car: Vehicle {
    var gear = 1
    override var description: String {
        return super.description + " in gear \(gear)"
    }
}
```

`description` プロパティのオーバーライドは、`Vehicle` クラスの `description` プロパティを返す `super.description` を呼び出すことから始まります。次に、`Car` クラスの `description` は、現在のギアに関する情報を提供するために、この説明の最後にいくつかのテキストを追加しています。

`Car` クラスのインスタンスを作成し、その `gear` プロパティと `currentSpeed` プロパティを設定すると、`description` プロパティが `Car` クラス内で定義されたカスタマイズされた説明を返します。

```swift
let car = Car()
car.currentSpeed = 25.0
car.gear = 3
print("Car: \(car.description)")
// Car: traveling at 25.0 miles per hour in gear 3
```

#### <a id="overriding-property-observers">プロパティオブザーバのオーバーライド\(Overriding Property Observers\)</a>

プロパティのオーバーライドを使用して、継承したプロパティのプロパティオブザーバを追加できます。これにより、継承したプロパティの値が変更されたときに、そのプロパティが最初にどのように実装されたかに関係なく通知を受け取ることができます。プロパティオブザーバの詳細については、[Property Observers\(プロパティオブザーバ\)](../language-guide-gaido/properties#property-observers)を参照ください。

> NOTE  
> 継承した定数の格納プロパティまたは継承した読み取り専用計算プロパティにプロパティオブザーバを追加することはできません。これらのプロパティの値は変更できないため、オーバーライドの一部として `willSet` または `didSet` の実装を提供することは適切ではありません。 同じプロパティに対して、set のオーバーライドとプロパティオブザーバのオーバーライドの、両方を提供することはできないことにも注意してください。プロパティの値の変更を監視する必要があり、そのプロパティのカスタムの set を既に提供している場合は、カスタムの set 内から値の変更が簡単に監視できます。

次の例では、`Car` のサブクラスの `AutomaticCar` という新しいクラスを定義しています。`AutomaticCar` クラスは、現在の速度に基づいて使用する適切なギアを自動的に選択する自動ギアボックスを備えた車を表します:

```swift
class AutomaticCar: Car {
    override var currentSpeed: Double {
        didSet {
            gear = Int(currentSpeed / 10.0) + 1
        }
    }
}
```

`AutomaticCar` インスタンスの `currentSpeed` プロパティを設定するたびに、プロパティの `didSet` オブザーバは、インスタンスの `gear` プロパティに新しい速度に適したギアを選択します。具体的には、プロパティオブザーバは、新しい `currentSpeed` 値を `10` で除算し、最も近い整数に切り捨てられた値に `1` を加えたギアを選択します。速度 `35.0` はギア `4` を生成します。

```swift
let automatic = AutomaticCar()
automatic.currentSpeed = 35.0
print("AutomaticCar: \(automatic.description)")
// AutomaticCar: traveling at 35.0 miles per hour in gear 4
```

## <a id="preventing-overrides">オーバーライドを防ぐ\(Preventing Overrides\)</a>

メソッド、プロパティ、または subscript をオーバーライドされないようにすることができます。これを行うには、メソッド、プロパティ、または subscript のキーワードの前に `final` 修飾子を記述します\(`final var`、`final func`、`final class func` や `final subscript` など\)。

サブクラスで `final` メソッド、プロパティ、または subscript をオーバーライドしようとすると、コンパイルエラーになります。extension でクラスに追加するメソッド、プロパティ、または subscript にも、`final` をマークすることができます。

クラスの定義の `class` キーワードの前に `final` 修飾子を記述することで、クラス全体を `final` としてマークできます\(`final class`\)。`final` クラスをサブクラス化しようとすると、コンパイルエラーになります。

