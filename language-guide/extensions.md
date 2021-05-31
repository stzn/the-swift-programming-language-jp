# Extensions

最終更新日:

拡張機能(*extension*)は、既存のクラス、構造体、列挙型、またはプロトコル型に新しい機能を追加します。これには、元のソースコードにアクセスできない型を拡張する(遡及モデリング(*retroactive modeling*)と呼ばれる)機能が含まれます。拡張機能は、Objective-C のカテゴリに似ています。(Objective-C カテゴリとは異なり、Swift 拡張機能には名前がありません)

Swift の拡張機能は次のことができます。

* 計算インスタンスプロパティと計算型プロパティを追加する
* インスタンスメソッドと型メソッドを定義する
* 新しいイニシャライザを提供する
* subscript を定義する
* 新しくネストされた型を定義して使用する
* 既存の型をプロトコルに準拠させる

Swift では、プロトコルを拡張してその要件の実装を提供したり、準拠する型が利用できる追加機能を追加したりすることもできます。詳細については、[Protocol Extensions](./protocols.md#protocol-extensionsプロトコルの拡張)を参照ください。

> NOTE  
> 拡張機能は型に新しい機能を追加できますが、既存の機能をオーバーライドすることはできません。

## Extension Syntax(拡張機能の構文)

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

この方法でプロトコル準拠を追加する方法は、[Adding Protocol Conformance with an Extension](./protocols.md#adding-protocol-conformance-with-an-extension拡張機能を使用したプロトコル準拠の追加)で説明されています。

拡張機能は、[Extending a Generic Type](./generics.md#extending-a-generic-typeジェネリック型の拡張)で説明されているように、既存のジェネリック型を拡張するためにも使用できます。ジェネリック型を拡張して、条件付きで機能を追加することもできます([Extensions with a Generic Where Clause](./generics.md#extensions-with-a-generic-where-clauseジェネリックなWhere句を使用した拡張機能)を参照ください)。

> NOTE  
> 拡張機能を定義して既存の型に新しい機能を追加すると、拡張機能が定義される前に作成されたものであっても、その型の既存の全てのインスタンスで新しい機能を使用できます。

## Computed Properties(計算プロパティ)

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

これらの計算プロパティは、`Double` 値を特定の長さの単位と見なしています。計算プロパティとして実装されていますが、ドット構文(`.`)を使用して浮動小数点リテラルの後に追加して使用できます。

この例では、`Double` 値 `1.0` が「1 メートル」を表しているため、計算プロパティ `m` は `self` を返します。式 `1.m` は、`Double` 値 `1.0` を計算していることを表しています。

他の単位では、メートルで測定された値として表現するために何らかの変換が必要です。1 キロメートルは 1,000 メートルに等しいので、計算プロパティ `km` は値に `1_000.00` を掛けてメートル単位の数値に変換します。同様に、メートルは 3.28084 フィート、に等しいので、 計算プロパティ `ft` は、基になる `Double` 値を `3.28084` で除算して、フィートからメートルに変換します。

これらのプロパティは読み取り専用の計算プロパティのため、簡潔にするために `get` キーワードなしで表されています。それらの戻り値は `Double` 型で、`Double` が使用できるところでは、計算に使用することができます：

```swift
let aMarathon = 42.km + 195.m
print("A marathon is \(aMarathon) meters long")
// "A marathon is 42195.0 meters long"
```

> NOTE  
> 拡張機能は新しい計算プロパティを追加できますが、格納プロパティを追加したり、既存のプロパティにプロパティオブザーバを追加したりすることはできません。

## Initializers(イニシャライザ)

## Methods(メソッド)

### Mutating Instance Methods(mutatingインスタンスメソッド)

## Subscripts

## Nested Types(ネストされた型)
