# Collection Types

最終更新日:

Swift は、配列(arrays)、セット(set)、辞書(dictionaries)と呼ばれる 3 つの基本的なコレクション型を提供しています。配列は順序が決まったコレクションです。セットは値の重複がない順序のないコレクションです。辞書はキーと値が紐づいた順序のないコレクションです。

![Collection Types](./../.gitbook/assets/collectiontypes_intro_2x.png)

Swift の配列、セット、辞書は常に保持できるキーや値の型が明確です。つまり、間違って違う型を挿入することができません。これはまた、コレクションから取得する値の型がはっきりとわかっていることでもあります。

> NOTE  
> Swift の配列、セット、辞書は、ジェネリックなコレクションとして実装されています。ジェネリック型やコレクションについての詳細は[Generics](./generics.md)を参照ください。

## Mutability of Collections

もし配列、セット、辞書を変数に代入した場合、生成されたコレクションを可変です。つまり、後々に追加、削除、要素の変更など、そのコレクションを取り替え(または変更)できます。もし配列、セット、辞書を定数に代入した場合、コレクションは不変で、そのサイズや内容は変更できません。

> NOTE  
> コレクションの変更が必要ない場合は、不変なコレクションを作成するのが良いプラクティスです。そうすることで、コードを理解しやすくし、Swiftのコンパイラもコレクションのパフォーマンスを最適化することができます。

## Arrays

配列は同じ型の値を順序立ったリストの中に保持します。配列の中で同じ値を複数回入れることができます。

> NOTE  
> `Array`はFoundationの`NSArray`とスムーズにやりとりできるようにしています。  
> FoundationとCocoaを使った`Array`の使用方法に関しては、[Bridging Between Array and NSArray](https://developer.apple.com/documentation/swift/array#2846730)を参照ください

### Array Type Shorthand Syntax

Swift の配列の型は全体で`Array<Element>`と書きます。`Element`はその配列が保持できる値の型です。ショートハンドとして`[Element]`とも書けます。この 2 つの形式は機能的に同じですが、ショートハンド版の方が好まれ、このガイド内でも配列の型を参照する際はこちらの形式を使います。

### Creating an Empty Array

イニシャライザのシンタックスを使って、ある型の空の配列を生成できます。

```swift
var someInts = [Int]()
print("someInts is of type [Int] with \(someInts.count) items.")
// Prints "someInts is of type [Int] with 0 items."
```

`someInts`変数の型は、イニシャライザから`[Int]`と推論されます。

他にも、文脈的に型情報がわかっている場合(関数の引数や既に型が決まっている変数や定数など)、`[]`(空の角括弧ペア)と書く空配列リテラルを使って、空の配列を作成することができます。

```swift
someInts.append(3)
// someInts Int型の3を含んでいる
someInts = []
// someInts は空の配列だけど[Int]型
```

### Creating an Array with a Default Value

`Array`は同じデフォルト値を設定した特定にサイズの配列を作成するイニシャライザも提供しています。このイニシャライザに適切な型のデフォルト値(`repeating`)と、その値の繰り返し回数(`count`)を渡します。

```swift
var threeDoubles = Array(repeating: 0.0, count: 3)
// threeDoubles は [Double]型で、 [0.0, 0.0, 0.0] と等しい
```

### Creating an Array by Adding Two Arrays Together

加算演算子(`+`)を使って、既存の型互換のある 2 つの配列を合わせて新しい配列を作成することもできます。この新しい配列の型は連結させた配列の型から推論されます:

```swift
var anotherThreeDoubles = Array(repeating: 2.5, count: 3)
// anotherThreeDoubles は [Double]型で [2.5, 2.5, 2.5] と等しい

var sixDoubles = threeDoubles + anotherThreeDoubles
// sixDoubles は [Double] と推論され、 [0.0, 0.0, 0.0, 2.5, 2.5, 2.5] と等しい
```

### Creating an Array with an Array Literal

配列リテラルからも配列を初期化できます。これは、1 つ以上の要素を持った配列コレクションを書くショートハンドです。配列リテラルはカンマ区切りの、角括弧([])で囲んだ値のリストです:

![Array Literal](./../.gitbook/assets/arrayliteral_2x.png)

下記の例は、`String`を保持する`shoppingList`という配列を作成しています。

```swift
var shoppingList: [String] = ["Eggs", "Milk"]
// shoppingList は 2つの 初期値で初期化されている
```

`shoppingList`変数は、`[String]`と書くことで「文字列を保持する配列」として宣言されています。この配列は値の型が`String`で特定されているので、`String`のみを保持することができます。ここで、`shoppingList`配列は、配列リテラルの中で`"Eggs"`と`"Milk"`の 2 つの値で初期化しています。

> NOTE  
> 後の例でさらにアイテムを追加するので、`shoppingList`配列は`let`で宣言された定数ではなく、`var`で変数で宣言されてます。

この例では、配列リテラルには 2 つの`String`を含んでいます。これは`shoppingList`変数の宣言時の型(`String`しか含めない)に合致しているので、`shoppingList`に配列リテラルを代入して 2 つに初期値で初期化することができます。

Swift の型推論のおかげで、同じ型の値を含んだ配列リテラルで初期化する時に、配列の型を書く必要ありません。`shoppingList`の初期化で代わりにより短い方法で書くことができました:

```swift
var shoppingList = ["Eggs", "Milk"]
```

配列リテラルの全ての値は同じ型なので、Swift は`shoppingList`変数で使われている型は`[String]`が適切だと推論できます。

### Accessing and Modifying an Array

### Iterating Over an Array

## Sets

### Hash Values for Set Types

### Set Type Syntax

### Creating and Initializing an Empty Set

### Creating a Set with an Array Literal

### Accessing and Modifying a Set

### Iterating Over a Set

## Performing Set Operations

### Fundamental Set Operations

### Set Membership and Equality

## Dictionaries

### Dictionary Type Shorthand Syntax

### Creating an Empty Dictionary

### Creating a Dictionary with a Dictionary Literal

### Accessing and Modifying a Dictionary

### Iterating Over a Dictionary