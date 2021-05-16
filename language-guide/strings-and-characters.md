# Strings and Characters

最終更新日:

文字列(*string*)は`"hello, world"`や`"albatross"`のような一連の文字です。Swift の文字列は`String`型で表されます。`String`の内容には、様々な方法でアクセスすることができます(`Character`型の値のコレクションとしてなど)。

Swift の`String`と`Character`型は、高速で、Unicode に準拠した方法でテキストを扱うことができます。文字列の生成と操作のシンタックスは、C 言語に似た文字列リテラルのシンタックスを使い、軽量で読みやすくなっています。文字列の連結は`+`演算子を使って 2 つの文字列の連結をするのと同じくらいシンプルで、文字列が変更が可能かどうかは、他の型と同じように、定数か変数かを選択することで管理できます。また、文字列補間と呼ばれるプロセスの中で、定数、変数、リテラル、より長い文字列の中での式として、文字列を使用できます。こういった特徴から、画面、ストレージ、出力のためのカスタム文字列も簡単に作ることができます。

このシンプルなシンタックスにも関わらず、Swift の`String`型は、高速で、モダンな文字列実装になっています。それぞれの文字は、エンコーダとは独立した Unicode 文字群構成され、様々な Unicode 形式でアクセスできるようになっています。

> NOTE  
> Swift の`String`型は、Foundationの`NSString`クラスとの違いを埋めます。同様にFoundationも`NSString`で定義されたメソッドを使用できるようにするために、`String`を拡張しています。つまり、Foundationをインポートすると、castなしで`NSString`のメソッドに`String`からアクセスできます。  
>  
> FoundationとCocoaを使った`String`の使用方法に関しては、[Bridging Between String and NSString](https://developer.apple.com/documentation/swift/string#2919514)を参照ください

## String Literals

事前に定義された`String`値を文字列リテラル(*string literals*)として使うことができます。文字列リテラルはダブルクォテーション(`"`)で囲まれた一連の文字です。

```swift
let someString = "Some string literal value"
```

`someString`は文字列リテラルで初期化されているので、`String`型と推論されていることに注目してください

### Multiline String Literals

複数行に渡った文字列が必要な場合、複数行文字列リテラルを使いましょう。一連の文字を 3 つのダブルクォテーション(`"`)で囲みます。

```swift
let quotation = """
The White Rabbit put on his spectacles.  "Where shall I begin,
please your Majesty?" he asked.

"Begin at the beginning," the King said gravely, "and go on
till you come to the end; then stop."
"""
```

複数行文字列リテラルは、全てを開始と終了のクォテーションマークの間に含めます。文字列は、開始クォテーションの次のから始め、終了クォテーションの 1 つ前の行で終了します。つまり、文字列を改行で開始、終了することができません。

```swift
let singleLineString = "These are the same."
let multilineString = """
These are the same.
"""
```

もし改行が含まれている場合、文字列にも反映されます。もしより見やすくしたいけれども改行を文字列の中に入れたくない場合は、行の最後にバックスラッシュ(`\`)を書きましょう:

```swift
let softWrappedQuotation = """
The White Rabbit put on his spectacles.  "Where shall I begin, \
please your Majesty?" he asked.

"Begin at the beginning," the King said gravely, "and go on \
till you come to the end; then stop."
"""
```

複数行文字列リテラルを改行で開始、終了したい場合は、最初、と最後に空白の行を入れましょう。例えば:

```swift
let lineBreaks = """

This string starts with a line break.
It also ends with a line break.

"""
```

複数行文字列は、囲み文字に一致するようにすることができます。終了クォテーションマーク(`"""`)の前に空白を加えると、他の行に存在する同じ位置の空白も無視するようになります。一方で、終了クォテーションマーク(`"""`)の空白のさらに後に空白を追加すると、その行の空白は含まれるようになります。

![複数行の空白](./../.gitbook/assets/multilinestringwhitespace_2x.png)

上記の例では、全ての複数行文字列にインデントが加えられていますが、最初と最後の行は冒頭に空白はありません。真ん中の行は終了クォテーションマーク(`"""`)よりもさらにインデントを追加しているため、4 つ空白インデントから開始します。

### Special Characters in String Literals

文字列リテラルには次の特殊文字が含まれています。

* エスケープされた文字`\0 (null文字)`、`\\ (バックスラッシュ)`、`\t (水平タブ)`、`\n (ラインフィード)`、`\r (キャリッジリターン)`、`\" (ダブルクォテーション)`、`\' (シングルクォテーション)`
* `\u{n}`で書ける随意の Unicode スカラ値、`n`には 1〜8 桁の 16 進数が入ります(Unicode については下記の[Unicode](#unicode)に書かれています)

下記のコードは特殊文字の 4 つの例を示しています。`wiseWords`定数は 2 つのエスケープされた 2 つのダブルクォテーションを含んでいます。`dollarSign`と`blackHeart`と`sparklingHeart`定数は、Unicode スカラ形式を示しています:

```swift
let wiseWords = "\"Imagination is more important than knowledge\" - Einstein"
// "Imagination is more important than knowledge" - Einstein
let dollarSign = "\u{24}"        // $,  Unicode scalar U+0024
let blackHeart = "\u{2665}"      // ♥,  Unicode scalar U+2665
let sparklingHeart = "\u{1F496}" // 💖, Unicode scalar U+1F496
```

複数行文字列リテラルは 1 つではなく 3 つのを使っているので、複数行文字列リテラルの中にエスケープなしでダブルクォテーションを含めることができます。文字列にテキストとして`"""`を含めたい場合、少なくともその中の 1 つをエスケープしましょう。

```swift
let threeDoubleQuotationMarks = """
Escaping the first quotation mark \"""
Escaping all three quotation marks \"\"\"
"""
```

### Extended String Delimiters

拡張区切り文字(`extended delimiters`)の中に文字列を置くと、特殊文字をただの文字として含めることができます。文字列をクォテーションで囲み、さらにそれを番号記号(`number signs`)で囲みます。例えば、`#"Line 1\nLine 2"#`は 2 行の文字列を出力するのではなく、改行コード(`\n`)を出力します。

文字列内の文字の特殊効果が必要な場合は、エスケープ文字(\\)の後に番号記号を同じ数追加しましょう。例えば、`#"Line 1\nLine 2"#`で、改行したい場合は、`#"Line 1\#nLine 2"#`と書くことができます。同様に、`###"Line1\###nLine2"###`でも改行することができます。

拡張区切り文字は複数文字列リテラルにも使用できます。テキストとして`"""`を含めたい場合、文字列を終わらせる記号を変えましょう。例えば:

```swift
let threeMoreDoubleQuotationMarks = #"""
Here are three more double quotes: """
"""#
```

## Initializing an Empty String

長い文字列を構築する時に、初期値として空の文字列を作る時、文字列リテラルを変数に設定するか、`String`のイニシャライザを使って新しいインスタンスを初期化します。

```swift
var emptyString = ""               // 空の文字列
var anotherEmptyString = String()  // イニシャライザ
// 2つの変数はどちらも空の文字列で等しいです
```

`isEmpty`というブール値のプロパティをチェックすることで`String`値が空文字かどうかを判定できます。

```swift
if emptyString.isEmpty {
    print("Nothing to see here")
}
// Prints "Nothing to see here"
```

## String Mutability

`String`が変更可能かどうかは、変数(変更可能)か定数(変更不可能)のどちらに設定するかによって示すことができます。:

```swift
var variableString = "Horse"
variableString += " and carriage"
// variableString は "Horse and carriage"

let constantString = "Highlander"
constantString += " and another Highlander"
// コンパイルエラー - 定数は変更できません
```

> NOTE  
> これは Objective-C と Cocoa とは異なります。Objective-C と Cocoa では、変更の可不可を示すために 2 つの class (`NSString`と`NSMutableString`)から選択します。

## Strings Are Value Types

`String`型は値型(*Value Type*)です。新しい`String`値を生成すると、関数やメソッドの引数で渡される時や、他の定数、変数に代入される時に値のコピーが発生します。いずれの場合でも、既存の`String`値のコピーが生成され、元の値ではなく新しいコピーが渡され(または代入され)ます。値型は[Structures and Enumerations Are Value Types](./structures-and-classes.md)で記載されています。

Swift のデフォルトでコピーをする`String`の挙動は、`String`値が関数やメソッドの引数で渡される時に、どこからその値が来たとしても、正しい`String`値を所有していることを確実にします。つまり、渡ってきた文字列は、自身で変更しない限り、決して変更されることがないことを確信できます。

舞台裏では、Swift のコンパイラは、本当に必要な時だけ実際にコピーが発生するように最適化をしています。つまり、値型として文字列を扱う場合に、常に良いパフォーマンスを得ることができます。

## Working with Characters

`for-in`ループを使って文字列を繰り返し処理することで、`String`の個々の`Character`の値にアクセスすることができます。

```swift
for character in "Dog!🐶" {
    print(character)
}
// D
// o
// g
// !
// 🐶
```

`for-in`ループについては[For-In Loops](./control-flow.md#for-in-loops)に記載されています。

もしくは、`Character`型アノテーションを与えて 1 文字のリテラルから`Character`型の定数や変数を作ることもできます:

```swift
let exclamationMark: Character = "!"
```

`String`値は、`Character`値の配列をイニシャライザの引数に渡して構築することができます:

```swift
let catCharacters: [Character] = ["C", "a", "t", "!", "🐱"]
let catString = String(catCharacters)
print(catString)
// Prints "Cat!🐱"
```

## Concatenating Strings and Characters

`String`値同士は、加算演算子(`+`)使って連結して新しい`String`値を生成できます:

```swift
let string1 = "hello"
let string2 = " there"
var welcome = string1 + string2
// welcome は "hello there" と等しい
```

加算代入演算子(`+=`)を使って既存の`String`変数に`String`値を追加することもできます:

```swift
var instruction = "look over"
instruction += string2
// instruction は "look over there" と等しい
```

`String`変数に`String`の`append()`メソッドと使って`Character`値を追加できます。

```swift
let exclamationMark: Character = "!"
welcome.append(exclamationMark)
// welcome は "hello there!" と等しい
```

> NOTE  
> 既存の`Character`値に`String`や`Character`を追加することはできません。`Character`値は、 1 つの文字だけしか含められません。

より長い文字列を構築するために複数行文字列リテラルを使っている場合、最後の行も含めた全ての行を改行で終えたいでしょう。例えば:

```swift
let badStart = """
one
two
"""
let end = """
three
"""
print(badStart + end)
// Prints two lines:
// one
// twothree

let goodStart = """
one
two

"""
print(goodStart + end)
// Prints three lines:
// one
// two
// three
```

上記のコードは、`badStart`と`end`を連結すると、2 行の文字列を生成していますが、これは期待した結果ではあありません。`badStart`の最後の行に改行が入っていないため、`end`の最初の行と混ざってしまいます。一方で、`goodStart`の最初と最後の行は改行で終わっているので、`end`と連結した結果は 3 行になり、期待通りになりました。

## String Interpolation

文字列補間(*String Interpolation*)は複数の定数、変数、リテラル、式を文字列リテラルの中に含めることで新しい`String`値を構築する方法です。文字列補間は 1 行でも複数行でも使うことができます。文字列に入れるそれぞれの値は、両かっこで囲んで、その前にバックスラッシュ(\\)をつけます。

```swift
let multiplier = 3
let message = "\(multiplier) times 2.5 is \(Double(multiplier) * 2.5)"
// message は "3 times 2.5 is 7.5"
```

上記の例では、`multiplier`は`\(multiplier)`として文字列に挿入します。このプレースホルダは、実際に文字列を生成するタイミングで評価されて、`multiplier`の実際の値に置き換えられます。

`multiplier`は文字列の後半の大きな式の中でも使われています。この式は、`Double(multiplier) * 2.5`を計算して、その結果の(`7.5`)を文字列に挿入します。この場合、`\(Double(multiplier) * 2.5)`と書いて文字列リテラル含まれます。

文字列補間として通常扱われる文字を含んだ文字列を生成する場合、拡張区切り文字を使うことができます。例えば:

```swift
print(#"Write an interpolated string in Swift using \(multiplier)."#)
// Prints "Write an interpolated string in Swift using \(multiplier)."
```

拡張区切り文字を使った文字列の中で文字列補間を使う場合、バックスラッシュの後の番号記号の数を文字列の開始と終了の番号記号の数に合わせます。例えば:

```swift
print(#"6 times 7 is \#(6 * 7)."#)
// Prints "6 times 7 is 42."
```

> NOTE  
> 補間された文字列内のかっこの中に書かれた式は、エスケープしていないバックスラッシュ(\\)、キャリッジリターン、改行を含めることはできません。しかし、他の文字列リテラルは含めることができます。

## Unicode

*Unicode*は、さまざまな書記体系でテキストをエンコード、表現、および処理するための国際標準です。標準化された形式で、どんな言語のほとんど全ての文字を表現することができ、Web ページやテキストファイルなどの外部リソースへ読み書きできます。Swift の`String`や`Character`は、このセクションでも記載していますが、完全に Unicode に準拠しています。

### Unicode Scalar Values

舞台裏では、Swift 固有の`String`型は *Unicode* スカラ値(*Unicode Scalar Values*)から構築されています。*Unicode* スカラ値は 21 ビットの文字と修飾子で構成されています。例えば、`U+0061`は`LATIN SMALL LETTER A ("a")`、`U+1F425`は`FRONT-FACING BABY CHICK ("🐥")`です。

全ての 21 ビットのスカラ値が 1 つの文字に当てはまるわけではありません。いくつかは、将来的に必要になるために確保されていたり、UTF-16 で使われています。文字に割り当てられているスカラ値には、上記の`LATIN SMALL LETTER A`や`FRONT-FACING BABY CHICK`のように一般的に名前が付いています。

### Extended Grapheme Clusters

Swift の`Character`の全てのインスタンスは 1 つの拡張書記素クラスタ(*Extended Grapheme Clusters*)を表しています。拡張書記素クラスタは、人間が読み取れる 1 つの文字を生成するための 1 つ以上の Unicode スカラの配列です(1 つ以上 Unicode スカラが必要な場合は組み合わされます)。

下記に例を示します。文字`é`は、1 つの`é`(`LATIN SMALL LETTER E WITH ACUTE`または`U+00E9`)を表しすことができます。一方で、同じ文字をスカラのペアで表すこともできます。`e`(`LATIN SMALL LETTER E`または`U+0065`)の後ろに`COMBINING ACUTE ACCENT`スカラ(`U+0301`)を付けます。`COMBINING ACUTE ACCENT`スカラはその前のスカラに視覚的に適用されて、Unicode を理解できるシステムがテキストをレンダリングする際に、`e`を`é`に変換します。

どちらの場合も、`é`は拡張書記素クラスタの 1 つの Swift の``Character`型の値として表されます。最初のケースでは、1 つのスカラを含んでいるクラスタで、2 番目のケースは、2 つのスカラのクラスタとなります。

```swift
let eAcute: Character = "\u{E9}"                         // é
let combinedEAcute: Character = "\u{65}\u{301}"          // e の後ろに  ́
// eAcute は é, combinedEAcute は é
```

拡張書記素クラスタは、多くの複雑な文字スクリプトを 1 つの`Character`値として表す柔軟な方法です。例えば、韓国語のアルファベットのハングル文字は、合成または分解されたスカラの配列で表すことができます。これらのどちらも、 Swift では 1 つの`Character`値と見なされます。

```swift
let precomposed: Character = "\u{D55C}"                  // 한
let decomposed: Character = "\u{1112}\u{1161}\u{11AB}"   // ᄒ, ᅡ, ᆫ
// precomposed は 한, decomposed も 한
```

拡張書記素クラスタは、囲み記号(`COMBINING ENCLOSING CIRCLE`または`U+20DD`)のスカラを 1 つの`Character`値の一部として他の*Unicode* スカラを囲むことができます。

```swift
let enclosedEAcute: Character = "\u{E9}\u{20DD}"
// enclosedEAcute is é⃝
```

地域を示す*Unicode* スカラは、1 つの`Character`値を作成するために、ペアを組み合わせることができます。例えば、`REGIONAL INDICATOR SYMBOL LETTER U`(`U+1F1FA`)と`REGIONAL INDICATOR SYMBOL LETTER S`(`U+1F1F8`):

```swift
let regionalIndicatorForUS: Character = "\u{1F1FA}\u{1F1F8}"
// regionalIndicatorForUS は 🇺🇸
```

## Counting Characters

文字列の中の`Character`の数を得るためには、文字列の`count`プロパティを使いましょう。

```swift
let unusualMenagerie = "Koala 🐨, Snail 🐌, Penguin 🐧, Dromedary 🐪"
print("unusualMenagerie has \(unusualMenagerie.count) characters")
// Prints "unusualMenagerie has 40 characters"
```

`Character`値に拡張書記素クラスタを使っているということは、文字列の連結や変更が必ずしも文字列内の文字のカウントに影響を与えるわけではない、ということに注意してください。

例えば、`cafe`という単語は 4 つの文字から新しい文字列を初期化した場合、`COMBINING ACUTE ACCENT (U+0301)`を最後に追加すると、最後の文字は`e`から`é`に変わりますが、文字のカウントは`4`のままです。

```swift
var word = "cafe"
print("the number of characters in \(word) is \(word.count)")
// Prints "the number of characters in cafe is 4"

word += "\u{301}"    // COMBINING ACUTE ACCENT, U+0301

print("the number of characters in \(word) is \(word.count)")
// Prints "the number of characters in café is 4"
```

> NOTE  
> 拡張書記素クラスタは複数のUnicodeスカラを組み合わせることができます。つまり、異なった文字や、同じ文字でも異なったスカラで表された文字は、メモリ上に保持する際に、異なったメモリサイズが必要になる場合があります。これは、Swift の文字が、それぞれの文字に同じメモリ量を使ってはいない、ということです。結果として、文字列内の文字カウントは拡張書記素クラスタの境界を判断しなければならないため、文字列全体を反復しないと計算することができません。特に長い文字列を扱っている場合は、`count`プロパティを使うと、文字数をカウントするために文字列全体に反復処理を行なっていることに気をつけてください。  
>  
> `count`プロパティから返ってくる文字数は、同じ文字列であっても`NSString`の`length`と異なる場合があります。`NSString`の`length`は*UTF-16*での文字数カウントで、Unicode 拡張書記素クラスタの数ではありません。

## Accessing and Modifying a String

メソッドやプロパティを使ったり、script シンタックスを使って、文字列へのアクセスや変更をすることができます。

### String Indices

`String`値は｀String.Index｀という紐づいた index 型を持っており、それぞれは文字列の`Character`の位置に対応関係があります。

上記で述べたように、異なる文字へは、保持する際に異なるメモリ量が必要になります。つまり、`Character`の位置を特定するには、Unicode スカラを文字列の最初または最後から反復して探さなければなりません。こういった理由から Swift の`String`の index は数値にすることはできません。

`String`の最初の`Character`の位置を知るためには、`startIndex`プロパティを使いましょう。`endIndex`プロパティは`String`の最後の`Character`の位置の次の位置です。つまり、`endIndex`プロパティは文字列の subscript に使ってはいけません。`String`が空ならば、`startIndex`と`endIndex`は等しくなります。

`index(before:)`と`index(after:)`を使ってある index の前後の index にアクセスできます。ある index から離れた位置の index にアクセスするためには、上記 2 つのメソッドを繰り返し呼ぶのではなく、`index(_:offsetBy:)`を使います。

`String`のある特定の位置の`Character`へアクセスするには、subscript シンタックスを使います。

```swift
let greeting = "Guten Tag!"
greeting[greeting.startIndex]
// G
greeting[greeting.index(before: greeting.endIndex)]
// !
greeting[greeting.index(after: greeting.startIndex)]
// u
let index = greeting.index(greeting.startIndex, offsetBy: 7)
greeting[index]
// a
```

文字列の範囲を超えた index や`Character`にアクセスしたり、実行時エラーが起きます。

```swift
greeting[greeting.endIndex] // Error
greeting.index(after: greeting.endIndex) // Error
```

文字列内の個々の文字の全ての index にアクセスするためには`indices`プロパティを使いましょう。

```swift
for index in greeting.indices {
    print("\(greeting[index]) ", terminator: "")
}
// Prints "G u t e n   T a g ! "
```

> NOTE  
> `Collection`プロトコルに適合したどんな型にも、`startIndex`、`endIndex`プロパティ、index(before:)、index(after:)、index(_:offsetBy:)メソッドを使うことができます。これは、`Array`、 `Dictionary`、`Set`といったコレクションの型と同様に、今紹介している`String`も含んでいます。

### Inserting and Removing

特定の文字列の index に 1 つの文字を挿入するには、`insert(_:at:)`を使い、他の文字列を挿入したい場合は、`insert(contentsOf:at:)`を使います。

```swift
var welcome = "hello"
welcome.insert("!", at: welcome.endIndex)
// welcome は "hello!" と等しい

welcome.insert(contentsOf: " there", at: welcome.index(before: welcome.endIndex))
// welcome は "hello there!" と等しい
```

文字列の特定の index に 1 つの文字を削除するには、`remove(at:)`を使い、部分文字列を削除したい場合は、`removeSubrange(_:)`を使います。

```swift
welcome.remove(at: welcome.index(before: welcome.endIndex))
// welcome は "hello there" と等しい

let range = welcome.index(welcome.endIndex, offsetBy: -6)..<welcome.endIndex
welcome.removeSubrange(range)
// welcome は "hello" と等しい
```

> NOTE  
> `RangeReplaceableCollection`プロトコルに適合したどんな型にも、insert(_:at:)、insert(contentsOf:at:)、remove(at:)、removeSubrange(_:)メソッドを使うことができます。これは、`Array`、 `Dictionary`、`Set`といったコレクションの型と同様に、今紹介している`String`も含んでいます。

## Substrings

文字列から部分文字列を取得したい場合(例えば subscript や`prefix(_:)`などのメソッドを使うなど)、[Substring](https://developer.apple.com/documentation/swift/substring)インスタンスが結果として取得できます。は、部分文字列は文字列とほぼ同じメソッドを使うことができます。つまり、部分文字列を文字列と同じような方法で扱うことができます。しかし、文字列とは異なり、文字列に対して何かアクションを起こしている時に、ほんの短い間だけ、部分文字列を使います。処理の結果をより長い期間保持する時は、部分文字列を`String`のインスタンスに変換します。例えば:

```swift
let greeting = "Hello, world!"
let index = greeting.firstIndex(of: ",") ?? greeting.endIndex
let beginning = greeting[..<index]
// beginning は "Hello"

// より長期で使用するためにStringへ変換
let newString = String(beginning)
```

文字列のように、部分文字列もそれを構成する文字配列をメモリ領域に持っています。文字列と部分文字列の違いはパフォーマンス最適化として、部分文字列は元の文字列や他の部分文字列を保持するのに使用しているメモリの一部を再利用します。(文字列にも似たような最適化がありますが、2 つの文字列のメモリが共有されている場合、等しいと見なされます)この最適化は、文字列や部分文字列が変更されるまでメモリのコピーが発生するコストに注意を払わなくて済みます。上記で述べたように、部分文字列は、長期保持するには向いていません。部分文字列が元の文字列とメモリを共有しているため、元の文字列は部分文字列が使われている間はメモリ上に保持していなければなりません。

上記の例では、`greeting`は文字列です。つまり、文字列を構築する文字を保持したメモリ領域を持っています。`beginning`は`greeting`の部分文字列です。`greeting`が使っているメモリを再利用しています。反対に、`newString`は文字列で、部分文字列から生成された時に独自のメモリ領域を持ちます。下記の図はこの関係を示しています。

![文字列部分文字列の関係](./../.gitbook/assets/stringsubstring_2x.png)

> NOTE  
> 文字列と部分文字列は、[StringProtocol](https://developer.apple.com/documentation/swift/stringprotocol)に適合しています。つまり、文字列操作を行う関数は、*StringProtocol*の値を受け取るとしばしば便利なことがあります。文字列、部分文字列のどちらを使っても、その関数を使うことができます。

## Comparing Strings

Swift では、3 つの方法で文字列同士を比較する方法を提供しています: 文字列と文字の完全一致、前方一致、後方一致です。

### String and Character Equality

文字列と文字の完全一致は、は等価演算子(`==`)と不等演算子(`!=`)を使ってチェックします。[Comparison Operators](./basic-operators.md#comparison-operators)

```swift
let quotation = "We're a lot alike, you and I."
let sameQuotation = "We're a lot alike, you and I."
if quotation == sameQuotation {
    print("These two strings are considered equal")
}
// Prints "These two strings are considered equal"
```

2 つの文字列(または文字)は、拡張書記素クラスタが「規範的に」等しければ、等しいと見なされます。つまり、舞台裏では異なる Unicode スカラで構成されていたとしても、同じ言語的な意味と見た目が同じならば等しくなります。

例えば、`LATIN SMALL LETTER E WITH ACUTE`(`U+00E9`)は、`LATIN SMALL LETTER E`(`U+0065`)の最後に`COMBINING ACUTE ACCENT`(`U+0301`)を付け加えた文字列と「規範的に」等しくなります。どちらの拡張書記素クラスタも`é`という文字を表す妥当な方法なので、これらは「規範的に」等しいと見なされます。

```swift
// "Voulez-vous un café?"は LATIN SMALL LETTER E WITH ACUTE を使っている
let eAcuteQuestion = "Voulez-vous un caf\u{E9}?"

// "Voulez-vous un café?"は LATIN SMALL LETTER E and COMBINING ACUTE ACCENT を使っている
let combinedEAcuteQuestion = "Voulez-vous un caf\u{65}\u{301}?"

if eAcuteQuestion == combinedEAcuteQuestion {
    print("These two strings are considered equal")
}
// Prints "These two strings are considered equal"
```

反対に、英語で使われている`LATIN CAPITAL LETTER A`(`U+0041`または`"A"`)はロシア語で使われている`CYRILLIC CAPITAL LETTER A`(`U+0410`または`"А"`)と等しくありません。一見似ていますが、同じ言語的な意味を持っていません。

```swift
let latinCapitalLetterA: Character = "\u{41}"

let cyrillicCapitalLetterA: Character = "\u{0410}"

if latinCapitalLetterA != cyrillicCapitalLetterA {
    print("These two characters aren't equivalent.")
}
// Prints "These two characters aren't equivalent."
```

> NOTE  
> Swift の文字列と文字の比較は、ロケール依存です。

### Prefix and Suffix Equality

ある文字列が特定の前置文字や後置文字を含んでいるかどうかをチェックするために、`hasPrefix(_:)`と`hasSuffix(_:)`メソッドを使います。どちらも 1 つの`String`型の引数を取り、`Bool`値を返します。

下記の例では、シャークスピアのロミオとジュリエットの最初の 2 幕のシーンの場所を表した文字列の配列です。

```swift
let romeoAndJuliet = [
    "Act 1 Scene 1: Verona, A public place",
    "Act 1 Scene 2: Capulet's mansion",
    "Act 1 Scene 3: A room in Capulet's mansion",
    "Act 1 Scene 4: A street outside Capulet's mansion",
    "Act 1 Scene 5: The Great Hall in Capulet's mansion",
    "Act 2 Scene 1: Outside Capulet's mansion",
    "Act 2 Scene 2: Capulet's orchard",
    "Act 2 Scene 3: Outside Friar Lawrence's cell",
    "Act 2 Scene 4: A street in Verona",
    "Act 2 Scene 5: Capulet's mansion",
    "Act 2 Scene 6: Friar Lawrence's cell"
]
```

`hasPrefix(_:)`を使って`romeoAndJuliet`配列から*Act1*のシーンの数を数えます。

```swift
var act1SceneCount = 0
for scene in romeoAndJuliet {
    if scene.hasPrefix("Act 1 ") {
        act1SceneCount += 1
    }
}
print("There are \(act1SceneCount) scenes in Act 1")
// Prints "There are 5 scenes in Act 1"
```

同様に、`hasSuffix(_:)`を使って Capulet マンションや修道士ローレンスの周りで起きたシーンの数を数えてみます。

```swift
var mansionCount = 0
var cellCount = 0
for scene in romeoAndJuliet {
    if scene.hasSuffix("Capulet's mansion") {
        mansionCount += 1
    } else if scene.hasSuffix("Friar Lawrence's cell") {
        cellCount += 1
    }
}
print("\(mansionCount) mansion scenes; \(cellCount) cell scenes")
// Prints "6 mansion scenes; 2 cell scenes"
```

> NOTE  
> `hasPrefix(_:)`は`hasSuffix(_:)`文字ごとに各文字列の拡張書記素クラスタを使って「規範的に」等しいかどうかを調べます([String and Character Equality](#string-and-character-equality)に記載)。

## Unicode Representations of Strings

Unicode 文字列は、テキストファイルや他のストレージに書かれる時、文字列の Unicode スカラは、様々ある Unicode のエンコード形式の 1 つに符号化(エンコード)されます。それぞれの形式は、文字列を*コードユニット(code unit)*と呼ばれる小さな塊にエンコードします。これらには、UTF-8(8 ビットのコードユニットで文字列を符号化)、UTF-16(16 ビットのコードユニットで文字列を符号化)、UTF-32(32 ビットのコードユニットで文字列を符号化)があります。

Swift では、複数の Unicode 形式で文字列にアクセスできます。`for-in`ステートメントで文字列を反復処理する場合、Unicode 拡張書記素クラスタとして個々の`Character`にアクセスします。このプロセスは[Working with Characters](#working-with-characters)で記載しています。

他にも 3 つの Unicode 準拠形式で`String`値にアクセスできます:

* UTF-8 のコードユニットのコレクション(`utf8`プロパティを使ってアクセス)
* UTF-16 のコードユニットのコレクション(`utf16`プロパティを使ってアクセス)
* 21 ビット Unicode スカラ値のコレクション、UTF-32 に等しい (`unicodeScalars`プロパティを使ってアクセス)

下記の例は、次の文字列を異なる形式で表示しています。`D`、 `o`、 `g`、 `‼` (`DOUBLE EXCLAMATION MARK`または Unicode スカラ`U+203C`)と 🐶 文字 (`DOG FACE`, or Unicode スカラ`U+1F436`):

```swift
let dogString = "Dog‼🐶"
```

### UTF-8 Representation

### UTF-16 Representation

### Unicode Scalar Representation
