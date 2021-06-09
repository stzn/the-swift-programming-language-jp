# Concurrency

最終更新日:

Swift には、構造化された方法で非同期および並列コードを書くためのサポートが組み込まれています。非同期コード(*asynchronous code*)は、一度にプログラムの 1 箇所のみが実行されますが、後で中断(*suspend*)および再開(*resume*)できます。プログラム内のコードを一時中断して後に再開することで、ネットワーク上のデータの取得やファイルの解析などの長く時間のかかる操作の途中で、UI の更新などの短期的な操作を行い、その後引き続き操作を続けることができます。並列コード(*parallel code*)とは、複数のコードを同時に実行することを意味します。例えば、4 コアプロセッサを搭載したコンピュータは、各コアがタスクの 1 つを実行しながら、4 つのコードを同時に実行できます。並列および非同期コードを使用するプログラムは、一度に複数の操作を実行します。外部システムを待っている操作を一時停止し、メモリセーフな方法でこのコードを簡単に記述できます。

並列または非同期コードを使って様々なタスクを追加でスケジューリングできる柔軟性は、逆に複雑さが増すコストも伴います。Swift を使用すると、コンパイルラチェックで意図を表現できます。例えば、アクター(*actor*)を使用して可変状態に安全にアクセスできます。ただし、低速なものやバグを含んだコードに同時並行処理を追加しても、高速になったり正確になることは保証されません。実際、同時並行処理を追加すると、コードのデバッグが難しくなる可能性さえあります。ただし、Swift の言語レベルのサポートを使用して、同時並列実行する必要があるコードの同時並列実行時に Swift が問題をキャッチするのに役立ちます。

この章の残りの部分では、非同期コードと並列コードのこの一般的な組み合わせを参照するために *Concurrency* という用語を使用します。

> NOTE  
> 以前に同時並行コードを書いたことがある場合は、スレッドの操作に慣れている可能性があります。Swiftの Concurrency モデルはスレッドの上に構築されていますが、直接スレッドと対話することはありません。Swift の非同期関数は、実行中のスレッドを放棄することができ、最初の関数がブロックされている間、そのスレッド上で別の非同期関数を実行できます。

Swift の言語サポートを使用せずに同時並行処理を書くことは可能ですが、そのコードは読みにくい傾向があります。例えば、次のコードは写真名のリストをダウンロードし、そのリストの最初の写真をダウンロードし、その写真をユーザーに表示します。

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

非同期関数または非同期メソッドは、実行の途中で中断できる特別な種類の関数またはメソッドです。これは、完了するまで実行するか、エラーをスローするか、何も値を返さない通常の同期関数とメソッドとは対照的です。非同期関数またはメソッドは、これらの 3 つのことのいずれかを実行しますが、何かを待っているときに途中で一時中断することもできます。非同期関数またはメソッドの本文内で、実行を中断できるこれらの各場所をマークします。

関数またはメソッドが非同期なことを示すために、エラーをスローする機能を示すために `throws` を使用するのと同様に、引数の後に `async` キーワードをその宣言に書き込みます。関数またはメソッドが値を返す場合は、戻り値の型の矢印(`->`)の前に `async` を書いてください。例えば、ギャラリー内の写真の名前を取得する方法が次のとおりです:

```swift
func listPhotos(inGallery name: String) async -> [String] {
    let result = // ... ある非同期のネットワークコード...
    return result
}
```

非同期かつエラーをスローする関数またはメソッドの場合、`throws` の前に `async` を書きます。

非同期メソッドを呼び出すと、そのメソッドが戻るまで実行が中断されます中断の可能性のあるポイント(*suspension point*)を示すために、メソッド呼び出しの前に `await` を書きます。これは、エラースロー関数を呼び出すときに `try` を書くようなもので、エラーが発生した場合にプログラムの流れを変更する可能性を示します。非同期メソッド内では、実行フローは、(中断が暗黙的または割り込むものではない)別の非同期メソッドを呼び出す場合にのみ中断されます。つまり、可能な全ての中断点は `await` でマークされます。

例えば、下記のコードは、ギャラリー内の全ての画像の名前を取得し、最初の画像を表示します:

```swift
let photoNames = await listPhotos(inGallery: "Summer Vacation")
let sortedNames = photoNames.sorted()
let name = sortedNames[1]
let photo = await downloadPhoto(named: name)
show(photo)
```

`listPhotos(inGallery:)` と `downloadPhoto(named:)` 関数は両方ともネットワークリクエストをする必要があり、完了するまでかなり時間がかかる可能性があります。戻り値の型の矢印の前に `async` を書くことで両方とも非同期を作ると、このコードが画像を取得するのを待っている間も、アプリのコードの残りの部分は実行し続けることができます。

上記の例の同時並行性を理解するために、ここでは 1 つの実行順序が 1 つあります。

1. コードは最初の行から実行を開始し、最初の `await` まで実行されます。`listPhotos(inGallery:)` 関数を呼び出し、その関数が返されるのを待つ間、実行を中断します
2. このコードの実行は中断されていますが、同じプログラム内の他の同時並行処理が実行されます。例えば、長期間実行されるバックグラウンドタスクは、新しいフォトギャラリーのリストを更新し続けます。そのコードは、`await` が書かれた次の中断点、または処理が完了するまで実行されます
3. `listphotos(inGallery:)` が返された後、このコードはその地点から実行を再開され `photoNames` に返り値を割り当てます
4. `sortedNames` と `name` を定義する行は、通常の同期処理です。これらの行では何もマークされていないため、中断される可能性はありません
5. 次の `await` は、`downloadPhoto(named:)` 関数の呼び出しに示されています。このコードは、その関数が戻るまで再び実行を一時中断し、他の同時並行処理を実行する機会を与えます
6. `downloadPhoto(named:)` の戻り値を返し、その戻り値が `photo` に割り当てられてから、`show(_:)` を呼び出すときに引数として渡されます

`await` でマークされたコード内で中断する可能性のあるポイントは、非同期関数またはメソッドが戻るのを待っている間に、現在のコードが処理を一時中断することを示しています。内部では、Swift は、現在のスレッド上のコードの実行を中断し代わりにそのスレッド上の他のコードを実行するため、これはスレッドを降伏する(*yielding the thread*)と呼ばれます。`await` のコードは実行を中断できるようにする必要があるため、プログラム内の特定の場所だけが非同期関数またはメソッドを呼び出すことができます:

* 非同期の関数、メソッド、プロパティの本文
* `@main` でマークされている構造体、クラス、または列挙型のコード `static main()`
* 下記の[Unstructured Concurrency](#unstructured-concurrency非構造化同時並行処理)独立した子タスクのコード

> NOTE  
> `Task.sleep(_:)` メソッドは、同時並行処理が機能する方法を学ぶために簡単なコードを書くときに役立ちます。このメソッドは何もしませんが、それがリターンする前に少なくとも指定されたナノ秒数を待ちます。これが、ネットワーク操作の待機をシミュレートするために `sleep()` を使用する `listPhotos(inGallery:)` 関数のバージョンです。
> 
> ```swift
> func listPhotos(inGallery name: String) async -> [String] {
>     await Task.sleep(2 * 1_000_000_000)  // Two seconds
>     return ["IMG001", "IMG99", "IMG0404"]
> }
> ```

## Asynchronous Sequences(非同期シーケンス)

`listPhotos(inGallery:)` は、前のセクションの `listPhotos(inGallery:)` 関数が非同期的に配列全体を一度に返し、全ての配列の要素は準備ができています。もう 1 つの方法は、非同期シーケンス(*asynchronous sequence*)を使用して、一度にコレクションの 1 つの要素を待機することです。下記は非同期シーケンスを繰り返しています:

```swift
import Foundation

let handle = FileHandle.standardInput
for try await line in handle.bytes.lines {
    print(line)
}
```

通常の `for-in` ループを使用する代わりに、上記の例は `for` の後に `await` を書きます。非同期関数またはメソッドを呼び出す時のように、`await` は中断する可能性を示します。`for-await-in` ループは、次の要素が利用可能になるのを待っている間、各繰り返しの開始時に実行を中断する可能性があります。

[Sequence](https://developer.apple.com/documentation/swift/sequence)プロトコルへの準拠することで、`for-in` ループで独自の型を使用できるのと同じように、[AsyncSequence](https://developer.apple.com/documentation/swift/asyncsequence)プロトコルに準拠することで、`for-await-in` ループで独自の型を使用できます。

## Calling Asynchronous Functions in Parallel(非同期関数を並列に呼び出す)

`await` を使用して非同期関数を呼び出すと、一度に 1 つのコードしか実行されません。非同期コードが実行されている間、呼び出し側は、次のコード行を実行する前にそのコードが終了するのを待ちます。例えば、ギャラリーから最初の 3 つの写真を取得するには、次のように `downloadPhoto(named:)` 関数への 3 つの呼び出しを待つことができます:

```swift
let firstPhoto = await downloadPhoto(named: photoNames[0])
let secondPhoto = await downloadPhoto(named: photoNames[1])
let thirdPhoto = await downloadPhoto(named: photoNames[2])

let photos = [firstPhoto, secondPhoto, thirdPhoto]
show(photos)
```

このアプローチには重要な欠点があります: ダウンロードは非同期で、それが進行中の他の作業が行われますが、`downloadPhoto(named:)` の呼び出しは一度に 1 つの呼び出ししか実行されません。次の写真がダウンロードを開始する前に、各写真はダウンロードを完了します。しかし、これらの操作を待機する必要はありません。各写真は独立して、または同時にダウンロードできます。

非同期関数を呼び出して、それ周りのコードを使って同時並行にダウンロードを実行するには、`let` の前に `async` を書き、この定数を使用する度に `await` を書きます。

```swift
async let firstPhoto = downloadPhoto(named: photoNames[0])
async let secondPhoto = downloadPhoto(named: photoNames[1])
async let thirdPhoto = downloadPhoto(named: photoNames[2])

let photos = await [firstPhoto, secondPhoto, thirdPhoto]
show(photos)
```

この例では、`downloadPhoto(named:)` の 3 つの呼び出しは、前のものが完了するのを待たずに開始します。利用可能な十分なシステムリソースがある場合は、これらは同時並行に実行できます。これらの関数呼び出しには `await` がマークされていないため、関数の結果を待つためにコードが中断されません。代わりに、`photos` が定義されている行まで処理は継続し、その時点で、プログラムはこれらの非同期呼び出しの結果を必要とするため、3 つの写真が完了するまで実行を一時中断するために `await` を書きます。

下記が、これら 2 つのアプローチの違いについてどう考えるかを説明します:

* 次の行のコードがその機能の結果に依存している場合は、`await` を使用して非同期関数を呼び出します。これにより、順次実行されるタスクが作成されます
* 後々のコードまで​​結果が必要ない場合、`async-let` を使用して非同期関数を呼び出すと、同時並行に実行できるタスクが作成されます
* `await` と `async-let` のどちらも、中断されている間に他のコードを実行できるようにします
* どちらの場合も、非同期関数が返されるまで、必要に応じて実行が一時中断することを示すために、`await` を使って中断点を示します

これらのアプローチの両方を混在させることもできます。

## Tasks and Task Groups(タスクとタスクグループ)

タスク(*task*)は、プログラムの一部として非同期に実行できる作業単位です。全ての非同期コードはいくつかのタスクの一部として実行されます。前のセクションで説明されている `async-let` 構文は子タスク(*child task*)を作成します。タスクグループ(*task group*)を作成し、そのグループに子タスクを追加することもできます。これにより、優先順位とキャンセルをより制御しやすくし、動的な数のタスクを作成できます。

タスクは階層内に配置されています。タスクグループ内の各タスクは同じ親タスクを持ち、各タスクには子タスクを持つことができます。タスクとタスクグループの間の明示的な関係のために、このアプローチは構造化同時並行性(*structured concurrency*)と呼ばれます。正確さを保つためのいくつかの責務は開発者にありますが、タスク間の明示的な親子関係によって、Swift は、キャンセルの伝播のようないくつかの行動を迅速に処理し、そして迅速にコンパイル時にいくつかのエラーを検出することができます。

```swift
await withTaskGroup(of: Data.self) { taskGroup in
    let photoNames = await listPhotos(inGallery: "Summer Vacation")
    for name in photoNames {
        taskGroup.async { await downloadPhoto(named: name) }
    }
}
```

タスクグループの詳細については、[TaskGroup](https://developer.apple.com/documentation/swift/taskgroup)を参照ください。

### Unstructured Concurrency(非構造化同時並行処理)

前のセクションで説明されている同時並行処理への構造化アプローチに加えて、Swift は非構造化同時並行処理(*unstructured concurrency*)もサポートしています。タスクグループの一部のタスクとは異なり、非構造化タスクには親タスクがありません。プログラムが必要などんな方法でも、非構造化タスクを管理するのに完全な柔軟性があります。しかし、それらの正しさを保証することは完全に開発者が責任を負います。現在のアクター上で実行される非構造化タスクを作成するには、`[async(priority:operation:)](https://developer.apple.com/documentation/swift/3816404-async)` 関数を呼びます。現在のアクター上で実行されないタスクを作成するには、`[asyncDetached(priority:operation:)](https://developer.apple.com/documentation/swift/3816406-asyncdetached)` 関数を呼び出します。これらの関数は両方ともタスクハンドルを返し、例えば、その結果を待つかキャンセルすることができます。

```swift
let newPhoto = // ... ある写真データ ...
let handle = async {
    return await add(newPhoto, toGalleryNamed: "Spring Adventures")
}
let result = await handle.get()
```

独立したタスクの管理の詳細については、[Task.Handle](https://developer.apple.com/documentation/swift/task/handle)を参照ください。

### Task Cancellation(タスクのキャンセル)

Swift の同時並行処理は協調キャンセルモデル(*cooperative cancellation model*)を使用しています。各タスクは、実行中の適切な時点でキャンセルされたかどうかを確認し、適切などのような方法でもキャンセルに応答します。実行しているタスクに応じて、通常次のいずれかを意味します:

* `CancellecationError` のようなエラーをスローする
* `nil` または空のコレクションを返す
* 部分的に完成したタスクを返す

キャンセルを確認するには、タスクがキャンセルかどうかをチェックするには、キャンセルされていた場合に `CancellationError` をスローする[Task.checkCancellation()](https://developer.apple.com/documentation/swift/task/3814826-checkcancellation)を呼び出すか、[Task.isCancelled](https://developer.apple.com/documentation/swift/task/3814832-iscancelled)の値を確認し、自分でコードのキャンセルを処理します。例えば、ギャラリーから写真をダウンロードしているタスクは、部分的なダウンロードとネットワークを切断する必要があるかもしれません。

キャンセルを手動で伝播するには、`[Task.handle.cancel()](https://developer.apple.com/documentation/swift/task/handle/3814781-cancel)` を呼び出します。

## Actors(アクター)
