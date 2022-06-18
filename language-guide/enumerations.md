# 列挙型\(Enumerations\)

最終更新日: 2021/6/27  
原文: https://docs.swift.org/swift-book/LanguageGuide/Enumerations.html

_列挙型_は、関連する値のグループに共通の型を定義し、コード内で型安全にそれらの値を操作できるようにします。

C 言語に馴染みがあれば、C 言語の列挙型が、関連する名前のグループを整数値のセットに割り当てることを知っていると思います。Swift の列挙型は、それよりもはるかに柔軟性があり、列挙型の各ケースに値を割り当てる必要はありません。列挙ケースごとに\(_Raw Value_ と呼ばれる\)値が与えられている場合、値は文字列、文字、または任意の整数型または浮動小数点型にすることができます。

あるいは、他の言語の `unions` や `variants` のように、それぞれのケースで任意の型の関連値を格納できます。それぞれで適切な型の異なる値のセットで関連付けられたケースを列挙型の一部として定義できます。

Swift の列挙型は、それ自体が第一級の型です。これらは、列挙型の現在値の追加情報を提供する計算プロパティや、列挙型が表す値に関わる機能を提供するインスタンスメソッドなど、従来はクラスでのみサポートされていた多くの機能を使うことができます。列挙型は、初期のケース値を指定するイニシャライザも定義しています。`extension` で機能を拡張することもできます。また、プロトコルに準拠して標準機能を提供することもできます。

これらの機能の詳細については、[Properties](properties.md)、[Methods](methods.md)、[Initialization\(イニシャライザ\)](initialization.md)、[Extensions\(拡張\)](extensions.md)および[Protocols\(プロトコル\)](protocols.md)を参照ください。

## <a id="enumeration-syntax">列挙型構文\(Enumeration Syntax\)</a>

`enum` キーワードを使用して列挙型を導入し、それらの定義全体を中括弧のペア\(`{}`\)内に配置します。

```swift
enum SomeEnumeration {
    // 列挙型の定義をここに記載します
}
```

コンパスの 4 つの主要な方向の例を次に示します:

```swift
enum CompassPoint {
    case north
    case south
    case east
    case west
}
```

列挙型で定義された値\(`north`、`south`、`east`、`west` など\)は、その列挙ケースです。`case` キーワードを使用して、新しい列挙ケースを導入します。

> NOTE  
> C 言語や Objective-C などの言語とは異なり、Swift 列挙型のケースにはデフォルトで整数値が設定されていません。上記の `CompassPoint` の例では、`north`、`south`、`east`、`west` は暗黙的に `0`、`1`、`2`、`3` になりません。代わりに、異なる列挙ケースはそれ自体が値であり、明示的に定義された `CompassPoint` 型です。

複数のケースをカンマ\(`,`\)で区切って 1 行で表示できます。

```swift
enum Planet {
    case mercury, venus, earth, mars, jupiter, saturn, uranus, neptune
}
```

各列挙型の定義は、新しい型を定義します。Swift の他の型と同様に、それらの名前\(`CompassPoint` や `Planet` など\)は大文字で始まります。列挙型は、1 つのグループなことが自明なため、複数の名前ではなく単数の名前を付けましょう:

```swift
var directionToHead = CompassPoint.west
```

`directionToHead` の型は、`CompassPoint` のある値の 1 つで初期化された場合、型が推論されます。一度 `directionToHead` が `CompassPoint` として宣言されたら、次からは短いドット構文\(`.`\)を使用して別の `CompassPoint` 値を設定できます。

```swift
directionToHead = .east
```

`directionToHead` の型はすでにわかっているため、値を設定するときに型を省略できます。これにより、明示的に型指定された列挙型の値を操作するときに、非常に読みやすいコードが作成できます。

## <a id="matching-enumeration-values-with-a-switch-statement">switch 文を使った列挙値のパターンマッチング\(Matching Enumeration Values with a Switch Statement\)</a>

`switch` 文を使って、個々の列挙値をパターンマッチングできます。

```swift
directionToHead = .south
switch directionToHead {
case .north:
    print("Lots of planets have a north")
case .south:
    print("Watch out for penguins")
case .east:
    print("Where the sun rises")
case .west:
    print("Where the skies are blue")
}
// Watch out for penguins
```

このコードは次のように読むことができます。

「`directionToHead` の値を検証してください。`.north` に等しい場合は、「多くの惑星は北にあります」と出力します。`.south` に等しい場合は、「ペンギンに気をつけて」と出力してください」

…などなど。

[Control Flow\(制御フロー\)](control-flow.md)で説明されているように、列挙ケースを検証するときは、`switch` 文で全てのケースを網羅する必要があります。`.west` の `case` を省略した場合、このコードはコンパイルできません。これによって、列挙ケースが誤って省略できないようにします。

全ての列挙ケースの `case` を並べることが適切でない場合は、明示的に対処されていないケースをカバーする `default` のケースを提供できます。

```swift
let somePlanet = Planet.earth
switch somePlanet {
case .earth:
    print("Mostly harmless")
default:
    print("Not a safe place for humans")
}
// Mostly harmless
```

## <a id="iterating-over-enumeration-cases">列挙ケースの繰り返し処理\(Iterating over Enumeration Cases\)</a>

一部の列挙型では、その列挙型の全てのケースのコレクションがあると便利です。これを有効にするには、列挙型の名前の後に `:CaseIterable` を記述します。Swift は、全てのケースのコレクションを列挙型の `allCases` プロパティとして提供しています。次に例を示します:

```swift
enum Beverage: CaseIterable {
    case coffee, tea, juice
}
let numberOfChoices = Beverage.allCases.count
print("\(numberOfChoices) beverages available")
// 3 beverages available
```

上記の例では、`Beverage.allCases` を記述して、列挙型 `Beverage` の全てのケースを含むコレクションにアクセスします。`allCases` は、他のコレクションと同じように使用できます。コレクションの要素は列挙型のインスタンスで、今回は `Beverage` の値です。上記の例では、ケースの数をカウントし、下記の例では、`for` ループを使用して全てのケースを繰り返し処理しています。

```swift
for beverage in Beverage.allCases {
    print(beverage)
}
// coffee
// tea
// juice
```

上記の例で使用されている構文では、[`CaseIterable`](https://developer.apple.com/documentation/swift/caseiterable) プロトコルに準拠しています。プロトコルの詳細については、[Protocols\(プロトコル\)](protocols.md)を参照ください。

## <a id="associated-values">関連値\(Associated Values\)</a>

前のセクションの例は、列挙ケースがそれ自体を定義された\(および型指定された\)値にする方法を示しています。定数または変数を `Planet.earth` に設定し、後でこの値を確認できます。ただし、これらのケース値と一緒に他の型の値を保持できると便利な場合があります。この追加情報は_関連値_と呼ばれ、コードでそのケースの値を使用する度に異なります。

Swift の列挙型を定義して、任意の型を関連値に格納できます。また、必要に応じて、列挙ケースごとに値の型を変えることができます。これらに類似した列挙型は、他のプログラミング言語では、`discriminated unions`、`tagged unions`、または `variants` として知られています。

例えば、在庫追跡システムが 2 つの異なる型のバーコードで製品を追跡する必要があるとします。一部の製品には、`0〜9` の数字を使用する UPC 形式の 1D バーコードでラベルが付けられています。各バーコードには番号システムの数字があり、その後に 5 つのメーカーコード数字と 5 つの製品コード数字が続きます。これらの後にチェックディジットが続き、コードが正しくスキャンされたことを確認します。

![UPC&#x5F62;&#x5F0F;&#x306E;1D&#x30D0;&#x30FC;&#x30B3;&#x30FC;&#x30C9;](../assets/barcode_UPC_2x.png)

その他の製品は、QR コード形式の 2D バーコードでラベル付けされています。これは、ISO 8859-1 文字を使用でき、最大 2,953 文字の文字列をエンコードできます。

![QR&#x5F62;&#x5F0F;&#x306E;2D&#x30D0;&#x30FC;&#x30B3;&#x30FC;&#x30C9;](../assets/barcode_QR_2x.png)

在庫追跡システムでは、UPC バーコードを 4 つの整数のタプルとして保存し、QR コードバーコードを任意の長さの文字列として保存すると便利です。

Swift では、いずれかの型の製品バーコードを定義するための列挙型は次のようになります。

```swift
enum Barcode {
    case upc(Int, Int, Int, Int)
    case qrCode(String)
}
```

これは次のように読むことができます：

「`Barcode` と呼ばれる列挙型を定義します。これは、`upc` の値と `(Int、Int、Int、Int)` 型の関連値、または `qrCode` の値と `String` 型の関連値、のいずれかを取ることができます」

この定義は、実際の `Int` 値または `String` 値を提供するものではなく、`Barcode.upc` または `Barcode.qrCode` と等しい場合に `Barcode` の定数および変数が格納できる関連値の型を定義しているだけです。

次に、いずれかの型を使用して新しい `Barcode` を作成します。

```swift
var productBarcode = Barcode.upc(8, 85909, 51226, 3)
```

この例では、関連付けられたタプル値 `(8,85909, 51226, 3)` を持つ `Barcode.upc` の値を代入して、`productBarcode` という新しい変数を作成します。

同じ製品に異なる型のバーコードを代入することもできます。

```swift
productBarcode = .qrCode("ABCDEFGHIJKLMNOP")
```

この時点で、元の `Barcode.upc` とその整数値は、新しい `Barcode.qrCode` とその文字列値に置き換えられます。`Barcode` の定数と変数は、`.upc` または `.qrCode` のいずれかを\(関連値とともに\)格納できますが、一度に格納できるのはそのうちの 1 つだけです。

[Matching Enumeration Values with a Switch Statement\(switch 文を使った列挙値のパターンマッチング\)](../language-guide/enumerations.md#matching-enumeration-values-with-a-switch-statement)の例と同様に、`switch` 文を使用して様々なバーコード型を確認できます。ただし、今回は、関連値が `switch` 文の一部として抽出されます。`switch` のケースの本文内で使用するために、関連値を定数\(`let` プレフィックス\)または変数\(`var` プレフィックス\)として抽出します。

```swift
switch productBarcode {
case .upc(let numberSystem, let manufacturer, let product, let check):
    print("UPC: \(numberSystem), \(manufacturer), \(product), \(check).")
case .qrCode(let productCode):
    print("QR code: \(productCode).")
}
// QR code: ABCDEFGHIJKLMNOP.
```

列挙ケースの全ての関連値が定数として抽出される場合、または全てが変数として抽出される場合は、簡潔にするために、ケース名の前に 1 つの `var` または `let` を付けるだけで問題ありません:

```swift
switch productBarcode {
case let .upc(numberSystem, manufacturer, product, check):
    print("UPC : \(numberSystem), \(manufacturer), \(product), \(check).")
case let .qrCode(productCode):
    print("QR code: \(productCode).")
}
// QR code: ABCDEFGHIJKLMNOP.
```

## Raw Values

[Associated Values\(関連値\)](enumerations.md#associated-values)のバーコードの例は、列挙ケースが、様々な型に関連値を格納して宣言する方法を示しています。関連値の代わりに、列挙型には、全て同じ型のデフォルト値\(_Raw Values_と呼ばれる\)を事前に定義することもできます。

名前付きの列挙ケースと一緒に ASCII 値を格納する例を次に示します。

```swift
enum ASCIIControlCharacter: Character {
    case tab = "\t"
    case lineFeed = "\n"
    case carriageReturn = "\r"
}
```

ここで、`ASCIIControlCharacter` と呼ばれる列挙型の Raw Value は、`Character` 型で定義されており、一般的な ASCII 制御文字のいくつかが設定されています。`Character` 値は、[Strings and Characters\(文字列と文字\)](strings-and-characters.md)で説明しています。

Raw Value は、文字列、文字、または整数型または浮動小数点数型のいずれかです。各 Raw Value は、その列挙型内で一意でなければなりません。

> NOTE  
> Raw Valueは、関連値と同じではありません。上記の3つの ASCII コードのように、コードで列挙型を最初に定義するときに、Raw Value は事前入力された値に設定されます。特定の列挙ケースの Raw Value は常に同じです。関連値は、列挙ケースに基づいて新しい定数または変数を作成するときに設定され、作成する度に異なる可能性があります。

### <a id="implicitly-assigned-raw-values">暗黙的に割り当てられたRaw Value\(Implicitly Assigned Raw Values\)</a>

整数または文字列の Raw Value を格納する列挙型を操作する場合、それぞれのケースに Raw Value を明示的に割り当てる必要はありません。代わりに Swift が自動的に値を割り当てます。

例えば、Raw Value に整数が使用されている場合、各ケースの暗黙の値は前のケースより 1 つ増えた値になります。最初のケースに値が設定されていない場合、その値は `0` です。

以下の列挙型は、以前の `Planet` の列挙型を改良したもので、太陽からの各惑星の順序を表す整数の Raw Value が含まれています。

```swift
enum Planet: Int {
    case mercury = 1, venus, earth, mars, jupiter, saturn, uranus, neptune
}
```

上記の例では、`Planet.mercury` の明示的な Raw Value は `1` で、`Planet.venus` の暗黙的な Raw Value は `2` です。

文字列が Raw Value に使用される場合、各ケースの暗黙的な値は、そのケースの名前のテキストです。

下記の列挙型は、以前の `CompassPoint` 列挙型を改良したもので、各方向の名前を表す文字列の Raw Value が含まれています。

```swift
enum CompassPoint: String {
    case north, south, east, west
}
```

上記の例では、`CompassPoint.south` には `"south"` という暗黙の Raw Value があります。

`rawValue` プロパティを使用して列挙ケースの Raw Value にアクセスします。

```swift
let earthsOrder = Planet.earth.rawValue
// earthsOrder は 3

let sunsetDirection = CompassPoint.west.rawValue
// sunsetDirection は "west"
```

### Raw Valueからの初期化\(Initializing from a Raw Value\)

Raw Value 型で列挙型を定義すると、列挙型は、Raw Value の型の値を\(`rawValue` と呼ばれるパラメータとして\)受け取り、列挙型または `nil` のいずれかを返すイニシャライザを自動的に提供します。このイニシャライザを使用して、列挙型の新しいインスタンスを作成することができます。

この例では、Raw Value`7` から天王星を識別します。

```swift
let possiblePlanet = Planet(rawValue: 7)
// possiblePlanet の型は Planet? で Planet.uranus と等しい
```

ただし、全ての `Int` 値が惑星に一致するわけではありません。このため、Raw Value のイニシャライザは常にオプショナルの列挙型を返します。上記の例では、`possiblePlanet` は Planet?`または オプショナルの`Planet\`型です。

> NOTE  
> 全ての Raw Value が列挙型を返すわけではないため、Raw Valueイニシャライザは `nil` を返す可能性があります。詳細については、[Failable Initializers\(失敗可能イニシャライザ\)](../language-reference/declarations.md#declarations-failable-initializers)を参照ください。

位置が `11` の惑星を見つけようとすると、Raw Value のイニシャライザによって返されるオプショナルの `Planet` は `nil` になります。

```swift
let positionToFind = 11
if let somePlanet = Planet(rawValue: positionToFind) {
    switch somePlanet {
    case .earth:
        print("Mostly harmless")
    default:
        print("Not a safe place for humans")
    }
} else {
    print("There isn't a planet at position \(positionToFind)")
}
// There isn't a planet at position 11
```

この例では、オプションバインディングを使用して、Raw Value が `11` の惑星にアクセスしようとします。`if let somePlanet = Planet(rawValue: 11)` は、オプショナルの `Planet` を作成し、取得できる場合は、`somePlanet` をそのオプショナルの `Planet` の値に設定します。この場合、位置が `11` の惑星を取得することはできないため、代わりに `else` の分岐が実行されます。

## <a id="recursive-enumerations">再帰的列挙型\(Recursive Enumerations\)</a>

_再帰的列挙型_は、1 つ以上の列挙ケースの関連値としてその列挙型の別のインスタンスを持つ列挙型です。列挙型が再帰的だということを示すには、その前に `indirect` を記述します。これにより、コンパイラに `indirect` のネストがあることを伝えることができます。

例えば、シンプルな算術式を格納する列挙型は次のとおりです。

```swift
enum ArithmeticExpression {
    case number(Int)
    indirect case addition(ArithmeticExpression, ArithmeticExpression)
    indirect case multiplication(ArithmeticExpression, ArithmeticExpression)
}
```

列挙型の開始前に `indirect` を記述して、関連値を持つ全てのケースを `indirect` にすることもできます。

```swift
indirect enum ArithmeticExpression {
    case number(Int)
    case addition(ArithmeticExpression, ArithmeticExpression)
    case multiplication(ArithmeticExpression, ArithmeticExpression)
}
```

この列挙型には、シンプルな数値、2 つの式の加算、および乗算の 3 種類の算術式を格納できます。`addition` と `multiplication` のケースには、同じく算術式を関連値として格納しています。これらの関連値により、式をネストできます。例えば、式 `(5 + 4) * 2` には、乗算の右側に数値があり、乗算の左側に別の式があります。データがネストしているため、データを格納する列挙ケースもネストをサポートする必要があります。つまり、再帰的でなければなりません。下記のコードは、`(5 + 4) * 2` に対する `ArithmeticExpression` の再帰的列挙を作成する方法を示しています。

```swift
let five = ArithmeticExpression.number(5)
let four = ArithmeticExpression.number(4)
let sum = ArithmeticExpression.addition(five, four)
let product = ArithmeticExpression.multiplication(sum, ArithmeticExpression.number(2))
```

再帰関数は、再帰構造を持つデータを操作する簡単な方法です。例えば、算術式を評価する関数は次のとおりです。

```swift
func evaluate(_ expression: ArithmeticExpression) -> Int {
    switch expression {
    case let .number(value):
        return value
    case let .addition(left, right):
        return evaluate(left) + evaluate(right)
    case let .multiplication(left, right):
        return evaluate(left) * evaluate(right)
    }
}

print(evaluate(product))
// 18
```

この関数は、関連値を返すだけで単純な数値を評価します。左側の式を評価し、右側の式を評価してから、それらをさらに加算または乗算することにより、`addition` または `multiplication` を評価します。

