# The Basics

最終更新日: 2020/5/9

Swift は iOS, macOS, watchOS, tyOS のアプリ開発のための新しいプログラミング言語です。一方で、 Swift の多くの部分は C 言語と Objective-C の開発経験から馴染みのある感じを受けるでしょう。

Swift は C 言語と Objective-C の全ての基本的な型に対応した Swift バージョンの型を提供します。 数値に対する `Int`、浮動小数点数に対する `Double` と `Float`、Boolean に対する `Bool`、text データに対する `String`があります。また、3 つのより強力な collection 型(`Array`, `Set`, `Dictionary` )も提供します。詳細は [Collection Types](./language-guide/../collection-types.md)に記載。

C 言語のように、Swift は名前を特定することで、値を保持したり、その値を参照するために変数を使います。また、Swift は変数の値を変更できなくすることで、より幅広い方法で変数を利用しています。これらは定数として知られており、C 言語の定数よりもかなり強力です。定数は、値を変更する必要がない場合に、定数を利用することで、コードを意図的に、より安全に、よりわかりやすくするために Swift 全体で利用されます。

馴染みのある型に加え、Swift は Objective-C にはなかった、tuple のようなより応用的な型を導入します。tuple は値を 1 つのグループとして扱うことができます。tuple を使うと、関数から複数の値を 1 つの値の組み合わせとして返すことができます。

Swift は値が存在しないかもしれない値を扱う optional 型を導入します。optional は、「値が存在していて、これは x と等しい」もしくは「値は一切存在しない」ということを伝えます。optional は Objective-C のポインタと `nil` を扱うのに似ていますが、class だけではなく、あらゆる型に利用することができます。Objective-C の `nil` ポインタよりも、安全で表現的なだけでなく、多くの Swift の最も強力な特徴のコアな部分になります。

Swift は型安全な言語です。つまり、言語がコードで扱う値の型を明確にしてくれます。`String` が必要な場合、この型安全な特徴が、間違って `Int` を渡してしまうことを防いでくれます。同様に、型安全なことで、optional ではない `String` に optional の `String` を気がつかずに渡してしまうことも防いでくれます。型安全なことで、開発プロセスの中でできる限り早くエラーに気がついて、修正する手助けをしてくれます。

## Constants and Variables

定数と変数は特定の型の値(`10`、`"Hello"`など)と名前(`maximumNumberOfLoginAttempts`、`welcomeMessage`など)を関連付けます。定数は一度値を設定すると変更することはできません。一方で変数は先々に異なった値を設定できます。

### Declaring Constants and Variables

定数と変数は、使用する前に定義されていなければなりません。定数は `let`、変数は `var` キーワードで定義します。ここにユーザが何回ログインをしようとしたか(試行回数)を追跡する定数と変数の例を紹介します。

```swift
let maximumNumberOfLoginAttempts = 10
var currentLoginAttempt = 0
```

このコードは下記のように読み取れます。

「`maximumNumberOfLoginAttempts`と呼ばれる定数を定義して、`10`という値を設定します。次に`currentLoginAttempt`という変数を定義して、`0`という初期値を設定する」

この例では、最大回数は変更しないので、定数で定義しています。試行回数はログインを試みる度に増加させなければならないため、変数として今の試行回数を定義しています。

1 行の中で、カンマで区切って複数の定数や変数を定義することもできます。

```swift
var x = 0.0, y = 0.0, z = 0.0
```

> NOTE  
> コードで保持している値がこの先変更されない場合は、 `let` を使って定数として定義しましょう。
> 変数は変更できるようにする必要がある場合にのみ使用しましょう。

### Type Annotations

定数や変数を定義する時に、保持する値の種類をより明確にするために、型アノテーションを使用することもできます。定数や変数の名前の後にコロンを置いて、型アノテーションを書きましょう。

この例では `welcomeMessage` という変数に型アノテーションを書いて `String` が保持されることを示しています。

```swift
var welcomeMessage: String
```

定義の中のコロンは、「~型の~」を意味します。なので、このコードは下記のように読み取れます。

「`String`型の`welcomeMessage`という変数を定義する」

「`String`型の」というフェーズは、どんな`String`型の値も保持できることを意味します。つまり、保持することができる「物の型(物の種類)」だと考えましょう。

`welcomeMessage`にはエラーなしに文字列を設定できます。

```swift
welcomeMessage = "Hello"
```

1 行の中で、同じ型の複数の変数をカンマで区切って定義することもできます。この際、型アノテーションは最後の変数の後に 1 つ付けます。

```swift
var red, green, blue: Double
```

> NOTE  
> 実際に型アノテーションを書く必要はあまりありません。定義時に定数や変数に初期値を与えた場合、
> Swift はたいていそれらの型を推論できます(詳細は[Type Safety and Type Inference](#type-safety-and-type-inference))。
> 上記の`welcomeMessage`の例では、初期値を与えていないため、推論をすることができないため、`welcomeMessage`変数は型アノテーションで型を特定しています。

### Naming Constants and Variables

定数名と変数名には、Unicode 文字も含めた、ほとんどの文字を含めることができます。

```swift
let π = 3.14159
let 你好 = "你好世界"
let 🐶🐮 = "dogcow"
```

定数名と変数名に、ホワイトスペース、数学記号、矢印、公式ではない Unicode スカラ値、罫線素片罫線素片、書式文字は含められません。また、数字から始めることはできません(他の位置に数字を含めることはできます)

一度特定の型で定数や変数を定義すると、同じ名前で再定義することはできません。また、異なる型の値を保持することもできません。定数を変数に、変数を定数に変換することもできません。

> NOTE  
> Swift の予約語と同じ名前で定数や変数を使いたい場合、そのキーワードをバッククォート(``)で囲みます。
> とはいうものの、予約語を名前に使用することは、本当に他に選択肢がない以外は避けましょう。

既存の変数の値を他の互換性のある型の値に変更することができます。この例では、`friendlyWelcome`の値を`"Hello!"`から`"Bonjour!"`に変更しています。

```swift
var friendlyWelcome = "Hello!"
friendlyWelcome = "Bonjour!"
// friendlyWelcome is now "Bonjour!"
```

変数とは異なり、定数の値は最初に設定した後に変更できません。試みようとしても、コンパイル時にエラーになります。

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

`print(_:separator:terminator:)`関数はグローバル関数で、1 つ以上の値を適切なアウトプット先に出力します。Xcode では、`print(_:separator:terminator:)`関数を使うと、
Xcode のコンソールパネルへ値を出力します。`separator` と `terminator`引数には、デフォルト値が用意されているので省略可能です。デフォルトでは、最後に改行を追加します。改行を付けたくない場合は、`terminator`に空文字を渡しましょう。例えば`print(someValue, terminator: "")`。詳細は[Default Parameter Values](./functions.md#default-parameter-values)。

Swift はより長い文字列の中で定数や変数をプレースホルダとして使用したい場合、 string interpolation\(文字列補間\)を使い、定数や変数の現在値に置き換えるように Swift に伝えることができます。名前を括弧(())で囲み、開始括弧の前にバックスラッシュ(\\)を付けます。

```swift
print("The current value of friendlyWelcome is \(friendlyWelcome)")
// Prints "The current value of friendlyWelcome is Bonjour!"
```

> NOTE  
> string interpolation\(文字列補間\)で使用できるオブションは[String Interpolation](./strings-and-characters.md#string-interpolation)に記載しています

## Comments

コード内に実行されないテキスト(メモやリマインダーなど)を含めるためにはコメントを使います。コメントはコンパイル時に Swift のコンパイラからは無視されます。

Swift のコンパイラは C 言語のコメントにとてもよく似ています。1 行のコメントは 2 つのスラッシュ(//)で開始します。

```swift
// This is a comment.
```

複数行の場合は、アスタリスク+スラッシュ(/\*)で開始し、スラッシュ+アスタリスク(\*)で終了します。

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

他の言語とは異なり、Swift ではコードの 1 つ 1 つのステートメントの最後にセミコロンを付ける必要がありません。(付けることも可能ではあります)一方で、1 行に複数のステートメントを書きたい場合は必要になります。

```swift
let cat = "🐱"; print(cat)
// Prints "🐱"
```

## Integers

*Integers* は小数点のない整数値全部を含みます(`42`、`-23`など)。Integers は *signed*(正の値、または 0、または負の値) または *unsigned*(正の値、または 0) です。

Swift は signed と unsigned の整数値を 8, 16, 32, 64 ビットの形式で提供します。これらは C 言語の名前に合わせて命名されています。8 ビットの unsigned integer は`UInt8`、32 ビットの signed integer は`Int32`です。Swift の全ての型と同様に、これらの整数値型は頭文字は大文字です。

### Integer Bounds

`min`と`max`プロパティを使って、各整数値型の最小値と最大値にアクセスすることができます。

```swift
let minValue = UInt8.min  // minValue is equal to 0, and is of type UInt8
let maxValue = UInt8.max  // maxValue is equal to 255, and is of type UInt8
```

これらのプロパティの値は、適切なサイズの整数値型で(上記の例だと``UInt8)で、同じ型の他の値と一緒に式の中で使用することができます。

### Int

多くの場合、整数値のサイズを特定する必要はありません。Swift は現在のプラットフォーム由来のサイズと同じサイズをもつ`Int` という追加の型を提供しています。

* 32 ビットのプラットフォームの場合、`Int` は `Int32` と等しい
* 64 ビットのプラットフォームの場合、`Int` は `Int64` と等しい

特定のサイズの整数値を扱う必要がない限り、常に `Int` を使うようにしましょう、これはコードの一貫性と互換性を保つ手助けとなります。32 ビットのプラットフォームでも``Int` は `-2,147,483,648`から`2,147,483,647`まで保持することができます。これは整数値が使われる多くの場合で、十分に大きい範囲です。

### UInt

Swift は unsigned の整数値型も提供しています。これも、現在のプラットフォーム由来のサイズと同じサイズを持ちます。

* 32 ビットのプラットフォームの場合、`UInt` は `UInt32` と等しい
* 64 ビットのプラットフォームの場合、`UInt` は `UInt64` と等しい

> NOTE  
> `UInt`は特別にプラットフォーム由来のサイズと同じサイズの unsigned integer 型を暑いかいたい場合にのみ使用しましょう。そうでない場合、負の値にならないとしても`Int`を使う方が好ましいです。一貫して`Int` を整数値に使用することは、互換性を保つ手助けとなり、異なる整数値型の変換を避け、整数値型の推論に合致します。詳細は[Type Safety and Type Inference](#type-safety-and-type-inference)。

## Floating-Point Numbers

浮動小数点数は、小数部分を持つ数値です(`3.14159`, `0.1`, `-273.15`など)。

浮動小数型は、整数値型よりもより広い範囲の値を表し、`Int` より大きい(またはより小さい)値を保持できます。Swift は 2 つの signed 浮動小数点数型を提供しています。

* `Double` は 64 ビットの浮動小数点数を表します
* `Float` は 32 ビットの浮動小数点数を表します

> NOTE  
> `Double` は最小で15少数桁の精度を持ち、`Float`は6少数桁の精度を持ちます。適切な浮動小数点数型は、扱いたい数値の特性と範囲によります。どちらでも良い場合は、`Double`が好まれます。

## Type Safety and Type Inference

Swwift は型安全な言語です。型安全な言語は、扱っている値の型が明確になるようにします。`String` が必要な場合、間違って`Int`を渡すようなことはありません。

型安全ということの理由に、コンパイル時に型チェックが行われ、間違った型に対しては、エラーを示します。これによって開発時に素早くエラーに気がつき修正をすることができます。

型チェックは異なる型の値を扱う場合にエラーを回避する手助けをしてくれます。しかし、だからといって、全ての定数と変数の定義で型を特定しなければならない、ということではありません。特定しなければ、Swift は適切な型への推論を行います。コンパイラが、コンパイル時に、与えられた値を調べることで、式の型を自動で推論するのです。

この型推論のおかげで、Swift は C 言語や Objective-C のような言語と比べて、型を宣言する必要が多くありません。定数と変数は明示的に型を記載する場合もありますが、型を特定する多くの作業は Swift の側で行なってくれます。

型推論は、特に初期値を伴って変数や定数を宣言する時に役に立ちます。特に、*literal* な値を設定する際によく機能します。(literal な値とは、下記の例にあるような、コードに直接出てくる`42, 3.14159`のような値のことです)

例えば、`42`を新しい定数に型を記載せずに設定すると、整数値型に見える数値リテラルで初期化していることから、Swift はその型を推論して`Int` と判断します。

```swift
let meaningOfLife = 42
// meaningOfLife is inferred to be of type Int
```

同様に、浮動小数点数を型なしで宣言した場合、Swift は`Double`を定義したいという判断をします。

```swift
let pi = 3.14159
// pi is inferred to be of type Double
```

Swift は浮動小数点数を推論する際に、常に(`Float`よりも)`Double`を選択します。

式の中で整数値と浮動小数点数を組み合わせた場合、文脈から`Double`と推論されるでしょう。

```swift
let anotherPi = 3 + 0.14159
// anotherPi is also inferred to be of type Double
```

`3` には明確な型がありませんが、後の浮動小数点数の値から `Double` が推論されます。

## Numeric Literals

数値リテラルは、下記の方法で書くことができます。

* プレフィックスなしの 10 進数
* `0b`をプレフィックスにしたの 2 進数
* `0o`をプレフィックスにしたの 8 進数
* `0x`をプレフィックスにしたの 16 進数

これらの数値リテラルは、`17`という 10 進数を下記のように保持します。

```swift
let decimalInteger = 17
let binaryInteger = 0b10001       // 17 in binary notation
let octalInteger = 0o21           // 17 in octal notation
let hexadecimalInteger = 0x11     // 17 in hexadecimal notation
```

浮動小数点数リテラルは、10 進数と 16 進数にできます。小数点の左と右の両方に必ず整数値が必要です。10 進数の浮動小数点数は、指数を持つ場合があります(小文字または大文字の `e` )。16 進数の浮動小数点数は必ず指数を指定しなければなりません(小文字または大文字の `p` )。

`e`(`exp`)を持つ 10 進数では、元の値に 10 の指数(`10exp`)を掛けます。

* `1.25e2` は `1.25 x 10^2` または 125.0 と等しい
* `1.25e-2` は `1.25 x 10^-2` または `0.0125` と等しい

`p`(`exp`)を持つ 16 進数では、元の値に 2 の指数(`2exp`)を掛けます。

* `0xFp2` は `15 x 2^2` または 60.0 と等しい
* `0xFp-2` は `15 x 2^-2` または `3.75` と等しい

下記の浮動小数点数は、全て`12.1875`という 10 進数を表しています。

```swift
let decimalDouble = 12.1875
let exponentDouble = 1.21875e1
let hexadecimalDouble = 0xC.3p0
```

整数値リテラルは、読みやすくするために追加でフォーマットを含んでいる場合があります。整数値や浮動小数点数へ、追加の 0 を加えたり、アンダースコア(_)を追加することで可読性を向上させることができます。このフォーマットで整数値自体への影響はありません。

```swift
let paddedDouble = 000123.456
let oneMillion = 1_000_000
let justOverOneMillion = 1_000_000.000_000_1
```

## Numeric Type Conversion

通常整数値を扱う際は、負の値にならないことがわかっていたとしても定数や変数に`Int`型を使いましょう。全ての状況でデフォルトに`Int`を使うことは、整数値の定数と変数を特別な手続きなしに互換可能にし、、数値リテラルから推論される型にも適合するでしょう。

特別な理由ある場合のみ、他の整数値型を使いましょう。例えば、外部リソースから特定サイズの型が指定されている場合やパフォーマンス、メモリーの使用量や他の最適化が必要な場合などがこれに当たります。明示的にサイズを指定することで思わぬオーバーフローを起こしたり、暗黙的にそのデータの特性を表現することができます。

### Integer Conversion

整数値の定数や変数に保持できる整数値の範囲は、それぞれの整数値型によって異なります。`Int8` の定数や変数は、−128 から 127 まで保持できます。`UInt8` の定数や変数は、0 から 255 まで保持できます。サイズを特定した整数値型の定数や変数に合わない整数値を設定しようとすると、エラーになります。

```swift
let cannotBeNegative: UInt8 = -1
// UInt8 can't store negative numbers, and so this will report an error
let tooBig: Int8 = Int8.max + 1
// Int8 can't store a number larger than its maximum value,
// and so this will also report an error
```

このように整数値型によって保持できる範囲が異なるため、ケースごとに型の変換をしなければなりません。このオプトインの方法によって隠れた変換ミスを防ぎ、コードで型変換が起こっていることを明確に表現することができます。

ある特定の整数値型を他の型に変換するためには、既存の整数値から変換したい型の新しい値を生成する必要があります。下記の例では、定数`twoThousand`は`UInt16`ですが、定数の`one`は`UInt8`です。この 2 つは同じ型ではないので直接足し算をすることができません。そこで、この例では、`one`を使って`UInt16`の新しい値を作るために`UInt16(one)`を呼び、元の例に置き換えて使います:

```swift
let twoThousand: UInt16 = 2_000
let one: UInt8 = 1
let twoThousandAndOne = twoThousand + UInt16(one)
```

今はどちらも`UInt16`なので、足し算は可能です。そして計算結果の定数(`twoThousandAndOne`)も`UInt16`に推論されます。

`SomeType(ofInitialValue)`という形式は、初期値を渡して初期化を行う Swift のデフォルトの方法です。裏側では、`UInt16`型が`UInt8`の値を受け取って、新しい`UInt16`型の値を生成しています。とはいっても、あらゆる型を渡せるわけではありません。`UInt16`型提供するイニシャライザに合った型が必要です。新しい型を渡して初期化する方法は、[Extensions](./extensions.md)に記載しています。

### Integer and Floating-Point Conversion

整数値と浮動小数点数の変換は明示的に行わなければなりません:

```swift
let three = 3
let pointOneFourOneFiveNine = 0.14159
let pi = Double(three) + pointOneFourOneFiveNine
// pi equals 3.14159, and is inferred to be of type Double
```

ここに、定数`3`の値が`Double`型の新しい値を作って使われています。そのため、両方の型が同じになり、足し算は可能です。ここで変換をしない場合、足し算はできません。

整数値から浮動小数点数への変換は、明示的に行わなければなりません。整数値は`Double`や`Float`で初期化できます。

```swift
let integerPi = Int(pi)
// integerPi equals 3, and is inferred to be of type Int
```

浮動小数点数の値は新しい整数値として初期化される場合、必ず小数点は切り捨てられます。つまり、`4.75`は`4`、`-3.9`は`-3`になります。

> NOTE  
> 整数値の定数と変数を組み合わせるルールは、数値リテラルのルールとは異なります。リテラル値の`3`はリテラル値の`0.14159`とそのまま加算できます。リテラル値は明示的な型を有していないからです。それらは、コンパイラがその値を評価する時にのみ、推論されます。

## Type Aliases

*Type Aliases(タイプエイリアス)* は既存の型に別の名前を定義します。`typealias`キーワードを使います。

タイプエイリアスは既存の型に文脈上より適切な名前で参照したい場合に有効です。例えば、外部リソースの特定のサイズのデータを扱いたい場合など:

```swift
typealias AudioSample = UInt16

```

定義したタイプエイリアスは、元の名前で使っていた所で、そのエイリアスを使うことができます:

```swift
var maxAmplitudeFound = AudioSample.min
// maxAmplitudeFound is now 0
```

ここでは、`AudioSample` を `UInt16` のタイプエイリアスとして定義されています。エイリアスなので、`AudioSample.min` の実態は `UInt16.min` で、`maxAmplitudeFound`の初期値は `0`になります。

## Booleans

Swift は、`Bool`と呼ばれる基本的なブール値を持っています。ブール値は、真(true)か偽(false)のみを値として取ることから、*logical (論理値)*として参照されます。Swift では`true`と`false`という 2 つのブール値の定数を提供しています。

```swift
let orangesAreOrange = true
let turnipsAreDelicious = false
```

`orangesAreOrange` と `turnipsAreDelicious` はブール値リテラルで初期化されていることから、`Bool`と推論されます。これまで見てきた`Int`や`Double`のように、`true`や`false`で値を設定すれば、明示的に`Bool`と指定する必要はありません。既に型がわかっている他の定数や変数を使って初期化する際に、Swift の型推論を活用することで、より簡潔で読みやすいコードを書くことができます。

ブール値は、条件文を扱う際にとても有効です。例えば、`if`文など:

```swift
if turnipsAreDelicious {
    print("Mmm, tasty turnips!")
} else {
    print("Eww, turnips are horrible.")
}
// Prints "Eww, turnips are horrible."
```

`if`のような条件文については、[Control Flow](./control-flow.md)でより詳細に記載しています。

Swift の型安全なので、`Bool`の値を他の型に置き換えることはできません。次の例はコンパイルエラーになります:

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

`i == 1`の比較結果は`Bool`なので、型チェックを通過できます。`i == 1`のような比較については、[Basic Operators](./basic-operators.md)でより詳しく記載しています。

他の型安全性を示した例と同様に、型安全なことで、予期せぬエラーを発生させることを防ぎ、型は明確なことから、そのコードの意図をより明確にすることができます。

## Tuples

## Optionals

### nil

### If Statements and Forced Unwrapping

### Optional Binding

### Implicitly Unwrapped Optionals

## Error Handling

## Assertions and Preconditions

### Debugging with Assertions

### Enforcing Preconditions

