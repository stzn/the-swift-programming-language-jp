# Generics \(ジェネリクス\)

最終更新日: 2021/6/30

ジェネリックなコードを使用すると、定義した要件に従って、任意の型で機能する柔軟で再利用可能な関数と型を作成できます。重複を避け、その意図を明確で抽象的な方法で表現するコードを書くことができます。

*ジェネリクス*は Swift の最も強力な機能の 1 つで、Swift 標準ライブラリの多くはジェネリックなコードで構築されています。気づかないかもしれませんが、この*言語ガイド*全体でもジェネリクスを使用しています。例えば、Swift の `Array` 型と `Dictionary` 型はどちらもジェネリックなコレクションです。`Int` 値を保持する配列、`String` 値を保持する配列、または Swift で作成できる他のどんな型の配列も作成できます。同様に、指定された型の値を格納する辞書を作成でき、その型に制限はありません。

## The Problem That Generics Solve\(ジェネリクスが解決する問題\)

これは `swapTwoInts(_:_:)` と呼ばれる標準の非ジェネリック関数で、2 つの `Int` 値を交換します:

```swift
func swapTwoInts(_ a: inout Int, _ b: inout Int) {
    let temporaryA = a
    a = b
    b = temporaryA
}
```

この関数は、<a href="https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/declarations#declarations-in-out-parameters" target="_self">In-Out Parameters(In-Out パラメータ)</a>で説明されているように、in-out パラメータを使用して `a` と `b` の値を交換します。

`swapTwoInts(_:_:)` 関数は、`b` の値を `a` に、`a` の値を `b` に入れ替えます。この関数を呼び出して、2 つの `Int` 値の変数を交換できます:

```swift
var someInt = 3
var anotherInt = 107
swapTwoInts(&someInt, &anotherInt)
print("someInt is now \(someInt), and anotherInt is now \(anotherInt)")
// "someInt is now 107, and anotherInt is now 3"
```

`swapTwoInts(_:_:)` 関数は便利ですが、`Int` 値でのみ使用できます。2 つの `String` 値または 2 つの `Double` 値を交換する場合は、下記に示す `swapTwoStrings(_:_:)` および `swapTwoDoubles(_:_:)` 関数などの関数をさらに作成する必要があります:

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

`swapTwoInts(_:_:)`、`swapTwoStrings(_:_:)`、および `swapTwoDoubles(_:_:)` 関数の本文が同一なことに気付いたかもしれません。唯一の違いは、受け入れられる値の型\(`Int`、`String`、および `Double`\)です。

任意の型の 2 つの値を交換する 1 つの関数を作成する方が、より便利で、はるかに柔軟です。ジェネリックなコードを使用すると、そのような関数を作成できます。\(これらの関数のジェネリックなバージョンは、下記で定義されています\)

> NOTE  
> 3 つの関数全てで、`a` と `b` の型は同じでなければなりません。`a` と `b` が同じ型でない場合、それらの値を交換することはできません。Swift は型安全な言語であり、\(例えば\)`String` 型の変数と `Double` 型の変数が互いに値を交換することを許可しません。そうしようとすると、コンパイルエラーが発生します。

## Generic Functions\(ジェネリック関数\)

ジェネリック関数は、どの型でも機能します。これは、上記の `swapTwoInts(_:_:)` 関数のジェネリックなバージョンで、`swapTwoValues(_:_:)` と呼ばれます:

```swift
func swapTwoValues<T>(_ a: inout T, _ b: inout T) {
    let temporaryA = a
    a = b
    b = temporaryA
}
```

`swapTwoValues(_:_:)` 関数の本文は、`swapTwoInts(_:_:)` 関数の本文と同じです。ただし、`swapTwoValues(_:_:)` の最初の行は `swapTwoInts(_:_:)` とは少し異なります。最初の行を比較すると次のようになります:

```swift
func swapTwoInts(_ a: inout Int, _ b: inout Int)
func swapTwoValues<T>(_ a: inout T, _ b: inout T)
```

ジェネリックバージョンの関数は、実際の型名\(`Int`、`String`、`Double` など\)の代わりにプレースホルダの型名 \(この場合は `T`\) を使用します。プレースホルダの型名は、`T` に何が必要かについては指定しませんが、`T` が表すものにかかわらず、`a` と `b` の両方が、同じ型 `T` でなければならないことを示しています。`T` の代わりに使用する実際の型は、`swapTwoValues(_:_:)` 関数が呼び出される度に決定されます。

ジェネリック関数と非ジェネリック関数のもう 1 つの違いは、ジェネリック関数の名前\(`swapTwoValues(_:_:)`\)の後に山括弧内にプレースホルダの型名\(`<T>`\)を記述することです。山括弧は、`T` が `swapTwoValues(_:_:)` 関数定義内のプレースホルダの型名だと Swift に伝えます。`T` はプレースホルダのため、Swift は `T` という実際の型を探しません。

`swapTwoValues(_:_:)` 関数は `swapTwoInts` と同じ方法で呼び出すことができますが、2 つの値が互いに同じ型の限り、任意の型の 2 つの値を渡すことができます。`swapTwoValues(_:_:)` が呼び出される度に、`T` に使用する型は、関数に渡される値の型から推論されます。

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
> 上記で定義された `swapTwoValues(_:_:)` 関数は、Swift 標準ライブラリの `swap` と呼ばれる自動で利用可能なジェネリック関数に由来します。独自のコードで `swapTwoValues(_:_:)` 関数の動作が必要な場合は、独自の実装を提供するのではなく、Swift の既存の `swap(_:_:)` 関数を使用できます。

## Type Parameters\(型パラメータ\)

上記の `swapTwoValues(_:_:)` の例では、プレースホルダ型 `T` は型パラメータの例です。型パラメータは、\(`<T>` のように\)プレースホルダ型を指定して名前を付け、関数名の直後に山括弧のペアの間に記述されます。

型パラメータを指定すると、それを使用して関数のパラメータの型\(`swapTwoValues(_:_:)` 関数の `a` および `b` パラメータなど\)を定義したり、関数の戻り値の型として、または関数の本文内の型注釈として定義できます。いずれの場合も、関数が呼び出される度に、型パラメータが実際の型に置き換えられます。\(上記の `swapTwoValues(_:_:)` の例では、関数が最初に呼び出されたときに `T` が `Int` に置き換えられ、2 回目に呼び出されたときは `String` に置き換えられました\)

山括弧内に複数の型パラメータ名をカンマ\(`,`\)で区切って記述することにより、複数の型パラメータも指定できます。

## Naming Type Parameters\(型パラメータの命名\)

ほとんどの場合、型パラメータには、`Dictionary<Key, Value>` の `Key` と `Value`、`Array<Element>` の `Element` などのその型パラメータの意味を表す名前が付いています。ただし、型パラメータに意味がない場合、上記の `swapTwoValues(_:_:)` 関数の `T` など、`T`、`U`、`V` などの 1 文字の名前を付けるのが伝統的です。

> NOTE  
> 型パラメータには常に大文字のキャメルケース名\(`T` や `MyTypeParameter` など\)を指定して、それらが値ではなく型のプレースホルダであることを示します。

## <a id="generic-types">Generic Types\(ジェネリック型\)</a>

ジェネリック関数に加えて、Swift では独自のジェネリック型を定義できます。これらは、`Array` や `Dictionary` と同様の方法で、独自のクラス、構造体、および列挙型の任意の型で機能します。

このセクションでは、`Stack` と呼ばれるジェネリックなコレクション型を作成する方法を示します。スタックは、配列に似た順序付けられた値のセットですが、`Array` 型よりも操作が制限されています。配列を使用すると、配列内の任意の場所で新しいアイテムを挿入および削除できます。ただし、スタックでは、コレクションの最後にのみ新しいアイテムを追加できます\(スタックに新しい値をプッシュする\(_pushing_\)と呼ばれています\)。同様に、スタックでは、コレクションの最後からのみアイテムを削除できます\(スタックから値をポップする\(_popping_\)と呼ばれています\)。

> NOTE  
> `UINavigationController` クラスは、スタックの概念を使用して、ナビゲーション階層内の view controller をモデル化します。`UINavigationController` クラスの `pushViewController(_:animated:)` メソッドを呼び出して view controller をナビゲーションスタックに追加\(プッシュ\)し、popViewControllerAnimated\(\_:\) メソッドを呼び出してナビゲーションスタックから view controller を削除\(ポップ\)します。スタックは、コレクションの管理に厳密な「後入れ先出し\(LIFO\)」アプローチが必要な場合に役立つコレクションモデルです。

下記の図は、スタックのプッシュとポップの動作を示しています:

![スタックのプッシュとポップ](../.gitbook/assets/stackPushPop_2x.png)

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

Stack のジェネリックバージョンは、本質的に非ジェネリックバージョンと同じですが、実際の型 `Int` の代わりに `Element` という型パラメータを使用していることに注意してください。この型パラメータは、構造体の名前の直後に、一対の山括弧 \(`<Element>`\) 内に記述されます。

`Element` は、後で提供される型のプレースホルダ名を定義します。この未来の型は、構造体の定義内のどこでも `Element` として参照できます。この場合、`Element` は 3 つの場所でプレースホルダとして使用されてます:

* `Element` 型の値の空の配列で初期化される、`items` というプロパティを作成します
* `push(_:)` メソッドに `item` と呼ばれる `Element` 型の単一パラメータを指定しなければなりません
* `pop()` メソッドの戻り値が `Element` 型を指定しなければなりません

ジェネリック型のため、`Stack` を使用して、`Array` および `Dictionary` と同様の方法で、Swift で有効な型のスタックを作成できます。

スタックに格納する型を山括弧\(`<>`\)で囲んで、新しいスタック インスタンスを作成します。例えば、文字列の新しいスタックを作成するには、`Stack<String>()` を記述します。

```swift
var stackOfStrings = Stack<String>()
stackOfStrings.push("uno")
stackOfStrings.push("dos")
stackOfStrings.push("tres")
stackOfStrings.push("cuatro")
// スタックは 4 つの文字列を格納しています
```

これらの 4 つの値をスタックにプッシュした後、`stackOfStrings` がどのように見えるかを次に示します:

![4 つの値をスタックにプッシュ](../.gitbook/assets/stackPushedFourStrings_2x.png)

スタックから値をポップすると、最上位の値 `"cuatro"` が削除されて返されます:

```swift
let fromTheTop = stackOfStrings.pop()
// fromTheTop は "cuatro" に等しく、スタックは 3 つの文字列を含んでいます
```

最上位の値をポップした後のスタックは次のとおりです:

![最上位の値をポップ](../.gitbook/assets/stackPoppedOneString_2x.png)

## <a id="extending-a-generic-type">Extending a Generic Type\(ジェネリック型の拡張\)</a>

ジェネリック型を拡張する場合、extension の定義で型パラメータリストを提供しません。代わりに、元の型定義の型パラメータリストは extension の本文内でも使用でき、元の型パラメータ名は元の定義の型パラメータを参照するために使用します。

次の例では、ジェネリックな `Stack` 型を拡張して、`topItem` と呼ばれる読み取り専用の計算プロパティを追加します。これは、スタックからポップせずにスタックの一番上のアイテムを返します:

```swift
extension Stack {
    var topItem: Element? {
        return items.isEmpty ? nil : items[items.count - 1]
    }
}
```

`topItem` プロパティは、`Element` 型のオプショナルの値を返します。スタックが空の場合、`topItem` は `nil` を返します。スタックが空でない場合、`topItem` は `items` 配列の最後のアイテムを返します。

この extension は型パラメータリストを定義していないことに注目してください。代わりに、`Stack` 型の既存の型パラメータ名の `Element` が extension 内で使用され、`topItem` 計算プロパティのオプショナルの型を示しています。

任意の `Stack` インスタンスで `topItem` 計算プロパティを使用して最後のアイテムへ削除せずにアクセスできるようになりました。

```swift
if let topItem = stackOfStrings.topItem {
    print("The top item on the stack is \(topItem).")
}
// "The top item on the stack is tres."
```

ジェネリック型の extension には、下記の<a href="#extensions-with-a-generic-where-clause" target="_self">Extensions with a Generic Where Clause(ジェネリック where 句を使った拡張)</a>で説明されているように、新しい機能を取得するために拡張した型のインスタンスが満たさなければならない要件を含めることもできます。

## <a id="type-constraints">Type Constraints\(型制約\)</a>

`swapTwoValues(_:_:)` 関数と `Stack` 型は、どの型でも使用できます。ただし、ジェネリック関数およびジェネリック型で使用できる型に特定の型制約を適用すると便利な場合があります。型制約は、型パラメータが特定のクラスを継承する必要があるか、特定のプロトコルまたはプロトコル合成に準拠する必要があるかなどを指定できます。

例えば、Swift の `Dictionary` 型では、辞書のキーとして使用できる型に制限があります。<a href="https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide-gaido/collection-types#dictionaries" target="_self">Dictionaries(辞書)</a>で説明されているように、辞書のキーの型はハッシュ可能でなければなりません。つまり、個々のキーがユニークだということを表明する方法を提供する必要があります。`Dictionary` は、特定のキーの値がすでに含まれているかどうかを確認できるように、そのキーがハッシュ可能なことが必要な場合があります。この要件がなければ、`Dictionary` は特定のキーの値を挿入または置換するべきかどうかを判断できず、すでに存在する特定のキーの値を見つけることもできません。

この要件は、`Dictionary` のキーの型制約によって強制されています。これは、キーの型が Swift 標準ライブラリで定義された `Hashable` プロトコルに準拠する必要があることを指定します。Swift の全ての基本型\(`String`、`Int`、`Double`、`Bool` など\)は、デフォルトでハッシュ可能です。独自の型を `Hashable` プロトコルに準拠させる方法については、[Conforming to the Hashable Protocol](https://developer.apple.com/documentation/swift/hashable#2849490)を参照ください。

独自のジェネリック型を作成するときに、独自の型制約を定義できます。これらの制約は、多くのジェネリックプログラミングに強力な機能を提供します。`Hashable` のような抽象的な概念は、具体的な型ではなく、概念的な特性の観点から型を特徴付けます。

### Type Constraint Syntax\(型制約構文\)

型パラメータリストの一部として、型パラメータの名前の後にコロン\(`:`\)で区切って単一のクラスまたはプロトコルを配置することで、型制約を記述します。ジェネリック関数の型制約の基本的な構文を以下に示します\(構文はジェネリック型でも同じです\)。

```swift
func someFunction<T: SomeClass, U: SomeProtocol>(someT: T, someU: U) {
     // ここに関数本文が入ります
}
```

上記の関数には、2 つの型パラメータがあります。最初の型パラメータ `T` には、`T` が `SomeClass` を継承する必要がある型制約です。2 番目の型パラメータ `U` には、`U` が `SomeProtocol` プロトコルに準拠する必要がある型制約です。

### Type Constraints in Action\(型制約の挙動\)

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

ただし、配列内の値のインデックスを見つけるという動作は、文字列だけで有用というわけではありません。文字列を何らかの `T` 型の値に置き換えることで、ジェネリック関数と同じ機能を作成できます。

次は、`findIndex(of:in:)` と呼ばれる、`findIndex(ofString:in:)` のジェネリックバージョンを示しています。この関数は配列のオプショナルの値ではなく、オプショナルのインデックスの番号を返すため、この関数の戻り値の型は引き続き `Int?` なことに注目してください。ただし、次の例の後に説明する理由により、この関数はコンパイルできないことに注意してください:

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

この関数はコンパイルできません。問題は、等価チェック `"if value == valueToFind"` にあります。Swift の全ての型を等価演算子\(`==`\)で比較できるわけではありません。例えば、複雑なデータモデルを表す独自のクラスまたは構造体を作成する場合、そのクラスまたは構造体の「等しい」の意味は Swift が推論できるものではありません。このため、このコードが全ての型 `T` で機能することを保証することはできず、コードをコンパイルしようとすると、適切なエラーが報告されます。

ただし、全てが失われるわけではありません。Swift 標準ライブラリは、`Equatable` と呼ばれるプロトコルを定義しています。このプロトコルでは、準拠する型で、その型の 2 つの値を比較するために、等価演算子\(`==`\)および不等価演算子\(`!=`\)を実装する必要があります。Swift の全ての標準型は、`Equatable` プロトコルを自動的にサポートしています。

`Equatable` に準拠する全ての型は、等価演算子をサポートすることが保証されているため、`findIndex(of:in:)` 関数で安全に使用できます。この事実を表現するには、関数を定義するときに、型パラメータの定義に `Equatable` の型制約を記述します:

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

`findIndex(of:in:)` の単一の型パラメータは `T: Equatable` と記述されます。これは、「`Equatable` プロトコルに準拠する任意の型 `T`」を意味します。

`findIndex(of:in:)` 関数は正常にコンパイルされ、`Double` や `String` などの `Equatable` に準拠する任意の型で使用できるようになりました:

```swift
let doubleIndex = findIndex(of: 9.3, in: [3.14159, 0.1, 0.25])
// doubleIndex は、9.3 が配列にないため、オプショナルの Int です。
let stringIndex = findIndex(of: "Andrea", in: ["Mike", "Malcolm", "Andrea"])
// stringIndex は、2 を含むオプショナルの Int です
```

## <a id="associated-types">Associated Types\(関連型\)</a>

プロトコルを定義するとき、1 つ以上の*関連型*を宣言すると便利な場合があります。関連型は、プロトコル内で使用される型にプレースホルダ名を与えます。その関連型に使用する実際の型は、プロトコルが準拠されるまで特定されません。関連型は、`associatedtype` キーワードで指定します。

### Associated Types in Action\(関連型の挙動\)

下記は、`Item` と呼ばれる関連型を持つ `Container` と呼ばれるプロトコルの例です:

```swift
protocol Container {
    associatedtype Item
    mutating func append(_ item: Item)
    var count: Int { get }
    subscript(i: Int) -> Item { get }
}
```

`Container` プロトコルは、コンテナが提供しなければならない 3 つの必須機能を定義しています:

* `append(_:)` メソッドを使用して、コンテナに新しいアイテムを追加できる必要があります
* `Int` 値を返す `count` プロパティを介して、コンテナ内のアイテムの数にアクセスできる必要があります
* `Int` のインデックスを受け取る subscript を使用して、コンテナ内の各アイテムを取得できる必要があります

このプロトコルは、コンテナ内のアイテムの保存方法や、許可される型を指定していません。プロトコルは、`Container` と見なされるために型が提供しなければならない 3 つ小さな機能のみを指定します。準拠する型は、これら 3 つの要件を満たす限り、追加の機能を提供できます。

`Container` プロトコルに準拠する全ての型は、格納する値の型を指定する必要があります。具体的には、正しい型のアイテムのみがコンテナに追加されるようにする必要があり、subscript で返されるアイテムの型を明確にする必要があります。

これらの要件を定義するために、`Container` プロトコルは、コンテナが保持する要素の型を参照する必要がありますが、特定のコンテナの型が何かを知る必要はありません。`Container` プロトコルは、`append(_:)` メソッドに渡される値をコンテナの要素の型と同じにする必要があり、コンテナの subscript によって返される値をコンテナの要素の型と同じ型を指定する必要があります。

これを実現するために、`Container` プロトコルは `Item` と呼ばれる関連型を `associatedtype Item` として記述することで宣言しています。プロトコルは `Item` が何かを定義しません。その情報は、準拠する型が提供します。それにもかかわらず、`Item` エイリアスを通して、`Container` 内のアイテムの型への参照、`append(_:)` メソッドと `subscript` で使用する型の定義は、どんな `Container` でも期待通りに動くことを保証しています。

下記は、`Container` プロトコルに準拠した、上記の<a href="#generic-types" target="_self">Generic Types(ジェネリック型)</a>の非ジェネリック型 `IntStack` のジェネリックバージョンです:

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

Swift の型推論のおかげで、`IntStack` の定義の中で `Item` が `Int` なこと宣言する必要は実際はありません。`IntStack` は `Container` プロトコルの全ての要件に準拠しているため、`append(_:)` メソッドの `item` パラメータの型と subscript の戻り値の型から、使用する適切な `Item` を推論できます。実際、上記のコードから `typealias Item = Int` 行を削除しても、`Item` に使用する型が明確になっているため、全てが引き続き機能します。

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

今回は、`append(_:)` メソッドの `item` パラメータの型と subscript の戻り値の型として型パラメータ `Element` を使用しています。したがって、Swift は、`Element` がこの特定のコンテナの `Item` として使用する型を適切に推論できます。

### Extending an Existing Type to Specify an Associated Type\(関連型を特定するための既存の型の拡張\)

<a href="https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide-gaido/protocols#adding-protocol-conformance-with-an-extension" target="_self">Adding Protocol Conformance with an Extension(拡張機能を使ったプロトコル準拠の追加)</a>で説明されているように、既存の型を拡張してプロトコルへの準拠を追加できます。これには、関連型を持つプロトコルが含まれます。

Swift の `Array` 型は、要素を取得するために、`append(_:)` メソッド、`count` プロパティ、および `Int` インデックスの subscript を既存で提供しています。これら 3 つの機能は、`Container` プロトコルの要件に準拠します。これは、`Array` がプロトコルに準拠することを宣言するだけで、`Container` プロトコルに準拠するように `Array` を拡張できることを意味します。<a href="https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide-gaido/protocols#declaring-protocol-adoption-with-an-extension" target="_self">Declaring Protocol Adoption with an Extension(拡張機能を使ったプロトコル準拠の宣言)</a>で説明されているように、これは空の extension で行います:

```swift
extension Array: Container {}
```

`Array` の既存の `append(_:)` メソッドと subscript により、Swift は、上記のジェネリックな `Stack` 型と同様に、`Item` に使用する適切な型を推論できます。この extension を定義した後、任意の配列を `Container` として使用できます。

### Adding Constraints to an Associated Type\(関連型への制約の追加\)

プロトコルの関連型に型制約を追加して、準拠する型がそれらの制約を満たすことを要求できます。例えば、次のコードは、`Container` 内のアイテムが*等価比較可能*なバージョンを定義しています。

```swift
protocol Container {
    associatedtype Item: Equatable
    mutating func append(_ item: Item)
    var count: Int { get }
    subscript(i: Int) -> Item { get }
}
```

このバージョンのコンテナに準拠するには、コンテナのアイテム型が `Equatable` プロトコルに準拠している必要があります。

### Using a Protocol in Its Associated Type’s Constraints\(関連型の制約へのプロトコルの使用\)

プロトコルは、独自の要件を表現できます。例えば、下記は `Container` プロトコルを改良し、`suffix(_:)` メソッドの要件を追加するプロトコルです。`suffix(_:)` メソッドは、コンテナの最後から指定された数の要素を返し、それらを `Suffix` 型のインスタンスに格納します。

```swift
protocol SuffixableContainer: Container {
    associatedtype Suffix: SuffixableContainer where Suffix.Item == Item
    func suffix(_ size: Int) -> Suffix
}
```

このプロトコルでは、`Suffix` は、上記の `Container` の例の `Item` 型のように、関連型です。`Suffix` には 2 つの制約があります。`SuffixableContainer` プロトコル\(現在定義されているプロトコル\)に準拠している必要があり、その `Item` 型は `Container` の `Item` 型と同じだということが必要です。`Item` の制約はジェネリック `where` 句です。これについては、下記の<a href="#associated-types-with-a-generic-where-clause" target="_self">Associated Types with a Generic Where Clause(ジェネリック where 句を使用した関連型)</a>で説明します。

上記の<a href="#generic-types" target="_self">Generic Types(ジェネリック型)</a>の `Stack` 型に `SuffixableContainer` プロトコルへの準拠を追加した拡張を次に示します:

```swift
extension Stack: SuffixableContainer {
    func suffix(_ size: Int) -> Stack {
        var result = Stack()
        for index in (count-size)..<count {
            result.append(self[index])
        }
        return result
    }
    // Suffix が Stack であると推論されます
}
var stackOfInts = Stack<Int>()
stackOfInts.append(10)
stackOfInts.append(20)
stackOfInts.append(30)
let suffix = stackOfInts.suffix(2)
// Suffix には 20 と 30 が含まれます
```

上記の例では、`Suffix` の関連型も `Stack` のため、`Stack` の suffix 操作は別の `Stack` を返します。他にも、`SuffixableContainer` に準拠する型は、それ自体とは異なる `Suffix` 型を持つことができます。つまり、suffix 操作は異なる型を返すことができます。例えば、`IntStack` の代わりに `Stack<Int>` を `Suffix` 型として使用して、非ジェネリック型 `IntStack` が `SuffixableContainer` への準拠を追加する extension を次に示します:

```swift
extension IntStack: SuffixableContainer {
    func suffix(_ size: Int) -> Stack<Int> {
        var result = Stack<Int>()
        for index in (count-size)..<count {
            result.append(self[index])
        }
        return result
    }
    // Suffix が Stack<Int> だと推論されます
}
```

## <a id="generic-where-clauses">Generic Where Clauses\(ジェネリック where 句\)</a>

<a href="https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide-gaido/generics#type-constraints" target="_self">Type Constraints(型制約)</a>で説明されているように、型制約を使用すると、ジェネリック関数、subscript、または型に関連した型パラメータの要件を定義できます。

関連型の要件を定義するのにも役立ちます。これを行うには、*ジェネリック where 句*を定義します。ジェネリック `where` 句を使用すると、関連型が特定のプロトコルに準拠する必要があること、または特定の型パラメータと関連型を同じにする必要があることを要求できます。ジェネリック `where` 句は `where` キーワードで始まり、その後に関連型の制約、または型と関連型の間の等価関係が続きます。型または関数の本文の開始中括弧\(`{`\)の直前に、ジェネリック `where` 句を記述します。

下記の例では、2 つの `Container` インスタンスに同じアイテムが同じ順序で含まれているかどうかを確認する `allItemsMatch` というジェネリック関数を定義しています。この関数は、全てのアイテムが一致する場合はブール値 `true` を返し、一致しない場合は `false` の値を返します。

チェックする 2 つのコンテナは、必ずしも同じ型の必要はありませんが\(同じ型のコンテナでもかまいません\)、同じ型のアイテムを保持している必要があります。この要件は、型制約とジェネリック `where` 句の組み合わせによって表現されます:

```swift
func allItemsMatch<C1: Container, C2: Container>
    (_ someContainer: C1, _ anotherContainer: C2) -> Bool
    where C1.Item == C2.Item, C1.Item: Equatable {

        // 両方のコンテナに同じ数のアイテムが含まれていることを確認します
        if someContainer.count != anotherContainer.count {
            return false
        }

        // アイテムの各ペアをチェックして、同等かどうかを確認します
        for i in 0..<someContainer.count {
            if someContainer[i] != anotherContainer[i] {
                return false
            }
        }

        // 全てのアイテムが一致するため、true を返します
        return true
}
```

この関数は、`someContainer` と `anotherContainer` という 2 つの引数を取ります。`someContainer` 引数は `C1` 型で、`otherContainer` 引数は `C2` 型です。`C1` と `C2` はどちらも、関数が呼び出されたときに決定される 2 つの `Container` 型の型パラメータです。

関数の 2 つの型パラメータには、次の要件が適用されます。

* `C1` は、`Container` プロトコル\(`C1: Container` と表記\)に準拠する必要があります
* `C2` も、`Container` プロトコル\(`C2: Container` と表記\)に準拠する必要があります
* `C1` の `Item` は、`C2` の `Item` と同じにする必要があります\(`C1.Item == C2.Item` と記述\)
* `C1` の `Item` は、`Equatable` プロトコル\(`C1.Item: Equatable` と表記\)に準拠する必要があります

1 番目と 2 番目の要件は関数の型パラメータリストで定義され、3 番目と 4 番目の要件は関数のジェネリック `where` 句で定義されます。

これらの要件は次のことを意味します。

* `someContainer` は `C1` 型のコンテナです
* `anotherContainer` は、`C2` 型のコンテナです
* `someContainer` と `anotherContainer` には、同じ型のアイテムが含まれています
* `someContainer` 内の `Item` は、不等演算子 \(`!=`\) を使用して、互いに異なるかどうかを確認できます

3 番目と 4 番目の要件を組み合わせると、`otherContainer` のアイテムは `someContainer` のアイテムとまったく同じ型のため、`!=` 演算子を使用してチェックすることもできます。

これらの要件により、`allItemsMatch(_:_:)` 関数は、2 つのコンテナが異なるコンテナ型でも、それらを比較できます。

`allItemsMatch(_:_:)` 関数は、両方のコンテナに同じ数のアイテムが含まれていることを確認することから始まります。それらに含まれるアイテムの数が異なる場合、一致する可能性はなく、関数は `false` を返します。

このチェックを行った後、関数は `for-in` ループと半開範囲演算子\(`..<`\)を使用して `someContainer` 内の全てのアイテムを反復します。各アイテムについて、関数は `someContainer` のアイテムが `anotherContainer` の対応するアイテムと等しくないかどうかをチェックします。2 つのアイテムが等しくない場合、2 つのコンテナは一致せず、関数は `false` を返します。

不一致が見つからずにループが終了した場合、2 つのコンテナは一致し、関数は `true` を返します。

`allItemsMatch(_:_:)` 関数の実際の動作は次のとおりです:

```swift
var stackOfStrings = Stack<String>()
stackOfStrings.push("uno")
stackOfStrings.push("dos")
stackOfStrings.push("tres")

var arrayOfStrings = ["uno", "dos", "tres"]

if allItemsMatch(stackOfStrings, arrayOfStrings) {
    print("All items match.")
} else {
    print("Not all items match.")
}
// "All items match."
```

上記の例では、`String` 値を格納する `Stack` インスタンスを作成し、3 つの文字列をスタックにプッシュします。この例では、スタックと同じ 3 つの文字列を含む配列リテラルで初期化された `Array` インスタンスも作成します。スタックと配列は型が異なりますが、どちらも `Container` プロトコルに準拠しており、両方に同じ型の値が含まれています。したがって、これら 2 つのコンテナを引数として `allItemsMatch(_:_:)` 関数を呼び出すことができます。上記の例では、`allItemsMatch(_:_:)` 関数は、2 つのアイテムの全てが合致していることを出力します。

## <a id="extensions-with-a-generic-where-clause">Extensions with a Generic Where Clause\(ジェネリック where 句を使った拡張\)</a>

extension にジェネリック `where` 句を使用することもできます。下記の例では、前の例のジェネリックな `Stack` 構造体を拡張して、`isTop(_:)` メソッドを追加しています:

```swift
extension Stack where Element: Equatable {
    func isTop(_ item: Element) -> Bool {
        guard let topItem = items.last else {
            return false
        }
        return topItem == item
    }
}
```

この新しい `isTop(_:)` メソッドは、最初にスタックが空でないことを確認してから、指定されたアイテムをスタックの最上位のアイテムと比較します。`isTop(_:)` の実装では `==` 演算子を使用していますが、`Stack` の定義ではそのアイテムが `Equatable` の要件を設定していないため、`==` 演算子はコンパイルエラーになります。ジェネリック `where` 句を使用すると、 extension に新しい要件を追加できるため、extension はスタック内のアイテムが `Equatable` の場合にのみ `isTop(_:)` メソッドを追加します。

`isTop(_:)` メソッドの実際の動作は次のとおりです:

```swift
if stackOfStrings.isTop("tres") {
    print("Top element is tres.")
} else {
    print("Top element is something else.")
}
// "Top element is tres."
```

要素が `Equatable` でないスタックで `isTop(_:)` メソッドを呼び出そうとすると、コンパイルエラーが発生します。

```swift
struct NotEquatable { }
var notEquatableStack = Stack<NotEquatable>()
let notEquatableValue = NotEquatable()
notEquatableStack.push(notEquatableValue)
notEquatableStack.isTop(notEquatableValue)  // エラー
```

プロトコルの extension にもジェネリック `where` 句を使用できます。下記の例では、前の例の `Container` プロトコルを拡張して、`startsWith(_:)` メソッドを追加しています。

```swift
extension Container where Item: Equatable {
    func startsWith(_ item: Item) -> Bool {
        return count >= 1 && self[0] == item
    }
}
```

`startsWith(_:)` メソッドは、まずコンテナに少なくとも 1 つのアイテムがあることを確認し、次にコンテナ内の最初のアイテムが指定されたアイテムと一致するかどうかを確認します。この新しい `startsWith(_:)` メソッドは、コンテナのアイテムが同等な限り、`Container` プロトコルに準拠する任意の型で使用できます。

```swift
if [9, 9, 9].startsWith(42) {
    print("Starts with 42.")
} else {
    print("Starts with something else.")
}
// "Starts with something else."
```

上記の例のジェネリック `where` 句では、`Item` がプロトコルに準拠する必要がありますが、`Item` の型を特定する必要があるジェネリック `where` 句を記述することもできます。例えば:

```swift
extension Container where Item == Double {
    func average() -> Double {
        var sum = 0.0
        for index in 0..<count {
            sum += self[index]
        }
        return sum / Double(count)
    }
}
print([1260.0, 1200.0, 98.6, 37.0].average())
// "648.9"
```

この例では、`Item` 型が `Double` のコンテナに `average()` メソッドを追加します。コンテナ内のアイテムを繰り返して合計し、コンテナの数で除算して平均を計算します。浮動小数点除算ができるように、`count` を `Int` から `Double` に明示的に変換します。

他の場所で記述したジェネリック `where` 句の場合と同様に、extension にあるジェネリック `where` 句に複数の要件を含めることができます。各要件はカンマ\(`,`\)で区切ります。

## <a id="contextual-where-clauses">Contextual Where Clauses\(コンテキスト上のWhere句\)</a>

既にジェネリック型を使っているコンテキスト上で、型制約を持たない宣言にジェネリック `where` 句を記述できます。例えば、ジェネリック型の subscript、またはジェネリック型の extension のメソッドにジェネリック `where` 句を記述できます。`Container` 構造体はジェネリックで、下記の例の `where` 句は、新しいメソッドを `Container` で使用できるようにするために満たす必要のある型制約を指定しています。

```swift
extension Container {
    func average() -> Double where Item == Int {
        var sum = 0.0
        for index in 0..<count {
            sum += Double(self[index])
        }
        return sum / Double(count)
    }
    func endsWith(_ item: Item) -> Bool where Item: Equatable {
        return count >= 1 && self[count-1] == item
    }
}
let numbers = [1260, 1200, 98, 37]
print(numbers.average())
// "648.75"
print(numbers.endsWith(37))
// "true"
```

この例では、アイテムが整数の場合は `average()` メソッドを `Container` に追加し、アイテムが `Equatable` の場合は `endsWith(_:)` メソッドを追加します。どちらの関数にも、`Container` で宣言したジェネリックな `Item` 型パラメータに型制約を追加するジェネリック `where` 句が含まれています。

コンテキスト上の `where` 句を使用せずにこのコードを記述したい場合は、ジェネリック `where` 句ごとに 1 つずつ、2 つの extension を記述します。上記の例と下記の例は同じ動きをします。

```swift
extension Container where Item == Int {
    func average() -> Double {
        var sum = 0.0
        for index in 0..<count {
            sum += Double(self[index])
        }
        return sum / Double(count)
    }
}
extension Container where Item: Equatable {
    func endsWith(_ item: Item) -> Bool {
        return count >= 1 && self[count-1] == item
    }
}
```

コンテキスト上の `where` 句を使用する例のバージョンでは、各メソッドのジェネリック `where` 句が、そのメソッドを使用可能にするために満たす必要がある要件を示しているため、`average()` と `endsWith(_:)` の実装は両方とも同じ extension 内にありました。これらの要件を extension のジェネリック `where` 句に移動すると、同じ状況でメソッドを使用できるようになりますが、要件ごとに 1 つの extension が必要になります。

## <a id="associated-types-with-a-generic-where-clause">Associated Types with a Generic Where Clause\(ジェネリック where 句を使用した関連型\)</a>

関連型にジェネリック `where` 句を含めることができます。例えば、標準ライブラリの `Sequence` プロトコルで使用するようなイテレータを含む `Container` のバージョンを作成するとします。書き方は次のとおりです:

```swift
protocol Container {
    associatedtype Item
    mutating func append(_ item: Item)
    var count: Int { get }
    subscript(i: Int) -> Item { get }

    associatedtype Iterator: IteratorProtocol where Iterator.Element == Item
    func makeIterator() -> Iterator
}
```

`Iterator` のジェネリック `where` 句では、`Iterator` の型に関係なく、コンテナのアイテムと同じアイテム型の要素をイテレータが反復する必要があります。`makeIterator()` 関数は、コンテナのイテレータへのアクセスを提供します。

別のプロトコルを継承したプロトコルの場合、プロトコル宣言にジェネリック `where` 句を含めることにより、継承した関連型に制約を追加できます。例えば、次のコードは、`Item` が `Comparable` に準拠することを要求する `ComparableContainer` プロトコルを宣言しています:

```swift
protocol ComparableContainer: Container where Item: Comparable { }
```

## <a id="generic-subscripts">Generic Subscripts\(ジェネリックsubscript\)</a>

subscript はジェネリックにすることができ、ジェネリック `where` 句を含めることができます。subscript の後の山括弧内\(`<>`\)にプレースホルダの型名を記述し、subscript の本文の開始の中括弧\(`{`\)の直前にジェネリック `where` 句を記述します。例えば:

```swift
extension Container {
    subscript<Indices: Sequence>(indices: Indices) -> [Item]
        where Indices.Iterator.Element == Int {
            var result = [Item]()
            for index in indices {
                result.append(self[index])
            }
            return result
    }
}
```

`Container` プロトコルへのこの extension は、インデックスのシーケンスを受け取り、指定された各インデックスのアイテムを含む配列を返す subscript を追加します。このジェネリックな subscript は、次のように制約されます:

* 山括弧\(`<>`\)内のジェネリックパラメータのインデックスは、標準ライブラリの `Sequence` プロトコルに準拠した型にする必要があります
* subscript は、単一の `Indices` 型のインスタンスのパラメータ `indices` を受け取ります
* ジェネリック `where` 句では、シーケンスのイテレータが `Int` 型の要素を繰り返し処理することを要求します。これにより、シーケンス内のインデックスは、コンテナに使用されるインデックスと同じ型だということが保証されます

まとめると、これらの制約は、`indices` パラメータに渡される値が整数のシーケンスだということを意味します。
