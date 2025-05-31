# 並行処理\(Concurrency\)

最終更新日: 2025/05/31
原文: https://docs.swift.org/swift-book/LanguageGuide/Concurrency.html

非同期操作を行う。

Swift には、構造化された方法で非同期および並行コードを書くためのサポートが組み込まれています。非同期コード\(_asynchronous code_\)は、コードは一度にプログラムの 1 箇所のみで実行されますが、後で中断\(_suspend_\)および再開\(_resume_\)できます。こうすることで、ネットワーク上のデータの取得やファイルの解析などの長く時間のかかる操作の途中で、UI の更新などの短い時間で完了できる操作を行い、その後引き続き操作を続けることができます。並列コード\(_parallel code_\)とは、複数のコードを同時に実行することを意味します。例えば、4 コアプロセッサを搭載したコンピュータは、各コアがタスクを 1 つ実行し、4 つのコードを同時に実行できます。並列および非同期コードを使用するプログラムは、一度に複数の操作を実行します。そして、外部システムからの結果を待つ操作を中断します。この章の残りの部分では、非同期コードと並列コードのこのよくある組み合わせを参照するために並行\(_concurrency_\) という用語を使用します。

並列または非同期コードを使って様々なタスクを追加でスケジューリングできる柔軟性は、逆に複雑さが増すコストも伴います。並行コードを書く際、どのコードが同時に実行されるかを事前に知ることはできず、また、コードが実行される順序も常に把握できるとは限りません。並行コードにおけるよくある問題は、複数のコード片が何らかの共有された可変状態にアクセスしようとするときに発生します。これはデータ競合\(_data race_\)として知られています。言語レベルの並行処理サポートを使用すると、Swift はデータ競合を検出し防止します。そして、ほとんどのデータ競合はコンパイルエラーを生成します。一部のデータ競合は、コードが実行されるまで検出できず、コードの実行を終了させます。この章で説明するように、データ競合から保護するためにアクターと隔離を使用します。

> NOTE  
> 過去に並行コードを書いたことがある場合は、スレッドの操作に慣れているかもしれません。Swiftの 並行モデルはスレッドの上に構築されていますが、直接スレッドとやり取りすることはありません。Swift の非同期関数は、実行中のスレッドを放棄することができ、最初の関数がブロックされている間、そのスレッド上で別の非同期関数を実行できます。非同期関数が再開するとき、その関数がどのスレッドで実行されるかについて、Swiftは何も保証しません。

Swift の言語サポートを使用せずに並行処理を書くことは可能ですが、そのコードは読みにくくなる傾向があります。例えば、次のコードは写真名のリストをダウンロードし、そのリストの最初の写真をダウンロードし、その写真をユーザに表示します。

```swift
listPhotos(inGallery: "夏休み") { photoNames in
    let sortedNames = photoNames.sorted()
    let name = sortedNames[0]
    downloadPhoto(named: name) { photo in
        show(photo)
    }
}
```

このような単純なケースでも、コードには一連の完了ハンドラを書く必要があるため、ネストしてクロージャを書くことになります。このスタイルでは、より複雑な深いネストを持つコードはすぐに扱いにくくなってしまいます。

## 非同期関数の定義と呼び出し\(Defining and Calling Asynchronous Functions\)

非同期関数\(_asynchronous function_\)または非同期メソッド\(_asynchronous method_\)は、実行の途中で中断できる特別な種類の関数またはメソッドです。これは、完了するまで実行するか、エラーをスローするか、何も値を返さない通常の同期関数\(_synchronous function_\)や同期メソッド\(_synchronous method_\)とは対照的です。非同期関数またはメソッドは、これら 3 つのことのいずれかを行いますが、何かを待っているときに途中で一時中断する可能性があります。非同期関数またはメソッドの本文内で、実行が中断する可能性のある箇所を示します。

関数またはメソッドが非同期だということを示すには、エラーをスローすることを示すために `throws` を使用するのと同様に、パラメータの後に `async` キーワードをその宣言に書きます。関数またはメソッドが値を返す場合は、戻り値の型の矢印\(`->`\)の前に `async` を書いてください。例えば、ギャラリ内の写真の名前を取得する方法が次のとおりです:

```swift
func listPhotos(inGallery name: String) async -> [String] {
    let result = // ... ある非同期のネットワークコード...
    return result
}
```

非同期かつエラーをスローする関数またはメソッドの場合は、`throws` の前に `async` を書きます。

非同期メソッドを呼び出すと、そのメソッドが戻るまで実行が中断されます。中断する可能性のある_中断ポイント_\(_suspension point_\)を示すために、メソッド呼び出しの前に `await` を書きます。これは、スロー関数を呼び出すときにエラーが発生した場合にプログラムの流れを変更する可能性を示すために `try` を書くのと同様です。非同期メソッド内では、別の非同期メソッドを呼び出す場合にのみ、実行フローが中断される可能性があります。中断は決して暗黙的に処理を割り込むものではありません。つまり、全ての中断する可能性があるポイントには `await` をマークします。
コードに中断する可能性のあるポイントを全てマークすることで、同時実行されるコードを読みやすくし、理解しやすくできます。


例えば、下記のコードは、ギャラリ内の全ての写真名を取得し、最初の写真を表示します:

```swift
let photoNames = await listPhotos(inGallery: "夏休み")
let sortedNames = photoNames.sorted()
let name = sortedNames[0]
let photo = await downloadPhoto(named: name)
show(photo)
```

`listPhotos(inGallery:)` と `downloadPhoto(named:)` 関数は両方ともネットワークリクエストをする必要があり、完了するまでかなり時間がかかる可能性があります。戻り値の型の矢印の前に `async` を書くことで両方とも非同期にすると、このコードが写真を取得するのを待っている間も、他のアプリのコードは実行し続けることができます。

上記の例の並行性を理解するために、ここでは 1 つの起こりうる実行順序について示します:

1. コードは最初の行から実行を開始し、最初の `await` まで実行されます。`listPhotos(inGallery:)` 関数を呼び出し、その関数が返されるのを待つ間、実行を中断します
2. このコードの実行は中断されていますが、同じプログラム内の他の並行処理が行われます。例えば、長期間実行されるバックグラウンドタスクは、新しいフォトギャラリのリストを更新し続けます。そのコードは、`await` が書かれた次の中断ポイント、または処理が完了するまで実行されます
3. `listphotos(inGallery:)` から戻り値が返された後、その地点から処理は再開され `photoNames` に戻り値を割り当てます
4. `sortedNames` と `name` を定義する行は、通常の同期処理です。これらの行では何もマークされていないため、中断される可能性はありません
5. 次の `await` は、`downloadPhoto(named:)` 関数の呼び出し時に示されています。このコードは、その関数が戻り値を返すまで再び実行を一時中断し、他の並行処理を実行する機会を与えます
6. `downloadPhoto(named:)` が戻り値を返し、その戻り値が `photo` に割り当てられ、`show(_:)` を呼び出すときに引数として渡されています

`await` がマークされた、コード内で処理が中断する可能性のあるポイントは、非同期関数またはメソッドが戻るのを待っている間に、現在のコードが処理を一時中断することを示しています。内部では、Swift は、現在のスレッド上のコードの実行を中断し、代わりにそのスレッド上の他のコードを実行するため、これはスレッドを譲る\(_yielding the thread_\)と呼ばれます。`await` 時にコードを中断できるようにする必要があるため、プログラム内の特定の場所だけが非同期関数またはメソッドを呼び出すことができます:

* 非同期の関数、メソッド、プロパティの本文
* `@main` がマークされている構造体、クラス、または列挙型の `static main()` メソッド内
* 下記の[Unstructured Concurrency\(非構造化並行処理\)](concurrency.md#unstructured-concurrency)で示す非構造化子タスク\(_child task_\)のコード

[`Task.sleep(for:tolerance:clock:)`](https://developer.apple.com/documentation/swift/task/sleep(for:tolerance:clock:))メソッドは並行処理がどのように機能するかを学ぶためにシンプルなコードを書くのに便利です。このメソッドは、少なくとも指定した時間だけ現在のタスクを中断します。以下は `listPhotos(inGallery:)` 関数で、`sleep(for:tolerance:clock:)` を使ってネットワーク操作の待ち時間をシミュレートしています：

```swift
func listPhotos(inGallery name: String) async throws -> [String] {
    try await Task.sleep(for: .seconds(2))
    return ["IMG001", "IMG99", "IMG0404"]
}
```

上記のコードのバージョンの `listPhotos(inGallery:)` は、`Task.sleep(until:tolerance:clock:)` がエラーをするため、非同期かつ throwing です。このバージョンの `listPhotos(inGallery:)` を呼び出す際は、`try` と `await` 両方書きます。

```swift
let photos = try await listPhotos(inGallery: "雨の週末")
```

非同期関数は throwing 関数といくつか似ている点があります。
非同期や throwing 関数を定義する際は、`async` と `throws` を付け、呼び出し側は `await` と `try` を付けます。
throwing 関数は他の throwing 関数を呼び出すことができるのと同じように、非同期関数は他の非同期関数を呼び出すことができます。

しかし、重要な違いがあります。エラーをハンドリングをするために、エラーをスローするコードを `do-catch` ブロックで囲めたり、他の場所でエラーをハンドリングするために、コードで起こったエラー保持するために `Result` を使うことができます。
これらのアプローチは、エラーをスローしない関数から throwing 関数を呼び出せるようにします。
例えば、

```swift
func availableRainyWeekendPhotos() -> Result<[String], Error> {
    return Result {
        try listDownloadedPhotos(inGallery: "雨の週末")
    }
}
```

対照的に、同期コードから非同期コードを呼び出して結果を待つことができるように、非同期コードをラップする安全な方法はありません。Swift の標準ライブラリは、意図的にこの安全でない機能を省略しています。なぜなら、自分で実装しようとすると、わかりづらいデータ競合やスレッド問題、デッドロックのような問題につながる可能性があるからです。既存のプロジェクトに Concurrency コードを追加するときは、トップダウンで作業します。具体的には、コードの最上位レイヤーを並行処理に変換することから始め、そのコードが呼び出す関数やメソッドの変換を開始し、プロジェクトのアーキテクチャーを一度に 1 レイヤーずつ変換していきます。ボトムアップのアプローチは、これまでのところ同期コードが非同期コードを呼び出すことができないためできません。

## 非同期シーケンス\(Asynchronous Sequences\)

前のセクションの `listPhotos(inGallery:)` は、非同期に、全ての配列の要素が取得できてから配列全体を一度にまとめて返します。もう 1 つの方法として、非同期シーケンス\(_asynchronous sequence_\)を使用して、コレクションの要素を 1 つずつ待機することができます。下記では非同期シーケンスを使って配列の要素を 1 つ 1 つ取得しています:

```swift
import Foundation

let handle = FileHandle.standardInput
for try await line in handle.bytes.lines {
    print(line)
}
```

通常の `for-in` ループを使用する代わりに、上記の例は `for` の後に `await` を書きます。非同期関数またはメソッドを呼び出すときのように、`await` は中断する可能性を示します。`for-await-in` ループは、次の要素が利用可能になるのを待っている間、各繰り返しの開始時に処理の実行を中断する可能性があります。

[Sequence](https://developer.apple.com/documentation/swift/sequence)プロトコルに準拠することで、`for-in` ループで独自の型を使用できるのと同じように、[AsyncSequence](https://developer.apple.com/documentation/swift/asyncsequence)プロトコルに準拠することで、`for-await-in` ループで独自の型を使用できます。

## 非同期関数を並列に呼び出す\(Calling Asynchronous Functions in Parallel\)

`await` を使用して非同期関数を呼び出すと、一度に 1 つのコードしか実行されません。非同期コードが実行されている間、呼び出し側は、次のコード行を実行する前にそのコードが終了するのを待ちます。例えば、ギャラリから最初の 3 つの写真を取得するには、次のように `downloadPhoto(named:)` 関数を 3 回呼び出して、結果を待つことができます:

```swift
let firstPhoto = await downloadPhoto(named: photoNames[0])
let secondPhoto = await downloadPhoto(named: photoNames[1])
let thirdPhoto = await downloadPhoto(named: photoNames[2])

let photos = [firstPhoto, secondPhoto, thirdPhoto]
show(photos)
```

しかし、このアプローチには重要な欠点があります: ダウンロードは非同期で、その実行中に他のタスクが実行できますが、`downloadPhoto(named:)` の呼び出しは一度に 1 つの呼び出ししか実行されません。つまり、次の写真のダウンロードを開始する前に、前の写真のダウンロードの完了を待ちます。しかし、これらの操作を待機する必要はありません。各写真の処理は独立して、同時にダウンロードできます。

非同期関数を並行に実行するには、`let` の前に `async` を書き、この定数を使用する度に `await` を書きます。

```swift
async let firstPhoto = downloadPhoto(named: photoNames[0])
async let secondPhoto = downloadPhoto(named: photoNames[1])
async let thirdPhoto = downloadPhoto(named: photoNames[2])

let photos = await [firstPhoto, secondPhoto, thirdPhoto]
show(photos)
```

この例では、`downloadPhoto(named:)` の 3 つの呼び出しは、前のものが完了するのを待たずに開始します。利用可能な十分なシステムリソースがある場合は、これらは並行に実行できます。これらの関数呼び出しには `await` がマークされていないため、関数の結果を待つためにコードが中断されません。代わりに、`photos` が定義されている行まで処理は継続し、その時点で、プログラムはこれらの非同期呼び出しの結果を必要とするため、3 つの写真のダウンロードが完了するまで実行を一時中断するため、 `await` を書きます。

下記に、この 2 つのアプローチの違いについて説明します:

* 次の行のコードでその関数の結果が必要な場合は、`await` を使用して非同期関数を呼び出します。これにより、順番に実行されるタスクが作成されます
* 後々のコードまで​​結果が必要ない場合、`async-let` を使用して非同期関数を呼び出すと、並行に実行できるタスクが作成されます
* `await` と `async-let` のどちらも、中断されている間に他のコードを実行できるようにします
* どちらの場合も、非同期関数が結果を返すまで、必要に応じて実行が一時中断することを示すために、`await` を使って中断ポイントを示します

これらのアプローチの両方を混在させることもできます。

## タスクとタスクグループ\(Tasks and Task Groups\)

タスク\(_task_\)は、プログラムの一部として非同期に実行できる作業単位です。全ての非同期コードはいくつかのタスクの一部として実行されます。タスク自体は一度に 1 つのことしか行いませんが、複数のタスクを作成した際、Swift はそれらのタスクを同時に実行するためにスケジューリングします。

前のセクションで説明されている `async-let` 構文は、暗黙的に子タスク\(_child task_\)を作成します。この構文は、すでにどのプログラムを実行する必要があるかがわかっている場合に便利です。
タスクグループ\(_task group_\)([`TaskGroup`](https://developer.apple.com/documentation/swift/taskgroup) インスタンス)を作成し、明示的にそのグループに子タスクを追加することもできます。こうすることで、グループでの優先順位とキャンセルをより制御しやすくし、動的な数のタスクを作成することもできます。

タスクは階層を構築します。タスクグループ内の各タスクは同じ親タスク\(_parent task_\)を持ち、各タスクは子タスクを持つことができます。タスクとタスクグループの間の明示的な関係のために、このアプローチは構造化された並行性\(_structured concurrency_\)と呼ばれます。
タスク間のこの明示的な親子関係にはさまざまなメリットがあります。

- 親タスクが子タスクの完了を待つことを忘れることはありません
- 子タスクに高い優先度を設定すると、親タスクの優先度が自動的にエスカレーションされます
- 親タスクがキャンセルされると、子タスクも自動的にキャンセルされます
- タスクローカルの値は、効率的かつ自動的に子タスクに伝搬します

以下は、任意の枚数の写真を処理する、写真をダウンロードするコードの別バージョンです:

```swift
await withTaskGroup(of: Data.self) { group in
    let photoNames = await listPhotos(inGallery: "夏休み")
    for name in photoNames {
        group.addTask { await downloadPhoto(named: name) }
    }

    for await photo in group {
        show(photo)
    }
}
```

上記のコードは、新しいタスクグループを作成し、ギャラリーの各写真をダウンロードする子タスクを作成します。Swift は条件が許す限り、これらのタスクを同時に実行します。子タスクが写真のダウンロードを終了するとすぐに、その写真が表示されます。子タスクが完了する順番についての保証はないので、このギャラリーの写真は任意の順番で表示されます。

> NOTE: 写真をダウンロードするコードがエラーを投げる可能性がある場合、代わりに `withThrowingTaskGroup(of:returning:body:)` を呼び出します。

上のコードリストでは、各写真はダウンロードされてから表示されるので、タスクグループは結果を返しません。結果を返すタスクグループの場合は、`withTaskGroup(of:returning:body:)` に渡すクロージャの中に結果を蓄積するコードを追加します。

```swift
let photos = await withTaskGroup(of: Data.self) { group in
    let photoNames = await listPhotos(inGallery: "夏休み")
    for name in photoNames {
        group.addTask {
            return await downloadPhoto(named: name)
        }
    }

    var results: [Data] = []
    for await photo in group {
        results.append(photo)
    }

    return results
}
```

前の例と同じように、この例でも写真ごとに子タスクを作成してダウンロードしています。前の例とは異なり、`for-await-in` ループは次の子タスクの終了を待ち、そのタスクの結果を結果の配列に追加して、全ての子タスクが終了するまで待ち続けます。最後に、タスクグループはダウンロードした写真の配列を全体の結果として返します。

### <a id="task_cancellation">タスクキャンセル\(Task Cancellation\)</a>

Swift の並行処理では、協調キャンセルモデルを使用します。各タスクは、実行中の適切な時点でキャンセルされたかどうかをチェックし、キャンセルに適切に応答します。タスクがどのような作業をしているかに応じて、キャンセルに応答することは、通常、次のいずれかを意味します:

- `CancellationError` のようなエラーをスローする
- `nil` または空のコレクションを返す
- 部分的に完了した作業を返す

写真が大きかったり、ネットワークが遅かったりすると、写真のダウンロードに時間がかかることがあります。全てのタスクが完了するのを待たずに、ユーザーがこの作業を停止できるようにするには、タスクがキャンセルをチェックし、キャンセルされたら実行を停止する必要があります。タスクがこれを行うには、[`Task.checkCancellation()` 型メソッド](https://developer.apple.com/documentation/swift/task/3814826-checkcancellation)を呼び出す方法と、[`Task.isCancelled` 型プロパティ](https://developer.apple.com/documentation/swift/task/iscancelled-swift.type.property)を読み取る方法の 2 つがあります。`checkCancellation()` を呼び出すと、タスクがキャンセルされていた場合にエラーがスローされます。スローされたタスクは、エラーをタスクの外に伝播し、タスクの全ての作業を停止することができます。これは実装が簡単で理解しやすいという利点があります。より柔軟にするには、`isCancelled` プロパティを使用します。このプロパティを使用すると、ネットワーク接続を閉じたり、一時ファイルを削除するなど、タスクの停止の一部としてクリーンアップ作業を実行できます。

```swift
let photos = await withTaskGroup { group in
    let photoNames = await listPhotos(inGallery: "夏休み")
    for name in photoNames {
        let added = group.addTaskUnlessCancelled {
            Task.isCancelled ? nil : await downloadPhoto(named: name)
        }
        guard added else { break }
    }

    var results: [Data] = []
    for await photo in group {
        if let photo { results.append(photo) }
    }
    return results
}
```

上記のコードでは、以前のバージョンからいくつかの変更が加えられています:

- 各タスクは [`TaskGroup.addTaskUnlessCancelled(priority:operation:)`](https://developer.apple.com/documentation/swift/taskgroup/addtaskunlesscancelled(priority:operation:))メソッドを使用して追加され、キャンセル後に新しい作業が開始されるのを防ぎます
- `addTaskUnlessCancelled(priority:operation:)` を呼び出すたびに、コードは新しい子タスクが追加されたことを確認します。グループがキャンセルされた場合、`added` の値は `false` になります。上記の例の場合、コードは追加の写真をダウンロードしようとするのを止めます
- 各タスクは、写真のダウンロードを開始する前に、キャンセルされたかどうかをチェックします。キャンセルされた場合、タスクは `nil` を返します
- 最後に、タスクグループは結果を集める際に `nil` 値をスキップします。`nil` を返すことでキャンセルを処理することは、タスクグループが完了した作業を破棄する代わりに、部分的な結果(キャンセル時にすでにダウンロードされていた写真)を返すことができるということです

 > NOTE: タスクの外側からタスクがキャンセルされたかどうかを確認するには、型プロパティの代わりに [`Task.isCancelled` インスタンスプロパティ](https://developer.apple.com/documentation/swift/task/iscancelled-swift.property)を使用します。

キャンセルの即時通知が必要な作業には、[`Task.withTaskCancellationHandler(operation:onCancel:)`](https://developer.apple.com/documentation/swift/withtaskcancellationhandler(operation:oncancel:))メソッドを使用します。たとえば:

```swift
let task = await Task.withTaskCancellationHandler {
    // ...
} onCancel: {
    print("キャンセルされた!")
}

// ... 少し経って...
task.cancel()  // Prints "キャンセルされた!"
```

キャンセルハンドラーを使用する場合、タスクのキャンセルはまだ協調的です。つまり、タスクを完了まで実行するか、途中でキャンセルをチェックして早期に停止する必要があります。キャンセルハンドラーが開始してもタスクはまだ実行中なので、タスクとキャンセルハンドラーの間で状態を共有することは避けてください。

### <a id="unstructured-concurrency">非構造化並行処理\(Unstructured Concurrency\)</a>

前のセクションで説明されている構造化された並行処理のアプローチに加えて、Swift は非構造化並行処理\(_unstructured concurrency_\)もサポートしています。タスクグループの一部のタスクとは異なり、非構造化タスク\(_unstructured task_\)には親タスクがありません。どんな方法で使われたとしても、非構造化タスクを完全に柔軟に管理することができます。しかし、それらの正しい動作を保証することは完全に開発者の責任です。

周囲のコードと同様に実行される非構造化タスクを作成するには、[Task.init\(priority:operation:\)](https://developer.apple.com/documentation/swift/task/3856790-init) イニシャライザを呼びます。デフォルトで、現在のタスクと同じアクター隔離、優先度、およびタスクローカル状態で実行されます。周囲のコードからより独立した非構造化タスク（より具体的には切り離されたタスク\(_detached task_\)として知られるもの）を作成するには、[Task.detached\(priority:operation:\)](https://developer.apple.com/documentation/swift/task/3856786-detached) 関数を呼び出します。新しいタスクは、デフォルトでどのアクターにも隔離されずに実行され、現在のタスクの優先度やタスクローカル状態を継承しません。これらの操作はいずれも操作可能なタスクを返します。例えば、その結果を待機したり、キャンセルしたりできます。

```swift
let newPhoto = // ... ある写真データ ...
let handle = Task {
    return await add(newPhoto, toGalleryNamed: "Spring Adventures")
}
let result = await handle.value
```

切り離されたタスクの管理の詳細については、[Task](https://developer.apple.com/documentation/swift/task)を参照ください。

## 隔離

前のセクションでは、並行処理を分割するアプローチについて説明しました。その処理には、アプリの UI などの共有データの変更が含まれる場合があります。コードの異なる部分が同じデータを同時に変更できる場合、データ競合が発生するリスクがあります。Swift はコード内のデータ競合から保護します。データを読み取ったり変更したりするたびに、Swift は他のコードが同時にそれを変更していないことを保証します。この保証はデータ隔離\(_data isolation_\)と呼ばれます。データを隔離するには、主に 3 つの方法があります。

1. 不変データは常に隔離されている。定数は変更できないため、読み取りと同時に他のコードが定数を変更するリスクはない
2. 現在のタスクのみから参照されるデータは常に隔離されている。ローカル変数は、タスク外のコードがそのメモリへの参照を持たないため、安全に読み書きできる。したがって、他のコードがそのデータを変更することはできない。さらに、変数をクロージャでキャプチャする場合、Swift はそのクロージャが同時に使用されないことを保証する
3. アクターによって保護されているデータは、そのデータにアクセスするコードもアクターに隔離されている場合に隔離される。現在の関数がアクターに隔離されている場合、そのアクターによって保護されているデータの読み書きは安全。なぜなら、同じアクターに隔離されている他のコードは、実行前に順番を待つ必要があるからだ

## <a id="the-main-actor">メインアクター\(The Main Actor\)</a>

アクターとは、コードに可変データへのアクセスを順番に行わせることで、そのデータへのアクセスを保護するオブジェクトです。多くのプログラムにおいて最も重要なアクターは、メインアクター\(_Main Actor_\)です。アプリでは、メインアクターは UI を表示するために使用される全てのデータを保護します。メインアクターは、UI のレンダリング、UI イベントの処理、そしてあなたが書いた UI のクエリや更新が必要なコードを順番に実行します。

並行処理を使い始める前は、全てのコードがメインアクター上で実行されます。長時間実行される、またはリソースを大量に消費するコードを見つけた場合、安全かつ正しい方法でこの作業をメインアクターから移動させることができます。

> 注: メインアクターはメインスレッドと密接に関連していますが、同じものではありません。メインアクターはプライベートな可変状態を持ち、メインスレッドはその状態へのアクセスを直列化します。メインアクター上でコードを実行すると、Swift はそのコードをメインスレッドで実行します。この関連性のため、これら 2 つの用語が同じ意味で使用されるのを見ることがあるかもしれません。あなたのコードはメインアクターとやり取りします。メインスレッドはより低レベルの実装詳細です。

メインアクター上で作業を実行するにはいくつかの方法があります。関数が常にメインアクター上で実行されるようにするには、`@MainActor` 属性をマークします。

```swift
@MainActor
func show(_: Data) {
    // ... 写真を表示するためのUIコード ...
}
```

上記のコードでは、`show(_:)` 関数にある `@MainActor` 属性は、この関数がメインアクター上でのみ実行されることを要求します。メインアクター上で実行されている他のコード内では、`show(_:)` を同期関数として呼び出すことができます。しかし、メインアクター上で実行されていないコードから `show(_:)` を呼び出すには、メインアクターへの切り替えが潜在的な中断ポイントになるため、`await` をマークして非同期関数として呼び出す必要があります。例えば、

```swift
func downloadAndShowPhoto(named name: String) async {
    let photo = await downloadPhoto(named: name)
    await show(photo)
}
```

上記のコードでは、`downloadPhoto(named:)` 関数と `show(_:)` 関数の両方とも、呼び出すと中断する可能性があります。このコードはまた、よくあるパターンも示しています。つまり、長時間実行され CPU 負荷の高い作業をバックグラウンドで実行し、その後 UI を更新するためにメインアクターに切り替えるというパターンです。`downloadAndShowPhoto(named:)` 関数はメインアクター上にないため、`downloadPhoto(named:)` 内の作業もメインアクター上では実行されません。`show(_:)` 内の UI を更新するための作業のみがメインアクター上で実行されます。なぜなら、その関数は `@MainActor` 属性がマークされているからです。

クロージャがメインアクター上で実行されるようにするには、クロージャの先頭、キャプチャリストの前、かつ `in` の前に `@MainActor` を書きます。

```swift
let photo = await downloadPhoto(named: "Trees at Sunrise")
Task { @MainActor in
    show(photo)
}
```

上記のコードは、前のコードリストの `downloadAndShowPhoto(named:)` に似ていますが、この例のコードは UI の更新を待機しません。また、構造体、クラス、または列挙型に `@MainActor` を書くことで、その全てのメソッドとプロパティへの全てのアクセスがメインアクター上で実行されるようにできます。

```swift
@MainActor
struct PhotoGallery {
    var photoNames: [String]
    func drawUI() { /* ... その他のUIコード ... */ }
}
```

上記のコードの `PhotoGallery` 構造体は、`photoNames` プロパティの名前を使用して表示する写真を決定し、写真を画面に描画します。`photoNames` は UI に影響を与えるため、それを変更するコードは、そのアクセスを直列化するためにメインアクター上で実行される必要があります。

フレームワーク上に構築する場合、そのフレームワークのプロトコルと基底クラスは通常、すでに `@MainActor` がマークされているため、その場合は通常、独自の型に `@MainActor` を書きません。以下に簡単な例を示します。

```swift
@MainActor
protocol View { /* ... */ }

// 暗黙的に@MainActor
struct PhotoGalleryView: View { /* ... */ }
```

上記のコードでは、SwiftUI のようなフレームワークが `View` プロトコルを定義しています。プロトコル宣言に `@MainActor` を書くことで、プロトコルに準拠する `PhotoGalleryView` のような型も暗黙的に `@MainActor` がマークされます。`View` が基底クラスで `PhotoGalleryView` がサブクラスであった場合も同様の動作になり、サブクラスは暗黙的に `@MainActor` がマークされます。

上記の例では、`PhotoGallery` は構造体全体をメインアクターで保護します。よりきめ細かく制御するために、メインスレッドでアクセスまたは実行する必要があるプロパティまたはメソッドにのみ `@MainActor` を書けます。

```swift
struct PhotoGallery {
    @MainActor var photoNames: [String]
    var hasCachedPhotos = false

    @MainActor func drawUI() { /* ... UIコード ... */ }
    func cachePhotos() { /* ... ネットワークコード ... */ }
}
```

上記の `PhotoGallery` のバージョンでは、`drawUI()` メソッドはギャラリーの写真を画面に描画するため、メインアクターに隔離する必要があります。`photoNames` プロパティは UI を直接作成しませんが、`drawUI()` 関数が UI を描画するために使用する状態を格納するため、このプロパティもメインアクター上でのみアクセスされる必要があります。対照的に、`hasCachedPhotos` プロパティへの変更は UI と対話せず、`cachePhotos()` メソッドはメインアクターでの実行を必要とする状態にアクセスしません。そのため、これらは `@MainActor` がマークされていません。

前の例と同様に、UI を構築するためにフレームワークを使用している場合、そのフレームワークのプロパティラッパは、おそらくすでに UI の状態プロパティを `@MainActor` でマークしています。プロパティラッパを定義する際、その `wrappedValue` プロパティが `@MainActor` がマークされている場合、そのプロパティラッパを適用する全てのプロパティも暗黙的に `@MainActor` がマークされます。

## アクター\(Actors\)

Swift にはメインアクターが用意されており、また独自のアクターを定義することもできます。アクターは並行コード間で情報を安全に共有することを可能にします。

クラスのようにアクターは参照型なので、[Classes Are Reference Types\(クラスは参照型\)](../language-guide/structures-and-classes.md#classes-are-reference-types)の中の値型と参照型の比較はアクターにも当てはまります。しかし、クラスとは異なり、アクターの可変状態\(_mutable state_\)にアクセスできるのは一度に 1 つのタスクだけです。これにより、複数のタスクが、同じアクターのインスタンスとやり取りする必要があるコードでも、安全にアクセスできるようになります。例えば、下記は気温を記録するアクターです:

```swift
actor TemperatureLogger {
    let label: String
    var measurements: [Int]
    private(set) var max: Int

    init(label: String, measurement: Int) {
        self.label = label
        self.measurements = [measurement]
        self.max = measurement
    }
}
```

中括弧ペア\(`{}`\)の定義の前に `actor` キーワードを付けてアクターを導入します。`TemperatureLogger` アクターには、外部の他のコードがアクセスできるプロパティがあり、`max` プロパティはアクター内のコードのみが最大値を更新できるように制限されています。

構造体やクラスと同じイニシャライザの構文を使用して、アクターのインスタンスを作成します。アクターのプロパティやメソッドにアクセスするとき、中断する可能性のあるポイントを示すために `await` を使用します。例えば:

```swift
let logger = TemperatureLogger(label: "アウトドア", measurement: 25)
print(await logger.max)
// 25
```

この例では、`logger.max` へのアクセスは中断する可能性があります。アクターはその可変状態にアクセスすることを一度に 1 つのタスクのみに制限しているため、別のタスクからのコードが既にロガーとやり取りしている場合、このコードはプロパティへのアクセスを待機します。

対照的に、アクター内のコードはアクターのプロパティにアクセスするときに、`await` を記載しません。例えば、下記は `TemperatureLogger` を新しい温度で更新するメソッドです:

```swift
extension TemperatureLogger {
    func update(with measurement: Int) {
        measurements.append(measurement)
        if measurement > max {
            max = measurement
        }
    }
}
```

`update(with:)` メソッドは既にアクター上で実行されているので、`max` のようなプロパティへのアクセスに `await` を書きません。また、このメソッドは、なぜアクターが一度に 1 つのタスクしかそれらの可変状態とやり取りすることを許さないのかの理由の 1 つを示しています: アクターの状態の更新は一時的に不変性を壊します。`TemperatureLogger` アクターは温度のリストと最大温度を追跡し、新しい測定を記録するときに最高温度を更新します。アップデートの途中で、新しい測定値が追加されても、`max` が更新される前の状態だと、温度ロガーは一時的に矛盾した状態になります。複数のタスクが同じインスタンスと同時にやり取りする問題を防ぐことで、次のような問題を防ぎます:

1. `update(with:)` を呼び出して、まず `measurements` 配列を更新します
2. コードが `max` を更新する前に、他の場所で最大値と気温の配列を読み取ります
3. コードは `max` を変更することによって更新を終了します

この場合、データが一時的に不正になっていた間に、`update(with:)` の途中で割り込んでアクターへアクセスされた場合、他の場所で実行されているコードが不正な情報を読み取るかもしれません。Swift のアクターを使用すると、それらの状態に対して一度に 1 つの操作のみアクセス可能なことと、`await` をマークした場所でのみ中断される可能性があるため、この問題を防ぐことができます。`update(with:)` では中断ポイントが含まれていないため、他のコードは `update` の途中で他のコードからデータにアクセスできません。

アクターの外のコードが、構造体やクラスのプロパティにアクセスするように、これらのプロパティに直接アクセスしようとすると、コンパイル時にエラーが発生します。例えば:

```swift
print(logger.max)  // エラー
```

アクターのプロパティはそのアクターの独立したローカル状態の一部のため、`await` を書かないと `logger.max` へのアクセスは失敗します。このプロパティにアクセスするコードは、アクタの一部として実行する必要があり、これは非同期操作であり、`await` をマークする必要があります。Swift は、実行されているコードのみがアクターに隔離されたローカルの状態にアクセスできることを保証します。この保証はアクター隔離\(_actor isolation_\)と呼ばれています。

Swift の並行処理モデルの以下の側面は、共有された可変状態について推論しやすくするために一緒に機能します:

- 中断する可能性のあるポイントの間のコードは、他の並行処理コードから中断される可能性はなく、順次実行される。しかし、複数の並行コードは同時に実行される場合があるため、他のコードが同時に実行されている可能性がある
- アクターのローカル状態と相互作用するコードは、そのアクター上でのみ実行される
- アクターは一度に 1 つのコードだけを実行する

このような保証があるため、アクターの内部にあるコードは `await` を含まず、プログラム内の他の場所で一時的に不正な状態になるリスクなしに更新できます。例えば、以下のコードは測定された温度を華氏から摂氏に変換します:

```swift
extension TemperatureLogger {
    func convertFahrenheitToCelsius() {
        for i in measurements.indices {
            measurements[i] = (measurements[i] - 32) * 5 / 9
        }
    }
}
```

上のコードでは、測定値の配列を一度に 1 つずつ変換しています。map 操作の進行中、いくつかの温度は華氏で、他の温度は摂氏です。しかし、どのコードにも `await` が含まれていないため、このメソッドには潜在的な中断ポイントがありません。このメソッドが変更する状態はアクターに属し、そのアクター上ではないコードが読んだり変更することから保護されています。つまり、単位変換の進行中に、他のコードが部分的に変換された温度のリストを読む方法がないということです。

潜在的な中断ポイントをなくすことで一時的な不正状態から保護されているアクター内にコードを書くことに加えて、そのコードを同期メソッドに移すことができます。上記の `convertFahrenheitToCelsius()` メソッドは同期メソッドなので、潜在的な中断ポイントを含まないことが保証されています。この関数は、一時的にデータモデルの一貫性を失わせるコードをカプセル化し、データの一貫性を取り戻す前に他のコードが実行できないようにして作業を完了させることを、コードを読んだ人が簡単に認識できるようにします。その期間中、Swift がこのコードからプログラムの別の部分のコードを実行するために切り替えないことが重要です。将来、この関数に並行コードを追加して、潜在的な中断ポイントを導入しようとすると、バグを導入する代わりにコンパイルエラーが発生します。

## グローバルアクター\(Global Actors\)

メインアクターは、[MainActor](https://developer.apple.com/documentation/swift/mainactor) 型のグローバルなシングルトンインスタンスです。通常、アクターは複数のインスタンスを持つことができ、各インスタンスは独立した隔離を提供します。そのため、アクターに隔離されたデータはすべて、そのアクターのインスタンスプロパティとして宣言します。しかし、`MainActor` はシングルトンであるため（この型のインスタンスは常に 1 つしか存在しません）、型のみでアクターを識別するには十分であり、属性を使用するだけでメインアクター隔離をマークできます。このアプローチにより、自身にとって最適な方法でコードを構成するための、より大きな柔軟性が得られます。

[globalActor](../language-reference/attributes.md#globalactor)で説明されているように、`@globalActor` 属性を使用して、独自のシングルトンのグローバルアクターを定義できます。

## <a id="sendable-types">`Sendable` 型\(Sendable Types\)</a>

タスクとアクターを使用すると、プログラムを安全に同時に実行できるように、分割することができます。タスクまたはアクターのインスタンスの内部では、変数やプロパティなどの可変状態を含むプログラムの部分は、「同時実行ドメイン」と呼ばれます。一部の種類のデータは、データに可変状態が含まれ、かつ重複するアクセスから保護できないことから、同時実行ドメイン間で共有できません。

ある同時実行ドメインから別のドメインに共有できる型は、「`Sendable` 型\(_Sendable Types_\)」と呼ばれます。例えば、アクターメソッドを呼び出すときに引数として渡すことも、タスクの結果として返すこともできます。この章の前半の例では、同時実行ドメイン間で渡されるデータに対して常に安全に共有できる単純な値型を使用しているため、`Sendable` かどうか\(_sendability_\)については説明していません。一方で、一部の型は同時実行ドメイン間で安全に渡すことができません。例えば、可変プロパティを含み、それらのプロパティへのアクセスを直列化していないクラスは、異なるタスク間でそのクラスのインスタンスを渡すときに、予測できない誤った結果を生成する可能性があります。

`Sendable` プロトコルへの準拠を宣言することにより、型が `Sendable` なことを示すことができます。`Sendable` プロトコルにはコード要件はありませんが、Swift が強制するセマンティック要件があります。一般に、型を `Sendable` にするには 3 つの方法があります。

- 値型で、その内部の可変状態が全て他の `Sendable` なデータで構成されている場合。例えば、`Sendable` な格納プロパティを持つ構造体や、`Sendable` な関連値を持つ列挙型など
- 可変状態がなく、不変で `Sendable` なデータのみで構成されている場合。例えば、計算プロパティのみを持つ構造体やクラスなど
- `@MainActor` の付いたクラスや、特定のスレッドまたはキューのプロパティへのアクセスを直列化しているクラスなど、可変状態の安全性を保証するコードが含まれている場合

セマンティック要件の詳細については、[Sendable](https://developer.apple.com/documentation/swift/sendable) プロトコルを参照ください。

`Sendable` なプロパティのみを持つ構造体や、`Sendable` な関連値のみを持つ列挙体など、一部の型は常に `Sendable` です。例えば:

```swift
struct TemperatureReading: Sendable {
    var measurement: Int
}

extension TemperatureLogger {
    func addReading(from reading: TemperatureReading) {
        measurements.append(reading.measurement)
    }
}

let logger = TemperatureLogger(label: "紅茶用のやかん", measurement: 85)
let reading = TemperatureReading(measurement: 45)
await logger.addReading(from: reading)
```

`TemperatureReading` は `Sendable` なプロパティのみを持つ構造体で、`public` または `@usableFromInline` としてマークされていないため、暗黙的に `Sendable` です。`Sendable` プロトコルへの準拠が暗示されている構造体のバージョンは次のとおりです:

```swift
struct TemperatureReading {
    var measurement: Int
}
```

`Sendable` プロトコルへの暗黙の準拠を上書きするには、`~Sendable` と書きます:

```swift
struct FileDescriptor: ~Sendable {
    let rawValue: Int
}
```

プロトコルへの暗黙の準拠を抑制する詳細な情報は、[暗黙の制約\(Implicit Constraints\)](./generics.md#暗黙の制約implicit-constraints) を参照ください。