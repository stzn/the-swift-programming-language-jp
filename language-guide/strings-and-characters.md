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

### Multiline String Literals

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
