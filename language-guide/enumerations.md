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

列挙型で定義された値（`north`、`south`、`east`、`west` など）は、その列挙型のケースです。 `case` キーワードを使用して、新しい列挙ケースを導入します。

> NOTE  
> C 言語や Objective-C などの言語とは異なり、Swift 列挙型のケースにはデフォルトで整数値が設定されていません。上記の `CompassPoint` の例では、`north`、`south`、`east`、`west` は暗黙的に `0`、`1`、`2`、`3` になりません。代わりに、異なる列挙ケースはそれ自体が値であり、`CompassPoint` 明示的に定義された型です。

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

[Control Flow](./control-flow.md)で説明されているように、列挙型のケースを検証するときは、`switch` 文で全てのケースを網羅する必要があります。`.west` の `case` を省略した場合、このコードはコンパイルできません。網羅性を要求することで、列挙ケースが誤って省略されないようにします。

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

## Iterating over Enumeration Cases

## Associated Values

## Raw Values

### Implicitly Assigned Raw Values

### Initializing from a Raw Value

## Recursive Enumerations
