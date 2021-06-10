# Concurrency

最終更新日: 2021/6/10

Swift には、構造化された方法で非同期および同時並行コードを書くためのサポートが組み込まれています。非同期コード(*asynchronous code*)は、一度にプログラムの 1 箇所のみでコードが実行されますが、後で中断(*suspend*)および再開(*resume*)できます。こうすることで、ネットワーク上のデータの取得やファイルの解析などの長く時間のかかる操作の途中で、UI の更新などの短い時間で完了できる操作を行い、その後引き続き操作を続けることができます。並列コード(*parallel code*)とは、複数のコードを同時に実行することを意味します。例えば、4 コアプロセッサを搭載したコンピュータは、各コアがタスクを 1 つ実行し、4 つのコードを同時に実行できます。並列および非同期コードを使用するプログラムは、一度に複数の操作を実行します。外部システムを待っている操作を一時中断し、メモリセーフな方法でこのコードを簡単に記述できます。

並列または非同期コードを使って様々なタスクを追加でスケジューリングできる柔軟性は、逆に複雑さが増すコストも伴います。Swift を使用すると、コンパイル時のチェックでコードの意図を表現できます。例えば、アクター(*actor*)を使用してその可変状態(*mutable state*)に安全にアクセスできます。ただし、元々処理速度の遅いコードやバグを含んだコードに同時並行処理を追加しても、処理速度が速くなったりコードが正しくなる、というようなことは保証されません。実際、同時並行処理を追加すると、コードのデバッグが難しくなる可能性さえあります。ただし、同時並行に処理をする必要があるコードに Swift の言語レベルのサポートを使用することで、コンパイル時に問題をキャッチすることができます。

この章の残りの部分では、非同期コードと並列コードのこの一般的な組み合わせを参照するために同時並行(*concurrency*) という用語を使用します。

> NOTE  
> 過去に同時並行コードを書いたことがある場合は、スレッドの操作に慣れているかもしれません。Swiftの 同時並行モデルはスレッドの上に構築されていますが、直接スレッドとやり取りすることはありません。Swift の非同期関数は、実行中のスレッドを放棄することができ、最初の関数がブロックされている間、そのスレッド上で別の非同期関数を実行できます。

Swift の言語サポートを使用せずに同時並行処理を書くことは可能ですが、そのコードは読みにくくなりやすい傾向があります。例えば、次のコードは写真名のリストをダウンロードし、そのリストの最初の写真をダウンロードし、その写真をユーザに表示します。

```swift
listPhotos(inGallery: "Summer Vacation") { photoNames in
    let sortedNames = photoNames.sorted()
    let name = sortedNames[1]
    downloadPhoto(named: name) { photo in
        show(photo)
    }
}
```

このような単純なケースでも、コードには一連の完了ハンドラを記述する必要があるため、ネストされたクロージャを書くことになります。このスタイルでは、深いネストを持つより複雑なコードはすぐに扱いにくくなる可能性があります。

## Defining and Calling Asynchronous Functions(非同期関数の定義と呼び出し)

非同期関数または非同期メソッド(*asynchronous function or asynchronous method*)は、実行の途中で中断できる特別な種類の関数またはメソッドです。これは、完了するまで実行するか、エラーをスローするか、何も値を返さない通常の同期関数や同期メソッド(*synchronous function and method*)とは対照的です。非同期関数またはメソッドは、これら 3 つのことのいずれかを行いますが、何かを待っているときに途中で一時中断することもできます。非同期関数またはメソッドの本文内で、実行を中断する可能性のある各箇所を示します。

関数またはメソッドが非同期なことを示すには、エラーをスローする機能を示すために `throws` を使用するのと同様に、引数の後に `async` キーワードをその宣言に書きます。関数またはメソッドが値を返す場合は、戻り値の型の矢印(`->`)の前に `async` を書いてください。例えば、ギャラリ内の写真の名前を取得する方法が次のとおりです:

```swift
func listPhotos(inGallery name: String) async -> [String] {
    let result = // ... ある非同期のネットワークコード...
    return result
}
```

非同期かつエラーをスローする関数またはメソッドの場合は、`throws` の前に `async` を書きます。

非同期メソッドを呼び出すと、そのメソッドが戻るまで実行が中断されます。中断する可能性のあるポイント(中断ポイント*suspension point*)を示すために、メソッド呼び出しの前に `await` を書きます。これは、スロー関数を呼び出すときに `try` を書くのと同様で、エラーが発生した場合にプログラムの流れを変更する可能性を示します。非同期メソッド内では、別の非同期メソッドを呼び出す場合にのみ、実行フローが中断されます。中断は決して暗黙的または処理を割り込むものではありません。つまり、全ての中断する可能性があるポイントには `await` をマークします。

例えば、下記のコードは、ギャラリ内の全ての写真名を取得し、最初の写真を表示します:

```swift
let photoNames = await listPhotos(inGallery: "Summer Vacation")
let sortedNames = photoNames.sorted()
let name = sortedNames[1]
let photo = await downloadPhoto(named: name)
show(photo)
```

`listPhotos(inGallery:)` と `downloadPhoto(named:)` 関数は両方ともネットワークリクエストをする必要があり、完了するまでかなり時間がかかる可能性があります。戻り値の型の矢印の前に `async` を書くことで両方とも非同期にすると、このコードが写真を取得するのを待っている間も、他のアプリのコードのは実行し続けることができます。

上記の例の同時並行性を理解するために、ここでは 1 つの起こりうる実行順序について示します:

1. コードは最初の行から実行を開始し、最初の `await` まで実行されます。`listPhotos(inGallery:)` 関数を呼び出し、その関数が返されるのを待つ間、実行を中断します
2. このコードの実行は中断されていますが、同じプログラム内の他の同時並行処理が行われます。例えば、長期間実行されるバックグラウンドタスクは、新しいフォトギャラリのリストを更新し続けます。そのコードは、`await` が書かれた次の中断ポイント、または処理が完了するまで実行されます
3. `listphotos(inGallery:)` から戻り値が返された後、その地点から処理は再開され `photoNames` 戻り値を割り当てます
4. `sortedNames` と `name` を定義する行は、通常の同期処理です。これらの行では何もマークされていないため、中断される可能性はありません
5. 次の `await` は、`downloadPhoto(named:)` 関数の呼び出し時に示されています。このコードは、その関数が戻り値が返すまで再び実行を一時中断し、他の同時並行処理を実行する機会を与えます
6. `downloadPhoto(named:)` が戻り値を返し、その戻り値が `photo` に割り当てられ、`show(_:)` を呼び出すときに引数として渡されています

`await` でマークされた、コード内で処理が中断する可能性のあるポイントは、非同期関数またはメソッドが戻るのを待っている間に、現在のコードが処理を一時中断することを示しています。内部では、Swift は、現在のスレッド上のコードの実行を中断し、代わりにそのスレッド上の他のコードを実行するため、これはスレッドを譲る(*yielding the thread*)と呼ばれます。`await` 時にコードを中断できるようにする必要があるため、プログラム内の特定の場所だけが非同期関数またはメソッドを呼び出すことができます:

* 非同期の関数、メソッド、プロパティの本文
* `@main` でマークされている構造体、クラス、または列挙型の `static main()` メソッド内
* 下記の[Unstructured Concurrency](#unstructured-concurrency非構造同時並行処理)で示す独立した子タスク(*child task*)のコード

> NOTE  
> `Task.sleep(_:)` メソッドは、同時並行処理が機能する方法を学ぶために簡単なコードを書くときに役立ちます。このメソッドは何もしませんが、それがリターンする前に少なくとも指定されたナノ秒数処理を待ちます。下記は、ネットワーク操作の待機をシミュレートするために `sleep()` を使用する `listPhotos(inGallery:)` 関数のバージョンです。
> 
> ```swift
> func listPhotos(inGallery name: String) async -> [String] {
>     await Task.sleep(2 * 1_000_000_000)  // 2 秒
>     return ["IMG001", "IMG99", "IMG0404"]
> }
> ```

## Asynchronous Sequences(非同期シーケンス)

前のセクションの `listPhotos(inGallery:)` は、非同期に配列全体を一度にまとめて返し、全ての配列の要素は既に取得できています。もう 1 つの方法として、非同期シーケンス(*asynchronous sequence*)を使用して、一度にコレクションの 1 つの要素を待機することができます。下記では非同期シーケンスを使って配列の要素を 1 つ 1 つ取得しています:

```swift
import Foundation

let handle = FileHandle.standardInput
for try await line in handle.bytes.lines {
    print(line)
}
```

通常の `for-in` ループを使用する代わりに、上記の例は `for` の後に `await` を書きます。非同期関数またはメソッドを呼び出す時のように、`await` は中断する可能性を示します。`for-await-in` ループは、次の要素が利用可能になるのを待っている間、各繰り返しの開始時に処理の実行を中断する可能性があります。

[Sequence](https://developer.apple.com/documentation/swift/sequence)プロトコルに準拠することで、`for-in` ループで独自の型を使用できるのと同じように、[AsyncSequence](https://developer.apple.com/documentation/swift/asyncsequence)プロトコルに準拠することで、`for-await-in` ループで独自の型を使用できます。

## Calling Asynchronous Functions in Parallel(非同期関数を並列に呼び出す)

`await` を使用して非同期関数を呼び出すと、一度に 1 つのコードしか実行されません。非同期コードが実行されている間、呼び出し側は、次のコード行を実行する前にそのコードが終了するのを待ちます。例えば、ギャラリから最初の 3 つの写真を取得するには、次のように `downloadPhoto(named:)` 関数を 3 回の呼び出して、結果を待つことができます:

```swift
let firstPhoto = await downloadPhoto(named: photoNames[0])
let secondPhoto = await downloadPhoto(named: photoNames[1])
let thirdPhoto = await downloadPhoto(named: photoNames[2])

let photos = [firstPhoto, secondPhoto, thirdPhoto]
show(photos)
```

しかし、このアプローチには重要な欠点があります: ダウンロードは非同期で、その実行中に他のタスクが実行できますが、`downloadPhoto(named:)` の呼び出しは一度に 1 つの呼び出ししか実行されません。つまり、次の写真のダウンロードを開始する前に、各写真のダウンロードの完了を待ちます。しかし、これらの操作を待機する必要はありません。各写真は独立して、または同時にダウンロードできます。

非同期関数を同時並行に実行するには、`let` の前に `async` を書き、この定数を使用する度に `await` を書きます。

```swift
async let firstPhoto = downloadPhoto(named: photoNames[0])
async let secondPhoto = downloadPhoto(named: photoNames[1])
async let thirdPhoto = downloadPhoto(named: photoNames[2])

let photos = await [firstPhoto, secondPhoto, thirdPhoto]
show(photos)
```

この例では、`downloadPhoto(named:)` の 3 つの呼び出しは、前のものが完了するのを待たずに開始します。利用可能な十分なシステムリソースがある場合は、これらは同時並行に実行できます。これらの関数呼び出しには `await` がマークされていないため、関数の結果を待つためにコードが中断されません。代わりに、`photos` が定義されている行まで処理は継続し、その時点で、プログラムはこれらの非同期呼び出しの結果を必要とするため、3 つの写真のダウンロードが完了するまで実行を一時中断するために `await` を書きます。

下記に、この 2 つのアプローチの違いについて説明します:

* 次の行のコードでその関数の結果が必要な場合は、`await` を使用して非同期関数を呼び出します。これにより、順次実行されるタスクが作成されます
* 後々のコードまで​​結果が必要ない場合、`async-let` を使用して非同期関数を呼び出すと、同時並行に実行できるタスクが作成されます
* `await` と `async-let` のどちらも、中断されている間に他のコードを実行できるようにします
* どちらの場合も、非同期関数が結果を返すまで、必要に応じて実行が一時中断することを示すために、`await` を使って中断ポイントを示します

これらのアプローチの両方を混在させることもできます。

## Tasks and Task Groups(タスクとタスクグループ)

タスク(*task*)は、プログラムの一部として非同期に実行できる作業単位です。全ての非同期コードはいくつかのタスクの一部として実行されます。前のセクションで説明されている `async-let` 構文は子タスク(*child task*)を作成します。タスクグループ(*task group*)を作成し、そのグループに子タスクを追加することもできます。こうすることで、グループでの優先順位とキャンセルをより制御しやすくし、動的な数のタスクを作成することもできます。

タスクは階層を構築します。タスクグループ内の各タスクは同じ親タスク(*parent task*)を持ち、各タスクには子タスクを持つことができます。タスクとタスクグループの間の明示的な関係のために、このアプローチは構造同時並行性(*structured concurrency*)と呼ばれます。処理の正しさを保つためのいくつかの責務は開発者にありますが、この明示的な親子関係によって、Swift は、キャンセルの伝播のような行動を処理したり、コンパイル時にいくつかのエラーを検出することができます。

```swift
await withTaskGroup(of: Data.self) { taskGroup in
    let photoNames = await listPhotos(inGallery: "Summer Vacation")
    for name in photoNames {
        taskGroup.async { await downloadPhoto(named: name) }
    }
}
```

タスクグループの詳細については、[TaskGroup](https://developer.apple.com/documentation/swift/taskgroup)を参照ください。

### Unstructured Concurrency(非構造同時並行処理)

前のセクションで説明されている構造同時並行処理のアプローチに加えて、Swift は非構造同時並行処理(*unstructured concurrency*)もサポートしています。タスクグループの一部のタスクとは異なり、非構造タスクには親タスクがありません。どんな方法で使われたとしても、非構造タスクを完全に柔軟に管理することができます。しかし、それらの正しい動作を保証することは完全に開発者の責任です。現在のアクター(*actor*)上で実行される非構造タスクを作成するには、[async(priority:operation:)](https://developer.apple.com/documentation/swift/3816404-async) 関数を呼びます。現在のアクター上で実行されないタスク(特にデタッチタスク(*detached task*))を作成するには、[asyncDetached(priority:operation:)](https://developer.apple.com/documentation/swift/3816406-asyncdetached) 関数を呼び出します。これらの関数は両方ともタスクハンドル(*task handle*)を返し、例えば、その結果を待つかキャンセルすることができます。

```swift
let newPhoto = // ... ある写真データ ...
let handle = async {
    return await add(newPhoto, toGalleryNamed: "Spring Adventures")
}
let result = await handle.get()
```

デタッチタスクの管理の詳細については、[Task.Handle](https://developer.apple.com/documentation/swift/task/handle)を参照ください。

### Task Cancellation(タスクのキャンセル)

Swift の同時並行処理は協調キャンセルモデル(*cooperative cancellation model*)を使用しています。各タスクは、実行中の適切な時点でキャンセルされたかどうかを確認し、適切ないかなる方法でもキャンセルに応答します。実行しているタスクに応じて、通常次のいずれかを意味します:

* `CancellecationError` のようなエラーをスローする
* `nil` または空のコレクションを返す
* 部分的に完了したタスクを返す

タスクがキャンセルされたかどうかをチェックするには、キャンセルされていた場合に `CancellationError` をスローする[Task.checkCancellation()](https://developer.apple.com/documentation/swift/task/3814826-checkcancellation)を呼び出すか、[Task.isCancelled](https://developer.apple.com/documentation/swift/task/3814832-iscancelled)の値を確認し、自分でコードのキャンセルを処理します。例えば、ギャラリから写真をダウンロードしているタスクは、一部だけダウンロードされたデータを削除してとネットワークを切断する必要があるかもしれません。

キャンセルを手動で伝播するには、`[Task.handle.cancel()](https://developer.apple.com/documentation/swift/task/handle/3814781-cancel)` を呼び出します。

## Actors(アクター)

クラスのように、アクター(*actors*)は参照型ですので、[Classes Are Reference Types](./structures-and-classes.md#classes-are-reference-typesclassは参照型)の中のクラスの値型と参照型の比較はアクターにも当てはまります。しかし、クラスとは異なり、アクターの可変状態(*mutable state*)にアクセスできるのは一度に 1 つのタスクだけです。これにより、複数のタスクが、同じアクターのインスタンスとやり取りする必要があるコードでも、安全にアクセスできるようになります。例えば、下記は気温を記録するアクターです:

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

中括弧ペア(`{}`)の定義の前に `actor` キーワードを付けてアクターを導入します `TemperatureLogger` アクターには、外部の他のコードがアクセスできるプロパティがあり、`max` プロパティはアクター内のコードのみが最大値を更新できるように制限されています。

構造体やクラスと同じイニシャライザの構文を使用して、アクターのインスタンスを作成します。アクターのプロパティやメソッドにアクセスする時、中断する可能性のあるポイントを示すために `await` を使用します。例えば:

```swift
let logger = TemperatureLogger(label: "Outdoors", measurement: 25)
print(await logger.max)
// "25"
```

この例では、`logger.max` へのアクセスは中断する可能性があります。アクターはその可変状態にアクセスすることを一度に 1 つのタスクのみに制限しているため、別のタスクからのコードが既にロガーとやり取りしている場合、このコードはプロパティへのアクセスを待機します。

対照的に、アクター内のコードはアクターのプロパティにアクセスするときに、`await` を記載しません。例えば、下記は `TemperatureLogsger` を新しい温度で更新するメソッドです:

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

`update(with:)` メソッドは既にアクター上で実行されているので、`max` のようなプロパティへのアクセスに `await` を書きません。また、このメソッドは、なぜアクターが一度に 1 つのタスクしかそれらの可変状態とやり取りすること許さないのかの理由の 1 つを示しています: アクターの状態の更新は一時的に不変性を壊します。`TemperatureLogger` アクターは温度のリストと最大温度を追跡し、新しい測定を記録するときに最高温度を更新します。アップデートの途中で、新しい測定値が追加されても、`max` が更新される前の状態だと、温度ロガーは一時的に矛盾した状態にあります。複数のタスクが同じインスタンスと同時にやり取りする問題を防ぐことで、次のような問題を防ぎます:

1. `update(with:)` を呼び出して、まず `measurements` 配列を更新します
2. コードが `max` を更新する前に、他の場所で最大値と気温の配列を読み取ります
3. コードは `max` を変更することによって更新を終了します

この場合、データが一時的に不正になっていた間に、`update(with:)` の途中で割り込んでアクターへアクセスされた場合、他の場所に実行されているコードが不正な情報を読み取るかもしれません。Swift のアクターを使用すると、それらの状態に対して一度に 1 つの操作のみアクセス可能なことと、`await` をマークすした場所でのみ中断される可能性があるため、この問題を防ぐことができます。`update(with:)` では中断ポイントが含まれていないため、他のコードは `update` の途中で他のコードからデータにアクセスできません。

クラスのインスタンスなどのアクターの外部からアクターのプロパティにアクセスしようとすると、コンパイルエラーが発生します。例えば:

```swift
print(logger.max)  // エラー
```

アクターのプロパティはそのアクターの独立したローカル状態の一部のため、`await` を書かないと `logger.max` へのアクセスは失敗します。Swift は、アクター内のコードのみがアクターの隔離されたローカルの状態にアクセスできることを保証します。この保証はアクターの隔離(*actor isolation*)と呼ばれています。
