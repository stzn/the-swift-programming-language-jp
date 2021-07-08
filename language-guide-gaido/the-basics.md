# 基本\(The Basics\)

最終更新日: 2021/7/4

Swift は iOS, macOS, watchOS, tyOS アプリ開発のための新しいプログラミング言語です。新しい言語ではあるものの、Swift の多くの部分は C 言語と Objective-C の開発経験があれば慣れ親しんだ部分もあるかと思います。

Swift は C 言語と Objective-C の全ての基本的な型に対応した Swift 版の型を提供します。数値に対する `Int`、浮動小数点数に対する `Double` と `Float`、ブール値に対する `Bool`、文字データに対する `String` があります。また、3 つのより強力な collection 型\(`Array`, `Set`, `Dictionary`\)も提供しています。詳細は [Collection Types\(コレクション型\)](collection-types.md)に記載しています。

C 言語のように、Swift は、名前を特定して、値を保持したり、その値を参照するために変数を使います。また、変数の値を変更できなくすることで、より幅広い方法で変数を使用することができます。これらは、定数として知られており、C 言語の定数よりもかなり強力です。定数は、値を変更する必要がない場合に、コードを、意図的に、より安全に、よりわかりやすくするために Swift 全体で使われます。

これまで見たことがあるような型に加え、Swift は Objective-C にはなかった、タプルのような、より応用的な型を導入します。タプルは値を 1 つのグループとして扱うことができます。タプルを使用すると、複数の値を 1 つの値の組み合わせとして関数から返すことができます。

Swift は値が存在しないかもしれない値を扱うオプショナル型を導入しています。オプショナルは、「値が存在していて、これは x と等しい」もしくは「値は一切存在しない」ということを表します。オプショナルは Objective-C のポインタで `nil` を扱うのに似ていますが、クラスだけではなく、あらゆる型に使用することができます。Objective-C の `nil` ポインタよりも、安全で表現豊かなだけでなく、Swift の数々の強力な特徴の中心に位置します。

Swift は_型安全_な言語です。つまり、言語として、コードで扱う値の型を明確にしてくれます。例えば、`String` が必要な場合に、この型安全な特徴が、間違って `Int` を渡してしまうことを防いでくれます。同様に、オプショナルではない `String` にオプショナルの `String` を誤って渡してしまうことも防いでくれます。型安全なことで、開発中に、素早くエラーの発見と修正をすることができます。

## <a id="constants-and-variables">定数と変数\(Constants and Variables\)</a>

定数と変数は特定の型の値\(数字の `10` や文字列の `"Hello"` など\)と名前\(`maximumNumberOfLoginAttempts` や `welcomeMessage`\)を関連付けます。_定数_は一度値を設定すると変更することはできません。一方で、_変数_は後で異なった値を設定できます。

### 定数と変数の宣言\(Declaring Constants and Variables\)

定数と変数は、使用する前に定義されていなければなりません。定数は `let`、変数は `var` キーワードで定義します。ここで、ユーザが何回ログインをしようとしたか試行回数を追跡する定数と変数の例を紹介します。

```swift
let maximumNumberOfLoginAttempts = 10
var currentLoginAttempt = 0
```

このコードは下記のように読み取れます。

「`maximumNumberOfLoginAttempts` という定数を定義して、`10` という値を設定します。次に `currentLoginAttempt` という変数を定義して、`0` という初期値を設定します」

この例では、最大回数は変更しないので定数で定義しています。試行回数はログインを試みる度に増やさなければならないため、変数として今の試行回数を定義しています。

カンマ\(`,`\)区切りで 1 行の中に複数の定数や変数を定義することもできます。

```swift
var x = 0.0, y = 0.0, z = 0.0
```

> NOTE  
> コードで保持している値がこの先変更されない場合は、 `let` を使用して定数として定義してください。変数は変更できるようにする必要がある場合にのみ使用してください。

### <a id="type-annotations">型注釈\(Type Annotations\)</a>

定数や変数を定義するときに、保持する値の種類をより明確にするために、型注釈を使用することができます。定数や変数の名前の後にコロン\(`:`\)を置いて、型注釈を書きましょう。

この例では `welcomeMessage` という変数に型注釈を書いて `String` が保持されることを示しています。

```swift
var welcomeMessage: String
```

定義の中のコロンは、「~型の~」を意味します。なので、このコードは下記のように読み取れます。

「`String` 型の `welcomeMessage` という変数を定義します」

「`String` 型の」という句は、どんな `String` 型の値も保持できることを意味します。つまり、保持することができる「物事の型\(種類\)」だと考えましょう。

`welcomeMessage` にはエラーなしに文字列を設定できます。

```swift
welcomeMessage = "Hello"
```

1 行の中で、同じ型の複数の変数をカンマで区切って定義することもできます。この際、型注釈は最後の変数の後に 1 つ付けます。

```swift
var red, green, blue: Double
```

> NOTE  
> 実際に型注釈を書く必要はあまりありません。定義時に定数や変数に初期値を与えた場合、 Swift はたいていそれらの型を推論できます\(詳細は[Type Safety and Type Inference\(型安全と型推論\)](the-basics.md#type-safety-and-type-inferenceto)。上記の `welcomeMessage` の例では、初期値を与えていないため、推論をすることができないため、`welcomeMessage` 変数は型注釈で型を特定しています。

### 定数と変数の命名\(Naming Constants and Variables\)

定数名と変数名には、Unicode 文字も含めた、ほとんどの文字を含めることができます。

```swift
let π = 3.14159
let 你好 = "你好世界"
let 🐶🐮 = "dogcow"
```

定数名と変数名に、ホワイトスペース、数学記号、矢印、公式ではない Unicode スカラ値、罫線素片、書式文字は含められません。また、数字から始めることはできません\(他の位置に数字を含めることはできます\)

一度ある型の定数や変数を定義すると、同じ名前で再定義することはできません。また、異なる型の値を保持することもできません。定数を変数に、変数を定数に変換することもできません。

> NOTE  
> Swift の予約語と同じ名前の定数や変数を使いたい場合、そのキーワードをバッククォート\(`\`\)で囲みます。可能ではありますが、予約語を名前に使用することは本当に他に選択肢がない以外は避けましょう。

既存の変数の値を他の互換性のある型の値に変更することはできます。下記の例では、`friendlyWelcome` の値を `"Hello!"` から `"Bonjour!"` に変更しています。

```swift
var friendlyWelcome = "Hello!"
friendlyWelcome = "Bonjour!"
// friendlyWelcome は "Bonjour!"
```

変数とは異なり、定数の値は最初に設定した後に変更できません。試みても、コンパイル時にエラーになります。

```swift
let languageName = "Swift"
languageName = "Swift++"
// コンパイルエラー: languageName は変更できません
```

### <a id="printing-constants-and-variables">定数と変数の出力\(Printing Constants and Variables\)</a>

`print(_:separator:terminator:)` 関数を使用して、定数や変数の現在の値を出力することができます。

```swift
print(friendlyWelcome)
// "Bonjour!"
```

`print(_:separator:terminator:)` 関数はグローバル関数で、1 つ以上の値を適切なアウトプット先に出力します。Xcode では、`print(_:separator:terminator:)` 関数を使用すると、 Xcode のコンソールパネルへ値を出力します。`separator` と `terminator` パラメータには、デフォルト値が用意されているので省略可能です。デフォルトでは最後に改行を追加します。改行を付けたくない場合は `terminator` に空文字を渡してください。例えば `print(someValue, terminator: "")`。詳細は[Default Parameter Values\(デフォルトパラメータ値\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide-gaido/functions#default-parameter-values)。

Swift は長い文字列の中で定数や変数をプレースホルダとして使用したい場合、_文字列補間_を使い、定数や変数の現在値に置き換えるように Swift に伝えることができます。名前を括弧\(`()`\)で囲み、開始括弧の前にバックスラッシュ\(`\`\)を付けます。

```swift
print("The current value of friendlyWelcome is \(friendlyWelcome)")
// "The current value of friendlyWelcome is Bonjour!"
```

> NOTE  
> 文字列補間で使用できるオブションは[String Interpolation\(文字列補間\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide-gaido/strings-and-characters#string-interpolation)に記載しています

## コメント\(Comments\)

コード内に実行されないテキスト\(メモやリマインダーなど\)を含めるためにはコメントを使います。コメントはコンパイル時に Swift のコンパイラからは無視されます。

Swift のコメントは C 言語のコメントにとてもよく似ています。1 行のコメントは 2 つのスラッシュ\(`//`\)で開始します。

```swift
// これはコメントです
```

複数行の場合は、スラッシュ+アスタリスク\(`/*`\)で開始し、アスタリスク+スラッシュ\(`*/`\)で終了します。

```swift
/* これはコメントです
複数行に渡って書くこともできます */
```

C 言語の複数行コメントとは異なり、Swift ではコメントを他の複数行コメントにネストさせることができます。1 つ目の複数行コメントブロックの中で、2 つ目の複数行ブロックコメントを開始します。2 つ目のブロックは、1 つ目のブロックの前にコメントを閉じます。

```swift
/* これは最初の複数行コメントの開始です
 /* これはネストした2番目の複数行コメントの開始です
これはネストした2番目の複数行コメントの終わりです */
これは最初の複数行コメントの終わりです */
```

ネストした複数行コメントを使用することで、長い複数行コメントの中でも簡単に素早くその部分だけコメントアウトすることができます。

## セミコロン\(Semicolons\)

他の言語とは異なり、Swift ではコードの 1 つ 1 つの宣言の最後に、セミコロン\(`;`\)を付ける必要がありません\(付けることも可能ではあります\)。一方で、1 行に複数のステートメントを書きたい場合は必要になります。

```swift
let cat = "🐱"; print(cat)
// "🐱"
```

## <a id="integers">整数\(Integers\)</a>

_整数_は小数点のない整数値を全部含みます\(`42`、`-23` など\)。整数とは_符号付き_\(正の値、または 0、または負の値\) または_符号なし_\(正の値、または 0\) です。

Swift は符号付きと符号なしの整数値を 8, 16, 32, 64 ビットの形式で提供します。これらは C 言語の名前に合わせて命名されています。8 ビットの符号なし整数は `UInt8`、32 ビットの符号付き整数は `Int32` です。Swift の全ての型と同様に、これらの整数値型の頭文字は大文字です。

### 整数の境界\(Integer Bounds\)

`min` と `max` プロパティを使用して、各整数値型の最小値と最大値にアクセスすることができます。

```swift
let minValue = UInt8.min  // minValue は 0 で、 UInt8 型です
let maxValue = UInt8.max  // maxValue は 255 で UInt8 型です
```

これらのプロパティの値は、適切なサイズの整数値型で\(上記の例だと `UInt8`\)で、同じ型の他の値と一緒に式の中で使用することができます。

### Int

多くの場合、整数値のサイズを特定する必要はありません。Swift はプラットフォームに応じて決まるサイズと同じサイズをもつ `Int` という型を提供しています。

* 32 ビットのプラットフォームの場合、`Int` は `Int32` と等しい
* 64 ビットのプラットフォームの場合、`Int` は `Int64` と等しい

特定のサイズの整数値を扱う必要がない限り常に `Int` を使用するようにしてください、これはコードの一貫性と互換性を保つ手助けとなります。32 ビットのプラットフォームでは、`Int` は `-2,147,483,648` から `2,147,483,647` まで保持することができます。これは整数値が使われる多くの場合において、十分に大きい範囲です。

### UInt

Swift は符号なしの整数値型も提供しています。これも、プラットフォームに応じて決まるサイズと同じサイズを持ちます。

* 32 ビットのプラットフォームの場合、`UInt` は `UInt32` と等しい
* 64 ビットのプラットフォームの場合、`UInt` は `UInt64` と等しい

> NOTE  
> `UInt` は特別にプラットフォームに応じて決まるサイズと同じサイズの 符号なし整数型を扱いたい場合にのみ使用してください。そうでない場合、負の値にならないとしても `Int` を使用する方が好ましいです。一貫して `Int` を整数値に使用することで、異なる整数値型間の変換を避け、合致する型をコンパイラが推論することで、互換性を保つことができます。詳細は[Type Safety and Type Inference\(型安全と型推論\)](the-basics.md#type-safety-and-type-inferenceto)。

## 浮動小数点数\(Floating-Point Numbers\)

浮動小数点数は、小数部分を持つ数値です\(`3.14159`, `0.1`, `-273.15` など\)。

浮動小数点数値型は、整数値型よりもより広い範囲の値を表現でき、`Int` より大きい\(またはより小さい\)値を保持できます。Swift は 2 つの符号付き浮動小数点数値型を提供しています。

* `Double` は 64 ビットの浮動小数点数を表します
* `Float` は 32 ビットの浮動小数点数を表します

> NOTE  
> `Double` は最小で15桁の精度の小数値を持ち、`Float` は6桁の精度の小数値を持ちます。適切な浮動小数点数値型は、扱いたい数値の特性と範囲によります。どちらでも良い場合は、`Double` が好まれます。

## 型安全と型推論\(Type Safety and Type Inference\)

Swift は_型安全_な言語です。型安全な言語は、扱っている値の型を明確にします。`String` が必要な場合に、間違って `Int` を渡すようなことはありません。

型安全なことから、コンパイル時に型チェックが行われ、間違った型に対してはエラーを示します。これによって開発時に素早くエラーに気がつき修正をすることができます。

型チェックによって、異なる型の値を扱う場合に間違った型を代入してしまうエラーを回避することができます。しかし、全ての定数と変数の定義時に型を特定しなければならないということではありません。特定しない場合、Swift は適切な型への推論を行います。コンパイラがコンパイル時に与えられた値を調べることで、式の型を自動で推論してくれます。

この型推論のおかげで、Swift は C 言語や Objective-C のような言語と比べて、型を宣言する必要はあまりありません。定数と変数は明示的に型を記載する場合もありますが、型を特定する多くの作業は Swift の側で行なってくれます。

型推論は、特に初期値を伴って変数や定数を宣言するときに役に立ちます。特に、_リテラル値\(または literal\)_な値を代入する際によく機能します。\(リテラルな値とは、下記の例にあるようなコードに直接出てくる `42, 3.14159` のような値のことです\)

例えば、`42` を新しい定数に型を記載せずに代入すると、整数値型に見える数値リテラルで初期化していることから、Swift はその型を推論して `Int` と判断します。

```swift
let meaningOfLife = 42
// meaningOfLife は Int 型と推論されます
```

同様に、浮動小数点数を型なしで宣言した場合、Swift は `Double` を定義したと推論します。

```swift
let pi = 3.14159
// pi は Double 型と推論されます
```

Swift は浮動小数点数を推論する際に、常に\(`Float` よりも\)`Double` を選択します。

式の中で整数値と浮動小数点数を組み合わせた場合は、コンテキストから多くの場合 `Double` と推論されます。

```swift
let anotherPi = 3 + 0.14159
// anotherPi は Double 型と推論されます
```

`3` には明確な型がありませんが、後ろの浮動小数点数の値から `Double` と推論されます。

## 数値リテラル\(Numeric Literals\)

数値リテラルは、下記の方法で書くことができます。

* プレフィックスなしの 10 進数
* `0b` をプレフィックスにした 2 進数
* `0o` をプレフィックスにした 8 進数
* `0x` をプレフィックスにした 16 進数

これらの数値リテラルは、`17` という 10 進数を下記のように保持します。

```swift
let decimalInteger = 17
let binaryInteger = 0b10001       // 17 は２進数表記
let octalInteger = 0o21           // 17 は8進数表記
let hexadecimalInteger = 0x11     // 17 は16進数表記
```

浮動小数点数リテラルは、10 進数と 16 進数にできます。小数点の左と右の両方に必ず整数値が必要です。10 進数の浮動小数点数は、指数を持つ場合があります\(小文字または大文字の `e`\)。16 進数の浮動小数点数は必ず指数を指定しなければなりません\(小文字または大文字の `p`\)。

`exp` の指数を持つ 10 進数では、元の値に$$10^{exp}$$を掛けます。

* `1.25e2` は 1.25 x $$10^2$$ または `125.0` と等しい
* `1.25e-2` は 1.25 x $$10^{-2}$$ または `0.0125` と等しい

`exp` の指数を持つ 16 進数では、元の値に$$2^{exp}$$を掛けます。

* `0xFp2` は 15 x $$2^2$$ または `60.0` と等しい
* `0xFp-2` は 15 x $$2^{-2}$$ または `3.75` と等しい

下記の浮動小数点数は、全て `12.1875` という 10 進数を表しています。

```swift
let decimalDouble = 12.1875
let exponentDouble = 1.21875e1
let hexadecimalDouble = 0xC.3p0
```

整数値リテラルは、読みやすくするために追加でフォーマットを含めることができます。整数値や浮動小数点数へ追加の 0 を加えたり、アンダースコア\(`_`\)を追加することで可読性を向上させることができます。このフォーマットに整数値自体への影響はありません。

```swift
let paddedDouble = 000123.456
let oneMillion = 1_000_000
let justOverOneMillion = 1_000_000.000_000_1
```

## 数値型の変換\(Numeric Type Conversion\)

通常整数値を扱う際は、負の値にならないことがわかっていたとしても定数や変数に `Int` 型を使いましょう。全ての状況でデフォルトに `Int` を使用することで、整数値の定数と変数を特別な手順なしに互換可能にし、数値リテラルから推論される型とも合致します。

特別な理由ある場合のみ、他の整数値型を使いましょう。例えば、外部リソースから特定サイズの型が指定されている場合やパフォーマンス、メモリの使用量や他の最適化が必要な場合など、がこれに当たります。明示的にサイズを指定することで思わぬオーバーフローを起こしたり、暗黙的にそのデータの特性を表現することができます。

### 整数の変換\(Integer Conversion\)

整数値の定数や変数に保持できる範囲は、それぞれの整数値型によって異なります。`Int8` の定数や変数は、`−128` から `127` まで保持できます。`UInt8` の定数や変数は、`0` から `255` まで保持できます。指定したサイズの整数値型の定数や変数に合わない値を設定しようとするとエラーになります。

```swift
let cannotBeNegative: UInt8 = -1
// UInt8 はマイナス値を保持できません。そのためエラーになります
let tooBig: Int8 = Int8.max + 1
// Int8 は最大値以上のマイナス値を保持できません。
// そのためこれもエラーになります
```

このように、整数値型によって保持できる範囲が異なるため、ケースごとに型の変換をしなければなりません。この明示的に指定する方法によって、隠れた変換ミスを防ぎ、コードで型変換が起こっていることを明確に表現することができます。

ある特定の整数値型を他の型に変換するためには、既存の整数値から変換したい型の新しい値を生成する必要があります。下記の例では、定数 `twoThousand` は `UInt16` ですが、定数の `one` は `UInt8` です。この 2 つは同じ型ではないので直接足し算をすることができません。そこで、この例では、`one` を使用して `UInt16` の新しい値を作るために `UInt16(one)` を呼び、元の値に置き換えて使います:

```swift
let twoThousand: UInt16 = 2_000
let one: UInt8 = 1
let twoThousandAndOne = twoThousand + UInt16(one)
```

今はどちらも `UInt16` なので、足し算は可能です。そして計算結果の定数\(`twoThousandAndOne`\)も `UInt16` に推論されます。

`SomeType(ofInitialValue)` という形式は、初期値を渡して初期化を行う Swift のデフォルトの方法です。裏側では、`UInt16` 型が `UInt8` の値を受け取って、新しい `UInt16` 型の値を生成しています。とはいっても、あらゆる型を渡せるわけではありません。`UInt16` 型が提供するイニシャライザに合った型が必要です。新しい型を渡して初期化する方法は、[Extensions\(拡張\)](extensions.md)に記載しています。

### 整数と浮動小数点数の変換\(Integer and Floating-Point Conversion\)

整数値と浮動小数点数の変換は明示的に行わなければなりません:

```swift
let three = 3
let pointOneFourOneFiveNine = 0.14159
let pi = Double(three) + pointOneFourOneFiveNine
// pi は 3.14159 で Double 型と推論されます
```

ここに、定数 `3` の値が `Double` 型の新しい値を作って使われています。そのため、両方の型が同じになり、足し算が可能です。変換をしない場合、足し算はできません。

整数値から浮動小数点数への変換は明示的に行わなければなりません。整数値は `Double` や `Float` で初期化できます。

```swift
let integerPi = Int(pi)
// integerPi は 3 で Int 型と推論されます
```

浮動小数点数の値は新しい整数値として初期化される場合、必ず小数点は切り捨てられます。つまり、`4.75` は `4`、`-3.9` は `-3` になります。

> NOTE  
> 整数値の定数と変数を組み合わせるルールは、数値リテラルのルールとは異なります。リテラル値の `3` はリテラル値の `0.14159` とそのまま加算できます。リテラル値は明示的な型を有していないため、コンパイラがその値を評価するときに型を推論します。

## タイプエイリアス\(Type Aliases\)

_タイプエイリアス\(Type Aliases\)_は既存の型に別の名前を定義します。`typealias` キーワードを使います。

タイプエイリアスは、既存の型をコンテキストに沿ったより適切な名前で参照したい場合に有効です。例えば、外部リソースの特定のサイズのデータを扱いたい場合など:

```swift
typealias AudioSample = UInt16
```

定義したタイプエイリアスは、元の名前で使用していた所にも使用することができます:

```swift
var maxAmplitudeFound = AudioSample.min
// maxAmplitudeFound は 0
```

ここでは、`AudioSample` が `UInt16` のタイプエイリアスとして定義されています。エイリアスなので、`AudioSample.min` の実態は `UInt16.min` で、`maxAmplitudeFound` の初期値は `0` になります。

## ブール値\(Booleans\)

Swift は、`Bool` と呼ばれる基本的なブール値を持っています。ブール値は、真\(true\)か偽\(false\)のみを値として取ることから、_論理値\(logical\)_として参照されます。Swift では `true` と `false` の 2 つのブールの定数を提供しています。

```swift
let orangesAreOrange = true
let turnipsAreDelicious = false
```

`orangesAreOrange` と `turnipsAreDelicious` は、ブール値リテラルで初期化されていることから `Bool` と推論されます。これまで見てきた `Int` や `Double` のように、`true` や `false` を値に設定すれば、明示的に `Bool` と指定する必要はありません。既に型がわかっている他の定数や変数を使用して初期化する際に、Swift の型推論を活用することで、より簡潔で読みやすいコードを書くことができます。

ブール値は、条件文を扱う際にとても有効です。例えば、`if` 文など:

```swift
if turnipsAreDelicious {
    print("Mmm, tasty turnips!")
} else {
    print("Eww, turnips are horrible.")
}
// "Eww, turnips are horrible."
```

`if` のような条件文については、[Control Flow\(制御フロー\)](control-flow.md)でより詳細に記載しています。

Swift は型安全なので、`Bool` 以外の値を `Bool` として使うことはできません。次の例はコンパイルエラーになります:

```swift
let i = 1
if i {
    // この例はコンパイルされず、エラーになります
}
```

一方で、次の例は問題ありません:

```swift
let i = 1
if i == 1 {
    // コンパイルできます
}
```

`i == 1` の比較結果は `Bool` なので、型チェックを通過できます。`i == 1` のような比較については、[Basic Operators\(基本演算子\)](basic-operators.md)でより詳しく記載しています。

他の型安全性を示した例と同様に、型の違いによる予期せぬエラーの発生を防ぎ、型が明確なので、コードの意図をより明確に表すことができます。

## タプル\(Tuples\)

_タプル\(Tuples\)_は、複数の値を 1 つのまとまりにグループ化します。タプル内の値にはどんな型も入れることができ、全ての型を同じにする必要はありません。

下記の例の `(404, "Not Found")` は、 HTTP ステータスコードを表したタプルです。HTTP ステータスコードは、Web ページを取得するリクエストを送ると Web サーバから返ってくる特別な値です。`404 Not Found` のステータスコードは、リクエストした Web ページが存在しなかった場合に返ってきます。

```swift
let http404Error = (404, "Not Found")
// http404Error は (Int, String)型で (404, "Not Found")と等しい
```

`(404, "Not Found")` タプルは、HTTP ステータスコードを 2 つの値:\(数値と人が理解できる説明文\)に分けた `Int` と `String` を 1 つのグループにまとめています。これは「`(Int, String)` 型のタプル」と説明できます。

タプルは、任意の順序で、異なる任意の型を組み合わせることができます。例えば、`(Int, Int, Int)` や `(String, Bool)` 型のタプルも作ることができますし、必要に応じて順番の入れ替えも可能です。

タプルの個々の内容をそれぞれ定数や変数に分けて扱うこともでき、他の値と同じようにアクセスすることができます:

```swift
let (statusCode, statusMessage) = http404Error
print("The status code is \(statusCode)")
// "The status code is 404"
print("The status message is \(statusMessage)")
// "The status message is Not Found"
```

もしタプルの一部だけが必要な場合、タプルを展開するときに、アンダースコア\(`_`\)を使用して無視することができます。

```swift
let (justTheStatusCode, _) = http404Error
print("The status code is \(justTheStatusCode)")
// "The status code is 404"
```

各値へのアクセス方法としては、0 から始まるインデックスを使用することもできます:

```swift
print("The status code is \(http404Error.0)")
// "The status code is 404"
print("The status message is \(http404Error.1)")
// "The status message is Not Found"
```

タプルの定義時に、名前を付けることもできます:

```swift
let http200Status = (statusCode: 200, description: "OK")
```

名前を付けた場合、その名前を使用して各値へアクセスすることができます:

```swift
print("The status code is \(http200Status.statusCode)")
// "The status code is 200"
print("The status message is \(http200Status.description)")
// "The status message is OK"
```

タプルは、特に関数の戻り値で有効に活用できます。Web ページを取得する関数は、取得の成否の結果を `(Int, String)` で返すかもしれません。2 つの異なる型の値を持ったタプルを返すことで、1 つの型の 1 つの値を返すよりも、関数はより有益な情報を提供できます。より詳しくは、[Functions with Multiple Return Values\(複数の戻り値がある関数\)](functions.md#functions-with-multiple-return-values複数の戻り値がある関数)を参照ください。

## <a id="optionals">オプショナル\(Optionals\)</a>

_オプショナル_は、値が存在しないかもしれないときに使用します。オプショナルは 2 つの可能性を表します: 値が_存在して_アンラップすることで値にアクセスすることができる、もしくは、値が全く_存在しない_

> NOTE  
> オプショナルの概念は、 C 言語や Objective-C には存在しません。Objective-C は、メソッドから `nil` かオブジェクトを返すことができますが、ここでいう `nil` は「妥当なオブジェクトが存在しない」、ということを意味します。しかし、これはオブジェクトのみで機能し、構造体や基本的な C 言語の型、列挙型には使用できません。これらの型へは、値が存在しないことを示す特別な値\(`NSNotFound` など\)を返すのが通例です。この方法ですと、メソッドの呼び出し元が、この特別な値が返ってくることを前提としてチェックすることを想定しています。Swift では、この特別な値を必要とせず、どんな型に対しても値が存在しないことを示すことができます。

下記にオプショナルがどのように値が存在しないことを表すかの例を示します。Swift の `Int` 型には `String` を `Int` へと変換するイニシャライザがあります。しかし、全ての文字列が `Int` に変換できるわけではありません。「`123`」という文字列は数値 `123` に変換できますが、「`hello world`」は変換することができません。

下記の例は `String` を `Int` へ変換するイニシャライザの例です。

```swift
let possibleNumber = "123"
let convertedNumber = Int(possibleNumber)
// convertedNumber Int? 型 または Optional<Int> と推論されます
```

イニシャライザは失敗するかもしれないので、`Int` ではなく、_オプショナル_の `Int` を返します。オプショナルの `Int` は、`Int` ではなく `Int?` と書きます。`?` はオプショナルを示し、_ある_ `Int` 型の値を含んでいる、または値が_全く存在しない_ことを表しています。\(`Bool` や `String` など他の型を含めることはできません。`Int` か `nil` です\)

### nil

オプショナルな変数は、特別な値 `nil` を代入することで、値のない状態を設定することができます:

```swift
var serverResponseCode: Int? = 404
// serverResponseCode Int の値の 404 を含んでいます
serverResponseCode = nil
// serverResponseCode は 値を含んでいません
```

> NOTE  
> オプショナルではない定数と変数には `nil` を設定することができません。もし、ある条件で値のない状態を扱う必要がある場合、適切な型のオプショナル値として定義してください

もし、デフォルトで値を与えずにオプショナルな変数を定義した場合、その変数には自動で `nil` が設定されます:

```swift
var surveyAnswer: String?
// surveyAnswer には自動で nil が設定されます
```

> NOTE  
> Swift の `nil` は Objective-C の `nil` と同じではありません。Objective-C では、`nil` は存在しないオブジェクトのポインタです。Swift では、`nil` はポインタではありません。ある型の存在しない値です。オブジェクト型だけではなく、どんな型のオプショナルにも `nil` を設定することができます。

### if 文と強制アンラップ\(If Statements and Forced Unwrapping\)

`if` 文を使用してオプショナル値を `nil` と比較することで、値を含んでいるかどうかのチェックができます。この比較は、等しい\(`==`\)または等しくない\(`!=`\)演算子を使用して行います。

オプショナルが値を含んでいる場合、`nil` と「等しくない」と見なされます。

```swift
if convertedNumber != nil {
    print("convertedNumber contains some integer value.")
}
// "convertedNumber contains some integer value."
```

オプショナルが値を含んでいることが一度分かると、オプショナル値の名前の後ろに `!` を付けることで、中の値に直接アクセスすることができます。`!` は「私はこのオプショナルが値を含んでいることが 100%わかっています。だから使わせてください」ということを実質的に宣言していることになります。これはオプショナルの_強制アンラップ_と呼ばれています。

```swift
if convertedNumber != nil {
    print("convertedNumber has an integer value of \(convertedNumber!).")
}
// "convertedNumber has an integer value of 123."
```

`if` 文についての詳細は、[Control Flow\(制御フロー\)](control-flow.md)を参照ください。

> NOTE  
> 値が存在しないオプショナル値に `!` を付けてアクセスしようとすると、実行時エラーが発生します。`!` を使った強制アンラップを行う際は、確実に `nil` ではないことを常に確かめましょう。

### <a id="optional-binding">オプショナルバインディング\(Optional Binding\)</a>

オプショナル値に、_オプショナルバインディング_を使用して、値を含んでいるかどうかを判定できます。もし含んでいる場合は、一時的な定数や変数として値を使用できるようになります。オプショナルバインディングは、`if` や `while` 文の 1 つのアクションで、オプショナル値に値が存在することを証明し、定数や変数にその内部の値を設定することを、まとめて行うことができます。`if` や `while` の詳細は[Control Flow\(制御フロー\)](control-flow.md)を参照ください。

`if` 文でオプショナルバインディングを行う場合、次のように書きます:

```swift
if let constantName = someOptional {
    statements
}
```

[Optionals\(オプショナル\)](the-basics.md#optionals)の中の例で出てきた `possibleNumber` は、強制アンラップの代わりに、オプショナルバインディングを使用して書き換えることができます。

```swift
if let actualNumber = Int(possibleNumber) {
    print("The string \(possibleNumber) has an integer value of \(actualNumber)")
} else {
    print("The string \(possibleNumber) couldn't be converted to an integer")
}
// "The string "123" has an integer value of 123"
```

このコードはこのような意味に読み取れます。

「`Int(possibleNumber)` が返すオプショナルの `Int` が値を含んでいた場合、`actualNumber` にその値を設定します」

この変換が成功した場合、`actualNumber` 定数は `if` 文の最初の分岐内で使用することができます。オプショナル_内に_ラップされている値で既に初期化は完了しているので、`!` を後ろに付ける必要はありません。この例では、`actualNumber` は変換した結果を出力します。

オプショナルバインディングは定数と変数の両方に使用することができます。`if` 文の最初の分岐内で `actualNumber` を変更したい場合は、`if var actualNumber` と書くことで、定数の代わりに変数としてこのオプショナル値を使用できます。

1 つの `if` 文の中に、複数のオプショナルバインディングとブール値をカンマ\(`,`\)区切りで含めることができます。そのうちのいずれかが `nil` または `false` の場合、`if` 文全体が `false` と判断されます。次の `if` 文はこれに該当します。

```swift
if let firstNumber = Int("4"), let secondNumber = Int("42"), firstNumber < secondNumber && secondNumber < 100 {
    print("\(firstNumber) < \(secondNumber) < 100")
}
// "4 < 42 < 100"

if let firstNumber = Int("4") {
    if let secondNumber = Int("42") {
        if firstNumber < secondNumber && secondNumber < 100 {
            print("\(firstNumber) < \(secondNumber) < 100")
        }
    }
}
// "4 < 42 < 100"
```

> NOTE  
> `if` 文の中でオプショナルバインディングによって作られた定数や変数は、`if` 文の中でしか使えません。もし他でも使用したい場合は、`guard` 文を使用することで、`guard` 文の次から使用することができます。詳細は[Early Exit\(早期リターン\)](control-flow.md#early-exit早期リターン)に記載しています。

### <a id="implicitly-unwrapped-optionals">暗黙アンラップオプショナル\(Implicitly Unwrapped Optionals\)</a>

上記で書いているように、オプショナルは定数や変数に「値が存在しない」可能性があることを示します。オプショナルは値が存在するかどうかを `if` 文の中でチェックでき、存在している場合は、オプショナル内の値にアクセスするために、オプショナルバインディングを使用してアンラップすることができます。

時々、オプショナルに一度値が設定された後は必ず値が存在するということが明らかなこともあります。このような場合、常に値があることはわかっているので、アクセスする度にオプショナル値のチェックとアンラップすることを省略できれば便利です。

このようなオプショナルは、_暗黙アンラップオプショナル_として定義されています。`?` の代わりに `!` を型の後に付けることで、暗黙アンラップオプショナルを書くことができます\(`String?` の代わりに `String!` と書くなど\)。コード内で使用するオプショナル値の後に `!` 付けるよりも、定義した型の後に `!` を付けます。

暗黙アンラップオプショナルは、オプショナル値の定義後すぐに値が設定され、それ以降はずっと値が存在していることが確実な場合に、役に立ちます。Swift での暗黙アンラップオプショナルの主な使われ方としては、クラスの初期化時があります。[Unowned References and Implicitly Unwrapped Optional Properties\(非所有参照と暗黙アンラップしたオプショナルプロパティ\)](automatic-reference-counting.md#unowned-references-and-implicitly-unwrapped-optional-properties非所有参照と暗黙アンラップしたオプショナルプロパティ)に記載しています。

暗黙アンラップオプショナルは、内部的には通常のオプショナルですが、オプショナルではない値のように使用することもできます。次の例は、オプショナルと暗黙アンラップオプショナルで、明示的に `String` を型として記載している値へアクセスするときの動きの違いを表しています。

```swift
let possibleString: String? = "An optional string."
let forcedString: String = possibleString! // ! が必要

let assumedString: String! = "An implicitly unwrapped optional string."
let implicitString: String = assumedString // ! は不要
```

暗黙アンラップオプショナルを必要なときにオプショナル値へ強制アンラップできるようにしていると見なすことができます。暗黙アンラップオプショナル値を使用するとき、Swift は強制アンラップします。上記のコードでは、オプショナル値の `assumedString` は、`implicitString` が明示的にオプショナルではない `String` を宣言しているため、代入される前に強制アンラップされています。下記のコードでは、`optionalString` は明示的に型を宣言していないため、通常はオプショナルになります。

```swift
let optionalString = assumedString
// optionalString は String? 型で assumedString は 強制アンラップする必要はありません
```

暗黙アンラップオプショナルが `nil` の場合に内部の値にアクセスしようとすると、実行時エラーが発生します。これは `!` を付けた通常のオプショナルで値が存在しない場合にアクセスしたときの動きと同じです。

暗黙アンラップオプショナルが `nil` かどうかのチェックは通常のオプショナルと同じ方法でできます。

```swift
if assumedString != nil {
    print(assumedString!)
}
// "An implicitly unwrapped optional string."
```

暗黙アンラップオプショナルはオプショナルバインディングもできます。1 つの文の中で、チェックとアンラップができます。

```swift
if let definiteString = assumedString {
    print(definiteString)
}
// "An implicitly unwrapped optional string."
```

> NOTE  
> 暗黙アンラップオプショナルを、後で `nil` になる可能性のある変数に使わないでください。変数が使用されている間に `nil` チェックが必要な場合は、通常のオプショナルを常に使いましょう

## エラーハンドリング\(Error Handling\)

実行中のエラーに対応するためには、_エラーハンドリング_を行います。

関数の成功や失敗を伝えるために、値の有無を利用するオプショナルと異なり、エラーハンドリングは裏側の失敗の原因を特定でき、必要ならばエラーをプログラムの他の箇所へ伝播させることができます。

関数がエラーに遭遇すると、エラーを_スロー_します。そして、この関数の呼び出し元でエラーを_キャッチ_して、適切に応答することができます。

```swift
func canThrowAnError() throws {
    // この関数はエラーをスローするかもしれません
}
```

エラーは、定義に `throws` キーワードを含めることで、エラーをスローすることを示せます。エラーをスローする関数を呼ぶ場合、式の前に `try` キーワードを付けます。

Swift は `catch` でエラーがキャッチされるまで、現在のスコープを抜けてエラーを自動で伝播します。

```swift
do {
    try canThrowAnError()
    // エラーはスローされませんでした
} catch {
    // エラーがスローされました
}
```

`do` は新しいスコープを生成して、エラーを 1 つ以上の `catch` 句でキャッチすることができます。

これは複数のエラーに応答するためにエラーハンドリングを使った例です:

```swift
func makeASandwich() throws {
    // ...
}

do {
    try makeASandwich()
    eatASandwich()
} catch SandwichError.outOfCleanDishes {
    washDishes()
} catch SandwichError.missingIngredients(let ingredients) {
    buyGroceries(ingredients)
}
```

この例では、`makeASandwich()` 関数は、綺麗な皿を使えない場合や材料が足りない場合、エラーをスローします。`makeASandwich()` はエラーをスローする可能性があるため、この関数の呼び出しは `try` 式で包まれています。`do` 文で関数の呼び出しを包み、スローされたエラーは `catch` 句でキャッチされます。

エラーがスローされない場合、`eatASandwich()` 関数が呼ばれます。エラーがスローされ、それが `SandwichError.outOfCleanDishes`case と合致する場合、`washDishes()` 関数が呼ばれます。`SandwichError.missingIngredients` ケースに合致する場合、`buyGroceries(_:)` 関数が `catch` でキャッチされた `[String]` 値をパラメータに呼び出されます。

エラーのスロー、キャッチそして伝播は、[Error Handling\(エラーハンドリング\)](error-handling.md)でより詳細に書かれています。

## <a id="assertions-and-preconditions">アサーションと事前条件\(Assertions and Preconditions\)</a>

_アサーション_と_事前条件_は実行時のチェックです。他のコードが実行される前に必要不可欠な条件が満たされているかどうかを確かめることができます。アサーションや事前条件のブール値が `true` と評価される場合、コードは通常通り継続します。`false` の場合、現在のプログラムの状態は不正となり、コードの実行は中断し、アプリは終了します。

アサーションと事前条件はコード上での前提となる条件や期待値を表すために使います。アサーションは開発中の間違いや間違った想定を見つけやすくし、事前条件は開発中の問題を検知しやすくします。

実行時の期待値を確認することに加えて、アサーションと事前条件はコード内のドキュメントとしても有用です。[Error Handling\(エラーハンドリング\)](the-basics.md#error-handlingエラーハンドリング)で記載したエラー条件とは異なり、アサーションと事前条件は復帰可能ではなく、期待されたエラーをキャッチする手段として使用することはできません。アサーションや事前条件の失敗は、不正なプログラムの状態を表し、失敗したアサーションをキャッチする方法はありません。

アサーションと事前条件を使用することは、不正な条件を起こさないためのツールとしてコードをデザインするための代用品にはなりません。妥当なデータや状態を強制することで、不正な状態が起きた場合に、予測しやすい状態でアプリ終了させたり、プログラムをデバッグしやすくします。不正な状態が起きた際にすぐに実行を止めることで、不正な状態が与えるダメージを抑えることができます。

アサーションと事前条件の違いは、チェックのタイミングにあります: アサーションはデバッグビルド時にしかチェックをせず、事前条件はデバッグとプロダクションの両方のビルドでチェックされます。プロダクションビルドでは、アサーションは評価されません。つまり、開発時にはアサーションを多用してもプロダクションのパフォーマンスに影響はありません。

### <a id="debugging-with-assertions">アサーションを使ったデバッグ\(Debugging with Assertions\)</a>

Swift の標準ライブラリの [assert\(_:_:file:line:\)](https://developer.apple.com/documentation/swift/1541112-assert)関数を呼ぶことでアサーションを書くことができます。`true` か `false` と評価される式と、`false` だった場合に出力するメッセージを式として渡すことができます。例えば:

```swift
let age = -3
assert(age >= 0, "A person's age can't be less than zero.")
// Thisアサーションfails because -3 isn't >= 0.
```

この例では、`age >= 0`、つまり負の値ではない場合、が `true` でコードは継続して実行されます。負の値の場合、`age >= 0` は `false` となり、アサーションは失敗しアプリは終了します。

アサーションのメッセージは省略することができます。例えば、ただ条件を繰り返している場合などです。

```swift
assert(age >= 0)
```

既にチェック済みの条件に対してアサーションを呼び出したい場合、[assertionFailure\(\_:file:line:\)](https://developer.apple.com/documentation/swift/1539616-assertionfailure)関数を使用して、アサーションが失敗したことを示すことができます。

```swift
if age > 10 {
    print("You can ride the roller-coaster or the ferris wheel.")
} else if age >= 0 {
    print("You can ride the ferris wheel.")
} else {
    assertionFailure("A person's age can't be less than zero.")
}
```

### 事前条件を強制する\(Enforcing Preconditions\)

`false` になる可能性があるものの、コードの実行を継続するためには_必ず_ `true` にならなければならない条件に対しては、事前条件を使いましょう。例えば、subscript が範囲超えエラーを起こしていないかの確認や、適切な値を関数に渡しているかなどには、事前条件を使いましょう。

事前条件は[precondition\(_:_:file:line:\)](https://developer.apple.com/documentation/swift/1540960-precondition)関数を呼ぶことで、事前条件を書けます。`true` か `false` と評価される式と、`false` だった場合に出力するメッセージを式として渡すことができます。例えば:

```swift
// subscript の実装内で
precondition(index > 0, "Index must be greater than zero.")
```

事前条件が失敗したことを示すために、[preconditionFailure\(\_:file:line:\)](https://developer.apple.com/documentation/swift/1539374-preconditionfailure)関数を使用することもできます。例えば、switch 文の中で、本来ならば他のケースで全ての妥当な入力値をカバーできるはずなのに、default のケースに入ってしまうケースなどがあります。

> NOTE もし\(`-Ounchecked`\)モードでコンパイルした場合、事前条件はチェックされません。コンパイラは事前条件を常に `true` とみなしてコードの最適化を行います。一方で、`fatalError(_:file:line:)` 関数は最適化の設定をしても、常に実行を中断します。`fatalError(_:file:line:)` 関数は、試作段階や開発の初期段階で、まだ未実装であることを示すためのスタブとして使用することができます\(`fatalError("Unimplemented")` と書くなど\)。fatal error はコードの最適化がされないため、アサーションと事前条件とは異なり、もしこのスタブメソッドに遭遇した場合は、確実に実行を中断させることができます

