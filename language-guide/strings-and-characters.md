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

事前に定義された`String`の値を文字列リテラル(*string literals*)として使うことができます。文字列リテラルはダブルクォテーション(`"`)で囲まれた一連の文字です。

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

## String Mutability

## Strings Are Value Types

## Working with Characters

## Concatenating Strings and Characters

## String Interpolation

## Unicode

### Unicode Scalar Values

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
