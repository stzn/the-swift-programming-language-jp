# Extensions

最終更新日:

拡張機能(*extension*)は、既存のクラス、構造体、列挙型、またはプロトコル型に新しい機能を追加します。これには、元のソースコードにアクセスできない型を拡張する(遡及モデリング(*retroactive modeling*)と呼ばれる)機能が含まれます。拡張機能は、Objective-C のカテゴリに似ています。(Objective-C カテゴリとは異なり、Swift 拡張機能には名前がありません)

Swift の拡張機能は次のことができます。

* 計算インスタンスプロパティと計算型プロパティを追加する
* インスタンスメソッドと型メソッドを定義する
* 新しいイニシャライザを提供する
* subscript を定義する
* 新しくネストした型を定義して使用する
* 既存の型をプロトコルに準拠させる

Swift では、プロトコルを拡張してその要件の実装を提供したり、準拠する型が利用できる追加機能を追加したりすることもできます。詳細については、[Protocol Extensions](./protocols.md#protocol-extensionsプロトコルの拡張)を参照ください。

> NOTE  
> 拡張機能は型に新しい機能を追加できますが、既存の機能をオーバーライドすることはできません。

## Extension Syntax

## Computed Properties

## Initializers

## Methods

### Mutating Instance Methods

## Subscripts

## Nested Types
