# The Basics

最終更新日: 2020/5/9

Swift は iOS, macOS, watchOS, tyOS アプリ開発のための新しいプログラミング言語です。新しい言語ではあるもの、 Swift の多くの部分は C 言語と Objective-C の開発経験があれば慣れ親しんだ部分もあるかと思います。

Swift は C 言語と Objective-C の全ての基本的な型に対応した Swift 版の型を提供します。 数値に対する`Int`、浮動小数点数に対する`Double`と`Float`、ブール値に対する`Bool`、文字データに対する`String`があります。また、3 つのより強力な collection 型\(`Array`, `Set`, `Dictionary` \)も提供しています。詳細は [Collection Types](collection-types.md)に記載しています。

C 言語のように、Swift は、名前を特定して、値を保持したり、その値を参照するために変数を使います。また、変数の値を変更できなくすることで、より幅広い方法で変数を使うことができます。これらは、定数として知られており、C 言語の定数よりもかなり強力です。定数は、値を変更する必要がない場合に、コードを、意図的に、より安全に、よりわかりやすくするために Swift 全体で使われます。

これまで見たことがあるような型に加え、Swift は Objective-C にはなかった、tuple のようなより応用的な型を導入します。tuple は値を 1 つのグループとして扱うことができます。tuple を使うと、関数から複数の値を 1 つの値の組み合わせにすることができます。

Swift は値が存在しないかもしれない値を扱う optional 型を導入しています。optional は、「値が存在していて、これは x と等しい」もしくは「値は一切存在しない」ということ表します。optional は Objective-C のポインタと`nil`を扱うのに似ていますが、class だけではなく、あらゆる型に使うことができます。Objective-C の`nil`ポインタよりも、安全で表現豊かなだけでなく、Swift の数々の強力な特徴の中心に位置します。

Swift は型安全な言語です。つまり、言語として、コードで扱う値の型を明確にしてくれます。例えば、`String`が必要な場合に、この型安全な特徴が、間違って`Int`を渡してしまうことを防いでくれます。同様に、optional ではない`String`に optional の`String`を誤って渡してしまうことも防いでくれます。型安全なことで、開発中に、素早いエラーの発見と修正をすることができます。

## Constants and Variables

定数と変数は特定の型の値\(`10`、`"Hello"`\)と名前\(`maximumNumberOfLoginAttempts`、`welcomeMessage`\)を関連付けます。定数は一度値を設定すると変更することはできません。一方で、変数は後で異なった値を設定できます。

### Declaring Constants and Variables

定数と変数は、使用する前に定義されていなければなりません。定数は`let`、変数は`var`キーワードで定義します。ここで、ユーザが何回ログインをしようとしたか\(試行回数\)を追跡する定数と変数の例を紹介します。

```swift
let maximumNumberOfLoginAttempts = 10
var currentLoginAttempt = 0
```

このコードは下記のように読み取れます。

「`maximumNumberOfLoginAttempts`という定数を定義して、`10`という値を設定します。次に`currentLoginAttempt`という変数を定義して、`0`という初期値を設定します」

この例では、最大回数は変更しないので定数で定義しています。試行回数はログインを試みる度に増やさなければならないため、変数として今の試行回数を定義しています。

カンマ\(`,`\)区切りで 1 行の中に複数の定数や変数を定義することもできます。

```swift
var x = 0.0, y = 0.0, z = 0.0
```

> NOTE  
> コードで保持している値がこの先変更されない場合は、 `let`を使って定数として定義しましょう。 変数は変更できるようにする必要がある場合にのみ使用しましょう。

### Type Annotations

定数や変数を定義する時に、保持する値の種類をより明確にするために、型アノテーションを使用することもできます。定数や変数の名前の後にコロン\(`:`\)を置いて、型アノテーションを書きましょう。

この例では`welcomeMessage`という変数に型アノテーションを書いて`String`が保持されることを示しています。

```swift
var welcomeMessage: String
```

定義の中のコロンは、「~型の~」を意味します。なので、このコードは下記のように読み取れます。

「`String`型の`welcomeMessage`という変数を定義します」

「`String`型の」というフェーズは、どんな`String`型の値も保持できることを意味します。つまり、保持することができる「物の型\(種類\)」だと考えましょう。

`welcomeMessage`にはエラーなしに文字列を設定できます。

```swift
welcomeMessage = "Hello"
```

1 行の中で、同じ型の複数の変数をカンマで区切って定義することもできます。この際、型アノテーションは最後の変数の後に 1 つ付けます。

```swift
var red, green, blue: Double
```

> NOTE  
> 実際に型アノテーションを書く必要はあまりありません。定義時に定数や変数に初期値を与えた場合、 Swift はたいていそれらの型を推論できます\(詳細は[Type Safety and Type Inference](the-basics.md#type-safety-and-type-inference)\)。  
> 上記の`welcomeMessage`の例では、初期値を与えていないため、推論をすることができないため、`welcomeMessage`変数は型アノテーションで型を特定しています。

### Naming Constants and Variables

定数名と変数名には、Unicode 文字も含めた、ほとんどの文字を含めることができます。

```swift
let π = 3.14159
let 你好 = "你好世界"
let 🐶🐮 = "dogcow"
```

定数名と変数名に、ホワイトスペース、数学記号、矢印、公式ではない Unicode スカラ値、罫線素片罫線素片、書式文字は含められません。また、数字から始めることはできません\(他の位置に数字を含めることはできます\)

一度ある型の定数や変数を定義すると、同じ名前で再定義することはできません。また、異なる型の値を保持することもできません。定数を変数に、変数を定数に変換することもできません。

> NOTE  
> Swift の予約語と同じ名前の定数や変数を使いたい場合、そのキーワードをバッククォート\(\`\`\)で囲みます。 可能ではありますが、予約語を名前に使用することは本当に他に選択肢がない以外は避けましょう。

既存の変数の値を他の互換性のある型の値に変更することはできます。下記の例では、`friendlyWelcome`の値を`"Hello!"`から`"Bonjour!"`に変更しています。

```swift
var friendlyWelcome = "Hello!"
friendlyWelcome = "Bonjour!"
// friendlyWelcome is now "Bonjour!"
```

変数とは異なり、定数の値は最初に設定した後に変更できません。試みても、コンパイル時にエラーになります。

```swift
let languageName = "Swift"
languageName = "Swift++"
// This is a compile-time error: languageName cannot be changed.
```

### Printing Constants and Variables

`print(_:separator:terminator:)`関数を使って、定数や変数の現在の値を出力することができます。

```swift
print(friendlyWelcome)
// Prints "Bonjour!"
```

`print(_:separator:terminator:)`関数はグローバル関数で、1 つ以上の値を適切なアウトプット先に出力します。Xcode では、`print(_:separator:terminator:)`関数を使うと、 Xcode のコンソールパネルへ値を出力します。`separator`と`terminator`引数には、デフォルト値が用意されているので省略可能です。デフォルトでは最後に改行を追加します。改行を付けたくない場合は`terminator`に空文字を渡しましょう。例えば`print(someValue, terminator: "")`。詳細は[Default Parameter Values](functions.md#default-parameter-values)。

Swift は長い文字列の中で定数や変数をプレースホルダとして使用したい場合、 string interpolation\(文字列補間\)を使い、定数や変数の現在値に置き換えるように Swift に伝えることができます。名前を括弧\(\(\)\)で囲み、開始括弧の前にバックスラッシュ\(\\)を付けます。

```swift
print("The current value of friendlyWelcome is \(friendlyWelcome)")
// Prints "The current value of friendlyWelcome is Bonjour!"
```

> NOTE  
> string interpolation\(文字列補間\)で使用できるオブションは[String Interpolation](strings-and-characters.md#string-interpolation)に記載しています

## Comments

コード内に実行されないテキスト\(メモやリマインダーなど\)を含めるためにはコメントを使います。コメントはコンパイル時に Swift のコンパイラからは無視されます。

Swift のコンパイラは C 言語のコメントにとてもよく似ています。1 行のコメントは 2 つのスラッシュ\(//\)で開始します。

```swift
// This is a comment.
```

複数行の場合は、アスタリスク+スラッシュ\(/\*\)で開始し、スラッシュ+アスタリスク\(\*\)で終了します。

```swift
/* This is also a comment
but is written over multiple lines. */
```

C 言語の複数行コメントとは異なり、Swift では他の複数行コメントにネストさせることができます。1 つ目の複数行コメントブロックの中で、2 つ目の複数行ブロックコメントを開始します。2 つ目のブロックは、1 つ目のブロックの前にコメントを閉じます。

```swift
/* This is the start of the first multiline comment.
 /* This is the second, nested multiline comment. */
This is the end of the first multiline comment. */
```

ネストした複数行コメントを使うことで、長い複数行コメントの中でも簡単に素早くその部分だけコメントアウトすることができます。

## Semicolons

他の言語とは異なり、Swift ではコードの 1 つ 1 つの宣言の最後に、セミコロン\(`;`\)を付ける必要がありません\(付けることも可能ではあります\)。一方で、1 行に複数のステートメントを書きたい場合は必要になります。

```swift
let cat = "🐱"; print(cat)
// Prints "🐱"
```

## Integers

_Integers_ は小数点のない整数値を全部含みます\(`42`、`-23`など\)。整数とは _signed_\(正の値、または 0、または負の値\) または _unsigned_\(正の値、または 0\) です。

Swift は signed と unsigned の整数値を 8, 16, 32, 64 ビットの形式で提供します。これらは C 言語の名前に合わせて命名されています。8 ビットの unsigned integer は`UInt8`、32 ビットの signed integer は`Int32`です。Swift の全ての型と同様に、これらの整数値型の頭文字は大文字です。

### Integer Bounds

`min`と`max`プロパティを使って、各整数値型の最小値と最大値にアクセスすることができます。

```swift
let minValue = UInt8.min  // minValue is equal to 0, and is of type UInt8
let maxValue = UInt8.max  // maxValue is equal to 255, and is of type UInt8
```

これらのプロパティの値は、適切なサイズの整数値型で\(上記の例だと\`\`UInt8\)で、同じ型の他の値と一緒に式の中で使用することができます。

### Int

多くの場合、整数値のサイズを特定する必要はありません。Swift はプラットフォームに応じて決まるサイズと同じサイズをもつ`Int`という型を追加で提供しています。

* 32 ビットのプラットフォームの場合、`Int`は`Int32`と等しい
* 64 ビットのプラットフォームの場合、`Int`は`Int64`と等しい

特定のサイズの整数値を扱う必要がない限り常に`Int`を使うようにしましょう、これはコードの一貫性と互換性を保つ手助けとなります。32 ビットのプラットフォームでは、`Int`は`-2,147,483,648`から`2,147,483,647`まで保持することができます。これは整数値が使われる多くの場合において、十分に大きい範囲です。

### UInt

Swift は unsigned の整数値型も提供しています。これも、プラットフォームに応じて決まるサイズと同じサイズを持ちます。

* 32 ビットのプラットフォームの場合、`UInt`は`UInt32`と等しい
* 64 ビットのプラットフォームの場合、`UInt`は`UInt64`と等しい

> NOTE  
> `UInt`は特別にプラットフォームに応じて決まるサイズと同じサイズの unsigned integer 型を扱いたい場合にのみ使用しましょう。そうでない場合、負の値にならないとしても`Int`を使う方が好ましいです。一貫して`Int`を整数値に使用することで、異なる整数値型間の変換を避け、合致する型をコンパイラが推論することで、互換性を保つことができます。詳細は[Type Safety and Type Inference](the-basics.md#type-safety-and-type-inference)。

## Floating-Point Numbers

浮動小数点数は、小数部分を持つ数値です\(`3.14159`, `0.1`, `-273.15`など\)。

浮動小数点数値型は、整数値型よりもより広い範囲の値を表現でき、`Int`より大きい\(またはより小さい\)値を保持できます。Swift は 2 つの signed 浮動小数点数値型を提供しています。

* `Double`は 64 ビットの浮動小数点数を表します
* `Float`は 32 ビットの浮動小数点数を表します

> NOTE  
> `Double`は最小で15桁の精度の小数値を持ち、`Float`は6桁の精度の小数値を持ちます。適切な浮動小数点数値型は、扱いたい数値の特性と範囲によります。どちらでも良い場合は、`Double`が好まれます。

## Type Safety and Type Inference

Swwift は型安全な言語です。型安全な言語は、扱っている値の型を明確にします。`String`が必要な場合に、間違って`Int`を渡すようなことはありません。

型安全なことから、コンパイル時に型チェックが行われ、間違った型に対してはエラーを示します。これによって開発時に素早くエラーに気がつき修正をすることができます。

型チェックによって、異なる型の値を扱う場合に間違った型を代入してしまうエラーを回避することができます。しかし、全ての定数と変数の定義時に、型を特定しなければならないということではありません。特定しない場合、Swift は適切な型への推論を行います。コンパイラが、コンパイル時に与えられた値を調べることで、式の型を自動で推論してくれます。

この型推論のおかげで、Swift は C 言語や Objective-C のような言語と比べて、型を宣言する必要はあまりありません。定数と変数は明示的に型を記載する場合もありますが、型を特定する多くの作業は Swift の側で行なってくれます。

型推論は、特に初期値を伴って変数や定数を宣言する時に役に立ちます。特に、_リテラル\(literal\)_ な値を設定する際によく機能します。\(リテラルな値とは、下記の例にあるようなコードに直接出てくる`42, 3.14159`のような値のことです\)

例えば、`42`を新しい定数に型を記載せずに設定すると、整数値型に見える数値リテラルで初期化していることから、Swift はその型を推論して`Int`と判断します。

```swift
let meaningOfLife = 42
// meaningOfLife is inferred to be of type Int
```

同様に、浮動小数点数を型なしで宣言した場合、Swift は`Double`を定義したと判断します。

```swift
let pi = 3.14159
// pi is inferred to be of type Double
```

Swift は浮動小数点数を推論する際に、常に\(`Float`よりも\)`Double`を選択します。

式の中で整数値と浮動小数点数を組み合わせた場合は、文脈から多くの場合`Double`と推論されます。

```swift
let anotherPi = 3 + 0.14159
// anotherPi is also inferred to be of type Double
```

`3`には明確な型がありませんが、後の浮動小数点数の値から`Double`と推論されます。

## Numeric Literals

数値リテラルは、下記の方法で書くことができます。

* プレフィックスなしの 10 進数
* `0b`をプレフィックスにした 2 進数
* `0o`をプレフィックスにした 8 進数
* `0x`をプレフィックスにした 16 進数

これらの数値リテラルは、`17`という 10 進数を下記のように保持します。

```swift
let decimalInteger = 17
let binaryInteger = 0b10001       // 17 in binary notation
let octalInteger = 0o21           // 17 in octal notation
let hexadecimalInteger = 0x11     // 17 in hexadecimal notation
```

浮動小数点数リテラルは、10 進数と 16 進数にできます。小数点の左と右の両方に必ず整数値が必要です。10 進数の浮動小数点数は、指数を持つ場合があります\(小文字または大文字の`e` \)。16 進数の浮動小数点数は必ず指数を指定しなければなりません\(小文字または大文字の`p` \)。

`e`\(`exp`\)を持つ 10 進数では、元の値に 10 の指数\(`10exp`\)を掛けます。

* `1.25e2`は 1.25 x $10^{2}$ または 125.0 と等しい
* `1.25e-2`は 1.25 x $10^{-2}$ または`0.0125`と等しい

`p`\(`exp`\)を持つ 16 進数では、元の値に 2 の指数\(`2exp`\)を掛けます。

* `0xFp2`は 15 x $2^{2}$ または`60.0`と等しい
* `0xFp-2`は 15 x $2^{-2}$ または`3.75`と等しい

下記の浮動小数点数は、全て`12.1875`という 10 進数を表しています。

```swift
let decimalDouble = 12.1875
let exponentDouble = 1.21875e1
let hexadecimalDouble = 0xC.3p0
```

整数値リテラルは、読みやすくするために追加でフォーマットを含めることができます。整数値や浮動小数点数へ追加の 0 を加えたり、アンダースコア\(\_\)を追加することで可読性を向上させることができます。このフォーマットに整数値自体への影響はありません。

```swift
let paddedDouble = 000123.456
let oneMillion = 1_000_000
let justOverOneMillion = 1_000_000.000_000_1
```

## Numeric Type Conversion

通常整数値を扱う際は、負の値にならないことがわかっていたとしても定数や変数に`Int`型を使いましょう。全ての状況でデフォルトに`Int`を使うことで、整数値の定数と変数を特別な手順なしに互換可能にし、数値リテラルから推論される型とも合致します。

特別な理由ある場合のみ、他の整数値型を使いましょう。例えば、外部リソースから特定サイズの型が指定されている場合やパフォーマンス、メモリの使用量や他の最適化が必要な場合など、がこれに当たります。明示的にサイズを指定することで思わぬオーバーフローを起こしたり、暗黙的にそのデータの特性を表現することができます。

### Integer Conversion

整数値の定数や変数に保持できる範囲は、それぞれの整数値型によって異なります。`Int8`の定数や変数は、−128 から 127 まで保持できます。`UInt8`の定数や変数は、0 から 255 まで保持できます。指定したサイズの整数値型の定数や変数に合わない値を設定しようとするとエラーになります。

```swift
let cannotBeNegative: UInt8 = -1
// UInt8 can't store negative numbers, and so this will report an error
let tooBig: Int8 = Int8.max + 1
// Int8 can't store a number larger than its maximum value,
// and so this will also report an error
```

このように、整数値型によって保持できる範囲が異なるため、ケースごとに型の変換をしなければなりません。この明示的に指定する方法によって、隠れた変換ミスを防ぎ、コードで型変換が起こっていることを明確に表現することができます。

ある特定の整数値型を他の型に変換するためには、既存の整数値から変換したい型の新しい値を生成する必要があります。下記の例では、定数`twoThousand`は`UInt16`ですが、定数の`one`は`UInt8`です。この 2 つは同じ型ではないので直接足し算をすることができません。そこで、この例では、`one`を使って`UInt16`の新しい値を作るために`UInt16(one)`を呼び、元の例に置き換えて使います:

```swift
let twoThousand: UInt16 = 2_000
let one: UInt8 = 1
let twoThousandAndOne = twoThousand + UInt16(one)
```

今はどちらも`UInt16`なので、足し算は可能です。そして計算結果の定数\(`twoThousandAndOne`\)も`UInt16`に推論されます。

`SomeType(ofInitialValue)`という形式は、初期値を渡して初期化を行う Swift のデフォルトの方法です。裏側では、`UInt16`型が`UInt8`の値を受け取って、新しい`UInt16`型の値を生成しています。とはいっても、あらゆる型を渡せるわけではありません。`UInt16`型提供するイニシャライザに合った型が必要です。新しい型を渡して初期化する方法は、[Extensions](extensions.md)に記載しています。

### Integer and Floating-Point Conversion

整数値と浮動小数点数の変換は明示的に行わなければなりません:

```swift
let three = 3
let pointOneFourOneFiveNine = 0.14159
let pi = Double(three) + pointOneFourOneFiveNine
// pi equals 3.14159, and is inferred to be of type Double
```

ここに、定数`3`の値が`Double`型の新しい値を作って使われています。そのため、両方の型が同じになり、足し算が可能です。変換をしない場合、足し算はできません。

整数値から浮動小数点数への変換は明示的に行わなければなりません。整数値は`Double`や`Float`で初期化できます。

```swift
let integerPi = Int(pi)
// integerPi equals 3, and is inferred to be of type Int
```

浮動小数点数の値は新しい整数値として初期化される場合、必ず小数点は切り捨てられます。つまり、`4.75`は`4`、`-3.9`は`-3`になります。

> NOTE  
> 整数値の定数と変数を組み合わせるルールは、数値リテラルのルールとは異なります。リテラル値の`3`はリテラル値の`0.14159`とそのまま加算できます。リテラル値は明示的な型を有していないため、コンパイラがその値を評価する時に型を推論します。

## Type Aliases

_Type Aliases\(タイプエイリアス\)_ は既存の型に別の名前を定義します。`typealias`キーワードを使います。

タイプエイリアスは、既存の型を文脈に沿ったより適切な名前で参照したい場合に有効です。例えば、外部リソースの特定のサイズのデータを扱いたい場合など:

```swift
typealias AudioSample = UInt16
```

定義したタイプエイリアスは、元の名前で使っていた所にも使うことができます:

```swift
var maxAmplitudeFound = AudioSample.min
// maxAmplitudeFound is now 0
```

ここでは、`AudioSample`が`UInt16`のタイプエイリアスとして定義されています。エイリアスなので、`AudioSample.min`の実態は`UInt16.min`で、`maxAmplitudeFound`の初期値は`0`になります。

## Booleans

Swift は、`Bool`と呼ばれる基本的なブール値を持っています。ブール値は、真\(true\)か偽\(false\)のみを値として取ることから、_論理値\(logical\)_として参照されます。Swift では`true`と`false`の 2 つのブール値の定数を提供しています。

```swift
let orangesAreOrange = true
let turnipsAreDelicious = false
```

`orangesAreOrange`と`turnipsAreDelicious`は、ブール値リテラルで初期化されていることから`Bool`と推論されます。これまで見てきた`Int`や`Double`のように、`true`や`false`を値に設定すれば、明示的に`Bool`と指定する必要はありません。既に型がわかっている他の定数や変数を使って初期化する際に、Swift の型推論を活用することで、より簡潔で読みやすいコードを書くことができます。

ブール値は、条件文を扱う際にとても有効です。例えば、`if`文など:

```swift
if turnipsAreDelicious {
    print("Mmm, tasty turnips!")
} else {
    print("Eww, turnips are horrible.")
}
// Prints "Eww, turnips are horrible."
```

`if`のような条件文については、[Control Flow](control-flow.md)でより詳細に記載しています。

Swift は型安全なので、`Bool`の値を他の型に置き換えることはできません。次の例はコンパイルエラーになります:

```swift
let i = 1
if i {
    // this example will not compile, and will report an error
}
```

一方で、次の例は問題ありません:

```swift
let i = 1
if i == 1 {
    // this example will compile successfully
}
```

`i == 1`の比較結果は`Bool`なので、型チェックを通過できます。`i == 1`のような比較については、[Basic Operators](basic-operators.md)でより詳しく記載しています。

他の型安全性を示した例と同様に、型の違いによる予期せぬエラーの発生を防ぎ、型が明確なので、コードの意図をより明確に表すことができます。

## Tuples

_タプル\(Tuples\)_は、複数の値を 1 つのまとまりにグループ化します。タプル内の値にはどんな型も入れることができ、全ての型を同じにする必要はありません。

下記の例の`(404, "Not Found")`は、 HTTP ステータスコードを表したタプルです。HTTP ステータスコードは、Web ページを取得するリクエストを送ると Web サーバから返ってくる特別な値です。`404 Not Found`のステータスコードは、リクエストした Web ページが存在しなかった場合に返ってきます。

```swift
let http404Error = (404, "Not Found")
// http404Error is of type (Int, String), and equals (404, "Not Found")
```

`(404, "Not Found")`タプルは、HTTP ステータスコードを 2 つの値:\(数値と人が理解できる説明文\)に分けた`Int`と`String`を 1 つのグループにまとめています。これは「`(Int, String)`型のタプル」と説明できます。

タプルは、任意の順序で、異なる任意の型を組み合わせることができます。例えば、`(Int, Int, Int)`や`(String, Bool)`型のタプルも作ることができますし、必要に応じて順番の入れ替えも可能です。

タプルの個々の内容をそれぞれ定数や変数に分けて扱うこともでき、他の値と同じようにアクセスすることができます:

```swift
let (statusCode, statusMessage) = http404Error
print("The status code is \(statusCode)")
// Prints "The status code is 404"
print("The status message is \(statusMessage)")
// Prints "The status message is Not Found"
```

もしタプルの一部だけが必要な場合、タプルを分解する時に、アンダースコア\(\_\)を使って無視することができます。

```swift
let (justTheStatusCode, _) = http404Error
print("The status code is \(justTheStatusCode)")
// Prints "The status code is 404"
```

各値へのアクセス方法としては、0 から始まるインデックスを使うこともできます:

```swift
print("The status code is \(http404Error.0)")
// Prints "The status code is 404"
print("The status message is \(http404Error.1)")
// Prints "The status message is Not Found"
```

タプルの定義時に、名前を付けることもできます:

```swift
let http200Status = (statusCode: 200, description: "OK")
```

名前を付けた場合、その名前を使って各値へアクセスすることができます:

```swift
print("The status code is \(http200Status.statusCode)")
// Prints "The status code is 200"
print("The status message is \(http200Status.description)")
// Prints "The status message is OK"
```

タプルは、特に関数の戻り値で有効に活用できます。Web ページを取得する関数は、取得の成否の結果を`(Int, String)`で返すかもしれません。2 つの異なる型の値を持ったタプルを返すことで、1 つの型の 1 つの値を返すよりも、関数はより有益な情報を提供できます。より詳しくは、[Functions with Multiple Return Values](functions.md#functions-with-multiple-return-values)を参照ください。

## Optionals

_optionals_は、値が存在しないかもしれない時に使用します。optional は 2 つの可能性を表します: 値が存在して unwrap することで値にアクセスすることができる、もしくは、値が全く存在しない

> NOTE  
> optional の概念は、 C 言語や Objective-C には存在しません。 Objective-C は、メソッドから`nil`かオブジェクトを返すことができますが、ここでいう`nil`は妥当なオブジェクトが存在しない、ということを意味します。しかし、これはオブジェクトのみで機能し、構造体や基本的な C 言語の型、列挙型には使用できません。これらの型へは、値が存在しないことを示す特別な値\(`NSNotFound`など\)を返すのが通例です。この方法ですと、メソッドの呼び出し側が、この特別な値が返ってくることを前提としてチェックすることを想定しています。Swift では、この特別な値を必要とせず、どんな型に対しても値が存在しないことを示すことができます。

下記に optional がどのように値が存在しないことを表すかの例を示します。Switf の`Int`型には`String`を`Int`へと変換するイニシャライザがあります。しかし、全ての文字列が`Int`に変換できるわけではありません。「`123`」という文字列は数値`123`に変換できますが、「`hello world`」は変換することができません。

下記の例は`String`を`Int`へ変換するイニシャライザの例です。

```swift
let possibleNumber = "123"
let convertedNumber = Int(possibleNumber)
// convertedNumber is inferred to be of type "Int?", or "optional Int"
```

イニシャライザは失敗するかもしれないので、`Int`ではなく、optional の`Int`を返します。optional の`Int`は、`Int`ではなく`Int?`と書きます。`?`は optional を示し、「ある`Int`型の値を含んでいる」ことを表しています。\(`Bool`や`String`など他の型を含めることはできません。`Int`、または値を持たない\(`nil`\)の 2 つのみ可能です\)

### nil

optional な変数は、特別な値`nil`を設定することで、値のない状態を設定することができます:

```swift
var serverResponseCode: Int? = 404
// serverResponseCode contains an actual Int value of 404
serverResponseCode = nil
// serverResponseCode now contains no value
```

> NOTE  
> optional ではない定数と変数には`nil`を設定することができません。もし、ある条件で値のない状態を扱う必要がある場合、適切な型の optional 値として定義しましょう

もし、デフォルトで値を与えずに optional な変数を定義した場合、その変数には自動で`nil`が設定されます:

```swift
var surveyAnswer: String?
// surveyAnswer is automatically set to nil
```

> NOTE  
> Swift の`nil`は Objective-C の`nil`と同じではありません。Objective-C では、`nil`は存在しないオブジェクトのポインタです。Swift では、`nil`はポインタではありません。ある型の存在しない値です。オブジェクト型だけではなく、どんな型の optional にも`nil`を設定することができます。

### If Statements and Forced Unwrapping

`if`文を使って optional 値を`nil`と比較することで、値を含んでいるかどうかのチェックができます。この比較は、等しい\(`==`\)または等しくない\(`!=`\)演算子を使って行います。

optional が値を含んでいる場合、`nil`と「等しくない」と見なされます。

```swift
if convertedNumber != nil {
    print("convertedNumber contains some integer value.")
}
// Prints "convertedNumber contains some integer value."
```

optional が値を含んでいることが一度分かると、optional 値の名前の後ろに`!`を付けることで、中の値に直接アクセスすることができます。`!`は「私はこの optional が値を含んでいることが 100%わかっています。だから使わせてください」ということを実質的に宣言していることになります。これは optional の強制アンラップ\(`forced unwrapping`\)と呼ばれています。

```swift
if convertedNumber != nil {
    print("convertedNumber has an integer value of \(convertedNumber!).")
}
// Prints "convertedNumber has an integer value of 123."
```

`if`文についての詳細は、[Control Flow](control-flow.md)を参照ください。

> NOTE  
> 値が存在しない optional 値に`!`と付けてアクセスしようとすると、実行時エラー\(runtime error\)が発生します。`!`を使った強制アンラップを行う際は、確実に non-`nil`であることを常に確かめましょう。

### Optional Binding

optional 値に、オプショナルバインディング\(_optional binding_\)を使って、値を含んでいるかどうかを判定できます。もし含んでいる場合は、一時的な定数や変数として値を使用できるようになります。オプショナルバインディングは、`if`や`while`文で optional 値に値が存在することを証明し、定数や変数にその内部の値を設定することを、1 つのアクションで行うことができます。`if`や`while`の詳細は[Control Flow](control-flow.md)を参照ください。

`if`文でオプショナルバインディングを行う場合、次のように書きます:

```swift
if let constantName = someOptional {
    statements
}
```

[Optionals](the-basics.md#optionals)の中の例で出てきた`possibleNumber`は、強制アンラップ\(`forced unwrapping`\)の代わりに、オプショナルバインディングを使って書き換えることができます。

```swift
if let actualNumber = Int(possibleNumber) {
    print("The string \"\(possibleNumber)\" has an integer value of \(actualNumber)")
} else {
    print("The string \"\(possibleNumber)\" couldn't be converted to an integer")
}
// Prints "The string "123" has an integer value of 123"
```

このコードはこのような意味に読み取れます。

「`Int(possibleNumber)`が返す optional`Int`が値を含んでいた場合、`actualNumber`にその値を設定する」

この変換が成功した場合、`actualNumber`定数は`if`文の最初の分岐内で使うことができます。optional の中で既に初期化は完了しているので、`!`を後ろに付ける必要はありません。この例では、`actualNumber`は変換した結果を出力します。

オプショナルバインディングは定数と変数の両方に使うことができます。`if`文の最初の分岐内で`actualNumber`したい場合は、`if var actualNumber`と書くことで、定数の代わりに変数としてこの optional 値を使用できます。

1 つの`if`文の中に、複数のオプショナルバインディングとブール値をカンマ\(`,`\)区切りで含めることができます。そのうちのいずれかが`nil`または`false`の場合、`if`文全体が`false`と判断されます。次の`if`文はこれに該当します。

```swift
if let firstNumber = Int("4"), let secondNumber = Int("42"), firstNumber < secondNumber && secondNumber < 100 {
    print("\(firstNumber) < \(secondNumber) < 100")
}
// Prints "4 < 42 < 100"

if let firstNumber = Int("4") {
    if let secondNumber = Int("42") {
        if firstNumber < secondNumber && secondNumber < 100 {
            print("\(firstNumber) < \(secondNumber) < 100")
        }
    }
}
// Prints "4 < 42 < 100"
```

> NOTE  
> `if`文の中でオプショナルバインディングによって作られた定数や変数は、`if`文の中でしか使えません。もし他でも使用したい場合は、`guard`文 を使うことで、`guard`文の次から使うことができます。詳細は[Early Exit](control-flow.md#early-exit)に記載しています。

### Implicitly Unwrapped Optionals

上記で書いているように、optional は定数や変数に「値が存在しない可能性がある」ことを示します。optional は値が存在するかどうかを`if`文の中でチェックでき、存在している場合は、optional 内の値にアクセスするために、オプショナルバインディング\(Optional Binding\)を使ってアンラップすることができます。

時々、optional に一度値が設定された後は必ず値が存在するということが明らかなこともあります。このような場合、常に値があることはわかっているので、アクセスする度に optional 値のチェックとアンラップすることを省略できれば便利です。

このような optional は、暗黙アンラップ optional\(implicitly unwrapped optionals\)として定義されます。`?`の代わりに`!`を型の後に付けることで、暗黙アンラップ optional を書くことができます\(`String?`の代わりに`String!`と書くなど\)。コード内で使用する optional 値の後に`!`付けるよりも、定義した型の後に`!`を付けます。

暗黙アンラップ optional は、optional 値が定義後すぐに値が設定され、それ以降はずっと値が存在していることが確実な場合に、役に立ちます。Swift での暗黙アンラップ optional の主な使われ方としては、`class`の初期化時があります。[Unowned References and Implicitly Unwrapped Optional Properties](automatic-reference-counting.md#unowned-references-and-implicitly-unwrapped-optional-properties)で記載しています。

暗黙アンラップ optional は、内部的には通常の optional ですが、optional ではない値のように使うこともできます。次の例は、optional と暗黙アンラップ optional で、明示的に`String`を型として記載している値へアクセスする時の動きの違いを表しています。

```swift
let possibleString: String? = "An optional string."
let forcedString: String = possibleString! // requires an exclamation point

let assumedString: String! = "An implicitly unwrapped optional string."
let implicitString: String = assumedString // no need for an exclamation point
```

暗黙アンラップ optional を必要な時に optional 値へ強制アンラップできるようにしていると見なすことができます。暗黙アンラップ optional 値を使う時、Swift は強制アンラップします。上記のコードでは、optional 値の`assumedString`は、`implicitString`が明示的に optional ではない`String`を宣言しているため、設定される前に強制アンラップされています。下記のコードでは、`optionalString`は明示的に型を宣言していないため、通常は optional になります。

```swift
let optionalString = assumedString
// The type of optionalString is "String?" and assumedString isn't force-unwrapped.
```

暗黙アンラップ optional が`nil`の場合に内部の値にアクセスしようとすると、実行時エラー\(runtime error\)が発生します。これはに`!`を付けた通常の optional で値が存在しない場合にアクセスした時の動きと同じです。

暗黙アンラップ optional が`nil`かどうかのチェックは通常の optional と同じ方法でできます。

```swift
if assumedString != nil {
    print(assumedString!)
}
// Prints "An implicitly unwrapped optional string."
```

暗黙アンラップ optional はオプショナルバインディングもできます。1 つの文の中で、チェックとアンラップができます。

```swift
if let definiteString = assumedString {
    print(definiteString)
}
// Prints "An implicitly unwrapped optional string."
```

> NOTE  
> 暗黙アンラップ optionalを、後でnilになる可能性のある変数に使わないでください。変数が使用されている間に`nil`チェックが必要な場合は、通常の optional を常に使いましょう

## Error Handling

実行中のエラーに対応するためには、エラーハンドリング\(_error handling_\)を使います。

関数の成功失敗を伝えるために値の有無を利用する optional と異なり、エラーハンドリングは背後にある失敗の原因を特定でき、必要ならばエラーをプログラムの他の箇所へ伝播させることができます。

関数がエラーに遭遇すると、エラーをスロー\(_throw_\)します。そして、この関数の呼び出し側でエラーを捕捉\(_catch_\)して、適切に応答することができます。

```swift
func canThrowAnError() throws {
    // this function may or may not throw an error
}
```

エラーは、定義に`throws`キーワードを含めることで、エラーをスローすることを示せます。エラーをスローする関数を呼ぶ場合、式の前に`try`キーワードを付けます。

Swift は`catch`でエラーが捕捉されるまで、現在のスコープを抜けてエラーを自動で伝播します。

```swift
do {
    try canThrowAnError()
    // no error was thrown
} catch {
    // an error was thrown
}
```

`do`は新しいスコープを生成して、エラーを 1 つ以上の`catch`句で捕捉することができます。

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

この例では、`makeASandwich()`関数は、綺麗な皿を使えない場合や材料が足りない場合、エラーを投げます。`makeASandwich()`はエラーをスローする可能性があるため、この関数の呼び出しは`try`式で包まれています。`do`文で関数の呼び出しを包み、スローされたエラーは`catch`句で捕捉されます。

エラーが throw されない場合、`eatASandwich()`関数が呼ばれます。エラーがスローされ、それが`SandwichError.outOfCleanDishes`case と合致する場合、`washDishes()`関数が呼ばれます。`SandwichError.missingIngredients`case に合致する場合、`buyGroceries(_:)`関数が`catch`で捕捉された`[String]`値を引数に呼び出されます。

エラーのスロー\(Throwing\), 捕捉\(catching\) そして伝播\(propagating\) は、[Error Handling](error-handling.md)でより詳細に書かれています。

## Assertions and Preconditions

_Assertions_と_Preconditions_は実行時のチェックです。他のコードが実行される前に必要不可欠な条件が満たされているかどうかを確かめることができます。assertion や precondition のブール値が`true`と評価される場合、コードは通常通りの継続します。`false`の場合、現在のプログラムの状態は不正となり、コードの実行は中断し、アプリは終了します。

assertion と precondition はコード上での前提条件や期待値を表すために使います。assertion は開発中の間違いや間違った想定を見つけやすくし、precondition は開発中の問題を検知しやすくします。

実行時の期待値を確認することに加えて、assertion と precondition はコード内のドキュメントとしても有用です。[Error Handling](the-basics.md#error-handling)で話したエラー条件とは異なり、assertion と precondition は復帰可能ではなく、期待されたエラーを捕捉する手段としても使うことはできません。assertion や precondition の失敗は、不正なプログラムの状態を表し、失敗した assertion を catch する方法はありません。

assertion と precondition を使うことは、不正な条件を起こさないためのツールとしてコードをデザインするための代用品にはなりません。妥当なデータや状態を強制することで、不正な状態が起きた場合に、予測しやすい状態でアプリ終了させたり、プログラムをデバッグしやすくします。不正な状態が起きた際にすぐに実行を止めることで、不正な状態が与えるダメージを抑えることができます。

assertion と precondition の違いは、チェックのタイミングにあります: assertion はデバッグビルド時にしかチェックをせず、precondition はデバッグとプロダクションの両方のビルドでチェックされます。プロダクションビルドでは、assertion は評価されません。つまり、開発時には assertion を多用してもプロダクションのパフォーマンスに影響はありません。

### Debugging with Assertions

Swift の標準ライブラリ\(standard library\) [assert\(_:_:file:line:\)](https://developer.apple.com/documentation/swift/1541112-assert)関数を呼ぶことで assertion を書くことができます。`true`か`false`と評価される式と、`false`だった場合に出力するメッセージを引数として渡すことができます。例えば:

```swift
let age = -3
assert(age >= 0, "A person's age can't be less than zero.")
// This assertion fails because -3 isn't >= 0.
```

この例では、`age >= 0`、つまり負の値ではない場合、が`true`でコードは継続して実行されます。負の値の場合、`age >= 0`は`false`となり、assertion は失敗しアプリは終了します。

assertion のメッセージは省略することができます。例えば、ただ条件を繰り返している場合などです。

```swift
assert(age >= 0)
```

既にチェック済みの条件に対して assertion を呼び出したい場合、[assertionFailure\(\_:file:line:\)](https://developer.apple.com/documentation/swift/1539616-assertionfailure)関数を使って、assertion が失敗したことを示すことができます。

```swift
if age > 10 {
    print("You can ride the roller-coaster or the ferris wheel.")
} else if age >= 0 {
    print("You can ride the ferris wheel.")
} else {
    assertionFailure("A person's age can't be less than zero.")
}
```

### Enforcing Preconditions

`false`になる可能性があるものの、コードの実行を継続するためには必ず true にならなければならない条件に対しては、 precondition を使いましょう。例えば、subscript が範囲超え\(out of bounds\)を起こしていないかの確認や、適切な値を関数の引数に渡しているかなどには、precondition を使いましょう。

precondition は[precondition\(_:_:file:line:\)](https://developer.apple.com/documentation/swift/1540960-precondition)関数を呼ぶことで、precondition を書けます。`true`か`false`と評価される式と、`false`だった場合に出力するメッセージを引数として渡すことができます。例えば:

```swift
// In the implementation of a subscript...
precondition(index > 0, "Index must be greater than zero.")
```

precondition が失敗したことを示すために、[preconditionFailure\(\_:file:line:\)](https://developer.apple.com/documentation/swift/1539374-preconditionfailure)関数を使うこともできます。例えば、switch 文の中で、本来ならば他の case で全ての妥当な入力値をカバーできるはずなのに、defalt の case の条件に当てはまるケースがあった場合などがあります。

> NOTE もし\(`-Ounchecked`\)モードでコンパイルした場合、precondition はチェックされません。コンパイラはprecondition を常に`true`と見なしてコードの最適化を行います。一方で、`fatalError(_:file:line:)`関数は最適化の設定をしても、常に実行を中断します。  
> `fatalError(_:file:line:)`関数は、試作段階や開発の初期段階で、まだ未実装であることを示すためのスタブとして使うことができます\(`fatalError("Unimplemented")`と書くなど\)。fatal error はコードの最適化がされないため、assertion と precondition とは異なり、もしこのスタブメソッドに遭遇した場合は、確実に実行が中断させることができます

