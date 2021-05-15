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

複数行文字列リテラルを境界線で開始、終了したい場合は、最初、と最後に空白の行を入れましょう。例えば:

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

### Extended String Delimiters

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
