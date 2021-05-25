# Enumerations(列挙型)

最終更新日:

列挙型(*enumerations*)は、関連する値のグループに共通の型を定義し、コード内で型安全にそれらの値を操作できるようにします。

C 言語に馴染みがあるれば、C 言語の列挙型が、関連する名前のグループを整数値のセットに割り当てることを知っていると思います。Swift の列挙型は、それよりもはるかに柔軟性があり、列挙型の各ケースに値を割り当てる必要はありません。列挙型ごとに（*raw value*と呼ばれる）値が与えられている場合、値は文字列、文字、または任意の整数型または浮動小数点型の値にすることができます。

あるいは、他の言語の `unions` や `variants` のように、それぞれの case で任意の型の関連値を格納できます。それぞれで適切な型の異なる値のセットで関連付けられた
case の共通セットをある列挙型の一部として定義できます。

Swift の列挙型は、それ自体がファーストクラスの型です。これらは、列挙型の現在値の追加情報を提供する計算プロパティや、列挙型が表す値に関わる機能を提供するインスタンスメソッドなど、従来は class でのみサポートされていた多くの機能を使うことができます。列挙型は、初期の case 値を指定するイニシャライザを定義することもできます。元の実装を超えて機能を拡張するように extension も使用することができます。また、プロトコルに準拠して標準機能を提供することもできます。

これらの機能の詳細については、[Properties](./properties.md), [Methods](./methods.md), [Initialization](./initialization.md), [Extensions](../language-guide/extensions.md)および[Protocols](./protocols.md)を参照ください。

## Enumeration Syntax(列挙型構文)

`enum` キーワードを使用して列挙型を導入し、それらの定義全体を中括弧のペア(`{}`)内に配置します。

```swift
enum SomeEnumeration {
    // 列挙型の定義をここに記載します
}
```

コンパスの 4 つの主要なポイントの例を次に示します:

```swift
enum CompassPoint {
    case north
    case south
    case east
    case west
}
```

列挙型で定義された値（`north`、`south`、`east`、`west` など）は、その列挙ケースです。 `case` キーワードを使用して、新しい列挙ケースを導入します。

> NOTE  
> C 言語や Objective-C などの言語とは異なり、Swift 列挙ケースにはデフォルトで整数値が設定されていません。上記の `CompassPoint` の例では、`north`、`south`、`east`、`west` は暗黙的に `0`、`1`、`2`、`3` になりません。代わりに、異なる列挙ケースはそれ自体が値であり、`CompassPoint` 明示的に定義された型です。

複数のケースをカンマ(`,`)で区切って 1 行に表示できます。

```swift
enum Planet {
    case mercury, venus, earth, mars, jupiter, saturn, uranus, neptune
}
```

各列挙定義は、新しいタイプを定義します。Swift の他のタイプと同様に、それらの名前（`CompassPoint` や `Planet` など）は大文字で始まります。列挙型は、1 つのグループなことが自明なため、複数の名前ではなく単数の名前を付けましょう:

```swift
var directionToHead = CompassPoint.west
```

`directionToHead` の型は、`CompassPoint` のある値の 1 つで初期化された場合、型が推論されます。一度 `directionToHead` が `CompassPoint` として宣言されたら、次からは短いドット構文(`.`)を使用して別の `CompassPoint` 値に設定できます。

```swift
directionToHead = .east
```

`directionToHead` の型はすでにわかっているため、値を設定するときにタイプを削除できます。これにより、明示的に型指定された列挙値を操作するときに、非常に読みやすいコードが作成されます。

## Matching Enumeration Values with a Switch Statement(switch文を使った列挙値のパターンマッチング)

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
// Prints "Watch out for penguins"
```

このコードは次のように読むことができます。

「`directionToHead` の値を検証してください。`.north` に等しい場合は、「多くの惑星は北にあります」と出力します。`.south` に等しい場合は、「ペンギンに気をつけて」と出力してください」

…などなど。

[Control Flow](./control-flow.md)で説明されているように、列挙ケースを検証するときは、`switch` 文で全てのケースを網羅する必要があります。`.west` の `case` を省略した場合、このコードはコンパイルできません。網羅性を要求することで、列挙ケースが誤って省略されないようにします。

全ての列挙ケースの `case` を並べることが適切でない場合は、明示的に対処されていないケースをカバーする `default` のケースを提供できます。

```swift
let somePlanet = Planet.earth
switch somePlanet {
case .earth:
    print("Mostly harmless")
default:
    print("Not a safe place for humans")
}
// Prints "Mostly harmless"
```

## Iterating over Enumeration Cases(列挙ケースの繰り返し処理)

一部の列挙型では、その列挙型の全てのケースのコレクションがあると便利です。これを有効にするには、列挙型の名前の後に `:CaseIterable` を記述します。Swift は、全てのケースのコレクションを列挙型の `allCases` プロパティとして公開してます。次に例を示します:

```swift
enum Beverage: CaseIterable {
    case coffee, tea, juice
}
let numberOfChoices = Beverage.allCases.count
print("\(numberOfChoices) beverages available")
// Prints "3 beverages available"
```

上記の例では、`Beverage.allCases` を記述して、列挙型 `Beverage` の全てのケースを含むコレクションにアクセスします。 `allCases` は、他のコレクションと同じように使用できます。コレクションの要素は列挙型のインスタンスで、今回は `Beverage` の値です。上記の例では、ケースの数をカウントし、下記の例では、`for` ループを使用して全てのケースを繰り返し処理しています。

```swift
for beverage in Beverage.allCases {
    print(beverage)
}
// coffee
// tea
// juice
```

上記の例で使用されている構文では、[`CaseIterable`](https://developer.apple.com/documentation/swift/caseiterable) プロトコルに準拠しています。プロトコルの詳細については、[Protocols](./protocols.md)を参照ください。

## Associated Values(関連値)

前のセクションの例は、列挙ケースがそれ自体で定義された（および型指定された）値とする方法を示しています。定数または変数を `Planet.earth` に設定し、後でこの値を確認できます。ただし、これらのケース値と一緒に他の型の値を保持できると便利な場合があります。この追加情報は関連値(*associated values*)と呼ばれ、コードでそのケースの値を使用する度に異なります。

Swift の列挙型を定義して、任意のタイプの関連する値を格納できます。また、必要に応じて、列挙ケースごとに値の型を変えることができます。これらに類似した列挙型は、他のプログラミング言語では、`discriminated unions`、`tagged unions`、または `variants` として知られています。

例えば、在庫追跡システムが 2 つの異なる型のバーコードで製品を追跡する必要があるとします。一部の製品には、`0〜9` の数字を使用する UPC 形式の 1D バーコードでラベルが付けられています。各バーコードには番号システムの数字があり、その後に 5 つのメーカーコード数字と 5 つの製品コード数字が続きます。これらの後にチェックディジットが続き、コードが正しくスキャンされたことを確認します。

![UPC 形式の 1D バーコード](./../.gitbook/assets/barcode_UPC_2x.png)

その他の製品は、QR コード形式の 2D バーコードでラベル付けされています。これは、ISO 8859-1 文字を使用でき、最大 2,953 文字の文字列をエンコードできます。

![QR コード形式の 2D バーコード](./../.gitbook/assets/barcode_QR_2x.png)

在庫追跡システムでは、UPC バーコードを 4 つの整数のタプルとして保存し、QR コードバーコードを任意の長さの文字列として保存すると便利です。

Swift では、いずれかのタイプの製品バーコードを定義するための列挙は次のようになります。

```swift
enum Barcode {
    case upc(Int, Int, Int, Int)
    case qrCode(String)
}
```

これは次のように読むことができます：

「`Barcode` と呼ばれる列挙型を定義します。これは、`upc` の値とそれに関連付けられた型の値 `(Int、Int、Int、Int)`、または `qrCode` の値と関連付けられた型の `String` のいずれかを取ることができます」

この定義は、実際の `Int` 値または `String` 値を提供するものではなく、`Barcode.upc` または `Barcode.qrCode` と等しい場合に `Barcode` の定数および変数が格納できる関連値の型を定義しているだけです。

次に、次のいずれかの型を使用して新しい `Barcode` を作成できます。

```swift
var productBarcode = Barcode.upc(8, 85909, 51226, 3)
```

この例では、関連付けられたタプル値 `(8,85909, 51226, 3)` を持つ `Barcode.upc` の値を代入して、`productBarcode` という新しい変数を作成します。

同じ製品に異なる型のバーコードを代入することができます。

```swift
productBarcode = .qrCode("ABCDEFGHIJKLMNOP")
```

この時点で、元の `Barcode.upc` とその整数値は、新しい `Barcode.qrCode` とその文字列値に置き換えられます。`Barcode` の定数と変数は、`.upc` または `.qrCode` のいずれかを（関連する値とともに）格納できますが、一度に格納できるのはそのうちの 1 つだけです。

[Matching Enumeration Values with a Switch Statement](#matching-enumeration-values-with-a-switch-statementswitch文を使った列挙値のパターンマッチング)の例と同様に、`switch` 文を使用してさまざまなバーコード型を確認できます。ただし、今回は、関連する値が `switch` 文の一部として抽出されます。`switch` のケースの本文内で使用するために、関連付けられた各値を定数（`let` プレフィックス）または変数（`var` プレフィックス）として抽出します。

```swift
switch productBarcode {
case .upc(let numberSystem, let manufacturer, let product, let check):
    print("UPC: \(numberSystem), \(manufacturer), \(product), \(check).")
case .qrCode(let productCode):
    print("QR code: \(productCode).")
}
// Prints "QR code: ABCDEFGHIJKLMNOP."
```

列挙ケースに関連付けられている全ての値が定数として抽出される場合、または全てが変数として抽出される場合は、簡潔にするために、ケース名の前に 1 つの `var` または `let` を付けるだけにできます:

```swift
switch productBarcode {
case let .upc(numberSystem, manufacturer, product, check):
    print("UPC : \(numberSystem), \(manufacturer), \(product), \(check).")
case let .qrCode(productCode):
    print("QR code: \(productCode).")
}
// Prints "QR code: ABCDEFGHIJKLMNOP."
```

## Raw Values

[Associated Values](#associated-values関連値)のバーコードの例は、列挙ケースが、様々な型に関連値を格納して宣言する方法を示しています。関連値の代わりに、列挙型には、全て同じ型のデフォルト値（*raw values*と呼ばれる）を事前に入力していることがあります。

名前付き列挙ケースと一緒にそのままの ASCII 値を格納する例を次に示します。

```swift
enum ASCIIControlCharacter: Character {
    case tab = "\t"
    case lineFeed = "\n"
    case carriageReturn = "\r"
}
```

ここで、`ASCIIControlCharacter` と呼ばれる列挙型の raw value は、`Character` 型で定義されており、より一般的な ASCII 制御文字のいくつかが設定されています。`Character` 値は、[Strings and Characters](./strings-and-characters.md)で説明しています。

raw value は、文字列、文字、または整数型または浮動小数点数型のいずれかです。各 raw value は、その列挙型内で一意でなければなりません。

> NOTE  
> raw valueは、関連値と同じではありません。上記の3つの ASCII コードのように、コードで列挙を最初に定義するときに、raw value は事前入力された値に設定されます。特定の列挙ケースのraw valueは常に同じです。関連値は、列挙ケースに基づいて新しい定数または変数を作成するときに設定され、作成する度に異なる可能性があります。

### Implicitly Assigned Raw Values

### Initializing from a Raw Value

## Recursive Enumerations
