# Generics

最終更新日:

ジェネリックなコードを使用すると、定義した要件に従って、任意の型で機能する柔軟で再利用可能な関数と型を作成できます。重複を避け、その意図を明確で抽象的な方法で表現するコードを書くことができます。

ジェネリクス(*Generics*)は Swift の最も強力な機能の 1 つで、Swift 標準ライブラリの多くはジェネリック コードで構築されています。気づかないかもしれませんが、この言語ガイド全体でもジェネリクスを使用しています。例えば、Swift の `Array` 型と `Dictionary` 型はどちらもジェネリックコレクションです。`Int` 値を保持する配列、`String` 値を保持する配列、または Swift で作成できる他のどんな型の配列を作成できます。同様に、指定された型の値を格納する辞書を作成でき、その型に制限はありません。

## The Problem That Generics Solve(ジェネリクスが解決する問題)

これは `swapTwoInts(_:_:)` と呼ばれる標準の非ジェネリック関数で、2 つの `Int` 値を交換します:

```swift
func swapTwoInts(_ a: inout Int, _ b: inout Int) {
    let temporaryA = a
    a = b
    b = temporaryA
}
```

この関数は、[In-Out Parameters](./functions.md#in-Out-ParametersIn-Out引数)で説明されているように、inout 引数を使用して `a` と `b` の値を交換します。

`swapTwoInts(_:_:)` 関数は、`b` の元の値を `a` に、`a` の元の値を `b` に入れ替えます。この関数を呼び出して、2 つの `Int` 変数の値を交換できます:

```swift
var someInt = 3
var anotherInt = 107
swapTwoInts(&someInt, &anotherInt)
print("someInt is now \(someInt), and anotherInt is now \(anotherInt)")
// "someInt is now 107, and anotherInt is now 3"
```

`swapTwoInts(_:_:)` 関数は便利ですが、`Int` 値でのみ使用できます。2 つの `String` 値または 2 つの `Double` 値を交換する場合は、以下に示す `swapTwoStrings(_:_:)` および `swapTwoDoubles(_:_:)` 関数などの関数をさらに作成する必要があります:

```swift
func swapTwoStrings(_ a: inout String, _ b: inout String) {
    let temporaryA = a
    a = b
    b = temporaryA
}

func swapTwoDoubles(_ a: inout Double, _ b: inout Double) {
    let temporaryA = a
    a = b
    b = temporaryA
}
```

`swapTwoInts(_:_:)`、`swapTwoStrings(_:_:)`、および `swapTwoDoubles(_:_:)` 関数の本体が同一なことに気付いたかもしれません。唯一の違いは、受け入れられる値の型(`Int`、`String`、および `Double`)です。

任意の型の 2 つの値を交換する 1 つの関数を作成する方が、より便利で、はるかに柔軟です。ジェネリックコードを使用すると、そのような関数を作成できます。(これらの関数の一般的なバージョンは、下記で定義されています)

> NOTE  
> 3 つの関数全てで、`a` と `b` の型は同じでなければなりません。`a` と `b` が同じ型でない場合、それらの値を交換することはできません。Swift は型安全な言語であり、(例えば)`String` 型の変数と `Double` 型の変数が互いに値を交換することを許可しません。そうしようとすると、コンパイルエラーが発生します。

## Generic Functions(ジェネリック関数)

ジェネリック関数は、どの型でも機能します。これは、上記の `swapTwoInts(_:_:)` 関数の一般的なバージョンで、`swapTwoValues(_:_:)` と呼ばれます:

```swift
func swapTwoValues<T>(_ a: inout T, _ b: inout T) {
    let temporaryA = a
    a = b
    b = temporaryA
}
```

`swapTwoValues(_:_:)` 関数の本体は、`swapTwoInts(_:_:)` 関数の本文と同じです。ただし、`swapTwoValues(_:_:)` の最初の行は    `swapTwoInts(_:_:)` とは少し異なります。最初の行を比較すると次のようになります:

```swift
func swapTwoInts(_ a: inout Int, _ b: inout Int)
func swapTwoValues<T>(_ a: inout T, _ b: inout T)
```

ジェネリックバージョンの関数は、実際の型名(`Int`、`String`、`Double` など)の代わりにプレースホルダ型名 (この場合は `T`) を使用します。プレースホルダの型名は、`T` に何が必要かについては何も言いませんが、`a` と `b` の両方が、`T` が表すものにかかわらず、同じ型 `T` でなければならないことを示しています。`T` の代わりに使用する実際の型は、`swapTwoValues(_:_:)` 関数が呼び出される度に決定されます。

ジェネリック関数と非ジェネリック関数のもう 1 つの違いは、ジェネリック関数の名前(`swapTwoValues(_:_:)`)の後に山かっこ(`<T>`)内にプレースホルダの型名(`T`)が続くことです。山かっこは、`T` が `swapTwoValues(_:_:)` 関数定義内のプレースホルダ型名なことを Swift に伝えます。`T` はプレースホルダのため、Swift は `T` という実際の型を探しません。

`swapTwoValues(_:_:)` 関数は `swapTwoInts` と同じ方法で呼び出すことができますが、2 つの値が互いに同じ型の限り、任意の型の 2 つの値を渡すことができます。`swapTwoValues(_:_:)` が呼び出されるたびに、`T` に使用する型は、関数に渡される値の型から推論されます。

下記の 2 つの例では、`T` はそれぞれ `Int` と `String` と推論されます:

```swift
var someInt = 3
var anotherInt = 107
swapTwoValues(&someInt, &anotherInt)
// someInt は 107 で anotherInt は 3

var someString = "hello"
var anotherString = "world"
swapTwoValues(&someString, &anotherString)
// someString は "world" で anotherString は "hello"
```

> NOTE  
> 上記で定義された `swapTwoValues(_:_:)` 関数は、Swift 標準ライブラリの `swap` と呼ばれる自動で利用可能なジェネリック関数から来ています。独自のコードで `swapTwoValues(_:_:)` 関数の動作が必要な場合は、独自の実装を提供するのではなく、Swift の既存の `swap(_:_:)` 関数を使用できます。

## Type Parameters(型引数)

上記の `swapTwoValues(_:_:)` の例では、プレースホルダ型 `T` は型引数の例です。型引数は、プレースホルダ型を指定して名前を付け、関数名の直後、一致する山括弧のペア(`<T>` など) の間に記述されます。

型引数を指定すると、それを使用して関数の引数の型(`swapTwoValues(_:_:)` 関数の `a` および `b` 引数など)を定義したり、関数の戻り型として、または関数の本文内のアノテーションの型として定義できます。いずれの場合も、関数が呼び出される度に、型引数が実際の型に置き換えられます。(上記の `swapTwoValues(_:_:)` の例では、関数が最初に呼び出されたときに `T` が `Int` に置き換えられ、2 回目に呼び出されたときは `String` に置き換えられました)

山かっこ内に複数の型引数名をカンマ(`,`)で区切って記述することにより、複数の型引数を指定できます。

## Naming Type Parameters(型引数の命名)

ほとんどの場合、型引数には、`Dictionary<Key, Value>` の `Key` と `Value`、`Array<Element>` の `Element` などのその型引数の意味を表す名前が付いています。ただし、型引数に意味がない場合、上記の `swapTwoValues(_:_:)` 関数の `T` など、`T`、`U`、`V` などの 1 文字の名前を付けるのが伝統的です。

> NOTE  
> 型引数には常に大文字のキャメルケース名(`T` や `MyTypeParameter` など)を指定して、それらが値ではなく型のプレースホルダであることを示します。

## Generic Types(ジェネリック型)

ジェネリック関数に加えて、Swift では独自のジェネリック型を定義できます。これらは、`Array` や `Dictionary` と同様の方法で、独自のクラス、構造体、および列挙型の任意の型で機能します。

このセクションでは、`Stack` と呼ばれる一般的なコレクション型を作成する方法を示します。スタックは、配列に似た順序付けられた値のセットですが、`Array` 型よりも操作が制限されています。配列を使用すると、配列内の任意の場所で新しい項目を挿入および削除できます。ただし、スタックでは、コレクションの最後にのみ新しい項目を追加できます(スタックに新しい値をプッシュする(*pushing*)と呼ばれています)。同様に、スタックでは、コレクションの最後からのみアイテムを削除できます(スタックから値をポップする(*popping*)と呼ばれています)。

> NOTE  
> `UINavigationController` クラスは、スタックの概念を使用して、ナビゲーション階層内の view controller をモデル化します。`UINavigationController` クラスの `pushViewController(_:animated:)` メソッドを呼び出して view controller をナビゲーションスタックに追加(プッシュ)し、popViewControllerAnimated(_:)    メソッドを呼び出してナビゲーションスタックから view controller を削除(ポップ)します。スタックは、コレクションの管理に厳密な「後入れ先出し(LIFO)」アプローチが必要な場合に役立つコレクションモデルです。

下記の図は、スタックのプッシュとポップの動作を示しています:

![スタックのプッシュとポップ](./../.gitbook/assets/stackPushPop_2x.png)

1. 現在、スタックには 3 つの値があります
2. 4 番目の値はスタックの一番上にプッシュされます
3. スタックには 4 つの値が保持され、最新の値が一番上になります
4. スタックの一番上のアイテムがポップされます
5. 値をポップした後、スタックは再び 3 つの値を保持します

スタックの非ジェネリックバージョンの `Int` のスタックを作成する方法は次のとおりです。

```swift
struct IntStack {
    var items = [Int]()
    mutating func push(_ item: Int) {
        items.append(item)
    }
    mutating func pop() -> Int {
        return items.removeLast()
    }
}
```

この構造体は、`items` と呼ばれる `Array` プロパティを使用して、値をスタックに格納します。`Stack` には、値をスタックにプッシュおよびポップするための 2 つのメソッド、`push` と `pop` が用意されています。これらのメソッドは、構造体の `items` 配列を変更する必要があるため、`mutating` とマークされています。

ただし、上記の `IntStack` 型は `Int` 値でのみ使用できます。任意の型の値のスタックを管理できるジェネリックな `Stack` クラスを定義する方がはるかに便利です。

同じコードのジェネリックなバージョンを次に示します:

```swift
struct Stack<Element> {
    var items = [Element]()
    mutating func push(_ item: Element) {
        items.append(item)
    }
    mutating func pop() -> Element {
        return items.removeLast()
    }
}
```

Stack のジェネリックバージョンは、本質的に非ジェネリックバージョンと同じですが、実際の型 `Int` の代わりに `Element` という型引数を使用していることに注意してください。この型引数は、構造体の名前の直後に、一対の山かっこ (`<Element>`) 内に記述されます。

`Element` は、後で提供される型のプレースホルダ名を定義します。この未来の型は、構造体の定義内のどこでも `Element` として参照できます。この場合、`Element` は 3 つの場所でプレースホルダとして使用されてます:

* `Element` 型の値の空の配列で初期化される、`items` というプロパティを作成します
* `push(_:)` メソッドに `item` と呼ばれる `Element` 型の単一の引数を指定しなければなりません
* `pop()` メソッドの戻り値が `Element` 型を指定しなければなりません

ジェネリック型のため、`Stack` を使用して、`Array` および `Dictionary` と同様の方法で、Swift で有効な型のスタックを作成できます。

スタックに格納する型を山かっこ(`<>`)で囲んで、新しいスタック インスタンスを作成します。例えば、文字列の新しいスタックを作成するには、`Stack<String>()` を記述します。

```swift
var stackOfStrings = Stack<String>()
stackOfStrings.push("uno")
stackOfStrings.push("dos")
stackOfStrings.push("tres")
stackOfStrings.push("cuatro")
// スタックは 4 つの文字列を格納しています
```

これらの 4 つの値をスタックにプッシュした後、`stackOfStrings` がどのように見えるかを次に示します:

![4つの値をスタックにプッシュ](./../.gitbook/assets/stackPushedFourStrings_2x.png)

スタックから値をポップすると、最上位の値 `"cuatro"` が削除されて返されます:

```swift
let fromTheTop = stackOfStrings.pop()
// fromTheTop は "cuatro" に等しく、スタックは 3 つの文字列を含んでいます
```

最上位の値をポップした後のスタックは次のとおりです:

![ポップした後のスタック](./../.gitbook/assets/stackPoppedOneString_2x.png)

## Extending a Generic Type(ジェネリック型の拡張)

ジェネリック型を拡張する場合、extension の定義で型引数リストを提供しません。代わりに、元の型定義の型引数リストは extension の本文内でも使用でき、元の型引数名は元の定義の型引数を参照するために使用します。

次の例では、ジェネリックな Stack 型を拡張して、`topItem` と呼ばれる読み取り専用の計算プロパティを追加します。これは、スタックからポップせずにスタックの一番上のアイテムを返します:

```swift
extension Stack {
    var topItem: Element? {
        return items.isEmpty ? nil : items[items.count - 1]
    }
}
```

`topItem` プロパティは、`Element` 型のオプショナルの値を返します。スタックが空の場合、`topItem` は `nil` を返します。スタックが空でない場合、`topItem` は `items` 配列の最後の項目を返します。

この extension は型引数リストを定義していないことに注目してください。代わりに、`Stack` 型の既存の型引数名の `Element` が extension 内で使用され、`topItem 計算プロパティのオプショナルの型を示しています。

任意の `Stack` インスタンスで `topItem` 計算プロパティを使用して最上位のアイテムを削除せずにアクセスできるようになりました。

```swift
if let topItem = stackOfStrings.topItem {
    print("The top item on the stack is \(topItem).")
}
// "The top item on the stack is tres."
```

ジェネリック型の extension には、下記の[Extensions with a Generic Where Clause](#extensions-with-a-generic-where-clauseジェネリックwhere句を使った拡張)で説明されているように、新しい機能を取得するために拡張した型のインスタンスが満たさなければならない要件を含めることもできます。

## Type Constraints(型制約)

`swapTwoValues(_:_:)` 関数と `Stack` 型は、どの型でも機能します。ただし、ジェネリック関数およびジェネリック型で使用できる型に特定の型制約を適用すると便利な場合があります。型制約は、型引数が特定のクラスから継承するか、特定のプロトコルまたは合成したプロトコルに準拠する必要があることを指定できます。

例えば、Swift の `Dictionary` 型では、辞書のキーとして使用できる型に制限があります。[Dictionaries](./collection-types.md#dictionaries辞書)で説明されているように、辞書のキーの型はハッシュ可能でなければなりません。つまり、個々のキーがユニークなことを表明する方法を提供する必要があります。`Dictionary` は、特定のキーの値がすでに含まれているかどうかを確認できるように、そのキーがハッシュ可能な必要があります。この要件がなければ、`Dictionary` は特定のキーの値を挿入または置換するべきかどうかを判断できず、すでに存在する特定のキーの値を見つけることもできません。

この要件は、`Dictionary` のキーの型制約によって強制されます。これは、キーの型が Swift 標準ライブラリで定義された `Hashable` プロトコルに準拠する必要があることを指定します。Swift の全ての基本型(`String`、`Int`、`Double`、`Bool` など)は、デフォルトでハッシュ可能です。独自の型を `Hashable` プロトコルに準拠させる方法については、[Conforming to the Hashable Protocol](https://developer.apple.com/documentation/swift/hashable#2849490)を参照ください。

独自のジェネリック型を作成するときに、独自の型制約を定義できます。これらの制約は、多くのジェネリックプログラミングの強力な機能を提供します。`Hashable` のような抽象的な概念は、具体的な型ではなく、概念的な特性の観点から型を特徴付けます。

### Type Constraint Syntax(型制約構文)

型引数リストの一部として、型引数の名前の後にコロン(`:`)で区切って単一のクラスまたはプロトコルを配置することで、型制約を記述します。ジェネリック関数の型制約の基本的な構文を以下に示します(構文はジェネリック型でも同じです)。

```swift
func someFunction<T: SomeClass, U: SomeProtocol>(someT:T, someU:U) {
     // ここに関数本文が入ります
}
```

上記の関数には、2 つの型引数があります。最初の型引数 `T` には、`T` が `SomeClass` のサブクラスの必要がある型制約があります。2 番目の型引数 `U` には、`U` が `SomeProtocol` プロトコルに準拠する必要がある型制約があります。

### Type Constraints in Action(型制約の挙動)

下記は、`findIndex(ofString:in:)` という名前の非ジェネリック関数です。これには、検索する `String` 値と、その `String` が含まれているかを検索する `String` の配列が与えられています。`findIndex(ofString:in:)` 関数は、オプショナルの `Int` 値を返します。これは、配列内で最初に一致する文字列が見つかった場合はそのインデックス、文字列が見つからない場合は `nil` になります。

```swift
func findIndex(ofString valueToFind: String, in array: [String]) -> Int? {
    for (index, value) in array.enumerated() {
        if value == valueToFind {
            return index
        }
    }
    return nil
}
```

`findIndex(ofString:in:)` 関数を使用して、文字列が配列内に存在するかどうかを検索できます。

```swift
let strings = ["cat", "dog", "llama", "parakeet", "terrapin"]
if let foundIndex = findIndex(ofString: "llama", in: strings) {
    print("The index of llama is \(foundIndex)")
}
// "The index of llama is 2"
```

ただし、配列内の値のインデックスを見つけるという動作は、文字列だけに役立つわけではありません。文字列の言及を何らかの `T` 型の値に置き換えることで、ジェネリック関数と同じ機能を作成できます。

次は、`findIndex(of:in:)` と呼ばれる、`findIndex(ofString:in:)` のジェネリックバージョンを示しています。この関数は配列のオプショナルの値ではなく、オプショナルのインデックスの番号を返すため、この関数の戻り値の型は引き続き `Int?` なことに注目してください。ただし、次の例の後に説明する理由により、この関数はコンパイルされないことに注意してください:

```swift
func findIndex<T>(of valueToFind: T, in array:[T]) -> Int? {
    for (index, value) in array.enumerated() {
        if value == valueToFind {
            return index
        }
    }
    return nil
}
```

この関数は上記のようにコンパイルできません。問題は、等価チェック `"if value == valueToFind"` にあります。Swift の全ての型を等号演算子(`==`)で比較できるわけではありません。例えば、複雑なデータモデルを表す独自のクラスまたは構造体を作成する場合、そのクラスまたは構造体の「等しい」の意味は Swift が推論できるものではありません。このため、このコードが全ての型 `T` で機能することを保証することはできず、コードをコンパイルしようとすると、適切なエラーが報告されます。

ただし、全てが失われるわけではありません。Swift 標準ライブラリは、`Equatable` と呼ばれるプロトコルを定義します。このプロトコルでは、準拠する型で、その型の 2 つの値を比較するために、等号演算子(`==`)および不等号演算子(`!=`)を実装する必要があります。Swift の全ての標準型は、`Equatable` プロトコルを自動的にサポートしています。

`Equatable` に準拠する全ての型は、等価演算子をサポートすることが保証されているため、`findIndex(of:in:)` 関数で安全に使用できます。この事実を表現するには、関数を定義するときに、型引数の定義に `Equatable` の型制約を記述します:

```swift
func findIndex<T: Equatable>(of valueToFind: T, in array:[T]) -> Int? {
    for (index, value) in array.enumerated() {
        if value == valueToFind {
            return index
        }
    }
    return nil
}
```

`findIndex(of:in:)` の単一の型引数は `T: Equatable` と記述されます。これは、"`Equatable` プロトコルに準拠する任意の型 `T`"を意味します。

`findIndex(of:in:)` 関数は正常にコンパイルされ、`Double` や `String` などの `Equatable` に準拠する任意の型で使用できるようになりました:

```swift
let doubleIndex = findIndex(of: 9.3, in: [3.14159, 0.1, 0.25])
// doubleIndex は、9.3 が配列にないため、オプショナルの Int です。
let stringIndex = findIndex(of: "Andrea", in: ["Mike", "Malcolm", "Andrea"])
// stringIndex は、2 を含むオプショナルの Int です
```

## Associated Types(関連型)

プロトコルを定義するとき、プロトコルの定義に 1 つ以上の関連型を宣言すると便利な場合があります。関連型は、プロトコルの一部として使用される型にプレースホルダ名を与えます。その関連型に使用する実際の型は、プロトコルに準拠するまで指定されません。関連型は、`associatedtype` キーワードで指定します。

### Associated Types in Action(関連型の挙動)

下記は、`Item` と呼ばれる関連型を宣言する `Container` と呼ばれるプロトコルの例です:

```swift
protocol Container {
    associatedtype Item
    mutating func append(_ item: Item)
    var count: Int { get }
    subscript(i: Int) -> Item { get }
}
```

`Container`  プロトコルは、コンテナが提供しなければならない 3 つの必須機能を定義しています:

* `append(_:)` メソッドを使用して、コンテナに新しいアイテムを追加できる必要があります
* `Int` 値を返す `count` プロパティを介して、コンテナ内のアイテムの数にアクセスできる必要があります
* `Int` のインデックスを取る subscript を使用して、コンテナ内の各アイテムを取得できる必要があります

このプロトコルは、コンテナ内のアイテムの保存方法や、許可される型を指定していません。プロトコルは、`Container` と見なされるために型が提供しなければならない 3 つ小さな機能のみを指定します。準拠する型は、これら 3 つの要件を満たす限り、追加の機能を提供できます。

`Container` プロトコルに準拠する全ての型は、格納する値の型を指定する必要があります。具体的には、正しい型のアイテムのみがコンテナに追加されるようにする必要があり、subscript で返されるアイテムの型を明確にする必要があります。

これらの要件を定義するために、`Container` プロトコルは、コンテナが保持する要素の型を参照する必要がありますが、特定のコンテナの型が何かを知る必要はありません。`Container` プロトコルは、`append(_:)` メソッドに渡される値がコンテナの要素の型と同じ型の必要があり、コンテナの `subscript` によって返される値がコンテナの要素の型と同じ型を指定する必要があります。

これを実現するために、`Container` プロトコルは `Item` と呼ばれる関連型を `associatedtype Item` として記述することで宣言しています。プロトコルはアイテムが何かを定義しません。その情報は、準拠する型が提供します。それにもかかわらず、`Item` エイリアスは、`Container` 内のアイテムの型を参照し、`append(_:)` メソッドと `subscript` で使用する型を定義して、どんな `Container` でも期待通りに動くことを保証する方法を提供します。

下記は、`Container` プロトコルに準拠した、上記の[Generic Types](#generic-typesジェネリック型)の非ジェネリック型 `IntStack` のジェネリックバージョンです:

```swift
struct IntStack: Container {
    // 元の IntStack 実装
    var items = [Int]()
    mutating func push(_ item: Int) {
        items.append(item)
    }
    mutating func pop() -> Int {
        return items.removeLast()
    }
    // Container プロトコルへの準拠
    typealias Item = Int
    mutating func append(_ item: Int) {
        self.push(item)
    }
    var count: Int {
        return items.count
    }
    subscript(i: Int) -> Int {
        return items[i]
    }
}
```

`IntStack` 型は、`Container` プロトコルの 3 つの要件全てを実装し、それぞれの要件を満たすために `IntStack` 型の既存の機能を一部使用してます。

さらに、`IntStack` は、`Container` の実装の中で使用する `Item` に `Int` 型を指定しています。`typealias Item = Int` の定義により、抽象的な `Item` が、具象的な `Int` に変わります。

Swift の型推論のおかげで、`IntStack` の定義の中で具体的な `Item` として `Int`  を実際に宣言する必要はありません。`IntStack` は `Container` プロトコルの全ての要件に準拠しているため、`append(_:)` メソッドの `item` 引数の型と subscript の戻り型から、使用する適切な `Item` を推論できます。実際、上記のコードから `typealias Item = Int` 行を削除しても、`Item` に使用する型が明確になっているため、全てが引き続き機能します。

ジェネリックな `Stack` 型を `Container` プロトコルに準拠させることもできます:

```swift
struct Stack<Element>: Container {
    // 元の Stack<Element> 実装
    var items = [Element]()
    mutating func push(_ item: Element) {
        items.append(item)
    }
    mutating func pop() -> Element {
        return items.removeLast()
    }
    // Container プロトコルへの準拠
    mutating func append(_ item: Element) {
        self.push(item)
    }
    var count: Int {
        return items.count
    }
    subscript(i: Int) -> Element {
        return items[i]
    }
}
```

今回は、`append(_:)` メソッドの `item` 引数の型と subscript の戻り値の型として型引数 `Element` を使用しています。したがって、Swift は、`Element` がこの特定のコンテナの `Item` として使用する型を適切に推論できます。

### Extending an Existing Type to Specify an Associated Type(関連型を特定するための既存の型の拡張)

### Adding Constraints to an Associated Type(関連型への制約の追加)

### Using a Protocol in Its Associated Type’s Constraints(関連型に制約へのプロトコルの使用)

## Generic Where Clauses(ジェネリックWhere句)

## Extensions with a Generic Where Clause(ジェネリックWhere句を使った拡張)

## Contextual Where Clauses(コンテキストのWhere句)

## Associated Types with a Generic Where Clause(ジェネリックWhere句を使用した関連型)

## Generic Subscripts(ジェネリックsubscript)
