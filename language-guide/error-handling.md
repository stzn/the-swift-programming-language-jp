# エラー処理\(Error Handling\)

最終更新日: 2024/6/7
原文: https://docs.swift.org/swift-book/LanguageGuide/ErrorHandling.html

エラーに対応し、エラーから回復する。

_エラー処理_は、プログラムのエラー状態に応答し、エラー状態から回復するプロセスです。Swift は、実行時に回復可能なエラーをスロー、キャッチ、伝播、および操作するための第一級クラスのサポートを提供します。

一部の操作は、常に実行を完了したり、有益な出力をするとは限りません。オプショナルは値がないことを表すために使用されますが、操作が失敗した場合、コードがそれに応じて応答できるように、失敗の原因を理解しておくと役立つことがよくあります。

例として、ディスク上のファイルからデータを読み取って処理するタスクを考えてみましょう。指定されたパスにファイルが存在しない、ファイルが読み取り権限を持たない、ファイルが互換性のある形式でエンコードされていないなど、このタスクが失敗する原因はいくつかあります。これらの様々な状況を区別することで、プログラムはいくつかのエラーを解決し、解決できないエラーをユーザに伝えることができます。

> NOTE
> Swift のエラー処理は、Cocoa および Objective-C の NSError クラスを使用するエラー処理パターンと相互運用します。このクラスの詳細については、[Handling Cocoa Errors in Swift](https://developer.apple.com/documentation/swift/cocoa_design_patterns/handling_cocoa_errors_in_swift)を参照ください。

## <a id="representing-and-throwing-errors">エラーの表現とスロー\(Representing and Throwing Errors\)</a>

Swift では、エラーは `Error` プロトコルに準拠した型の値によって表されます。この空のプロトコルは、エラー処理に型を使用できることを示します。

Swift の列挙型は、関連するエラー条件のグループをモデル化するのに特に適しています。関連値を使用すると、エラーの性質に関する追加情報を伝達できます。例えば、ゲーム内で自動販売機を操作する場合のエラー状態を表す方法は次のとおりです。

```swift
enum VendingMachineError: Error {
    case invalidSelection
    case insufficientFunds(coinsNeeded: Int)
    case outOfStock
}
```

エラーをスローすると、予期しないことが発生し、通常の実行フローを続行できないことを示すことができます。エラーをスローするには、`throw` 文を使用します。例えば、次のコードはエラーをスローして、自動販売機で 5 枚の追加のコインが必要だということを示します。

```swift
throw VendingMachineError.insufficientFunds(coinsNeeded: 5)
```

## エラー処理\(Handling Errors\)

エラーがスローされた場合、周囲のコードの一部がエラーを処理する必要があります。例えば、問題を修正する、別のアプローチを試みる、またはユーザにエラーを通知するなどです。

Swift でエラーを処理する方法は 4 つあります。関数からその関数を呼び出すコードにエラーを伝播したり、`do-catch` 文を使用してエラーを処理したり、オプショナル値としてエラーを処理したり、エラーが発生しないことをアサートしたりできます。各アプローチについては、以下のセクションで説明します。

関数がエラーをスローすると、プログラムのフローが変わるため、エラーをスローする可能性のあるコード内の場所をすばやく特定できることが重要です。コード内のこれらの場所を特定するには、`try` キーワード \(または `try?` または `try!`\) をエラーをスローする可能性のある関数、メソッド、またはイニシャライザを呼び出すコードの前に置きます。これらのキーワードについては、以下のセクションで説明します。

> NOTE
> Swift でのエラー処理は、他の言語での例外処理に似ており、`try`、`catch`、`throw` キーワードを使用しています。Objective-C を含む多くの言語の例外処理とは異なり、Swift でのエラー処理には、計算コストがかかる可能性のあるプロセスであるコールスタックの巻き戻しが含まれません。したがって、`throw` 文のパフォーマンスは、`return` 文のパフォーマンスに匹敵します。

### <a id="propagating-errors-using-throwing-functions">スロー関数を使用したエラーの伝播\(Propagating Errors Using Throwing Functions\)</a>

関数、メソッド、またはイニシャライザがエラーをスローできることを示すには、関数の宣言のパラメータの後に `throws` キーワードを記述します。`throws` でマークされた関数は、_スロー_関数と呼ばれます。関数が戻り値の型を指定する戻り矢印 \(`->`\) の前に `throws` キーワードを記述します:

```swift
func canThrowErrors() throws -> String

func cannotThrowErrors() -> String
```

スロー関数は、その内部でスローされたエラーを、呼び出されたスコープに伝播します。

> NOTE
> エラーを伝播できるのは、スローする関数だけです。スローされない関数内でスローされたエラーは、関数内で処理する必要があります。

下記の例では、`VendingMachine` クラスに `vend(itemNamed:)` メソッドがあり、要求されたアイテムが利用できない場合、在庫がない場合、または現在のお金が足りない場合に、適切な `VendingMachineError` をスローします:

```swift
struct Item {
    var price: Int
    var count: Int
}

class VendingMachine {
    var inventory = [
        "Candy Bar": Item(price: 12, count: 7),
        "Chips": Item(price: 10, count: 4),
        "Pretzels": Item(price: 7, count: 11)
    ]
    var coinsDeposited = 0

    func vend(itemNamed name: String) throws {
        guard let item = inventory[name] else {
            throw VendingMachineError.invalidSelection
        }

        guard item.count > 0 else {
            throw VendingMachineError.outOfStock
        }

        guard item.price <= coinsDeposited else {
            throw VendingMachineError.insufficientFunds(coinsNeeded: item.price - coinsDeposited)
        }

        coinsDeposited -= item.price

        var newItem = item
        newItem.count -= 1
        inventory[name] = newItem

        print("Dispensing \(name)")
    }
}
```

`vend(itemNamed:)` メソッドの実装では、`guard` 文を使用してメソッドを早期に終了し、スナックを購入するための要件が満たされていない場合に適切なエラーをスローします。`throw` 文はすぐにプログラム制御を転送するため、これら全ての要件が満たされた場合にのみアイテムが販売されます。

`vend(itemNamed:)` メソッドはスローしたエラーを全て伝播するため、このメソッドを呼び出すコードは、`do-catch` 文、`try?`、または `try!` を使用してエラーを処理するか、エラーを伝播し続ける必要があります。例えば、下記の例の `buyFavoriteSnack(person：vendingMachine:)` もスロー関数で、`vend(itemNamed:)` メソッドがスローするエラーを `buyFavoriteSnack(person：vendingMachine:)` 関数が呼ばれる部分まで伝播しています。

```swift
let favoriteSnacks = [
    "Alice": "Chips",
    "Bob": "Licorice",
    "Eve": "Pretzels",
]
func buyFavoriteSnack(person: String, vendingMachine: VendingMachine) throws {
    let snackName = favoriteSnacks[person] ?? "Candy Bar"
    try vendingMachine.vend(itemNamed: snackName)
}
```

この例では、`buyFavoriteSnack(person: vendingMachine:)` 関数は、指定された人のお気に入りのスナックを検索し、`vend(itemNamed:)` メソッドを呼び出して、その人のためにそれを購入しようとします。`vend(itemNamed:)` メソッドはエラーをスローする可能性があるため、その前に `try` キーワードを付けて呼び出されます。

イニシャライザがスローすると、関数をスローするのと同じ方法でエラーを伝播できます。例えば、下記のリストにある `PurchasedSnack` 構造体のイニシャライザは、初期化プロセスの一部としてスロー関数を呼び出し、発生したエラーを呼び出し元に伝播させることで処理します。

```swift
struct PurchasedSnack {
    let name: String
    init(name: String, vendingMachine: VendingMachine) throws {
        try vendingMachine.vend(itemNamed: name)
        self.name = name
    }
}
```

### <a id="handling-errors-using-do-catch">do catchを使ったエラー処理\(Handling Errors Using Do-Catch\)</a>

`do-catch` 文を使用して、コードブロックを実行することでエラーを処理します。`do` 句のコードによってエラーがスローされた場合、`catch` 句と照合され、エラーを処理できる `catch` 句を判断します。

`do-catch` 文の一般的な形式は次のとおりです:

```swift
do {
    try <#expression#>
    <#statements#>
} catch <#pattern 1#> {
    <#statements#>
} catch <#pattern 2#> where <#condition#> {
    <#statements#>
} catch <#pattern 3#>, <#pattern 4#> where <#condition#> {
    <#statements#>
} catch {
    <#statements#>
}
```

その句が処理できるエラーを示すには、`catch` の後にパターンを記述します。`catch` 句にパターンがない場合、句は全てのエラーに一致し、エラーを `error` という名前のローカル定数にバインドします。パターンマッチングの詳細については、[Patterns\(パターン\)](../language-reference/patterns.md)を参照ください。

例えば、次のコードは、`VendingMachineError` 列挙型の 3 つのケース全てに一致します:

```swift
var vendingMachine = VendingMachine()
vendingMachine.coinsDeposited = 8
do {
    try buyFavoriteSnack(person: "Alice", vendingMachine: vendingMachine)
    print("成功! おいしい。")
} catch VendingMachineError.invalidSelection {
    print("無効な選択です。")
} catch VendingMachineError.outOfStock {
    print("在庫切れです。")
} catch VendingMachineError.insufficientFunds(let coinsNeeded) {
    print("お金が足りません。あと \(coinsNeeded) コイン投入してください。")
} catch {
    print("予期しないエラー: \(error)。")
}
// お金が足りません。あと 2 コイン投入してください。
```

上記の例では、エラーをスローする可能性があるため、`buyFavoriteSnack(person:vendingMachine:)` 関数が `try` 式で呼び出されます。エラーがスローされた場合、実行はすぐに `catch` 句に移り、伝播を続行できるかどうかを決定します。一致するパターンがない場合、エラーは最後の `catch` 句によってキャッチされ、ローカル `error` 定数にバインドされます。エラーがスローされない場合は、`do` 文の残りの文が実行されます。

`do` 句のコードがスローする可能性のある全てのエラーを `catch` 句で処理する必要はありません。どの `catch` 句もエラーを処理しない場合、エラーは周囲のスコープに伝播します。ただし、伝播されたエラーは、周囲のスコープによって処理される必要があります。スローしない関数では、囲んでいる `do-catch` 文でエラーを処理する必要があります。スローする関数では、囲んでいる `do-catch` 文または呼び出し元がエラーを処理する必要があります。エラーが処理されずに最上位のスコープに伝播すると、実行時エラーが発生します。

例えば、上記の例は、`VendingMachineError` ではないエラーが代わりに呼び出し関数によってキャッチされるように記述できます:

```swift
func nourish(with item: String) throws {
    do {
        try vendingMachine.vend(itemNamed: item)
    } catch is VendingMachineError {
        print("自動販売機から買うことができませんでした。")
    }
}

do {
    try nourish(with: "Beet-Flavored Chips")
} catch {
    print("予期せぬ自動販売機とは関係ないエラーが発生: \(error)")
}
// 自動販売機から買うことができませんでした。
```

`nourish(with:)` 関数では、`vend(itemNamed:)` が `VendingMachineError` 列挙ケースの 1 つのエラーをスローした場合、`nourish(with:)` はメッセージを出力してエラーを処理します。それ以外の場合、`nourish(with:)` はエラーを呼び出し側に伝播します。その後、エラーは一般的な `catch` 句によってキャッチされます。

いくつかの関連するエラーをキャッチする別の方法は、キャッチの後にカンマ\(`,`\)で区切ってそれらをリストすることです。例えば:

```swift
func eat(item: String) throws {
    do {
        try vendingMachine.vend(itemNamed: item)
    } catch VendingMachineError.invalidSelection, VendingMachineError.insufficientFunds, VendingMachineError.outOfStock {
        print("無効な選択、在庫切れ、またはお金がたりません。")
    }
}
```

`eat(item:)` 関数は、キャッチする自動販売機のエラーをリストし、そのエラーテキストはそのリスト内のアイテムに対応します。リストされている 3 つのエラーのいずれかがスローされた場合、この `catch` 句はメッセージを出力してそれらを処理します。その他のエラーは、後で追加される可能性のある自動販売機のエラーを含め、周囲のスコープに伝播されます。

### <a id="converting-errors-to-optional-values">エラーからオプショナル値への変換\(Converting Errors to Optional Values\)</a>

エラーをオプショナル値に変換して処理するには `try?` を使います。`try?` 式を評価中にエラーがスローされた場合、式の値は `nil` です。例えば、次のコードでは、`x` と `y` の値は挙動が同じです:

```swift
func someThrowingFunction() throws -> Int {
    // ...
}

let x = try? someThrowingFunction()

let y: Int?
do {
    y = try someThrowingFunction()
} catch {
    y = nil
}
```

`someThrowingFunction()` がエラーをスローした場合、`x` と `y` の値は `nil` です。それ以外の場合、`x` と `y` の値は関数が返した値です。`x` と `y` は、`someThrowingFunction()` が返す型のオプショナルなことに注目してください。ここで、関数は整数を返すため、`x` と `y` はオプショナルの整数です。

全てのエラーを同じ方法で処理したい場合、`try?` を使って簡潔にエラー処理のコードを記述できます。例えば、次のコードはいくつかのアプローチを使用してデータを取得するか、全てのアプローチが失敗した場合は `nil` を返します:

```swift
func fetchData() -> Data? {
    if let data = try? fetchDataFromDisk() { return data }
    if let data = try? fetchDataFromServer() { return data }
    return nil
}
```

### エラーの伝播を抑える\(Disabling Error Propagation\)

実際には、スローする関数またはメソッドが実行時にエラーをスローしないことがわかっている場合があります。そのような場合は、式の前に `try!` を記述してエラーの伝播を無効にし、エラーがスローされないという実行時アサーションで呼び出しをラップできます。エラーが実際にスローされると、実行時エラーが発生します。

例えば、次のコードは `loadImage(atPath:)` 関数を使用しています。この関数は、指定されたパスで画像リソースを読み込むか、画像を読み込めない場合にエラーをスローします。この場合、イメージはアプリケーションに付属していて実行時にエラーがスローされないため、エラーの伝播を無効にするのが適切です。

```swift
let photo = try! loadImage(atPath: "./Resources/John Appleseed.jpg")
```

## <a id="specifying-the-error-type">エラーの型の特定\(Specifying the Error Type\)</a>

上記の例はすべて、最も一般的な種類のエラー処理を使用しており、コードがスローするエラーは、`Error` プロトコルに準拠する任意の型の値にできます。このアプローチは、特に他の場所でスローされたエラーを伝播する場合、コードの実行中に発生する可能性のあるすべてのエラーを事前に知ることができないという事実に即しています。またこれは、エラーの型が、時間の経過とともに変化する可能性があるという事実を反映しています。(依存しているライブラリが使用するもの含めた)ライブラリの新しいバージョンは、新しいエラーをスローする可能性があり、実際のユーザーの設定の複雑さに起因するような、開発中やテスト中には見つけられなかった障害が発生するケースが見つかることがあります。上記の例のエラー処理コードには、特定の `catch` 句がないエラーを処理するためのデフォルトのケースが常に含まれています。

ほとんどの Swift で書かれたコードは、スローするエラーの型を指定しません。ただし、次のような特別なケースでは、特定の 1 つの型のエラーのみをスローするようにコードを制限することがあるかもしれません。

- メモリの動的割り当てをサポートしていない組み込みシステムでコードを実行する場合。`Error` または別の Box プロトコル型のインスタンスをスローするには、実行時にエラーを格納するためのメモリを割り当てる必要がある。一方で、特定の型のエラーをスローすると、Swift はエラーインスタンスをヒープ割り当てるのを回避できる
- エラーがライブラリなどのコードユニットの実装の詳細であり、そのコードへのインターフェイスの一部ではない場合。エラーはライブラリからのみ発生し、他の依存関係やライブラリのクライアントからは発生しないため、想定される全ての障害の完全なリストを作成できる。また、これらのエラーはライブラリの実装の詳細であるため、常にそのライブラリ内で処理される
- クロージャを引数で受け取り、そのクロージャからエラーを伝播する関数のように、ジェネリックパラメータによって記述されたエラーのみを伝播するコード内。特定のエラー型を伝播する方法と `rethrows` を使うことの違いついては、[再スロー関数と再スローメソッド](../language-reference/declarations.md#再スロー関数と再スローメソッドrethrowing-functions-and-methods)を参照

例えば、評価を要約し、次のようなエラー型を使用するコードを考えてみましょう:

```swift
enum StatisticsError: Error {
    case noRatings
    case invalidRating(Int)
}
```

関数がエラーとして `StatisticsError` の値のみをスローするように指定するには、関数を宣言するときに、ただ `throws` と記述するのではなく、 `throws(StatisticsError)` と記述します。この構文は、宣言で `throws` の後にエラーの型を記述するため、*型付きスロー* とも呼ばれます。例えば、次の関数は、エラーとして `StatisticsError` の値をスローします。

```swift
func summarize(_ ratings: [Int]) throws(StatisticsError) {
    guard !ratings.isEmpty else { throw .noRatings }

    var counts = [1: 0, 2: 0, 3: 0]
    for rating in ratings {
        guard rating > 0 && rating <= 3 else { throw .invalidRating(rating) }
        counts[rating]! += 1
    }

    print("*", counts[1]!, "-- **", counts[2]!, "-- ***", counts[3]!)
}
```

上記のコードでは、`summary(_:)` 関数が 1 から 3 のスケールで表現された評価のリストを要約します。この関数は、入力が有効でない場合、`StatisticsError` のインスタンスをスローします。上記のコードの中のエラーをスローする 2 つの場所では、関数のエラー型がすでに定義されているため、エラーの型が省略されています。このような関数でエラーをスローする場合は、`throw StatisticsError.noRatings` と記述する代わりに、短縮して `throw .noRatings` を使用できます。

関数の先頭に特定のエラー型を記述すると、Swift は他のエラーがスローされないかどうかをチェックします。たとえば、このチャプターの前半に出てきた例の `VendingMachineError` を上記の `summary(_:)` 関数で使用しようとすると、そのコードはコンパイル時にエラーを出力します。

通常のエラーをスローする関数内から、型付きスローを使用する関数を呼び出せます。

```swift
func someThrowingFunction() -> throws {
    let ratings = [1, 2, 3, 2, 2, 1]
    try summarize(ratings)
}
```

上記のコードでは、`someThrowingFunction()` がエラーの型を指定していないため、`any Error` がスローされます。エラーの型を明示的に `throws(any Error)` として記述することもできます。以下のコードは、上記のコードと同等です。

```swift
func someThrowingFunction() -> throws(any Error) {
    let ratings = [1, 2, 3, 2, 2, 1]
    try summarize(ratings)
}
```

このコードでは、`someThrowingFunction()` は、`summarize(_:)` がスローするエラーをすべて伝播します。`summarize(_:)` からのエラーは常に `StatisticsError` の値であり、これは、`someThrowingFunction()` がスローする有効なエラーでもあります。

戻り値の型を `Never` にして戻り値を返さない関数を記述できるのと同様に、`throws(Never)` を使ってエラーをスローしない関数を記述できます。

```swift
func nonThrowingFunction() throws(Never) {
  // ...
}
```

`Never` 型をスローするの値は作成できないため、この関数はスローできません。

関数のエラーの型を指定するだけでなく、`do-catch` 文に特定のエラーの型を記述することもできます。例えば:

```swift
let ratings = []
do throws(StatisticsError) {
    try summarize(ratings)
} catch {
    switch error {
    case .noRatings:
        print("No ratings available")
    case .invalidRating(let rating):
        print("Invalid rating: \(rating)")
    }
}
// Prints "No ratings available"
```

このコードでは、`do throws(StatisticsError)` と書くことで、`do-catch` 文がエラーとして `StatisticsError` の値を投げることを示しています。他の `do-catch` 文と同様に、`catch` 節は起こり得るすべてのエラーを処理するか、未処理のエラーを周囲のスコープに伝播して処理させることができます。このコードでは、列挙値ごとに 1 つの `case` を持つ `switch` 文を使用して、すべてのエラーを処理しています。パターンを持たない他の `catch` 句と同様に、この句はあらゆるエラーにマッチし、エラーを `error` という名前のローカル定数にバインドします。`do-catch` 文は `StatisticsError` の値をスローするので、`error` は `StatisticsError` 型の値です。

上記の `catch` 句は、起こり得るエラーをそれぞれ照合して処理するために `switch` 文を使用しています。エラー処理コードを更新せずに `StatisticsError` に新しい `case` を追加しようとすると、`switch` 文が網羅的でなくなるため、Swift はエラーを出力します。独自のエラーを全てキャッチするライブラリの場合は、このアプローチを使用して、新しいエラーに対応する新しいコードが処理されることができます。

関数または `do` ブロックが単一のタイプのエラーのみをスローする場合、Swift はこのコードが型付きスロー を使用していると推論します。この短い構文を使用すると、上記の `do-catch` の例を次のように記述できます。

```swift
let ratings = []
do {
    try summarize(ratings)
} catch {
    switch error {
    case .noRatings:
        print("No ratings available")
    case .invalidRating(let rating):
        print("Invalid rating: \(rating)")
    }
}
// Prints "No ratings available"
```

上記の `do-catch` ブロックでは、スローするエラーの種類を指定していませんが、Swift は `StatisticsError` をスローすると推論します。Swift がスローする型を推論しないようにするためには、`throws(any Error)` と明示的に記述します。

## クリーンアップアクションの指定\(Specifying Cleanup Actions\)

`defer` 文を使用して、コードの実行が現在のコードブロックを離れる直前に一連の文を実行します。この文を使用すると、エラーのスローや、`return`、`break` のような文など、現在のコードブロックを離れる方法に関係なく、必要なクリーンアップを実行できます。例えば、`defer` 文を使用して、ファイル記述子を閉じ、手動で割り当てられたメモリを解放することができます。

`defer` 文は、終了するまで現在のスコープの実行を延期します。この文は、`defer` キーワードと後で実行される文で構成されます。`defer` 文には、`break` や `return` 文など、またはエラーをスローするなど、文から制御を他に移すコードを含めることはできません `defer` 文は、ソースコードに記述された順序とは逆の順序で実行されます。つまり、最初の `defer` 文のコードは最後に実行され、2 番目の `defer` 文のコードは最後から 2 番目に実行されます。ソースコードの順序で最後の `defer` 文が最初に実行されます。

```swift
func processFile(filename: String) throws {
    if exists(filename) {
        let file = open(filename)
        defer {
            close(file)
        }
        while let line = try file.readline() {
            // ファイルを使って処理をします
        }
        // close(file) がスコープの最後に呼ばれます
    }
}
```

上記の例では、`defer` 文を使用して、`open(_:)` 関数に対応する `close(_:)` への呼び出しが確実に実行されること保証しています。

エラー処理コードが含まれていない場合でも、`defer` 文を使用できます。
より詳細は[遅延アクション\(Deferred Actions\)](./control-flow.md#遅延アクションdeferred-actions)