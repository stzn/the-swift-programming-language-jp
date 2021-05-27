# Structures and Classes

最終更新日:

構造体(*struct*)とクラス(*class*)は、プログラムのコードの構成要素を構築する汎用的で柔軟な構造概念です。定数、変数、関数の定義に使用するのと同じ構文を使用して、プロパティとメソッドを定義して機能を追加します。

他のプログラミング言語とは異なり、Swift では、インターフェイスと実装ファイルを別々に作成する必要はありません。構造体またはクラスを 1 つのファイルで定義すると、その外部インターフェイスは自動的に他のコードで使用できるようになります。

> NOTE  
> 昔からオブジェクト(*object*)は class のインスタンスはとして知られています。ただし、 Swift の 構造体とクラスは、他の言語よりも機能がはるかに近く、この章でも、構造体またはクラス型のインスタンスのどちらにも当てはまる機能について説明しています。このため、より一般的な意味合いでインスタンスという用語を使用します。

## Comparing Structures and Classes(構造体とクラスの比較)

Swift の構造体とクラスには多くの共通点があります。いずれも下記のようなことができます:

* 値を格納するプロパティを定義
* 機能を提供するメソッドを定義
* `subscript` 構文を使用して値へのアクセスを提供する subscripts の定義します
* 初期状態を設定するためのイニシャライザの定義
* `extension` による機能拡張
* 特定の種類の標準機能を提供するためのプロトコルに準拠

これらの機能の詳細については、[Properties](./properties.md)、[Methods](./methods.md)、[Initialization](./initialization.md)、[Extensions](../language-guide/extensions.md)および[Protocols](./protocols.md)を参照ください。

クラスには、構造体にはない追加の機能があります。

* ある `class` が別の `class` の特性を継承
* 型キャストによる実行時の `class` インスタンスの型の確認と解釈
* デイニシャライザ(*`deinit`*)を使った `class` のインスタンスに割り当てられているリソースの解放
* 参照カウント(*reference counting*)を使って、ある同じ `class` インスタンスへの複数からの参照

これらの機能の詳細については、[Inheritance](./inheritance.md)、[Type Casting](./type-casting.md)、[Deinitialization](./deinitialization.md)、[Automatic Reference Counting](./automatic-reference-counting.md)を参照ください。

`class` は追加の機能をサポートしている文、複雑さが増します。一般的なガイドラインとして、構造体の方がわかりやすく推奨されます。クラスは適切または必要な場合にのみ使用しましょう。つまり、実際は、独自に定義するデータ型のほとんどが構造体と列挙型になることを意味します。より詳細な比較は[Choosing Between Structures and Classes](https://developer.apple.com/documentation/swift/choosing_between_structures_and_classes)を参照ください。

### Definition Syntax(定義構文)

構造体とクラスの定義構文は似ています。 `struct` キーワードを使用して構造体を導入し、`class` キーワードを使用してクラスを導入します。どちらも、定義全体を中括弧のペア(`{}`)内に配置します:

```swift
struct SomeStructure {
    // structure definition goes here
}
class SomeClass {
    // class definition goes here
}
```

> NOTE  
> 新しい構造体またはクラスを定義するときはいつでも、新しい Swift の型を定義します。 標準の Swift 型（`String`、`Int`、`Bool` など）の大文字と一致するように、型に `UpperCamelCase` 名（ここでは `SomeStructure` や `SomeClass` など）を付けます。 プロパティとメソッドに `lowerCamelCase` 名（`frameRate` や `incrementCount` など）を付けて、型名と区別します。

構造体定義とクラス定義の例を次に示します。

```swift
struct Resolution {
    var width = 0
    var height = 0
}
class VideoMode {
    var resolution = Resolution()
    var interlaced = false
    var frameRate = 0.0
    var name: String?
}
```

上記の例では、`Resolution` と呼ばれる新しい構造体を定義して、ピクセルベースのディスプレイ解像度を記述しています。 この構造体には、`width` と `height` という 2 つの格納プロパティ(*stored propert*)があります。格納プロパティは、構造体またはクラスの一部として紐付けおよび格納される定数または変数です。これらの 2 つのプロパティは、初期値に `0` を設定することにより、 Int 型だと推論されます。

上記の例では、ビデオ表示用の特定のビデオモードを記述するために、`VideoMode` と呼ばれる新しいクラスも定義しています。このクラスには、4 つの格納プロパティの変数があります。最初の `resolution` は、新しい `Resolution` 構造体インスタンスで初期化されており `Resolution` 型だと推論されます。他の 3 つのプロパティは、新しい `VideoMode` インスタンスは、`interlaced` に (「非インターレースビデオ」を意味する)`false` を設定し、再生フレームレート `0.0`、および `name` と呼ばれる optional の `String` 値で初期化されます。 `name` プロパティは optional 型のため、デフォルトで `nil` が自動的に設定されます。

### Structure and Class Instances(structとclassのインスタンス)

`Resolution` 構造体の定義と `VideoMode` クラスの定義は、`Resolution` または `VideoMode` がどのように見えるかを説明するだけです。それら自体は、特定の解像度やビデオモードについては説明していません。そのためには、構造体またはクラスのインスタンスを作成する必要があります。

インスタンスを作成するための構文は、構造とクラスの両方で非常に似ています。

```swift
let someResolution = Resolution()
let someVideoMode = VideoMode()
```

構造体とクラスはどちらも、新しいインスタンスに初期化構文を使用します。初期化構文の最も単純な形式では、クラスまたは構造体の型名の後に、`Resolution()` や `VideoMode()` などの空の括弧(`()`)が続きます。これにより、クラスまたは構造の新しいインスタンスが作成され、プロパティはデフォルト値で初期化されます。クラスと構造体の初期化については、[Initialization](./initialization.md)で詳しく説明しています。

### Accessing Properties(プロパティへのアクセス)

ドット構文を使用して、インスタンスのプロパティにアクセスできます。ドット構文では、インスタンス名の直後に、スペースを入れずにピリオド(`.`)で区切ってプロパティ名を記述します。

```swift
print("The width of someResolution is \(someResolution.width)")
// Prints "The width of someResolution is 0"
```

この例では、`someResolution.width` は `someResolution` の `width` プロパティを参照し、デフォルトの初期値 `0` を返します。

`VideoMode` の `resolution` プロパティの `width` プロパティなど、プロパティのプロパティなどより深い階層へもアクセスできます。

```swift
print("The width of someVideoMode is \(someVideoMode.resolution.width)")
// Prints "The width of someVideoMode is 0"
```

ドット構文を使用して、変数プロパティに新しい値を代入することもできます。

```swift
someVideoMode.resolution.width = 1280
print("The width of someVideoMode is now \(someVideoMode.resolution.width)")
// Prints "The width of someVideoMode is now 1280"
```

### Memberwise Initializers for Structure Types(structのMemberwiseイニシャライザ)

全ての構造体には、自動的に生成されるメンバーを全て初期化するイニシャライザがあります。これを使用して、新しい構造体インスタンスのメンバープロパティを初期化できます。新しいインスタンスのプロパティの初期値は、プロパティ名でイニシャライザに渡すことができます。

```swift
let vga = Resolution(width: 640, height: 480)
```

構造体とは異なり、クラスはデフォルトのイニシャライザを自動で生成しません。イニシャライザについては、[Initialization](./initialization.md)で詳しく説明しています。

## Structures and Enumerations Are Value Types(structと列挙型は値型)

## Classes Are Reference Types(classは参照型)

### Identity Operators(参照等価演算子)

### Pointers(ポインタ)
