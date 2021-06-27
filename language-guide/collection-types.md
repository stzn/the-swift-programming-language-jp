# Collection Types \(コレクション型\)

最終更新日: 2021/6/27

Swift は、配列、セット、辞書と呼ばれる 3 つの基本的な*コレクション型*を提供しています。配列は順序が決まったコレクションです。セットは値の重複と順序のないコレクションです。辞書はキーとバリューに関連性を持たせた順序のないコレクションです。

![Collection Types](../.gitbook/assets/collectiontypes_intro_2x.png)

Swift の配列、セット、辞書は、常に保持しているキーやバリューの型が明確です。つまり、間違った型の値を挿入することができません。これは、コレクションから取得する値の型がはっきりとわかっていることにもなります。

> NOTE  
> Swift の配列、セット、辞書は、*ジェネリックなコレクション*として実装されています。ジェネリックな型やコレクションについての詳細は[Generics](generics.md)を参照ください。

## Mutability of Collections\(コレクションの可変性\)

もし配列、セット、辞書を変数に代入した場合、作成されたコレクションは可変です。つまり、追加、削除、要素の変更など、後々そのコレクションを取り替え\(または変更\)できます。もし配列、セット、辞書を定数に代入した場合、コレクションは不変で、そのサイズや内容を変更できません。

> NOTE  
> コレクションの変更が必要ない場合は、不変なコレクションを作成するのが良いプラクティスです。そうすることで、コードを理解しやすくし、Swift のコンパイラもコレクションのパフォーマンスを最適化することができます。

## Arrays\(配列\)

配列は同じ型の値を順序立ったリストの中に保持します。配列の中に同じ値を複数回入れることができます。

> NOTE  
> `Array` は Foundation の `NSArray` とスムーズにやりとりできるようにしています。Foundation と Cocoa を使った `Array` の使用方法に関しては、[Bridging Between Array and NSArray](https://developer.apple.com/documentation/swift/array#2846730)を参照ください

### Array Type Shorthand Syntax\(配列型の短縮構文\)

Swift の配列の型は全体で `Array<Element>` と書きます。`Element` はその配列が保持できる値の型です。簡略記法として `[Element]` とも書けます。この 2 つの形式は機能的に同じですが、簡略記法の方が好まれ、このガイド内でも配列の型を参照する際はこちらの形式を使います。

### Creating an Empty Array\(空の配列の作成\)

イニシャライザの構文を使用して、ある型の空の配列を作成できます。

```swift
var someInts = [Int]()
print("someInts is of type [Int] with \(someInts.count) items.")
// "someInts is of type [Int] with 0 items."
```

`someInts` 変数の型は、イニシャライザから `[Int]` と推論されます。

他にも、コンテキスト的に型情報がわかっている場合\(関数の引数や既に型が決まっている変数や定数など\)は、空配列リテラルの `[]`\(空の角括弧ペア\)を使用して、空の配列を作成することができます。

```swift
someInts.append(3)
// someInts Int 型の 3 を含んでいます
someInts = []
// someInts は空の配列だけど [Int] 型
```

### Creating an Array with a Default Value\(デフォルト値を使った配列の作成\)

`Array` は同じデフォルト値を設定した特定にサイズの配列を作成するイニシャライザも提供しています。このイニシャライザに適切な型のデフォルト値\(`repeating`\)と、その値の繰り返し回数\(`count`\)を渡します。

```swift
var threeDoubles = Array(repeating: 0.0, count: 3)
// threeDoubles は [Double] 型で、 [0.0, 0.0, 0.0] と等しい
```

### Creating an Array by Adding Two Arrays Together\(2つの配列の結合\)

加算演算子\(`+`\)を使用して、既存の型互換のある 2 つの配列を合成して、新しい配列を作成することもできます。この新しい配列の型は連結させた配列の型から推論されます:

```swift
var anotherThreeDoubles = Array(repeating: 2.5, count: 3)
// anotherThreeDoubles は [Double] 型で [2.5, 2.5, 2.5] と等しい

var sixDoubles = threeDoubles + anotherThreeDoubles
// sixDoubles は [Double] と推論され、 [0.0, 0.0, 0.0, 2.5, 2.5, 2.5] と等しい
```

### Creating an Array with an Array Literal\(配列リテラルを使った配列の作成\)

配列リテラルからも配列を初期化できます。これは、1 つ以上の要素を持った配列コレクションの簡略記法です。配列リテラルはカンマ区切りの角括弧(`[]`)で囲んだ値のリストです:

![Array Literal](../.gitbook/assets/arrayliteral_2x.png)

下記の例は、`String` を保持する `shoppingList` という配列を作成しています。

```swift
var shoppingList: [String] = ["Eggs", "Milk"]
// shoppingList は 2 つの 初期値で初期化されている
```

`shoppingList` 変数は、`[String]` と書くことで「文字列を保持する配列」として宣言されています。この配列は値の型が `String` で特定されているので、`String` のみを保持することができます。ここで、`shoppingList` 配列は、配列リテラルの中で `"Eggs"` と `"Milk"` の 2 つの値で初期化しています。

> NOTE  
> 後の例でさらにアイテムを追加するので、`shoppingList` 配列は `let` で宣言された定数ではなく、`var` で変数として宣言されてます。

この例では、配列リテラルには 2 つの `String` を含んでいます。これは `shoppingList` 変数の宣言時の型\(`String` しか含めない\)に合致しているので、`shoppingList` に配列リテラルを代入して 2 つに初期値で初期化することができます。

Swift の型推論のおかげで、同じ型の値を含んだ配列リテラルで初期化するときに配列の型を書く必要はありません。先ほどの `shoppingList` の例の初期化は、より簡潔な方法で書くことができました:

```swift
var shoppingList = ["Eggs", "Milk"]
```

配列リテラルの全ての値は同じ型なので、Swift は `shoppingList` 変数で使われている型は `[String]` が適切だと推論できます。

### Accessing and Modifying an Array\(配列へのアクセスと変更\)

メソッドやプロパティ、subscript 構文を通して配列の要素へのアクセス、変更ができます。

配列のアイテムの数を調べるために、読み取り専用の `count` プロパティをチェックします。

```swift
print("The shopping list contains \(shoppingList.count) items.")
// "The shopping list contains 2 items."
```

`Bool` 型の `isEmpty` プロパティは、`count` プロパティが `0` かどうかをチェックする簡略記法です。

```swift
if shoppingList.isEmpty {
    print("The shopping list is empty.")
} else {
    print("The shopping list isn't empty.")
}
// "The shopping list isn't empty."
```

`append(_:)` メソッドを使用して、配列の末尾に新しいアイテムを追加することができます。

```swift
shoppingList.append("Flour")
// shoppingList は 3 つのアイテムを含んでいて、誰かがパンケーキを作っています
```

加算代入演算子\(`+=`\)を使用して 1 つ以上の互換性のある型のアイテムを追加することもできます。

```swift
shoppingList += ["Baking Powder"]
// shoppingList 4 つのアイテムを含んでいます
shoppingList += ["Chocolate Spread", "Cheese", "Butter"]
// shoppingList は 7 つのアイテムを含んでいます
```

subscript 構文を使用すると、配列から値を取得します。配列名のすぐ後の角括弧\(`[]`\)の中に、取得したい値のインデックスを渡します。

```swift
var firstItem = shoppingList[0]
// firstItem は "Eggs" と等しい
```

> NOTE  
> 配列の最初のアイテムのindexは `0` であって `1` ではありません。SwiftのArrayはいつも 0 から始まるインデックス方式です。

あるインデックスの既存の値を変更したい場合も subscript 構文を使います。

```swift
shoppingList[0] = "Six eggs"
// リストの最初のアイテムは "Eggs" ではなく、 "Six eggs"
```

subscript 構文を使用するとき、指定したインデックスは値の存在する範囲内でなければなりません。例えば、配列の最後に値を追加しようとして `shoppingList[shoppingList.count] = "Salt"` と書くと、実行時エラーになります。

ある一定範囲の値を一度に変更する場合にも、subscript 構文を使用することができます。これは置き換えたい値のセットの数と指定した置き換える範囲の長さが異なっていても可能です。次の例は `"Chocolate Spread"`、`"Cheese"`、`"Butter"` を `"Bananas"`、`"Apples"` に置き換えています。

```swift
shoppingList[4...6] = ["Bananas", "Apples"]
// shoppingList は 6 つのアイテムを含んでいます
```

配列の特定のインデックスにアイテムを挿入したい場合、`insert(_:at:)` メソッドを使います。

```swift
shoppingList.insert("Maple Syrup", at: 0)
// shoppingList は 7 つのアイテムを含んでいます
// "Maple Syrup" は最初のアイテムです
```

この例では `insert(_:at:)` に `"Maple Syrup"` とインデックス 0 を指定して、ショッピングリストの先頭に新しいアイテムを挿入しています。

同様に `remove(at:)` を使用して配列からアイテムを削除できます。このメソッドは特定のインデックスのアイテムを削除し、削除したアイテムを返します\(必要なければ戻り値は無視できます\):

```swift
let mapleSyrup = shoppingList.remove(at: 0)
// インデックス 0 にあったアイテムは削除されました
// shoppingList は 6 つのアイテムを含んでいますが、 Maple Syrupはありません
// mapleSyrup 定数は削除した "Maple Syrup" 文字列と等しい
```

> NOTE  
> 配列の既存の境界を超えたインデックスの値にアクセスしたり、変更したりしようとすると、実行時エラーになるでしょう。`count` プロパティとインデックスを比較して、インデックスが妥当かどうかをチェックしてください。配列は 0 から始まるインデックス方式なので、妥当な最大のインデックスは `count - 1` です。しかし、`count` が `0` のとき\(配列が空のとき\)、妥当なインデックスは存在しません。

アイテムが削除された時、配列内の隙間は埋められ、インデックス `0` の値は再び `"Six eggs"` になります。

```swift
firstItem = shoppingList[0]
// firstItem は "Six eggs" と等しい
```

配列の最後の値を削除したい場合、`count` プロパティを探すコストを避けるためには、`remove(at:)` よりも `removeLast()` を使います。`remove(at:)` と同様に `removeLast()` も削除したアイテムを返します:

```swift
let apples = shoppingList.removeLast()
// 最後のアイテムは削除されました
// shoppingList は 5 つのアイテムを含んでいますが、 apples はありません
// apples 定数は削除された "Apples" 文字列と等しい
```

### Iterating Over an Array\(配列の繰り返し処理\)

`for-in` ループを使用して配列の値全部に繰り返し処理をすることができます。

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

各アイテムのインデックスが必要な場合は、`enumerated()` を代わりに使いましょう。`enumerated()` は数値のインデックスとアイテムを組み合わせたタプルを返します。数値の開始は `0` で、`1` ずつ増加していきます。つまり全体を繰り返し処理すると、数値はアイテムのインデックスと一致します。繰り返し処理の中で、インデックスとアイテムのタプルを一時的な定数や変数に展開することができます。

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

`for-in` ループについては、[For-In Loops](control-flow.md#for-in-loopsFor-Inループ)を参照ください。

## Sets\(セット\)

*セット*はコレクション内に、同じ型の値を、決まった順序と値の重複なしに保持します。アイテムの順序が重要でない場合や、アイテムに重複がないことを保証したい場合に、配列の変わりにセットを使用することができます。

> `Set` は Foundation の `NSSet` とスムーズにやりとりできるようにしています。Foundation と Cocoa を使った `Set` の使用方法に関しては、[Bridging Between Set and NSSet](https://developer.apple.com/documentation/swift/set#2845530)を参照ください

### Hash Values for Set Types\(セット型のハッシュ値\)

セットに保存する型はハッシュ化が可能でなければなりません。つまり、その型はハッシュ値を計算する方法をセットに知らせる必要があります。ハッシュ値は、`Int` 型で、等価比較が可能な全てのオブジェクトで、例えば `a == b` の場合、`a` のハッシュ値は `b` のハッシュ値と等しくなります。

Swift の基本的な型\(`String`、`Int`、`Double`、`Bool` など\)は、デフォルトでハッシュ化が可能で、セットや辞書のキーに使用することができます。関連値を持たない列挙型のケース\([Enumerations](enumerations.md)\)もデフォルトでハッシュ化が可能です。

> NOTE  
> Swift 標準ライブラリの `Hashable` プロトコルに準拠することで、独自で作成した型をセットや辞書のキーに使用できます。`hash(into:)` メソッドの実装については、[Hashable](https://developer.apple.com/documentation/swift/hashable)を参照ください。プロトコルの準拠については、[Protocols](protocols.md)を参照ください。

### Set Type Syntax\(セット型構文\)

セット型は `Set<Element>` と書きます。`Element` はセットが保持できる型です。セットには、配列のような簡略記法\(`[Element]`\)はありません。

### Creating and Initializing an Empty Set\(空のセットの作成と初期化\)

イニシャライザの構文を使用して、ある型の空のセットを作成できます。

```swift
var letters = Set<Character>()
print("letters is of type Set<Character> with \(letters.count) items.")
// "letters is of type Set<Character> with 0 items."
```

> NOTE  
> `letters` 変数の型はイニシャライザの型から `Set<Character>` と推論されます。

他の方法として、関数の引数や型が明示されている変数や定数など型情報が既にわかっている場合は、空の配列リテラルを使用して空のセットを作成することができます。

```swift
letters.insert("a")
// letters は Character 型の値を 1 つ含んでいます
letters = []
// letters は空のセットですが、 型は Set<Character> のままです
```

### Creating a Set with an Array Literal\(配列リテラルを使ったセットの作成\)

簡略記法として、1 つ以上の値を配列リテラルを使用してセットを初期化することもできます。

下記の例は、`favoriteGenres` という `String` の値を保持するセットを作成しています。

```swift
var favoriteGenres: Set<String> = ["Rock", "Classical", "Hip hop"]
// favoriteGenres は 3 つ の初期値で初期化されている
```

`favoriteGenres` 変数は `Set<String>` と書くことで、「`String` のセット」を宣言しています。`String` 型の値を保持しているため、このセットには `String` しか保持できません。ここでは `favoriteGenres` セットに 3 つの `String` を含めた配列リテラルを使用して初期化しています。

> NOTE  
> 後の例でアイテムの追加や削除を行なっているため、`favoriteGenres` は定数ではなく変数で定義されています。

セットの型は配列リテラルのみからは型推論することはできず、`Set` を明示しなければなりません。しかし、Swift の型推論によって、1 つの型しか持たない配列リテラルの場合は、要素の型を書かなくても初期化できます。

`favoriteGenres` の初期化は下記のようにより簡単に書くことができました:

```swift
var favoriteGenres: Set = ["Rock", "Classical", "Hip hop"]
```

配列リテラルの値は全て同じ型なので、`favoriteGenres` 変数は `Set<String>` が正しい型だと推論できます。

### Accessing and Modifying a Set\(セットへのアクセスと変更\)

メソッドやプロパティを通してセットにアクセスしたり、変更できます。

セットのアイテムの数を調べるために、読み取り専用の `count` プロパティをチェックします。

```swift
print("I have \(favoriteGenres.count) favorite music genres.")
// "I have 3 favorite music genres."
```

`Bool` 型の `isEmpty` プロパティは、`count` プロパティが `0` かどうかをチェックする簡略記法です。

```swift
if favoriteGenres.isEmpty {
    print("As far as music goes, I'm not picky.")
} else {
    print("I have particular music preferences.")
}
// "I have particular music preferences."
```

`insert(_:)` メソッドを使用して、セットに新しいアイテムを追加することができます。

```swift
favoriteGenres.insert("Jazz")
// favoriteGenres 4 つのアイテムを含んでいます
```

`remove(_:)` を使用してセットからアイテムを削除できます。セットにアイテムが存在した場合は削除し、削除したアイテムを返します。もし存在しなけば `nil` を返します。他の方法として、全アイテムを削除するには `removeAll()` を使います。

```swift
if let removedGenre = favoriteGenres.remove("Rock") {
    print("\(removedGenre)? I'm over it.")
} else {
    print("I never much cared for that.")
}
// "Rock? I'm over it."
```

特定のアイテムが含まれているかどうかを調べるには、`contains(_:)` メソッドを使用することができます。

```swift
if favoriteGenres.contains("Funk") {
    print("I get up on the good foot.")
} else {
    print("It's too funky in here.")
}
// "It's too funky in here."
```

### Iterating Over a Set\(セットの繰り返し処理\)

`for-in` ループを使用してセットの要素を繰り返し処理することができます。

```swift
for genre in favoriteGenres {
    print("\(genre)")
}
// Classical
// Jazz
// Hip hop
```

`for-in` ループについては、[For-In Loops](control-flow.md#for-in-loopsFor-Inループ)を参照ください。

Swift の Set 型は決まった順序がありません。特定の順番で値を繰り返し処理したい場合、`sorted()` メソッドを使用すると、`<` 演算子を使用してソートされた配列として要素を返します。

```swift
for genre in favoriteGenres.sorted() {
    print("\(genre)")
}
// Classical
// Hip hop
// Jazz
```

## Performing Set Operations\(セットの操作\)

2 つのセットの、合成、共通の要素の発見、値が全部等しいのか、いくつか等しいのか、全く違うのかの比較など、基本的なセットの操作を効率的に行うことができます。

### Fundamental Set Operations\(基本的なセットの操作\)

下記のイラストでは、`a` と `b` の 2 つのセットに対して、それぞれ操作を行い、色の付いた部分が結果\(戻り値\)を表しています。

![Setのベン図](../.gitbook/assets/setvenndiagram_2x.png)

* `intersection(_:)` は、2 つのセットの共通要素を含めた新しいセットを作成します
* `symmetricDifference(_:)` は、どちらかのセットにあるものの、両方には含まれていない要素を含めた新しいセットを作成します
* `union(_:)` は、両方のセットに含まれている全ての要素を含めた新しいセットを作成します
* `subtracting(_:)` は、特定のセットには含まれていない要素を含めた新しいセットを作成します

```swift
let oddDigits: Set = [1, 3, 5, 7, 9]
let evenDigits: Set = [0, 2, 4, 6, 8]
let singleDigitPrimeNumbers: Set = [2, 3, 5, 7]

oddDigits.union(evenDigits).sorted()
// [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
oddDigits.intersection(evenDigits).sorted()
// []
oddDigits.subtracting(singleDigitPrimeNumbers).sorted()
// [1, 9]
oddDigits.symmetricDifference(singleDigitPrimeNumbers).sorted()
// [1, 2, 9]
```

### Set Membership and Equality\(セットの要素と等価性\)

下記のイラストでは、`a`、`b`、`c` の 3 つのセットで共有している要素の領域を表しています。
`a` は、`b` に含まれる全ての要素を含んでいるため、`b` の*上位集合*です。反対に、`b` は `a` の*下位集合*です。`b` と `c` は、共通の要素がないため、互いに*素*です。

![Setのオイラー図](../.gitbook/assets/seteulerdiagram_2x.png)

* 等号演算子(`==`)を使用して、2 つのセットの要素が全て同じかどうかを判定できます
* `isSubset(of:)` を使用してあるセットの要素が他のセットに全て含まれているかどうかを判定できます
* `isSuperset(of:)` を使用してあるセットが他のセットの全ての要素を含んでいるかどうかを判定できます
* `isStrictSubset(of:)` や `isStrictSuperset(of:)` は、あるセットが他のセットの上位集合か下位集合かどうかを判定できます\(等しい場合は `false` です\)
* `isDisjoint(with:)` は 2 つのセットに共通要素が全くないかどうかを判定できます

```swift
let houseAnimals: Set = ["🐶", "🐱"]
let farmAnimals: Set = ["🐮", "🐔", "🐑", "🐶", "🐱"]
let cityAnimals: Set = ["🐦", "🐭"]

houseAnimals.isSubset(of: farmAnimals)
// true
farmAnimals.isSuperset(of: houseAnimals)
// true
farmAnimals.isDisjoint(with: cityAnimals)
// true
```

## Dictionaries\(辞書\)

*辞書*は、特定の型のキーと、特定の型のバリューをペアとして関連づけ、順序なしでコレクションに保持します。それぞれのアイテムは、辞書内で識別子の役割を果たす一意なキーに紐づいています。配列のアイテムとは異なり、辞書のアイテムに順序はありません。辞書は、現実に特定の単語を辞書で調べるのと同じように、識別子から値を探したいときに使います。

> NOTE  
> `Dictionary` は Foundation の `NSDictionary` とスムーズにやりとりできるようにしています。Foundation と Cocoa を使った `Dictionary` の使用方法に関しては、[Bridging Between Dictionary and NSDictionary](https://developer.apple.com/documentation/swift/dictionary#2846239)を参照ください

### Dictionary Type Shorthand Syntax\(辞書型の短縮構文\)

Swift の辞書は、全体で `Dictionary<Key, Value>` と書きます。`Key` には辞書のキーとして使える値の型を、`Value` にはそのキーに紐づいた辞書で保持するバリューの型です。

> NOTE  
> 辞書の `Key` はセットの値のように、`Hashable` プロトコルに準拠しなければなりません。

辞書の型は、`[Key: Value]` と簡略記法で書くこともできます。機能的にはどちらの書き方も同じですが、簡略記法の方が好ましく、このガイド内では、辞書の型を参照する際はこちらの形式を使います。

### Creating an Empty Dictionary\(空の辞書型の作成\)

配列と同様に、ある型の空の `Dictionary` をイニシャライザの構文を使用して作成できます。

```swift
var namesOfIntegers = [Int: String]()
// namesOfIntegers は空の [Int: String] 辞書
```

この例では、数字を人が理解できる名前として保持するために、`[Int: String]` 型の空の辞書を作成しています。キーは `Int` 型でバリューは `String` 型です。

既に型情報が分かっている場合は、空の辞書リテラルを `[:]` と書くことができます。(角括弧(`[]`)の中にコロン(`:`)):

```swift
namesOfIntegers[16] = "sixteen"
// namesOfIntegers は 1 つのキーバリューペアを含んでいます
namesOfIntegers = [:]
// namesOfIntegers は再び [Int: String] の空の辞書
```

### Creating a Dictionary with a Dictionary Literal\(辞書リテラルを使った辞書の作成\)

先ほど見た配列リテラルと似たような構文の*辞書リテラル*を使った初期化もできます。辞書リテラルは、1 つ以上のキーバリューペアから `Dictionary` のコレクションを作成する簡略記法です。

*キーバリューペア*は、キーとバリューの組み合わせです。辞書リテラルは、それぞれのキーバリューペアのキーとバリューをコロン\(`:`\)で分けます。複数のキーバリューペアは、カンマ\(`,`\)区切りのリストを角括弧\(`[]`\)で囲みます:

![辞書の作成](../.gitbook/assets/04_dictionarycreating.png)

下記の例では、国際空港の名前を保持する辞書を作成します。この辞書は、3 文字の国際空港コードをキーに、空港名をバリューにしています。

```swift
var airports: [String: String] = ["YYZ": "Toronto Pearson", "DUB": "Dublin"]
```

`airports` 辞書は、`[String: String]` 型と宣言され、「`String` 型のキーと `String` 型のバリューの `Dictionary`」を意味します。

> NOTE  
> 後の例でさらに空港を追加するため、`airports` 辞書は定数ではなく変数で定義されています。

`airports` 辞書は、2 つのキーバリューペアの辞書リテラルで初期化されています。最初のペアのキーは `"YYZ"`、バリューは `"Toronto Pearson"`、2 番目のペアのキーは `"DUB"`、バリューは `"Dublin"` です。

この辞書リテラルは、`String: String` 型のペアを含んでいます。このキーバリューペアは `airports` 変数の宣言\(`String` 型のキーと `String` 型のバリューしか持つことができない辞書\)と一致するので、2 つの初期値を持つ `airports` 辞書を初期化するために、この辞書リテラルを代入することができます。

配列と同様に、キーとバリューの型がはっきりと分かる辞書リテラルを使用して初期化している場合、型を書く必要はありません。`airports` の初期化は、より簡潔に書くことができました:

```swift
var airports = ["YYZ": "Toronto Pearson", "DUB": "Dublin"]
```

キーの型も、バリューの型も全ての同じなので、Swift は `airports` 辞書は `[String: String]` が正しい型だと推論できます。

### Accessing and Modifying a Dictionary\(辞書へのアクセスと変更\)

メソッドやプロパティ、subscript 構文を通して、辞書へのアクセス、変更ができます。

配列と同様に、`Dictionary` のアイテムの数を調べるために、読み取り専用の `count` プロパティをチェックします。

```swift
print("The airports dictionary contains \(airports.count) items.")
// "The airports dictionary contains 2 items."
```

`Bool` 型の `isEmpty` プロパティは、`count` プロパティが `0` かどうかをチェックする簡略記法です。

```swift
if airports.isEmpty {
    print("The airports dictionary is empty.")
} else {
    print("The airports dictionary isn't empty.")
}
// "The airports dictionary isn't empty."
```

subscript 構文を使用して、新しいアイテムを追加することができます。適切な型の新しいキーを subscript のインデックスに入れ、適切な型の新しいバリューを代入できます。

```swift
airports["LHR"] = "London"
// airports 辞書は 3 つのアイテムを含んでいます
```

他には加算代入演算子\(`+=`\)を使用して 1 つ以上の互換性のある型のアイテムを追加できます。

```swift
shoppingList += ["Baking Powder"]
// shoppingList 4 つのアイテムを含んでいます
shoppingList += ["Chocolate Spread", "Cheese", "Butter"]
// shoppingList は 7 つのアイテムを含んでいます
```

subscript 構文を使用すると、特定のキーのバリューを変更することもできます。

```swift
airports["LHR"] = "London Heathrow"
// "LHR"の値は "London Heathrow" へ変更
```

別の方法として、`updateValue(_:forKey:)` メソッドを使用して、特定のキーのバリューの設定/更新ができます。subscript と同様に `updateValue(_:forKey:)` は、特定のキーのバリューがなければ新しく値を設定し、既にバリューが存在していたら更新します。subscript と違う点として、`updateValue(_:forKey:)` メソッドは、更新後に前の値を返します。こうすることで、更新が実際に起きたかどうかが確認できます。

`updateValue(_:forKey:)` メソッドは、辞書のバリューの型のオプショナル値を返します。例えば辞書が `String` を保持している場合、このメソッドは `String?`\(オプショナル `String`\)を返します。このオプショナル値は更新前にバリューが存在していた場合はその値のオプショナルを返し、存在していなかった場合は `nil` を返します:

```swift
if let oldValue = airports.updateValue("Dublin Airport", forKey: "DUB") {
    print("The old value for DUB was \(oldValue).")
}
// "The old value for DUB was Dublin."
```

subscript 構文を使用して特定のキーのバリューを取得することもできます。バリューの存在しないキーに対してもリクエストすることが可能で、バリューの型のオプショナル値を返します。存在しなければ `nil` を返します:

```swift
if let airportName = airports["DUB"] {
    print("The name of the airport is \(airportName).")
} else {
    print("That airport isn't in the airports dictionary.")
}
// "The name of the airport is Dublin Airport."
```

subscript 構文を使用して、キーに `nil` を代入することで、キーバリューペアを削除できます:

```swift
airports["APL"] = "Apple International"
// "Apple International" APL の空港ではないので削除します
airports["APL"] = nil
// APL は辞書から削除されました
```

他の方法として、`removeValue(forKey:)` メソッドを使用してキーバリューペアを削除できます。バリューが存在すれば削除した値を返し、存在しなければ `nil` を返します:

```swift
if let removedValue = airports.removeValue(forKey: "DUB") {
    print("The removed airport's name is \(removedValue).")
} else {
    print("The airports dictionary doesn't contain a value for DUB.")
}
// "The removed airport's name is Dublin Airport."
```

### Iterating Over a Dictionary\(辞書の繰り返し処理\)

`for-in` ループを使用して辞書のキーバリューペア全部に繰り返し処理することができます。

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

各アイテムのインデックスが必要な場合は、`enumerated()` を代わりに使いましょう。各アイテムは `(key, value)` のタプルを返し、繰り返し処理の中で、このタプルを一時的な定数や変数に展開することができます。

```swift
for (airportCode, airportName) in airports {
    print("\(airportCode): \(airportName)")
}
// LHR: London Heathrow
// YYZ: Toronto Pearson
```

`for-in` ループについては、[For-In Loops](control-flow.md#for-in-loopsFor-Inループ)を参照ください。

`keys` と `values` プロパティを使用して、キーとバリューそれぞれのリストを取得することもできます:

```swift
for airportCode in airports.keys {
    print("Airport code: \(airportCode)")
}
// Airport code: LHR
// Airport code: YYZ

for airportName in airports.values {
    print("Airport name: \(airportName)")
}
// Airport name: London Heathrow
// Airport name: Toronto Pearson
```

`Array` インスタンスを引数に受け取る API で辞書のキーとバリューを使いたい場合、`keys` と `values` プロパティを使用して新しい配列を初期化してください:

```swift
let airportCodes = [String](airports.keys)
// airportCodes is ["LHR", "YYZ"]

let airportNames = [String](airports.values)
// airportNames is ["London Heathrow", "Toronto Pearson"]
```

`Dictionary` 型には決まった順序がありません。特定の順番で値を繰り返し処理をしたい場合、`keys` か `values` プロパティの `sorted()` メソッドを使いましょう。
