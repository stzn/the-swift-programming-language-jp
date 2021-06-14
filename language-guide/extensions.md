# Extensions \(拡張\)

最終更新日: 2021/6/1

拡張機能\(_extension_\)は、既存のクラス、構造体、列挙型、またはプロトコル型に新しい機能を追加します。これは、\(遡及モデリング\(_retroactive modeling_\)と呼ばれる\)、元のソースコードにアクセスできない型を拡張することもできます。拡張機能は、Objective-C のカテゴリに似ています。\(ただし、Objective-C のカテゴリとは異なり、Swift の拡張機能には名前がありません\)

拡張機能は次のことができます。

* 計算インスタンスプロパティと計算型プロパティを追加する
* インスタンスメソッドと型メソッドを定義する
* 新しいイニシャライザを提供する
* subscript を定義する
* 新しく入れ子型を定義する
* 既存の型をプロトコルに準拠させる

Swift では、プロトコルを拡張してその要件の実装を提供したり、準拠する型が利用できる機能を追加することもできます。詳細については、[Protocol Extensions](protocols.md#protocol-extensionsプロトコルの拡張)を参照ください。

> NOTE  
> 拡張機能は型に新しい機能を追加できますが、既存の機能をオーバーライドすることはできません。

## Extension Syntax\(拡張機能の構文\)

`extension` キーワードを使用して拡張機能を宣言します:

```swift
extension SomeType {
    // SomeType に新しい機能を追加します
}
```

拡張機能は、既存の型を拡張して、1 つ以上のプロトコルに準拠することができます。プロトコルへの準拠を追加するには、クラスまたは構造体の場合と同じ方法でプロトコル名を記述します。

```swift
extension SomeType: SomeProtocol, AnotherProtocol {
    // protocol の要件をここで実装します
}
```

この方法でプロトコル準拠を追加する方法は、[Adding Protocol Conformance with an Extension](protocols.md#adding-protocol-conformance-with-an-extension拡張機能を使用したプロトコル準拠の追加)で説明されています。

拡張機能は、[Extending a Generic Type](generics.md#extending-a-generic-typeジェネリック型の拡張)で説明されているように、既存のジェネリック型を拡張するためにも使用できます。ジェネリック型を拡張して、条件付きで機能を追加することもできます\([Extensions with a Generic Where Clause](generics.md#extensions-with-a-generic-where-clauseジェネリックなWhere句を使用した拡張機能)を参照ください\)。

> NOTE  
> 拡張機能を定義して既存の型に新しい機能を追加すると、拡張機能が定義される前に作成されたものであっても、その型の既存の全てのインスタンスで新しい機能を使用できます。

## Computed Properties\(計算プロパティ\)

拡張機能は、計算インスタンスプロパティと計算型プロパティを既存の型に追加できます。この例では、5 つの計算インスタンスプロパティを Swift の `Double` 型に追加して、距離単位を使用するための基本的な機能を提供します。

```swift
extension Double {
    var km: Double { return self * 1_000.0 }
    var m: Double { return self }
    var cm: Double { return self / 100.0 }
    var mm: Double { return self / 1_000.0 }
    var ft: Double { return self / 3.28084 }
}
let oneInch = 25.4.mm
print("One inch is \(oneInch) meters")
// "One inch is 0.0254 meters"
let threeFeet = 3.ft
print("Three feet is \(threeFeet) meters")
// "Three feet is 0.914399970739201 meters"
```

これらの計算プロパティは、`Double` の値を特定の長さの単位と見なしています。計算プロパティとして実装されていますが、ドット構文\(`.`\)を使用して浮動小数点リテラルの後に追加して使用できます。

この例では、`Double` の値 `1.0` が「1 メートル」を表しているため、計算プロパティ `m` は `self` を返します。式 `1.m` は、`Double` の値 `1.0` を計算していると見なされます。

他の単位では、メートルで測定された値として表現するために何らかの変換が必要です。1 キロメートルは 1,000 メートルに等しいので、計算プロパティ `km` は値に `1_000.00` を掛けてメートル単位の数値に変換します。同様に、メートルは 3.28084 フィート、に等しいので、 計算プロパティ `ft` は、基になる `Double` の値を `3.28084` で除算して、フィートからメートルに変換します。

これらのプロパティは読み取り専用の計算プロパティのため、簡潔にするために `get` キーワードなしになっています。戻り値は `Double` 型で、`Double` が使用できるところでは、計算に使用することができます：

```swift
let aMarathon = 42.km + 195.m
print("A marathon is \(aMarathon) meters long")
// "A marathon is 42195.0 meters long"
```

> NOTE  
> 拡張機能は新しい計算プロパティを追加できますが、格納プロパティを追加したり、既存のプロパティにプロパティオブザーバを追加したりすることはできません。

## Initializers\(イニシャライザ\)

拡張機能は、既存の型に新しいイニシャライザを追加できます。他の型を拡張して、独自のカスタム型を初期化引数として受け入れたり、型の元の実装の一部として含まれていなかった追加の初期化オプションを提供したりできます。

拡張機能は、新しい convenience イニシャライザをクラスに追加できますが、新しい指定イニシャライザまたはデイニシャライザを追加することはできません。指定イニシャライザとデイニシャライザは、常に元のクラス実装によって提供される必要があります。

拡張機能を使用して、カスタムイニシャライザを定義しない値型に全ての格納プロパティの既定値を提供するイニシャライザを追加する場合は、拡張機能のイニシャライザ内からデフォルトイニシャライザとメンバワイズイニシャライザを呼び出すことができます。[Initializer Delegation for Value Types](initialization.md#initializer-delegation-for-value-types値型のイニシャライザの委譲)で説明されているように、値型の元の実装の一部としてイニシャライザを作成した場合は、これに当てはまりません。

別のモジュールで宣言された構造体にイニシャライザを追加する場合、新しいイニシャライザは、定義モジュールからイニシャライザを呼び出されるまで、`self` にアクセスできません。

下記の例では、幾何学的な長方形を表す独自の `Rect` 構造体を定義しています。この例では、`Size` と `Point` という 2 つの補助的な構造体も定義されており、どちらも全てのプロパティにデフォルト値 `0.0` を提供します:

```swift
struct Size {
    var width = 0.0, height = 0.0
}
struct Point {
    var x = 0.0, y = 0.0
}
struct Rect {
    var origin = Point()
    var size = Size()
}
```

`Rect` 構造体はその全てのプロパティに既定値を提供しているため、[Default Initializers](initialization.md#default-initializersデフォルトイニシャライザ)で説明されているように、デフォルトイニシャライザとメンバワイズイニシャライザを自動的に受け取ります。これらのイニシャライザを使用して、新しい `Rect` インスタンスを作成できます:

```swift
let defaultRect = Rect()
let memberwiseRect = Rect(origin: Point(x: 2.0, y: 2.0),
   size: Size(width: 5.0, height: 5.0))
```

`Rect` 構造体を拡張して、特定の中心点とサイズを取る追加のイニシャライザを提供できます:

```swift
extension Rect {
    init(center: Point, size: Size) {
        let originX = center.x - (size.width / 2)
        let originY = center.y - (size.height / 2)
        self.init(origin: Point(x: originX, y: originY), size: size)
    }
}
```

この新しいイニシャライザは、提供された `center` と `size` の値に基づいて適切な原点を計算することから始まります。次に、イニシャライザは、構造体が自動で受け取るメンバワイズイニシャライザ `init(origin:size:)` を呼び出します。これは、適切なプロパティに新しい原点とサイズの値を格納します:

```swift
let centerRect = Rect(center: Point(x: 4.0, y: 4.0),
                      size: Size(width: 3.0, height: 3.0))
// centerRect の origin は (2.5, 2.5) で size は (3.0, 3.0) です
```

> NOTE  
> 拡張機能で新しいイニシャライザを提供する場合でも、イニシャライザが完了前に完全に初期化が完了していなければなりません。

## Methods\(メソッド\)

拡張機能は、新しいインスタンスメソッドと型メソッドを既存の型に追加できます。次の例では、`repetitions` という新しいインスタンスメソッドを `Int` 型に追加しています:

```swift
extension Int {
    func repetitions(task: () -> Void) {
        for _ in 0..<self {
            task()
        }
    }
}
```

`repetitions(task:)` メソッドは、`() -> Void` 型の単一の引数を受け取ります。これは、引数がなく、値を返さない関数を示しています。

`repetitions(task:)` メソッドを呼び出して、任意の回数のタスクを繰り返し実行できます:

```swift
3.repetitions {
    print("Hello!")
}
// Hello!
// Hello!
// Hello!
```

### Mutating Instance Methods\(mutatingインスタンスメソッド\)

拡張機能を使用して追加されたインスタンスメソッドは、インスタンス自体を変更することもできます。`self` またはそのプロパティを変更する構造体および列挙型メソッドは、元の実装のメソッドを変更するのと同じように、インスタンスメソッドを `mutating` としてマークする必要があります。

下記の例では、`Int` 型に元の値を二乗する `square` と呼ばれる新しい `mutating` メソッドを追加しています:

```swift
extension Int {
    mutating func square() {
        self = self * self
    }
}
var someInt = 3
someInt.square()
// someInt は 9
```

## Subscripts

拡張機能は、既存の型に新しい subscript を追加できます。この例では、`Int` 型に整数の subscript を追加します。この `subscript[n]` は、数値の右から `n` 桁の `10` 進数を返します。

* `123456789[0]` は `9` を返します
* `123456789[1]` は `8` を返します

…など:

```swift
extension Int {
    subscript(digitIndex: Int) -> Int {
        var decimalBase = 1
        for _ in 0..<digitIndex {
            decimalBase *= 10
        }
        return (self / decimalBase) % 10
    }
}
746381295[0]
// 5 を返します
746381295[1]
// 9 を返します
746381295[2]
// 2 を返します
746381295[8]
// 7 を返します
```

要求されたインデックスに対して十分な桁がない場合、subscript の実装は、番号の左側にゼロが埋め込まれているかのように、`0` を返します。

```swift
746381295[9]
// 0 を返します
0746381295[9]
```

## Nested Types\(入れ子型\)

拡張機能は、新しい入れ子型を既存のクラス、構造体、および列挙型に追加できます:

```swift
extension Int {
    enum Kind {
        case negative, zero, positive
    }
    var kind: Kind {
        switch self {
        case 0:
            return .zero
        case let x where x > 0:
            return .positive
        default:
            return .negative
        }
    }
}
```

この例では、新しい入れ子列挙型を `Int` に追加します。`Kind` と呼ばれるこの列挙型は、特定の整数が表す数の種類を表します。具体的には、数値が負か、ゼロか、正かを表します。

この例では、`kind` と呼ばれる新しい計算インスタンスプロパティも `Int` に追加しています。これは、その整数に対して適切な `Kind` 列挙ケースを返します。

入れ子列挙型は、任意の `Int` 値で使用できるようになりました:

```swift
func printIntegerKinds(_ numbers: [Int]) {
    for number in numbers {
        switch number.kind {
        case .negative:
            print("- ", terminator: "")
        case .zero:
            print("0 ", terminator: "")
        case .positive:
            print("+ ", terminator: "")
        }
    }
    print("")
}
printIntegerKinds([3, 19, -27, 0, -6, 0, 7])
// "+ + - 0 - 0 + "
```

この関数 `printIntegerKinds(_:)` は、`Int` 値の入力配列を受け取り、それらの値を順番に繰り返します。配列内の各整数について、関数はその整数の `kind` 計算プロパティを検証し、適切な説明を出力します。

> NOTE  
> `number.kind` は `Int.Kind` 型であることがすでにわかっているため、`Int.Kind` の各ケースは、`Int.Kind.negative` ではなく `.negative` など、`switch` 文内では省略して記述できます。

