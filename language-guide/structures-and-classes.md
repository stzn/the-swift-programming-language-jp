# 構造体とクラス\(Structures and Classes\)

最終更新日: 2021/6/27

_構造体_と_クラス_は、プログラムのコードの構成要素を構築する汎用的で柔軟な構造概念です。定数、変数、関数の定義に使用するのと同じ構文を使用して、プロパティとメソッドを定義して機能を追加します。

他のプログラミング言語とは異なり、Swift では、インターフェイスと実装ファイルを別々に作成する必要はありません。構造体またはクラスを 1 つのファイルで定義すると、その外部インターフェイスは自動的に他のコードで使用できるようになります。

> NOTE  
> 昔から_オブジェクト_はクラスのインスタンスはとして知られています。ただし、 Swift の 構造体とクラスは、他の言語以上に機能がかなり似ていて、この章でも、構造体またはクラス型のインスタンスのどちらにも当てはまる機能について説明しています。このため、より一般的な意味合いでインスタンスという用語を使用します。

## 構造体とクラスの比較\(Comparing Structures and Classes\)

Swift の構造体とクラスには多くの共通点があります。いずれも下記のようなことができます:

* 値を格納するプロパティを定義できます
* 機能を提供するメソッドを定義できます
* subscript 構文を使用して値へのアクセスを提供するサブスクリプトを定義できます
* 初期状態を設定するためのイニシャライザを定義できます
* `extension` で機能を拡張できます
* 特定の種類の標準機能を提供するためにプロトコルに準拠できます

これらの機能の詳細については、[Properties\(プロパティ\)](properties.md)、[Methods\(メソッド\)](methods.md)、[Initialization\(初期化\)](initialization.md)、[Extensions\(拡張\)](extensions.md)および[Protocols\(プロトコル\)](protocols.md)を参照ください。

一方で、クラスには、構造体にはない追加の機能があります。

* あるクラスが別のクラスの特性を継承できます
* 実行時のクラスインスタンスの型の確認と解釈に型キャストが使えます
* _デイニシャライザ_を使ったクラスのインスタンスに割り当てられているリソースの解放
* _参照カウント_を使ったある同じクラスインスタンスへ複数から参照できます

これらの機能の詳細については、[Inheritance\(継承\)](inheritance.md)、[Type Casting\(型キャスト\)](type-casting.md)、[Deinitialization\(デイニシャライザ\)](deinitialization.md)、[Automatic Reference Counting\(自動参照カウント\)](automatic-reference-counting.md)を参照ください。

クラスは追加の機能をサポートしている分、複雑さが増します。一般的なガイドラインとして、構造体の方が扱いやすく推奨されます。クラスは適切または必要な場合にのみ使用してください。つまり、実際は、独自に定義するデータ型のほとんどが構造体と列挙型になることを意味します。より詳細な比較は[Choosing Between Structures and Classes](https://developer.apple.com/documentation/swift/choosing_between_structures_and_classes)を参照ください。

> NOTE  
> クラスとアクターは多くの共通の特徴や振る舞いを持ちます。アクターについては、[同時並行処理\(Concurrency\)](./concurrency.md)を参照ください。

### 定義構文\(Definition Syntax\)

構造体とクラスの定義構文は似ています。`struct` キーワードを使用して構造体を作成し、`class` キーワードを使用してクラスを作成します。どちらも、定義全体を中括弧のペア\(`{}`\)内に配置します:

```swift
struct SomeStructure {
    // 構造体の定義をここに
}
class SomeClass {
    // クラスの定義をここに
}
```

> NOTE  
> 新しい構造体またはクラスを定義するときはいつでも、新しい Swift の型を定義します。標準の Swift の型\(`String`、`Int`、`Bool` など\)と同じように、型に `UpperCamelCase` 名\(ここでは `SomeStructure` や `SomeClass` など\)にします。プロパティとメソッドは `lowerCamelCase` 名\(`frameRate` や `incrementCount` など\)として、型名と区別します。

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

上記の例では、`Resolution` と呼ばれる新しい構造体を定義して、ピクセルベースのディスプレイ解像度を記述しています。この構造体には、`width` と `height` という 2 つの_格納プロパティ_があります。格納プロパティは、構造体またはクラスの一部として紐付け、格納される定数または変数です。これらの 2 つのプロパティは、初期値に `0` を設定することにより、`Int` 型だと推論されます。

上記の例では、ビデオ表示用の特定のビデオモードを記述するために、`VideoMode` と呼ばれる新しいクラスも定義しています。このクラスには、4 つの格納プロパティの変数があります。最初の `resolution` は、新しい `Resolution` 構造体インスタンスで初期化されており `Resolution` 型だと推論されます。他の 3 つのプロパティに関しては、`interlaced` に \(「非インターレースビデオ」を意味する\)`false` を、再生フレームレートに `0.0`、および `name` と呼ばれるオプショナルの `String` 値で初期化されます。`name` プロパティはオプショナル型のため、デフォルトで `nil` が自動的に設定されます。

### struct と class のインスタンス\(Structure and Class Instances\)

`Resolution` 構造体の定義と `VideoMode` クラスの定義は、`Resolution` または `VideoMode` がどのようなものかを説明しているだけです。それら自体は、特定の解像度やビデオモードについては説明していません。そうするためには、構造体またはクラスのインスタンスを作成する必要があります。

インスタンスを作成するための構文は、構造体とクラスの両方で非常に似ています。

```swift
let someResolution = Resolution()
let someVideoMode = VideoMode()
```

構造体とクラスはどちらも、新しいインスタンスに初期化構文\(`init`\)を使用します。初期化構文の最もシンプルな形式では、クラスまたは構造体の型名の後に、`Resolution()` や `VideoMode()` などの空の括弧\(`()`\)が続きます。これにより、クラスまたは構造の新しいインスタンスが作成され、プロパティはデフォルト値で初期化されます。クラスと構造体の初期化については、[Initialization\(初期化\)](initialization.md)で詳しく説明しています。

### <a id="accessing-properties">プロパティへのアクセス\(Accessing Properties\)</a>

ドット構文を使用して、インスタンスのプロパティにアクセスできます。ドット構文では、インスタンス名の直後に、スペースを入れずにピリオド\(`.`\)で区切ってプロパティ名を記述します。

```swift
print("The width of someResolution is \(someResolution.width)")
// The width of someResolution is 0
```

この例では、`someResolution.width` は `someResolution` の `width` プロパティを参照し、デフォルトの初期値 `0` を返します。

`VideoMode` の `resolution` プロパティの `width` プロパティなど、より深い階層へもアクセスできます。

```swift
print("The width of someVideoMode is \(someVideoMode.resolution.width)")
// The width of someVideoMode is 0
```

ドット構文を使用して、変数プロパティに新しい値を代入することもできます。

```swift
someVideoMode.resolution.width = 1280
print("The width of someVideoMode is now \(someVideoMode.resolution.width)")
// The width of someVideoMode is now 1280
```

### <a id="structures-and-classes-memberwise-initializers-for-structure-types">構造体のメンバワイズイニシャライザ\(Memberwise Initializers for Structure Types\)</a>

全ての構造体には、メンバ全てに値を設定して初期化するイニシャライザ\(_メンバワイズイニシャライザ_\)が自動的に生成されます。新しいインスタンスのプロパティの初期値は、プロパティ名でイニシャライザに渡すことができます。

```swift
let vga = Resolution(width: 640, height: 480)
```

構造体とは異なり、クラスはデフォルトのイニシャライザを自動で生成しません。イニシャライザについては、[Initialization\(初期化\)](initialization.md)で詳しく説明しています。

## <a id="structures-and-enumerations-are-value-type">構造体と列挙型は値型\(Structures and Enumerations Are Value Types\)</a>

値型は、変数または定数に値が割り当てられたとき、または関数に渡されたときに値がコピーされる型\(_値型_\)です。

これまでの章では、実際に値型を多く使用してきました。実際、Swift の基本的な型\(整数、浮動小数点数、ブール値、文字列、配列、辞書\)はすべて値型で、内部では、構造体として実装されています。

全ての構造体と列挙型は値型です。つまり、作成した構造体と列挙型のインスタンス、およびそれらがプロパティとして持つ値型は、コードで渡されるときに常にコピーされます。

> NOTE  
> 配列、辞書、文字列などの標準ライブラリによって定義されたコレクションは、コピーのパフォーマンスコストを削減するように最適化されています。これらのコレクションは、すぐにコピーを作成する代わりに、元のインスタンスとコピーの間で要素が格納されているメモリを共有します。コレクションのコピーの1つが変更された場合、要素は変更の直前にコピーされます。動作上では、常にコピーがすぐに行われているかのように見えます。

前の例の `Resolution` 構造体を使用した場合について考えてみます。

```swift
let hd = Resolution(width: 1920, height: 1080)
var cinema = hd
```

この例では、`hd` という定数を宣言し、フル HD ビデオの幅と高さ\(幅 1920 ピクセル、高さ 1080 ピクセル\)で初期化された `Resolution` インスタンスを設定します。

次に、`cinema` という変数を宣言し、`hd` の現在の値を設定します。`Resolution` は構造体のため、既存のインスタンスのコピーが作成され、この新しいコピーが `cinema` に割り当てられます。`hd` と `cinema` の幅と高さは同じになりましたが、内部ではまったく異なる 2 つのインスタンスです。

次に、`cinema` の `width` プロパティは、デジタルシネマ映写に使用される、わずかに幅の広い 2K 標準の幅\(幅 2048 ピクセル、高さ 1080 ピクセル\)に修正されます。

```swift
cinema.width = 2048
```

`cinema` の `width` プロパティを確認すると、実際に 2048 に変更されていることがわかります。

```swift
print("cinema is now \(cinema.width) pixels wide")
// cinema is now 2048 pixels wide
```

ただし、元の `hd` インスタンスの `width` プロパティは、1920 のままです。

```swift
print("hd is still \(hd.width) pixels wide")
// hd is still 1920 pixels wide
```

`cinema` に `hd` の現在の値が与えられると、`hd` に保存されている値が新しい `cinema` インスタンスにコピーされます。これらは、同じ数値を含む 2 つの完全に別個のインスタンスですが、次の図に示すように、`cinema` の幅を `2048` に設定しても、`hd` に保存される幅には影響しません:

![&#x69CB;&#x9020;&#x4F53;&#x306E;&#x72B6;&#x614B;&#x306E;&#x5171;&#x6709;](../assets/sharedStateStruct_2x.png)

列挙型でも同じように動きます。

```swift
enum CompassPoint {
    case north, south, east, west
    mutating func turnNorth() {
        self = .north
    }
}
var currentDirection = CompassPoint.west
let rememberedDirection = currentDirection
currentDirection.turnNorth()

print("The current direction is \(currentDirection)")
print("The remembered direction is \(rememberedDirection)")
// The current direction is north
// The remembered direction is west
```

`RememberedDirection` に `currentDirection` の値が割り当てられると、実際にはその値のコピーが設定されます。その後、`currentDirection` の値を変更しても、`rememberedDirection` に保存されていた元の値には影響しません。

## <a id="classes-are-reference-types">クラスは参照型\(Classes Are Reference Types\)</a>

値型とは異なり、_参照型_は、変数または定数に割り当てられたとき、または関数に渡されたときにコピーされません。コピーではなく、同じ既存のインスタンスへの参照が使用されます。

上記で定義した `VideoMode` クラスを使用した例を次に示します。

```swift
let tenEighty = VideoMode()
tenEighty.resolution = hd
tenEighty.interlaced = true
tenEighty.name = "1080i"
tenEighty.frameRate = 25.0
```

この例では、`tenEighty` という新しい定数を宣言し、`VideoMode` クラスの新しいインスタンスを参照するように設定します。ビデオモードには、元々 `1920` x `1080` の HD 解像度のコピーが割り当てられています。インターレースされるように設定され、名前には `"1080i"` 、フレームレートは `25.0` フレーム/秒が設定されています。

次に、`tenEighty` を `alsoTenEighty` という新しい定数に割り当て、`alsoTenEighty` のフレームレートを変更しています。

```swift
let alsoTenEighty = tenEighty
alsoTenEighty.frameRate = 30.0
```

クラスは参照型のため、`tenEighty` と `alsoTenEighty` は両方とも同じ `VideoMode` インスタンスを参照しています。事実上、次の図に示すように、これらは同じインスタンスに 2 つの異なる名前を付けているだけです:

![&#x30AF;&#x30E9;&#x30B9;&#x306E;&#x72B6;&#x614B;&#x306E;&#x5171;&#x6709;](../assets/sharedStateClass_2x.png)

`tenEighty` の `frameRate` プロパティを確認すると、基になる `VideoMode` インスタンスから `30.0` の新しいフレームレートが正しく設定されていることがわかります。

```swift
print("The frameRate property of tenEighty is now \(tenEighty.frameRate)")
// The frameRate property of tenEighty is now 30.0
```

この例は、参照型の扱いがいかに難しいかを示しています。`tenEighty` と `alsoTenEighty` がコード内で大きく離れている場合、ビデオモードが変更される全ての場所を見つけるのは難しいかもしれません。`tenEighty` を使用する場合は常に `alsoTenEighty` を使用するコードも考慮する必要があります。その逆も同様です。対照的に、値型は、同じ値に作用する全てのコードは近くにあるため、考慮する点が少なく扱いが簡単です。

`tenEighty` と `alsoTenEighty` は、変数ではなく定数として宣言されていることに注目してください。それでも、`tenEighty` および `alsoTenEighty` 定数自体の値は実際には変更されないため、`tenEighty.frameRate` および `alsoTenEighty.frameRate` を変更することはできます。`tenEighty` と `alsoTenEighty` 自体は、`VideoMode` インスタンスを「格納」しません。代わりに、どちらも内部で同じ `VideoMode` インスタンスを参照します。変更されるのは、基になる `VideoMode` の `frameRate` プロパティで、その `VideoMode` への参照を持つ定数ではありません。

### <a id="identity-operators">恒等作用素\(Identity Operators\)</a>

クラスは参照型のため、複数の定数と変数が内部でクラスの同じインスタンスを参照する可能性があります。\(構造体や列挙型は、定数や変数に割り当てられたとき、または関数に渡されたときに常にコピーされるため異なります\)

2 つの定数または変数が同じインスタンスを参照しているかどうかを確認すると便利な場合があります。これを可能にするために、Swift は 2 つの_恒等作用素_を提供します:

* 同一\(`===`\)
* 同一ではない\(`!==`\)

これらの演算子を使用して、2 つの定数または変数が同じインスタンスを参照しているかどうかを確認します。

```swift
if tenEighty === alsoTenEighty {
    print("tenEighty and alsoTenEighty refer to the same VideoMode instance.")
}
// tenEighty and alsoTenEighty refer to the same VideoMode instance.
```

「同一」\(3 つの等号または `===`\)は、「等しい」\(2 つの等号または `==` で表される\)と同じではないことに注意してください。「同一」とは、クラスタイプの 2 つの定数または変数がまったく同じクラスインスタンスを参照することを意味します。「等しい」とは、型の設計者が定義する「等しい」という観点で、適切に 2 つのインスタンスの値が等しいまたは同等だと見なされることを意味します。

独自の構造体とクラスを定義するときは、2 つのインスタンスがどうすれば等しいと見なされるのかを決定するのはあなた次第です。`==` および `!=` 演算子の独自の実装を定義するプロセスは、[Equivalence Operators\(等価演算子\)](../language-guide/advanced-operators.md#equivalence-operators)で説明されています。

### ポインタ\(Pointers\)

C 言語、C++、または Objective-C の経験がある場合は、これらの言語が_ポインタ_を使用してメモリ内のアドレスを参照することを知っているかもしれません。ある参照型のインスタンスを参照する Swift の定数または変数は、C 言語のポインタに似ていますが、メモリ内のアドレスへのポインタを直接示しているではなく、参照を示すためにアスタリスク\(`*`\)を記述する必要はありません。代わりに、これらの参照は、Swift の他の定数または変数と同様に定義されます。標準ライブラリには、ポインタを直接操作する必要がある場合に使用できるポインタとバッファ型が用意されています。[Manual Memory Management](https://developer.apple.com/documentation/swift/swift_standard_library/manual_memory_management)を参照ください。

