# Error Handling(エラー処理)

最終更新日:

エラー処理(*error handling*)は、プログラムのエラー状態に応答し、エラー状態から回復するプロセスです。Swift は、実行時に回復可能なエラーをスロー、キャッチ、伝播、および操作するためのファーストクラスのサポートを提供します。

一部の操作は、常に実行を完了したり、有益な出力をするとは限りません。オプショナルは値がないことを表すために使用されますが、操作が失敗した場合、コードがそれに応じて応答できるように、失敗の原因を理解しておくと役立つことがよくあります。

例として、ディスク上のファイルからデータを読み取って処理するタスクを考えてみましょう。指定されたパスにファイルが存在しない、ファイルが読み取り権限を持たない、ファイルが互換性のある形式でエンコードされていないなど、このタスクが失敗する原因はいくつかあります。これらの様々な状況を区別することで、プログラムはいくつかのエラーを解決し、解決できないエラーをユーザに伝えることができます。

> NOTE  
> Swift のエラー処理は、Cocoa および Objective-C の NSError クラスを使用するエラー処理パターンと相互運用します。このクラスの詳細については、[Handling Cocoa Errors in Swift](https://developer.apple.com/documentation/swift/cocoa_design_patterns/handling_cocoa_errors_in_swift)を参照ください。

## Representing and Throwing Errors(エラーの表現とスロー)

Swift では、エラーは `Error` プロトコルに準拠した型の値によって表されます。この空のプロトコルは、エラー処理に型を使用できることを示します。

Swift の列挙型は、関連するエラー条件のグループをモデル化するのに特に適しています。関連する値を使用すると、エラーの性質に関する追加情報を伝達できます。例えば、ゲーム内で自動販売機を操作する場合のエラー状態を表す方法は次のとおりです。

```swift
enum VendingMachineError: Error {
    case invalidSelection
    case insufficientFunds(coinsNeeded: Int)
    case outOfStock
}
```

エラーをスローすると、予期しないことが発生し、通常の実行フローを続行できないことを示すことができます。エラーをスローするには、`throw` 文を使用します。例えば、次のコードはエラーをスローして、自動販売機で 5 枚の追加のコインが必要なことを示します。

```swift
throw VendingMachineError.insufficientFunds(coinsNeeded: 5)
```

## Handling Errors(エラー処理)

エラーがスローされた場合、周囲のコードの一部がエラーを処理する必要があります。例えば、問題を修正する、別のアプローチを試みる、またはユーザーにエラーを通知するなどです。

Swift でエラーを処理する方法は 4 つあります。関数からその関数を呼び出すコードにエラーを伝播したり、`do-catch` 文を使用してエラーを処理したり、オプションの値としてエラーを処理したり、エラーが発生しないことをアサートしたりできます。各アプローチについては、以下のセクションで説明します。

関数がエラーをスローすると、プログラムのフローが変わるため、エラーをスローする可能性のあるコード内の場所をすばやく特定できることが重要です。コード内のこれらの場所を特定するには、`try` キーワード (または `try?` または `try！`) をエラーをスローする可能性のある関数、メソッド、またはイニシャライザを呼び出すコードの前に置きます。これらのキーワードについては、以下のセクションで説明します。

> NOTE  
> Swift でのエラー処理は、他の言語での例外処理に似ており、`try`、`catch`、`throw` キーワードを使用しています。Objective-C を含む多くの言語の例外処理とは異なり、Swift でのエラー処理には、計算コストがかかる可能性のあるプロセスであるコールスタックの巻き戻しが含まれません。したがって、`throw` 文のパフォーマンスは、`return` 文のパフォーマンスに匹敵します。

### Propagating Errors Using Throwing Functions(スロー関数を使用したエラーの伝播)

関数、メソッド、またはイニシャライザがエラーをスローできることを示すには、関数の宣言のパラメータの後に `throws` キーワードを記述します。`throws` でマークされた関数は、スロー(*throwing*)関数と呼ばれます。関数が戻り型を指定する場合は、戻り矢印 (`->`) の前に `throws` キーワードを記述します:

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

`vend(itemNamed:)` メソッドの実装では、`guard` 文を使用してメソッドを早期に終了し、スナックを購入するための要件が満たされていない場合に適切なエラーをスローします。`throw` 文はすぐにプログラム制御を転送するため、これらすべての要件が満たされた場合にのみアイテムが販売されます。

`vend(itemNamed:)` メソッドはスローしたエラーをすべて伝搬するため、このメソッドを呼び出すコードは、`do-catch` 文、`try?`、または `try!` を使用してエラーを処理するか、エラーを伝搬し続ける必要があります。例えば、下記の例の `buyFavoriteSnack(person：vendingMachine:)` もスロー関数で、`vend(itemNamed:)` メソッドがスローするエラーを `buyFavoriteSnack(person：vendingMachine:)` 関数が呼ばれる部分まで伝播します。

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

### Handling Errors Using Do-Catch(do catchを使ったエラー処理)

### Converting Errors to Optional Values(エラーからオプショナル値への変換)

### Disabling Error Propagation(エラーの伝播を抑える)

## Specifying Cleanup Actions(クリーンアップアクションの指定)
