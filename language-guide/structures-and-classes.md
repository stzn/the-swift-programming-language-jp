# Structures and Classes

最終更新日:

構造体(*`struct`*)とクラス(*`class`*)は、プログラムのコードの構成要素を構築する汎用的で柔軟な構造概念です。定数、変数、関数の定義に使用するのと同じ構文を使用して、プロパティとメソッドを定義して機能を追加します。

他のプログラミング言語とは異なり、Swift では、インターフェイスと実装ファイルを別々に作成する必要はありません。`struct` または `class` を 1 つのファイルで定義すると、その外部インターフェイスは自動的に他のコードで使用できるようになります。

> NOTE  
> 昔からオブジェクト(*object*)は `class` のインスタンスはとして知られています。ただし、 Swift の `struct` と `class` は、他の言語よりも機能がはるかに近く、この章でも、`struct` または `class` 型のインスタンスのどちらにも当てはまる機能について説明しています。このため、より一般的な意味合いでインスタンスという用語を使用します。

## Comparing Structures and Classes(structとclassの比較)

Swift の `struct` と `class` には多くの共通点があります。いずれも下記のようなことができます:

* 値を格納するプロパティを定義
* 機能を提供するメソッドを定義
* `subscript` 構文を使用して値へのアクセスを提供する subscripts の定義します
* 初期状態を設定するためのイニシャライザの定義
* `extension` による機能拡張
* 特定の種類の標準機能を提供するためのプロトコルに準拠

これらの機能の詳細については、[Properties](./properties.md)、[Methods](./methods.md)、[Initialization](./initialization.md)、[Extensions](../language-guide/extensions.md)および[Protocols](./protocols.md)を参照ください。

`class` には、`struct` にはない追加の機能があります。

* ある `class` が別の `class` の特性を継承
* 型キャストによる実行時の `class` インスタンスの型の確認と解釈
* デイニシャライザ(*`deinit`*)を使った `class` のインスタンスに割り当てられているリソースの解放
* 参照カウント(*reference counting*)を使って、ある同じ `class` インスタンスへの複数からの参照

これらの機能の詳細については、[Inheritance](./inheritance.md)、[Type Casting](./type-casting.md)、[Deinitialization](./deinitialization.md)、[Automatic Reference Counting](./automatic-reference-counting.md)を参照ください。

`class` は追加の機能をサポートしている文、複雑さが増します。一般的なガイドラインとして、`struct` の方がわかりやすく推奨されます。`class` は適切または必要な場合にのみ使用しましょう。つまり、実際は、独自に定義するデータ型のほとんどが `struct` と `enum` になることを意味します。より詳細な比較は[Choosing Between Structures and Classes](https://developer.apple.com/documentation/swift/choosing_between_structures_and_classes)を参照ください。

### Definition Syntax(定義構文)

### Structure and Class Instances(structとclassのインスタンス)

### Accessing Properties(プロパティへのアクセス)

### Memberwise Initializers for Structure Types(structのMemberwiseイニシャライザ)

## Structures and Enumerations Are Value Types(structと列挙型は値型)

## Classes Are Reference Types(classは参照型)

### Identity Operators(参照等価演算子)

### Pointers(ポインタ)
