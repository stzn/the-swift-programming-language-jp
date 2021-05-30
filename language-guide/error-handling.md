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

### Propagating Errors Using Throwing Functions(スロー関数を使用したエラーの伝播)

### Handling Errors Using Do-Catch(do catchを使ったエラー処理)

### Converting Errors to Optional Values(エラーからオプショナル値への変換)

### Disabling Error Propagation(エラーの伝播を抑える)

## Specifying Cleanup Actions(クリーンアップアクションの指定)
