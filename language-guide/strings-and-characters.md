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



## Counting Characters

## Accessing and Modifying a String

### String Indices

### Inserting and Removing

## Substrings

## Comparing Strings

### String and Character Equality

### Prefix and Suffix Equality

## Unicode Representations of Strings

### UTF-8 Representation

### UTF-16 Representation

### Unicode Scalar Representation
