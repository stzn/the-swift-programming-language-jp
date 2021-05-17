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

メソッドやプロパティ、subscript シンタックスを通して配列にアクセスしたり、変更できます。

配列のアイテムの数を調べるために、読み取り専用の`count`プロパティをチェックします。

```swift
print("The shopping list contains \(shoppingList.count) items.")
// Prints "The shopping list contains 2 items."
```

`Bool`型の`isEmpty`プロパティは、`count`プロパティが`0`かどうかをチェックするショートハンドです。

```swift
if shoppingList.isEmpty {
    print("The shopping list is empty.")
} else {
    print("The shopping list isn't empty.")
}
// Prints "The shopping list isn't empty."
```

`append(_:)`メソッドを使って、配列の末尾に新しいアイテムを追加することができます。

```swift
shoppingList.append("Flour")
// shoppingList は 3 つのアイテムを含んでいて、誰かがパンケーキを作っています
```

他には加算代入演算子(`+=`)を使って 1 つ以上の互換性のある型のアイテムを追加できます。

```swift
shoppingList += ["Baking Powder"]
// shoppingList 4 つのアイテムを含んでいます
shoppingList += ["Chocolate Spread", "Cheese", "Butter"]
// shoppingList は 7 つのアイテムを含んでいます
```

`subscript`シンタックスを使うと、配列から値を取得します。配列名のすぐ後の角括弧([])の中に、取得したい値の index を渡します。

```swift
var firstItem = shoppingList[0]
// firstItem は "Eggs" と等しい
```

> NOTE  
> 配列の最初のアイテムのindexは`0`で`1`ではありません。SwiftのArrayはいつも0から始まるインデックス方式です。

ある index の既存の値を変更したい場合も`subscript`シンタックスを使います。

```swift
shoppingList[0] = "Six eggs"
// リストの最初のアイテムは"Eggs"ではなく、"Six eggs"
```

`subscript`シンタックスを使う時、指定した index は妥当でなければなりません。例えば、配列の最後にあたいを追加しようと`shoppingList[shoppingList.count] = "Salt"`と書くと、実行時エラーになります。

ある一定範囲の値を一度に変更するにも`subscript`シンタックスを使うことができます。これは置き換えたい値のセットの数と指定した置き換える範囲の長さが異なっていても可能です。次の例は`"Chocolate Spread"`、`"Cheese"`、`"Butter"`を`"Bananas"`、`"Apples"`に置き換えています。

```swift
shoppingList[4...6] = ["Bananas", "Apples"]
// shoppingList は 6 つのアイテムを含んでいます
```

配列の特定の index にアイテムを挿入したい場合、`insert(_:at:)`メソッドを使います。

```swift
shoppingList.insert("Maple Syrup", at: 0)
// shoppingList は 7 つのアイテムを含んでいます
// "Maple Syrup" は最初のアイテムです
```

この例では`insert(_:at:)`に`"Maple Syrup"`と index 0 を指定して、ショッピングリストの最初に新しいアイテムを挿入しています。

同様に`remove(at:)`を使って配列からアイテムを削除できます。このメソッドは特定の index のアイテムを削除し、削除したアイテムを返します(必要なければ戻り値は無視できます):

```swift
let mapleSyrup = shoppingList.remove(at: 0)
// index 0 にあったアイテムは削除されました
// shoppingList は 6 つのアイテムを含んでいますが、 Maple Syrupはありません
// mapleSyrup定数は削除した"Maple Syrup"文字列と等しい
```

> NOTE  
> 配列の既存の境界を超えたindexの値にアクセスしたり、変更したりしようとすると、実行時エラーになるでしょう。`count`プロパティとindexを比較して、indexが妥当かどうかをチェックしましょう。配列は0から始まるインデックス方式なので、妥当な最大のindexは`count - 1`です。しかし、`count`が`0`の時(配列が空の時)、妥当なindexはありません。

アイテムが削除された時、配列内のキャップは埋められ、index `0`の値は再び`"Six eggs"`になります。

```swift
firstItem = shoppingList[0]
// firstItem は "Six eggs" と等しい
```

配列の最後の値を削除したい場合、`count`プロパティを探索する必要を避けるために、`remove(at:)`よりも`removeLast()`を使います。`remove(at:)`同様に`removeLast()`も削除したアイテムを返します:

```swift
let apples = shoppingList.removeLast()
// 最後のアイテムは削除されました
// shoppingListは 5 つのアイテムを含んでいますが、 applesはありません
// apples定数は削除された"Apples"文字列と等しい
```

### Iterating Over an Array

`for-in`ループを使って配列の値全部に繰り返し処理をすることができます。

```swift
for item in shoppingList {
    print(item)
}
// Six eggs
// Milk
// Flour
// Baking Powder
// Bananas
```

各アイテムの index が必要な場合は、`enumerated()`を代わりに使いましょう。`enumerated()`は数値の index とアイテムを組み合わせたタプルを返します。数値の開始は`0`で`1`ずつ増加していきます。つまり全体を繰り返し処理すると、数値はアイテムの index と一致します。繰り返し処理の中で、index とアイテムのタプルを一時的な定数や変数に展開することができます。

```swift
for (index, value) in shoppingList.enumerated() {
    print("Item \(index + 1): \(value)")
}
// Item 1: Six eggs
// Item 2: Milk
// Item 3: Flour
// Item 4: Baking Powder
// Item 5: Bananas
```

`for-in`ループについては、[For-In Loops](./control-flow.md#for-in-loops)をご参照ください。

## Sets

セット(*set*)はコレクション内に同じ型の値を、決まった順序がない状態で、重複なく保持します。アイテムの順序が重要でない場合や、アイテムに重複がないことを確実にしたい場合に、配列(*array*)の変わりにセットを使うことができます。

> `Set`はFoundationの`NSSet`とスムーズにやりとりできるようにしています。  
> FoundationとCocoaを使った`Set`の使用方法に関しては、[Bridging Between Set and NSSet](https://developer.apple.com/documentation/swift/set#2845530)を参照ください

### Hash Values for Set Types

セットに保存する型はハッシュ化が可能でなければなりません。つまり、型はそのハッシュ値を計算する方法をセットに伝えなければなりません。ハッシュ値は、等しく比較するすべてのオブジェクトで`Int`型で、`a == b`の場合、`a`のハッシュ値は`b`のハッシュ値と等しくなります。

Swift の基本的な型(`String`、`Int`、`Double`、`Bool`など)は、デフォルトでハッシュ化が可能で、セットや辞書のキーに使うことができます。associated value を持たない列挙型の case ([Enumerations](./enumerations.md))もデフォルトでハッシュ化が可能です。

> NOTE  
> Swift標準ライブラリの`Hashable`プロトコルに適合することで、独自で作成した型をセットや辞書のキーに使用できます。`hash(into:)`メソッドの実装については、[Hashable](https://developer.apple.com/documentation/swift/hashable)を参照ください。プロトコルの適合については、[Protocols](./protocols.md)を参照ください。

### Set Type Syntax

セット(*Set*)型は`Set<Element>`と書きます。`Element`はセットが保持できる型です。配列と異なり、セットは同等のショートハンドはありません。

### Creating and Initializing an Empty Set

イニシャライザのシンタックスを使って、ある型の空のセットを生成できます。

```swift
var letters = Set<Character>()
print("letters is of type Set<Character> with \(letters.count) items.")
// Prints "letters is of type Set<Character> with 0 items."
```

> NOTE  
> `letters`変数の型はイニシャライザの型から`Set<Character>`と推論されます。

他の方法として、関数の引数や型が明示されている変数や定数など型情報が既にわかっている場合は、空の配列リテラルを使って空のセットを作成することができます。

```swift
letters.insert("a")
// letters は Character 型の値を 1 つ含んでいます
letters = []
// letters 空のセットですが、 型は Set<Character> のままです
```

### Creating a Set with an Array Literal

1 つ以上の値をセットのコレクションとして書くショートハンドとして、配列リテラルを使ってセットを初期化することもできます。

下記の例は、`favoriteGenres`という`String`の値を保持するセットを作成しています。

```swift
var favoriteGenres: Set<String> = ["Rock", "Classical", "Hip hop"]
// favoriteGenres は 3 つ の初期値で初期化されている
```

`favoriteGenres`変数は`Set<String>`と書くことで、「`String`のセット」を宣言しています。`String`型の値をしているため、このセットには`String`しか保持できません。ここでは`favoriteGenres`セットに 3 つの`String`含めた配列リテラルを使って初期化しています。

> NOTE  
> 後の例でアイテムの追加や削除を行なっているため、`favoriteGenres`は定数ではなく変数で定義されています。

セットの型は配列リテラルだけから型推論することはできず、`Set`の型を明示しなければなりません。しかし、Swift の型推論によって、1 つの型しか持たない配列リテラルの場合は、要素の型を書かなくても初期化できます。

`favoriteGenres`の初期化は下記のようにより簡単に書くことができました:

```swift
var favoriteGenres: Set = ["Rock", "Classical", "Hip hop"]
```

配列リテラルの値はすべて同じ型なので、`favoriteGenres`変数は`Set<String>`が正しい型だと推論できます。

### Accessing and Modifying a Set

メソッドやプロパティを通してセットにアクセスしたり、変更できます。

セットのアイテムの数を調べるために、読み取り専用の`count`プロパティをチェックします。

```swift
print("I have \(favoriteGenres.count) favorite music genres.")
// Prints "I have 3 favorite music genres."
```

`Bool`型の`isEmpty`プロパティは、`count`プロパティが`0`かどうかをチェックするショートハンドです。

```swift
if favoriteGenres.isEmpty {
    print("As far as music goes, I'm not picky.")
} else {
    print("I have particular music preferences.")
}
// Prints "I have particular music preferences."
```

`insert(_:)`メソッドを使って、セットに新しいアイテムを追加することができます。

```swift
favoriteGenres.insert("Jazz")
// favoriteGenres 4 つのアイテムを含んでいます
```

`remove(_:)`を使ってセットからアイテムを削除できます。セットにアイテムが存在した場合は削除し、削除したアイテムを返します。もし存在しなけば`nil`を返します。他の方法として、全アイテムを削除するには`removeAll()`を使います。

```swift
if let removedGenre = favoriteGenres.remove("Rock") {
    print("\(removedGenre)? I'm over it.")
} else {
    print("I never much cared for that.")
}
// Prints "Rock? I'm over it."
```

特定のアイテムが含まれているかどうかを調べるには、`contains(_:)`メソッドを使うことができます。

```swift
if favoriteGenres.contains("Funk") {
    print("I get up on the good foot.")
} else {
    print("It's too funky in here.")
}
// Prints "It's too funky in here."
```

### Iterating Over a Set

`for-in`ループを使ってセットの要素を繰り返し処理をすることができます。

```swift
for genre in favoriteGenres {
    print("\(genre)")
}
// Classical
// Jazz
// Hip hop
```

`for-in`ループについては、[For-In Loops](./control-flow.md#for-in-loops)をご参照ください。

Swift の Set 型は決まった順序がありません。特定の順番で値を繰り返し処理をしたい場合、`sorted()`メソッドを使うと、`<`演算子を使ってソートされた配列として要素を返します。

```swift
for genre in favoriteGenres.sorted() {
    print("\(genre)")
}
// Classical
// Hip hop
// Jazz
```

## Performing Set Operations

### Fundamental Set Operations

### Set Membership and Equality

## Dictionaries

### Dictionary Type Shorthand Syntax

### Creating an Empty Dictionary

### Creating a Dictionary with a Dictionary Literal

### Accessing and Modifying a Dictionary

### Iterating Over a Dictionary