# 属性\(Attributes\)

最終更新日: 2025/5/31  
原文: https://docs.swift.org/swift-book/ReferenceManual/Attributes.html

宣言と型に情報を追加する。

Swift には、宣言に適用される属性と型に適用される属性の 2 種類があります。属性は、宣言または型に関する追加情報を提供します。例えば、関数宣言の `discardableResult` 属性は、関数は値を返しますが、戻り値が使用されていない場合に、コンパイラが警告を生成しないことを示します。

`@` 記号に続けて属性の名前と、属性が受け入れる引数を書き込むことにより、属性を指定します。

```swift
@<#attribute name#>
@<#attribute name#>(<#attribute arguments#>)
```

一部の宣言属性は、属性とそれが特定の宣言にどのように適用されるかについての詳細情報を指定する引数を受け入れます。これらの _attribute arguments_ は括弧\(`()`\)で囲まれ、その形式は属する属性によって定義されています。

付属型マクロとプロパティラッパも属性構文を使用します。マクロの展開方法については、[マクロ展開](./expressions.md#マクロ展開式macro-expansion-expression)を参照してください。プロパティ・ラッパについては、[propertyWrapper](#propertywrapper)を参照してください。

## <a id="declaration-attributes">宣言属性\(Declaration Attributes\)</a>

宣言属性は宣言にのみ適用できます。

### attached

マクロ宣言に `attached` 属性を適用します。この属性の引数は、マクロの役割を示します。複数の役割を持つマクロの場合、各役割に対して 1 回ずつ、`attached` マクロを複数回適用します。

この属性の最初の引数は、マクロの役割を示します:

- ピアマクロ(Peer macros): この属性の第 1 引数に `peer` を書く。このマクロを実装する型は、`PeerMacro` プロトコルに準拠する。これらのマクロは、マクロが添付されている宣言と同じスコープで新しい宣言を生成する。例えば、構造体のメソッドにピアマクロを適用すると、その構造体に追加のメソッドとプロパティを定義することができる
- メンバマクロ(Member macro): この属性の最初の引数として `member` を記述する。このマクロを実装する型は、`MemberMacro` プロトコルに準拠する。これらのマクロは、マクロが添付されている型または `extension` のメンバの新しい宣言を生成する。例えば、構造体宣言にメンバマクロを適用すると、その構造体に追加のメソッドとプロパティを定義できる
- メンバ属性(Member attribute): この属性の第 1 引数として `memberAttribute` を書く。このマクロを実装する型は、MemberAttributeMacro プロトコルに準拠する。これらのマクロは、マクロが添付されている型または `extension` のメンバに属性を追加する
- アクセサマクロ(Accessor macros): この属性の第 1 引数としてアクセサを書く。このマクロを実装する型は `AccessorMacro` プロトコルに準拠する。これらのマクロは、マクロが添付されている格納プロパティにアクセサを追加し、それを計算プロパティに変換する
- Extension マクロ(Extension macros): この属性の最初の引数として `extension` を書く。このマクロを実装した型は、`ExtensionMacro` プロトコルに準拠する。このマクロは、プロトコルへの準拠、`where` 句およびこのマクロが添付されている型のメンバとして新しい宣言を追加する。マクロがプロトコルへの準拠を追加する場合、`conformances:` 引数を加えて準拠するプロトコルを特定する。準拠リストにはプロトコル名、準拠リスト内の項目を参照するタイプエイリアス、あるいは準拠リスト内の項目のプロトコル合成が含まれる。入れ子になった型の extension マクロは、そのファイルのトップレベルの extension として展開される。extension、タイプエイリアス、関数の内部に入れ子になっている型に extension マクロを記述したり、ピアマクロを持つ extension を追加するために extension マクロを使用することはできない

ピアメンバおよびアクセサマクロには、マクロが生成するシンボルの名前を列挙した `names:` 引数が必要です。マクロが extension の内部に宣言を追加する場合、extension マクロにも `names:` 引数が必要です。マクロ宣言に `names:` 引数が含まれる場合、マクロの実装は、そのリストに一致する名前を持つシンボルだけを生成しなければなりません。とはいえ、マクロは列挙されたすべての名前に対してシンボルを生成する必要はありません。この引数の値は、下記の内の 1 つまたはそれ以上のリストです:

- `named(<#name#>)` あらかじめ分かっている名前に対して固定のシンボル名を付与する*名前*
- `overloaded` 既存のシンボルと同じ名前に対して上書きする
- `prefixed(<#prefix#>)` 固定の文字列で始まる名前のシンボル名の前に付加される*プレフィックス*
- `suffixed(<#suffix#>)` 固定の文字列で終わる名前のシンボル名の後に付加される*サフィックス*
- `arbitrary` マクロが展開するまで確定できない名前に対して付与する

特殊なケースとして、プロパティラッパに似た動作をするマクロに `prefixed($)` を書くことができます。

### available

この属性を適用すると、特定の Swift 言語バージョンまたは特定のプラットフォームとオペレーティングシステムのバージョンに関連する宣言のライフサイクルを示します。

`available` 属性は、常に 2 つ以上のカンマ区切り\(`,`\)の属性引数のリストを伴います。これらの引数は、次のプラットフォーム名または言語名のいずれかで始まります:

* `iOS`
* `iOSApplicationExtension`
* `macOS`
* `macOSApplicationExtension`
* `macCatalyst`
* `macCatalystApplicationExtension`
* `watchOS`
* `watchOSApplicationExtension`
* `tvOS`
* `tvOSApplicationExtension`
* `visionOS`
* `visionOSApplicationExtension`
* `swift`

アスタリスク\(`*`\)を使用して、上記の全てのプラットフォーム名で宣言が使用可能だと示すこともできます。Swift のバージョン番号を使用してアベイラビリティを指定する `available` 属性では、アスタリスクを使用できません。

残りの引数は任意の順序で記述でき、重要なマイルストーンなど、宣言のライフサイクルに関する追加情報を指定できます。

* `unavailable` 引数は、指定されたプラットフォームで宣言が使用できないことを示します。この引数は、Swift のアベイラビリティバージョンを指定している場合は使用できません
* `introduced` 引数は、宣言が導入された特定のプラットフォームまたは言語の最初のバージョンを示します。形式は次のとおりです:

  ```swift
  introduced: <#version number#>
  ```

_version number_ は、ピリオド\(`.`\)で区切られた 1〜3 個の正の整数で構成されます。

* `deprecated` 引数は、宣言が非推奨になった特定のプラットフォームまたは言語の最初のバージョンを示します。形式は次のとおりです:

  ```swift
  deprecated: <#version number#>
  ```

任意の _version number_ は、ピリオド\(`.`\)で区切られた 1〜3 個の正の整数で構成されます。バージョン番号を省略すると、deprecated がいつ発生したかについての情報を提供せずに、宣言が現在 deprecated になっていることのみを示します。バージョン番号を省略する場合は、コロン\(`:`\)も省略してください。

* `obsoleted` 引数は、宣言が廃止された特定のプラットフォームまたは言語の最初のバージョンを示します。宣言が廃止されると、指定されたプラットフォームまたは言語から削除され、使用できなくなります。形式は次のとおりです:

  ```swift
  obsoleted: <#version number#>
  ```

_version number_ は、ピリオド\(`.`\)で区切られた 1〜3 個の正の整数で構成されます。

* `noasync` 引数は、宣言されたシンボルが非同期コンテキストで直接使用できないことを示します

Swift の並行処理では、潜在的な中断ポイント後に異なるスレッドで再開される可能性があるため、スレッドローカルストレージ、ロック、ミューテックス、セマフォなどを中断ポイントをまたいで使用すると、誤った結果につながる可能性があります。

この問題を回避するには、シンボルの宣言に `@available(*, noasync)` 属性を追加します:

```swift
extension pthread_mutex_t {
  @available(*, noasync)
  mutating func lock() {
      pthread_mutex_lock(&self)
  }

  @available(*, noasync)
  mutating func unlock() {
      pthread_mutex_unlock(&self)
  }
}
```

この属性は、誰かが非同期コンテキストでそのシンボルを使用しようとした場合にコンパイルエラーを発生させます。また、`message` 引数を使用してシンボルに関する追加情報も提供できます。

```swift
@available(*, noasync, message: "Migrate locks to Swift concurrency.")
mutating func lock() {
  pthread_mutex_lock(&self)
}
```

潜在的に安全でないシンボルを安全に使用できると保証できるのであれば、同期関数でラップし、その関数を非同期コンテキストから呼び出せます。

```swift
// noasync 宣言を持つメソッドの同期ラッパを提供する
extension pthread_mutex_t {
  mutating func withLock(_ operation: () -> ()) {
    self.lock()
    operation()
    self.unlock()
  }
}

func downloadAndStore(key: Int,
                    dataStore: MyKeyedStorage,
                    dataLock: inout pthread_mutex_t) async {
  // 非同期コンテキストでラッパを安全に呼び出す
  dataLock.withLock {
    dataStore[key] = downloadContent()
  }
}
```

`noasync` 引数はほとんどの宣言で使用できますが、デイニシャライザの宣言では使用できません。Swift は同期・非同期の両方のコンテキストからクラスのデイニシャライザを呼び出せる必要があります。

* `message` 引数は、`deprecated` や `obsoleted`、`noasync` が付与された宣言に使用した際に、コンパイラが表示する警告またはエラーのテキストメッセージを提供します。形式は次のとおりです:

  ```swift
  message: <#message#>
  ```

_message_ は文字列リテラルで構成されます。

* `renamed` 引数は、名前が変更された宣言の新しい名前を示すテキストメッセージを提供します。コンパイラは、名前が変更された宣言が使用された際にエラーを発行し、新しい名前を表示します。形式は次のとおりです:

  ```swift
  renamed: <#new name#>
  ```

_new name_ は文字列リテラルで構成されます。

下記に示すように、フレームワークまたはライブラリのリリース間で宣言の名前が変更されたことを示すために、タイプエイリアス宣言に `renamed` 引数と `unavailable` 引数を持つ `available` 属性を適用することができます。この組み合わせにより、宣言の名前が変更されたというコンパイルエラーが発生します。

```swift
// 初回リリース
protocol MyProtocol {
    // プロトコル定義
}
```

```swift
// MyProtocolの名前変更後のリリース
protocol MyRenamedProtocol {
    // プロトコル定義
}

@available(*, unavailable, renamed: "MyRenamedProtocol")
typealias MyProtocol = MyRenamedProtocol
```

1 つの宣言に複数の `available` 属性を適用して、様々なプラットフォームおよび様々なバージョンの Swift のアベイラビリティを指定できます。属性が現在のターゲットと一致しないプラットフォームまたは言語バージョンを指定している場合、`available` 属性が適用される宣言は無視されます。複数の `available` 属性を使用する場合、有効なアベイラビリティは、プラットフォームと Swift のアベイラビリティの組み合わせです。

`available` 属性がプラットフォームまたは言語名の引数に加えて `introduced` 引数を指定するだけの場合は、代わりに次の省略構文を使用できます:

```swift
@available(<#platform name#> <#version number#>, *)
@available(swift <#version number#>)
```

`available` 属性の省略構文は、複数のプラットフォームのアベイラビリティを簡潔に表しています。2 つの形式は機能的に同等ですが、可能な限り省略形が推奨されます。

```swift
@available(iOS 10.0, macOS 10.12, *)
class MyClass {
    // クラス定義
}
```

Swift バージョン番号を使用してアベイラビリティを指定する `available` 属性は、宣言のプラットフォームのアベイラビリティを追加で指定することはできません。代わりに、個別の `available` 属性を使用して、Swift バージョンのアベイラビリティと 1 つ以上のプラットフォームのアベイラビリティを指定します。

```swift
@available(swift 3.0.2)
@available(macOS 10.12, *)
struct MyStruct {
    // 構造体定義
}
```

### backDeployed

この属性を関数、メソッド、`subscript`、または計算プロパティに適用すると、シンボルを呼び出したりアクセスしたりするプログラムにシンボルの実装のコピーを含めることができます。この属性は、OS に含まれる API のように、プラットフォームの一部として出荷されるシンボルにアノテーションを付けるために使用します。この属性は、シンボルにアクセスするプログラムにその実装のコピーを含めることによって、遡って利用できるシンボルをマークします。実装をコピーすることは*クライアントの中に取り込む*とも言われます。

この属性は `before:` 引数を取り、このシンボルを提供するプラットフォームの最初のバージョンを指定します。これらのプラットフォームのバージョンは、`available` 属性で指定したプラットフォームのバージョンと同じ意味を持ちます。ただし、`available` 属性とは異なり、リストにアスタリスク(`*`)を入れて全てのバージョンを参照することはできません。例えば、次のコードを考えてみましょう。

```swift
@available(iOS 16, *)
@backDeployed(before: iOS 17)
func someFunction() { /* ... */ }
```

上記の例では、iOS SDK は iOS17 から `someFunction()` を提供しています。さらに、SDK はバックデプロイメントを使用して iOS16 で `someFunction()` を利用できるようにします。

この関数を呼び出すコードをコンパイルするとき、Swift は関数の実装を見つけるために間接的な層を挿入します。コードがこの関数を含む SDK のバージョンを使用して実行される場合、SDK の実装が使用されます。そうでなければ、呼び出し元に含まれるコピーが使用されます。上記の例では、`someFunction()` を呼び出すと、iOS17 以降で実行される場合は SDK の実装が使用され、iOS16 で実行される場合は、呼び出し元に含まれている `someFunction()` のコピーが使用されます。

> 注意: 呼び出し側の最小デプロイメントターゲットが、シンボルを含むSDKの最初のバージョンと同じかそれ以上の場合、
> コンパイラは実行時チェックを最適化してSDKの実装を直接呼び出すことができます。
> この場合、バックデプロイされたシンボルに直接アクセスすると
> コンパイラはクライアントからシンボルの実装のコピーを省略することもできます。

以下の条件を満たす関数、メソッド、`subscript`、および計算プロパティをバックデプロイできます。

- 宣言が `public` または `@usableFromInline`
- クラスのインスタンスメソッドおよびクラスの型メソッドで、そのメソッドが `final` とマークされかつ `@objc` とマークされていない
- 実装が、[inlinable](./attributes.md#inlinable) で説明されている、インライン化可能な関数の要件を満たしている

### discardableResult

この属性を関数またはメソッドの宣言に適用すると、値を返す関数またはメソッドがその結果を使用せずに呼び出されたときのコンパイラの警告を抑制します。

### dynamicCallable

この属性をクラスや構造体、列挙型、またはプロトコルに適用すると、型のインスタンスを呼び出し可能な関数として扱います。この型は、`dynamicallyCall(withArguments:)` メソッド、`dynamicallyCall(withKeywordArguments:)` メソッド、またはその両方を実装する必要があります。

動的に呼び出し可能な型のインスタンスは、任意の数の引数を取る関数のように呼び出すことができます。

```swift
@dynamicCallable
struct TelephoneExchange {
    func dynamicallyCall(withArguments phoneNumber: [Int]) {
        if phoneNumber == [4, 1, 1] {
            print("Get Swift help on forums.swift.org")
        } else {
            print("Unrecognized number")
        }
    }
}

let dial = TelephoneExchange()

// 動的メソッド呼び出しを使用します
dial(4, 1, 1)
// Get Swift help on forums.swift.org

dial(8, 6, 7, 5, 3, 0, 9)
// Unrecognized number

// 基になるメソッドを直接呼び出します
dial.dynamicallyCall(withArguments: [4, 1, 1])
```

`dynamicallyCall(withArguments:)` メソッドの宣言には、上記の例の `[Int]` のように、[ExpressibleByArrayLiteral](https://developer.apple.com/documentation/swift/expressiblebyarrayliteral)プロトコルに準拠する単一のパラメータが必要です。戻り値の型は任意の型にすることができます。

`dynamicallyCall(withKeywordArguments:)` メソッドを実装する場合は、動的メソッド呼び出しにラベルを含めることができます。

```swift
@dynamicCallable
struct Repeater {
    func dynamicallyCall(withKeywordArguments pairs: KeyValuePairs<String, Int>) -> String {
        return pairs
            .map { label, count in
                repeatElement(label, count: count).joined(separator: " ")
            }
            .joined(separator: "\n")
    }
}

let repeatLabels = Repeater()
print(repeatLabels(a: 1, b: 2, c: 3, b: 2, a: 1))
// a
// b b
// c c c
// b b
// a
```

`dynamicallyCall(withKeywordArguments:)` メソッドの宣言には、[ExpressibleByDictionaryLiteral](https://developer.apple.com/documentation/swift/expressiblebydictionaryliteral)プロトコルに準拠する単一のパラメータが必要で、戻り値の型は任意の型にすることができます。パラメータの[Key](https://developer.apple.com/documentation/swift/expressiblebydictionaryliteral/2294108-key)は[ExpressibleByStringLiteral](https://developer.apple.com/documentation/swift/expressiblebystringliteral)に準拠する必要があります。前の例では、パラメータの型として[KeyValuePairs](https://developer.apple.com/documentation/swift/keyvaluepairs)を使用しているため、呼び出し元は重複するパラメータラベルを含めることができます。つまり、`a` と `b` は、`repeatLabels` の呼び出し時に複数回使用されています。

両方の `dynamiclyCall` メソッドを実装する場合、メソッド呼び出しにキーワード引数が含まれていると、`dynamicallyCall(withKeywordArguments:)` が呼び出されます。他の全ての場合、`dynamicallyCall(withArguments:)` が呼び出されます。

動的に呼び出すことができるインスタンスは、`dynamicCall` メソッドの実装の 1 つで指定した型と一致する引数と戻り値を使用した場合にのみ呼び出すことができます。次の例の呼び出しは、`KeyValuePairs<String, String>` を受け取る `dynamicallyCall(withArguments:)` の実装がないため、コンパイルできません。

```swift
repeatLabels(a: "four") // エラー
```

### dynamicMemberLookup

この属性をクラスや構造体、列挙型、またはプロトコルに適用すると、実行時にメンバを名前で検索できるようになります。型は `subscript(dynamicMember:)` を実装する必要があります。

明示的メンバ式では、指定されたメンバに対応する宣言がない場合、式は型の `subscript(dynamicMember:)` の呼び出しとして解釈され、メンバに関する情報を引数として渡します。サブスクリプトは、KeyPath またはメンバ名のいずれかでパラメータを受け取ることができます。両方のサブスクリプトを実装する場合、KeyPath 引数を取るサブスクリプトが使用されます。

`subscript(dynamicMember:)` の実装は、[KeyPath](https://developer.apple.com/documentation/swift/keypath)、[WritableKeyPath](https://developer.apple.com/documentation/swift/writablekeypath)、または [ReferenceWritableKeyPath](https://developer.apple.com/documentation/swift/referencewritablekeypath)の引数を使用して KeyPath を受け取ることができます。[ExpressibleByStringLiteral](https://developer.apple.com/documentation/swift/expressiblebystringliteral)プロトコル\(ほとんどの場合、`String`\)に準拠する型の引数を使用して、メンバ名を受け入れることができます。サブスクリプトの戻り値の型は任意の型にすることができます。

メンバ名による動的なメンバ検索を使用して、他の言語のデータを Swift にブリッジする場合など、コンパイル時に型チェックできないデータのラッパ型を作成できます。例えば:

```swift
@dynamicMemberLookup
struct DynamicStruct {
    let dictionary = ["someDynamicMember": 325,
                      "someOtherMember": 787]
    subscript(dynamicMember member: String) -> Int {
        return dictionary[member] ?? 1054
    }
}
let s = DynamicStruct()

//動的メンバ検索を使用します。
let dynamic = s.someDynamicMember
print(dynamic)
// 325

// 基になるサブスクリプトを直接呼び出します
let equivalent = s[dynamicMember: "someDynamicMember"]
print(dynamic == equivalent)
// true
```

KeyPath による動的メンバ検索を使用して、コンパイル時の型チェックをサポートする方法でラッパ型を実装できます。例えば:

```swift
struct Point { var x, y: Int }

@dynamicMemberLookup
struct PassthroughWrapper<Value> {
    var value: Value
    subscript<T>(dynamicMember member: KeyPath<Value, T>) -> T {
        get { return value[keyPath: member] }
    }
}

let point = Point(x: 381, y: 431)
let wrapper = PassthroughWrapper(value: point)
print(wrapper.x)
```

### freestanding

自立型マクロの宣言に `freestanding` 属性を適用する。

### frozen

この属性を構造体または列挙型宣言に適用すると、型に加えることができる変更の種類を制限します。この属性は、ライブラリエボリューションモードでコンパイルする場合にのみ許可されます。ライブラリの将来のバージョンでは、列挙型のケースまたは構造体の格納インスタンスプロパティを追加、削除、または並べ替えて宣言を変更することはできません。これらの変更は nonfrozen 型で許可されますが、frozen 型の ABI 互換性が損なわれます。

> NOTE  
> コンパイラが ライブラリエボリューションモードでない場合、全ての構造体と列挙型は暗黙的に frozen にされ、この属性は無視されます。

ライブラリエボリューションモードでは、nonfrozen の構造体や列挙型のメンバとやり取りするコードは、ライブラリの将来のバージョンでその型のメンバの一部が追加、削除、または並べ替えられた場合でも、再コンパイルせずに作業を継続できるようにコンパイルされます。コンパイラは、実行時に情報を検索したり、間接層を追加したりするなどの手法を使用して、これを可能にします。構造体または列挙型を frozen でマークすると、パフォーマンスを向上させるためにこの柔軟性が失われます。ライブラリの将来のバージョンでは、型に限定的な変更しか加えることができませんが、コンパイラは、型のメンバとやり取りするコードに追加の最適化を行うことができます。

frozen 型、frozen 構造体の格納プロパティの型、および frozen 列挙ケースの関連値は、public か、`usableFromInline` 属性がマークされている必要があります。frozen 構造体のプロパティはプロパティオブザーバを持つことができず、格納インスタンスプロパティの初期値を提供する式は、[inlinable](../language-reference/attributes.md#inlinable)で説明されているように inlinable 関数と同じ制限に従う必要があります。

コマンドラインでライブラリエボリューションモードを有効にするには、`-enable-library-evolution` オプションを Swift コンパイラに渡します。Xcode で有効にするには、[Xcode Help](https://help.apple.com/xcode/mac/current/#/dev04b3a04ba)の説明に従って、「Build Libraries for Distribution」のビルド設定\(`BUILD_LIBRARY_FOR_DISTRIBUTION`\)を Yes に設定します。

frozen 列挙型の switch 文は、[Switching Over Future Enumeration Cases\(列挙型の将来のケースのスイッチング\)](../language-reference/statements.md#switching-over-future-enumeration-cases)で説明されているように、デフォルトのケースを必要としません。frozen 列挙型を切り替えるときに `default` または `@unknown default` のケースを含めると、そのコードは実行されないため、警告が生成されます。

### GKInspectable

この属性を適用して、独自の GameplayKit コンポーネントプロパティを SpriteKit エディタ UI に公開します。この属性を適用すると、`objc` 属性も暗黙的に追加されます。

### globalActor

この属性はアクター、構造体、列挙型、または `final` クラスに適用します。その型は、アクターの共有インスタンスを提供する `shared` という名前の静的プロパティを定義する必要があります。

グローバルアクターは、アクター隔離の概念を、コード内の複数の異なる場所（複数の型、ファイル、モジュールなど）に分散した状態で汎用化し、並行コードからグローバル変数に安全にアクセスできるようにします。グローバルアクターが `shared` プロパティの値として提供するアクターは、このすべての状態へのアクセスを直列化します。また、すべて同じスレッドで実行する必要があるコードのように、並行コードにおける制約をモデル化するためにグローバルアクターを使用することもできます。

グローバルアクターは暗黙的に [`GlobalActor`](https://developer.apple.com/documentation/swift/globalactor) プロトコルに準拠します。メインアクターは、[メインアクター\(The Main Actor\)](../language-guide/concurrency.md#メインアクターthe-main-actor)で説明されているように、標準ライブラリによって提供されるグローバルアクターです。ほとんどのコードは、新しいグローバルアクターを定義する代わりにメインアクターを使用できます。

### inlinable

この属性を関数、メソッド、計算プロパティ、サブスクリプト、convenience イニシャライザ、またはデイニシャライザ宣言に適用すると、モジュールのパブリックインターフェイスの一部としてその宣言の実装を公開します。コンパイラは、inlinable シンボルへの呼び出しを、呼び出し側でシンボルの実装のコピーに置き換えることができます。

inlinable コードは、任意のモジュールで宣言された `open` と `public` シンボルとやり取りし、`usableFromInline` 属性がマークされた同じモジュールで宣言された `internal` シンボルとやり取りできます。inline 化したコードは、`private` シンボルまたは fileprivate シンボルとやり取りできません。

この属性は、関数内にネストされている宣言や、`fileprivate` または `private` 宣言には適用できません。inlinable 関数内で定義された関数とクロージャは、この属性をマークすることはできませんが、暗黙的に inlinable です。

### main

この属性を構造体、クラス、または列挙型の宣言に適用すると、プログラムフローのトップレベルのエントリポイントが含まれていることを示します。型は、引数をとらずに `Void` を返す `main` 型関数を提供する必要があります。例えば:

```swift
@main
struct MyTopLevel {
    static func main() {
         // トップレベルのコードをここに
    }
}
```

`main` 属性の要件を記述する別の方法は、この属性を追加する型が、次の架空のプロトコルに準拠する型と同じ要件を満たさなければならないということです。

```swift
protocol ProvidesMain {
    static func main() throws
}
```

実行可能ファイルを作成するためにコンパイルする Swift コードには、[Top-Level Code\(トップレベルコード\)](../language-reference/declarations.md#top-level-code)で説明されているように、ただ 1 つのトップレベルのエントリポイントのみを含めます。

### nonobjc

この属性をメソッド、プロパティ、サブスクリプト、またはイニシャライザ宣言に適用すると、暗黙の `objc` 属性を抑制します。`nonobjc` 属性は、Objective-C で宣言を使用することができる場合でも、使用できないようにするようコンパイラに指示します。

この属性を extension に適用すると、`objc` 属性が明示的にマークされていないその extension の全てのメンバに適用するのと同じ効果があります。

`nonobjc` 属性を使用して、`objc` 属性がマークされたクラスのブリッジメソッドの循環性を解決し、`objc` 属性がマークされたクラスのメソッドとイニシャライザのオーバーロードを許可します。

`nonobjc` 属性がマークされたメソッドは、`objc` 属性がマークされたメソッドをオーバーライドできません。ただし、`objc` 属性がマークされたメソッドは、`nonobjc` 属性がマークされたメソッドをオーバーライドできます。同様に、`nonobjc` 属性がマークされたメソッドは、`objc` 属性がマークされたメソッドのプロトコル要件を満たすことができません。

### NSApplicationMain

> 非推奨:  
> この属性は非推奨です。  
> [main](./language-reference/attributes.md#main)を代わりに使用してください。  
> Swift6では、この属性を使用するとエラーになります。

この属性をクラスに適用すると、それが app delegate だということを示します。この属性を使用することは、`NSApplicationMain(_:_:)` 関数を呼び出すことと同じです。

この属性を使用しない場合は、次のように `NSApplicationMain(_:_:)` 関数を呼び出すトップレベルのコードを `main.swift` ファイルに指定します。

```swift
import AppKit
NSApplicationMain(CommandLine.argc, CommandLine.unsafeArgv)
```

実行可能ファイルを作成するためにコンパイルする Swift コードには、[Top-Level Code\(トップレベルコード\)](../language-reference/declarations.md#top-level-code)で説明されているように、ただ 1 つのトップレベルのエントリポイントのみを含めます。

### NSCopying

この属性をクラスの格納変数プロパティに適用します。この属性により、set は、プロパティ自体の値ではなく、`copyWithZone(_:)` メソッドによって返されるプロパティの値のコピーと同期されます。プロパティの型は、`NSCopying` プロトコルに準拠している必要があります。

`NSCopying` 属性は、Objective-C のコピープロパティ属性と同じように動作します。

### NSManaged

この属性を `NSManagedObject` を継承するクラスのインスタンスメソッドまたは格納変数プロパティに適用すると、関連付けられたエンティティの記述に基づいて、CoreData が実行時にその実装を動的に提供することを示します。`NSManaged` 属性がマークされたプロパティの場合、CoreData は実行時にストレージも提供します。この属性を適用すると、`objc` 属性も暗黙的に追加されます。

### objc

この属性は Objective-C で使用することができる全ての宣言に適用します。例えば、ネストされていないクラス、プロトコル、\(整数の Raw Value 型の\)非ジェネリック列挙型、クラスのプロパティとメソッド\(get と set を含む\)、プロトコル、およびオプショナルのプロトコル、イニシャライザ、およびサブスクリプトなど。`objc` 属性は、Objective-C コードで宣言を使用できることをコンパイラに通知します。

この属性を extension に適用すると、`nonobjc` 属性が明示的にマークされていないその extension 内の全てのメンバに適用するのと同じ効果があります。

コンパイラは、Objective-C で定義されたクラスのサブクラスに `objc` 属性を暗黙的に追加します。ただし、サブクラスはジェネリックにはできず、ジェネリッククラスから継承してはなりません。これらの条件を満たすサブクラスに `objc` 属性を明示的に追加して、下記で説明するように、その Objective-C 名を指定できます。`objc` 属性がマークされているプロトコルは、`objc` 属性がマークされていないプロトコルから継承できません。

objc 属性は、次の場合にも暗黙的に追加されます:

* 宣言がサブクラスのオーバーライドで、スーパークラスの宣言に `objc` 属性がある場合
* 宣言が `objc` 属性を持つプロトコルの要件を満たす場合
* 宣言に、`IBAction`、`IBSegueAction`、`IBOutlet`、`IBDesignable`、`IBInspectable`、`NSManaged`、または `GKInspectable` 属性がある場合

`objc` 属性を列挙型に適用すると、各列挙ケースは、列挙型名とケース名を連結して Objective-C コードに公開されます。ケース名の最初の文字は大文字です。例えば、Swift では `Planet` 列挙型内の `venus` という名前のケースは、`PlanetVenus` という名前のケースとして Objective-C コードに公開されます。

`objc` 属性は、任意で、識別子に単一の属性引数を受け取ります。識別子は、`objc` 属性が適用されるエンティティの Objective-C に公開される名前を指定します。この引数を使用して、クラス、列挙型、列挙ケース、プロトコル、メソッド、get、set、およびイニシャライザに名前を付けることができます。クラス、プロトコル、または列挙型に Objective-C 名を指定する場合は、[Programming with Objective-C](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/Introduction/Introduction.html#//apple_ref/doc/uid/TP40011210)の[Conventions](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/Conventions/Conventions.html#//apple_ref/doc/uid/TP40011210-CH10-SW1)で説明されているように、名前に 3 文字のプレフィックスを含めます。下記の例では、`ExampleClass` の `enabled` プロパティの get を、プロパティ自体の名前としてではなく、`isEnabled` として Objective-C コードに公開しています。

```swift
class ExampleClass: NSObject {
    @objc var enabled: Bool {
        @objc(isEnabled) get {
            // 適切な値を返します
        }
    }
}
```

詳細については、Swift の [Importing Swift into Objective-C](https://developer.apple.com/documentation/swift/imported_c_and_objective-c_apis/importing_swift_into_objective-c)を参照ください。

> NOTE  
> `objc` 属性の引数は、その宣言の実行時名を変更することもできます。`NSClassFromString` などの Objective-C ランタイムとやり取りする関数を呼び出すとき、およびアプリの `Info.plist` ファイルでクラス名を指定するときに、実行時名を使用します。引数を渡して名前を指定すると、その名前が Objective-C コードの名前および実行時名として使用されます。引数を省略すると、Objective-C コードで使用される名前は Swift コードの名前と一致し、実行時名は通常の Swift コンパイラの名前修飾規則に従います。

### objcMembers

この属性をクラス宣言に適用すると、クラスの全ての Objective-C に互換性のあるメンバ、その extension、そのサブクラス、およびそのサブクラスの全ての extension に `objc` 属性を暗黙的に追加します。

ほとんどのコードでは、代わりに `objc` 属性を使用して、必要な宣言のみを公開する必要があります。多くの宣言を公開する必要がある場合は、`objc` 属性を持つ extension でグループ化できます。`objcMembers` 属性は、Objective-C ランタイムのリフレクション機能を多用するライブラリにとって便利です。必要のないときに `objc` 属性を適用すると、バイナリサイズが大きくなり、パフォーマンスに悪影響を与える可能性があります。

### preconcurrency

この属性を宣言に適用すると、Strict Concurrency チェックを抑制できます。この属性は、以下の種類の宣言に適用できます:

- インポート
- 構造体、クラス、アクター
- 列挙型および列挙型のケース
- プロトコル
- 変数および定数
- subscript
- イニシャライザ
- 関数

`import` 宣言で、この属性は、インポートされたモジュールの型を使用するコードの Strict Concurrency チェックを軽減します。具体的には、インポートされたモジュールから明示的に non-`Sendable` とマークされていない型は、`Sendable` な型を必要とするコンテキストで使用できます。

その他の宣言では、この属性は、宣言されたシンボルを使用するコードの Concurrency チェックの厳密性を下げます。最小限(minimal)の Strict Concurrency チェックを行うスコープでこのシンボルを使用する場合、`Sendable` 要件やグローバルアクターなど、そのシンボルで指定された Concurrency 関連の制約はチェックされません。

この属性を次のように使用すると、Strict Concurrency チェックへのコードの移行に役立ちます:

1. Strict Concurrency チェックを有効にする
2. Strict Concurrency チェックを有効にしていないモジュールの `import` 宣言に `@preconcurrency` 属性を付ける
3. モジュールを Strict Concurrency チェックに移行した後、`@preconcurrency` 属性を削除する。コンパイラは、`import` 宣言の `@preconcurrency` 属性が、もはや何の効果も持たないため、削除すべき場所を警告する

その他の宣言では、Strict Concurrency チェックに移行していないクライアントがまだある場合、Concurrency 関連の制約を宣言に追加するときに `@preconcurrency` 属性を追加してください。そして、すべてのクライアントが移行したら、`@preconcurrency` 属性を削除してください。

Objective-C からの宣言は、常に `@preconcurrency` 属性でマークされたものとしてインポートされます。


### propertyWrapper

この属性をクラス、構造体、または列挙型宣言に適用すると、その型をプロパティラッパとして使用できます。この属性を型に適用すると、型と同じ名前のカスタム属性が作成されます。その新しい属性をクラス、構造体、または列挙型のプロパティに適用して、ラッパ型のインスタンスを介してプロパティへのアクセスをラップします。属性をローカルの格納変数宣言に適用して、変数へのアクセスを同じ方法でラップすることもできます。計算変数、グローバル変数、および定数には、プロパティラッパを使用できません。

ラッパは `wrappedValue` インスタンスプロパティを定義する必要があります。プロパティの_ラップされた値_は、このプロパティの get と set が公開する値です。ほとんどの場合、`wrappedValue` は計算値ですが、代わりに格納値にすることもできます。ラッパは、ラップされた値に必要なストレージを定義および管理します。コンパイラは、ラップされたプロパティの名前の前にアンダースコア\(`_`\)を付けることにより、ラッパ型のインスタンスのストレージを合成します。例えば、`someProperty` のラッパは `_someProperty` として格納されます。ラッパの合成ストレージのアクセス制御レベルは `private` です。

プロパティラッパを持つプロパティには、`willSet` ブロックと `didSet` ブロックを含めることができますが、コンパイラで合成された `get` ブロックまたは `set` ブロックをオーバーライドすることはできません。

Swift は、プロパティラッパを初期化するための 2 つの形式の糖衣構文\(シンタックスシュガー\)を提供します。ラップされた値の定義で代入構文を使用して、代入の右側にある式を、プロパティラッパのイニシャライザの `wrappedValue` パラメータとして渡すことができます。属性をプロパティに適用するときに属性に引数を指定することもでき、それらの引数はプロパティラッパのイニシャライザに渡されます。例えば、下記のコードでは、`SomeStruct` は `SomeWrapper` が定義する各イニシャライザを呼び出します。

```swift
@propertyWrapper
struct SomeWrapper {
    var wrappedValue: Int
    var someValue: Double
    init() {
        self.wrappedValue = 100
        self.someValue = 12.3
    }
    init(wrappedValue: Int) {
        self.wrappedValue = wrappedValue
        self.someValue = 45.6
    }
    init(wrappedValue value: Int, custom: Double) {
        self.wrappedValue = value
        self.someValue = custom
    }
}

struct SomeStruct {
    // init() を使用します
    @SomeWrapper var a: Int

    // init(wrappedValue:) を使用します
    @SomeWrapper var b = 10

    // どちらも init(wrappedValue:custom:) を使用します
    @SomeWrapper(custom: 98.7) var c = 30
    @SomeWrapper(wrappedValue: 30, custom: 98.7) var d
}
```

ラップされたプロパティの投影された値は、プロパティラッパが追加機能を公開するために使用できる 2 番目の値です。プロパティラッパ型の作成者は、その投影された値の意味を決定し、投影された値が公開するインターフェイスを定義する責任があります。プロパティラッパから値を投影された値を提供するには、ラッパ型で `projectedValue` インスタンスプロパティを定義します。コンパイラは、ラップされたプロパティの名前の前にドル記号\(`$`\)を付けることにより、投影された値の識別子を合成します。例えば、`someProperty` の投影された値は `$someProperty` です。投影された値は、元のラップされたプロパティと同じアクセス制御レベルを持ちます。

```swift
@propertyWrapper
struct WrapperWithProjection {
    var wrappedValue: Int
    var projectedValue: SomeProjection {
        return SomeProjection(wrapper: self)
    }
}
struct SomeProjection {
    var wrapper: WrapperWithProjection
}

struct SomeStruct {
    @WrapperWithProjection var x = 123
}
let s = SomeStruct()
s.x           // Int 値
s.$x          // SomeProjection 値
s.$x.wrapper  // WrapperWithProjection 値
```

### resultBuilder

この属性をクラスや構造体、列挙型に適用して、その型をリザルトビルダとして使用できます。_リザルトビルダ_は、ネストされたデータ構造を段階的に構築する型です。リザルトビルダを使用して、ネストされたデータ構造を自然で宣言的な方法で作成するためのドメイン固有言語(DSL)を実装します。`resultBuilder` 属性の使用方法の例については、[Result Builders(リザルトビルダ)](../language-guide/advanced-operators.md#result-builders)を参照ください。

#### Result-Building Methods

リザルトビルダは、下記で説明する静的メソッドを実装します。リザルトビルダの全ての機能は静的メソッドを介して公開されるため、その型のインスタンスを初期化することはありません。リザルトビルダは `buildBlock(_:)` メソッド、または `buildPartialBlock(first:)` と `buildPartialBlock(accumulated:next:)` メソッドの両方を実装する必要があります。その他のメソッド（DSL の追加機能を実現するメソッド）はオプションです。リザルトビルダ型の宣言は、実際にはプロトコルへの準拠を含む必要はありません。

静的メソッドの記述では、プレースホルダとして 3 つの型を使用しています。`Expression` 型は、リザルトビルダの入力の型のプレースホルダで、`Component` は、部分的な結果の型のプレースホルダで、`FinalResult` は、リザルトビルダが生成する最終的な結果の型のプレースホルダです。これらの型を、リザルトビルダが使用する実際の型に置き換えます。リザルトビルダメソッドで `Expression` または `FinalResult` の型が指定されていない場合、デフォルトで `Component` と同じになります。

ブロック構築メソッドは次のとおりです:

- `static func buildBlock(_ components: Component...) -> Component`: 部分的な結果の配列を単一の部分的な結果に結合する

- `static func buildPartialBlock(first: Component) -> Component`: 最初のコンポーネントから部分的な結果を構築する。このメソッドと `buildPartialBlock(accumulated:next:)` の両方を実装して、一度に 1 つのコンポーネントを構築するブロックをサポートする。`buildBlock(_:)` と比較して、この方法は、異なる数の引数を処理する汎用的なオーバーロードの必要性を減らせる

- `static func buildPartialBlock(accumulated: Component, next: Component) -> Component`: 蓄積されたコンポーネントと新しいコンポーネントを組み合わせて、部分的な結果を構築する。このメソッドと `buildPartialBlock(first:)` の両方を実装して、一度に 1 つのコンポーネントを構築するブロックをサポートする。`buildBlock(_:)` と比較して、このアプローチは、異なる数の引数を処理する汎用的オーバーロードの必要性を減らせる

リザルトビルダは、上記のブロック構築メソッドの 3 つすべてを実装できます。その場合、アベイラビリティによってどのメソッドが呼び出されるかが決まります。デフォルトでは、Swift は `buildPartialBlock(first:)` と `buildPartialBlock(accumulated:next:)` のメソッドを呼び出します。Swift が代わりに `buildBlock(_:)` を呼び出すようにするには、囲んでいる宣言のアベイラビリティが、`buildPartialBlock(first:)` と `buildPartialBlock(accumulated:next:)` に書いたアベイラビリティよりも広範囲になるようにマークしてください。

追加の結果構築メソッドは以下の通りです:

- `static func buildOptional(_ component: Component?) -> Component`: `nil` になる可能性のある部分的な結果から部分的な結果を構築する。このメソッドを実装して、`else` 句を含まない `if` 文をサポートする

- `static func buildEither(first: Component) -> Component`: 条件によって値が変化する部分的な結果を作成する。このメソッドと `buildEither(second:)` の両方を実装して、`switch` 文と `else` 句を含む `if` 文をサポートする

- `static func buildEither(second: Component) -> Component`: 条件によって値が変化する部分的な結果を作成する。このメソッドと `buildEither(first:)` の両方を実装して、`switch` 文と `else` 句を含む `if` 文をサポートする

- `static func buildArray(_ components: [Component]) -> Component`: 部分的な結果の配列から部分的な結果を構築する。このメソッドを実装して、ループをサポートする

- `static func buildExpression(_ expression: Expression) -> Component`: 式から部分的な結果を作成する。このメソッドを実装して、前処理(例えば、式を内部型に変換する)を実行したり、使用側で型推論のための追加情報を提供したりできる

- `static func buildFinalResult(_ component: Component) -> FinalResult`: 部分的な結果から最終結果を作成する。このメソッドは、部分結果と最終結果で異なる型を使用するリザルトビルダの一部として実装したり、結果を返す前に結果に対して後処理を実行したりできる
- `static func buildLimitedAvailability(_ component: Component) -> Component`: 型情報を消去する部分的な結果を構築する。コンパイラ制御文の外部へ型情報が伝播されるのを防ぐために、このメソッドを実装できる

例えば、下記のコードは、整数の配列を作成するシンプルなリザルトビルダを定義しています。このコードは、`Component` と `Expression` をタイプエイリアスとして定義し、下記の例を上記のメソッドのリストに簡単に一致させることができます。

```swift
@resultBuilder
struct ArrayBuilder {
    typealias Component = [Int]
    typealias Expression = Int
    static func buildExpression(_ element: Expression) -> Component {
        return [element]
    }
    static func buildOptional(_ component: Component?) -> Component {
        guard let component = component else { return [] }
        return component
    }
    static func buildEither(first component: Component) -> Component {
        return component
    }
    static func buildEither(second component: Component) -> Component {
        return component
    }
    static func buildArray(_ components: [Component]) -> Component {
        return Array(components.joined())
    }
    static func buildBlock(_ components: Component...) -> Component {
        return Array(components.joined())
    }
}
```

#### Result Transformations

次の構文変換は、リザルトビルダ構文を使用するコードを、リザルトビルダ型の静的メソッドを呼び出すコードに変換するために再帰的に適用されます。

* リザルトビルダに `buildExpression(_:)` メソッドがある場合、各式はそのメソッドの呼び出しになります。この変換は常に最初に行われます。例えば、次の宣言は同等です:

```swift
@ArrayBuilder var builderNumber: [Int] { 10 }
var manualNumber = ArrayBuilder.buildExpression(10)
```

* 代入文は式のように変換されますが、`()` に評価されると解釈されます。代入を具体的に処理するために `()` 型の引数を取る `buildExpression(_:)` のオーバーロードを定義できます
* アベイラビリティ条件をチェックする分岐文は、もし `buildLimitedAvailability(_:)` メソッドを実装していれば、それを呼び出しになります。`buildLimitedAvailability(_:)` を実装していなければ、アベイラビリティをチェックする分岐文は、他の分岐文と同じ変換を使用します。この変換は、`buildEither(first:)`、`buildEither(second:)`、または `buildOptional(_:)` の呼び出しに変換される前に行われます。`buildLimitedAvailability(_:)` メソッドを使用して、取得する分岐に応じて変化する型情報を消去します。例えば、下記の `buildEither(first:)` メソッドと `buildEither(second:)` メソッドは、両方の分岐に関する型情報をキャプチャするジェネリック型を使用します

```swift
protocol Drawable {
    func draw() -> String
}
struct Text: Drawable {
    var content: String
    init(_ content: String) { self.content = content }
    func draw() -> String { return content }
}
struct Line<D: Drawable>: Drawable {
    var elements: [D]
    func draw() -> String {
        return elements.map { $0.draw() }.joined(separator: "")
    }
}
struct DrawEither<First: Drawable, Second: Drawable>: Drawable {
    var content: Drawable
    func draw() -> String { return content.draw() }
}

@resultBuilder
struct DrawingBuilder {
    static func buildBlock<D: Drawable>(_ components: D...) -> Line<D> {
        return Line(elements: components)
    }
    static func buildEither<First, Second>(first: First)
        -> DrawEither<First, Second> {
            return DrawEither(content: first)
    }
    static func buildEither<First, Second>(second: Second)
        -> DrawEither<First, Second> {
            return DrawEither(content: second)
    }
}
```

ただし、このアプローチでは、アベイラビリティチェックがあるコードで問題が発生します:

```swift
@available(macOS 99, *)
struct FutureText: Drawable {
    var content: String
    init(_ content: String) { self.content = content }
    func draw() -> String { return content }
}
@DrawingBuilder var brokenDrawing: Drawable {
    if #available(macOS 99, *) {
        FutureText("Inside.future")  // Problem
    } else {
        Text("Inside.present")
    }
}
// brokenDrawing の型は Line<DrawEither<Line<FutureText>, Line<Text>>>
```

上記のコードでは、`FutureText` は `DrawEither` ジェネリック型の 1 つのため、`brokenDrawing` の型の一部として使用されています。これにより、`FutureText` が実行時に使用できない場合、その型が明示的に使用されていない場合でも、プログラムがクラッシュする可能性があります。

この問題を解決するには、`buildLimitedAvailability(_:)` メソッドを実装し、常に利用可能な型を返すことで、型情報を消去します。例えば、下記のコードは、アベイラビリティチェックから `AnyDrawable` 値を作成します。

```swift
struct AnyDrawable: Drawable {
    var content: Drawable
    func draw() -> String { return content.draw() }
}
extension DrawingBuilder {
    static func buildLimitedAvailability(_ content: some Drawable) -> AnyDrawable {
        return AnyDrawable(content: content)
    }
}

@DrawingBuilder var typeErasedDrawing: Drawable {
    if #available(macOS 99, *) {
        FutureText("Inside.future")
    } else {
        Text("Inside.present")
    }
}
// typeErasedDrawing の型は Line<DrawEither<AnyDrawable, Line<Text>>>
```

* 分岐文は、`buildEither(first:)` メソッドと `buildEither(second:)` メソッドへの一連のネストされた呼び出しになります。文の条件とケースはバイナリツリーのリーフノードにマッピングされ、文はルートノードからそのリーフノードへのパスをたどる `buildEither` メソッドへのネストされた呼び出しになります

例えば、3 つのケースを持つ `switch` 文を作成する場合、コンパイラは 3 つのリーフノードを持つバイナリツリーを使用します。同様に、ルートノードから 2 番目のケースへのパスは「2 番目の子」の「最初の子」のため、そのケースは `buildEither(first: buildEither(second: ... ))` のようなネストされた呼び出しになります。次の宣言は同等です:

```swift
let someNumber = 19
@ArrayBuilder var builderConditional: [Int] {
    if someNumber < 12 {
        31
    } else if someNumber == 19 {
        32
    } else {
        33
    }
}

var manualConditional: [Int]
if someNumber < 12 {
    let partialResult = ArrayBuilder.buildExpression(31)
    let outerPartialResult = ArrayBuilder.buildEither(first: partialResult)
    manualConditional = ArrayBuilder.buildEither(first: outerPartialResult)
} else if someNumber == 19 {
    let partialResult = ArrayBuilder.buildExpression(32)
    let outerPartialResult = ArrayBuilder.buildEither(second: partialResult)
    manualConditional = ArrayBuilder.buildEither(first: outerPartialResult)
} else {
    let partialResult = ArrayBuilder.buildExpression(33)
    manualConditional = ArrayBuilder.buildEither(second: partialResult)
}
```

* `else` 句のない `if` 文のように、値を生成しない可能性のある分岐文は、`buildOptional(_:)` の呼び出しになります。`if` 文の条件が満たされると、そのコードブロックが変換され、引数として渡されます。それ以外の場合、`buildOptional(_:)` は引数として `nil` を使用して呼び出されます。例えば、次の宣言は同等です:

```swift
@ArrayBuilder var builderOptional: [Int] {
    if (someNumber % 2) == 1 { 20 }
}

var partialResult: [Int]? = nil
if (someNumber % 2) == 1 {
    partialResult = ArrayBuilder.buildExpression(20)
}
var manualOptional = ArrayBuilder.buildOptional(partialResult)
```

リザルトビルダーが `buildPartialBlock(first:)` メソッドと `buildPartialBlock(accumulated:next:)` メソッドを実装している場合、コードブロックまたは `do` 文はこれらのメソッドを呼び出します。ブロック内の最初の宣言は `buildPartialBlock(first:)` メソッドの引数に変換され、残りの宣言は `buildPartialBlock(accumulated:next:)` メソッドのネストされた呼び出しになります。例えば、以下の宣言は等しいです:

```swift
struct DrawBoth<First: Drawable, Second: Drawable>: Drawable {
    var first: First
    var second: Second
    func draw() -> String { return first.draw() + second.draw() }
}

@resultBuilder
struct DrawingPartialBlockBuilder {
    static func buildPartialBlock<D: Drawable>(first: D) -> D {
        return first
    }
    static func buildPartialBlock<Accumulated: Drawable, Next: Drawable>(
        accumulated: Accumulated, next: Next
    ) -> DrawBoth<Accumulated, Next> {
        return DrawBoth(first: accumulated, second: next)
    }
}

@DrawingPartialBlockBuilder var builderBlock: some Drawable {
    Text("First")
    Line(elements: [Text("Second"), Text("Third")])
    Text("Last")
}

let partialResult1 = DrawingPartialBlockBuilder.buildPartialBlock(first: Text("first"))
let partialResult2 = DrawingPartialBlockBuilder.buildPartialBlock(
    accumulated: partialResult1,
    next: Line(elements: [Text("Second"), Text("Third")])
)
let manualResult = DrawingPartialBlockBuilder.buildPartialBlock(
    accumulated: partialResult2,
    next: Text("Last")
)
```

そうでなければ、コードブロックまたは `do` 文は、`buildBlock(_:)` メソッドの呼び出しになります。ブロック内の各文は一度に 1 つずつ変換され、`buildBlock(_:)` メソッドの引数になります。例えば、次の宣言は同等です:

```swift
@ArrayBuilder var builderBlock: [Int] {
    100
    200
    300
}

var manualBlock = ArrayBuilder.buildBlock(
    ArrayBuilder.buildExpression(100),
    ArrayBuilder.buildExpression(200),
    ArrayBuilder.buildExpression(300)
)
```

* `for` ループは一時変数、`for` ループ、`buildArray(_:)` メソッドの呼び出し、に分かれます。新しい `for` ループはシーケンスを繰り返し処理し、それぞれの部分的な結果をその配列に追加します。一時配列は、`buildArray(_:)` メソッドの引数として渡されます。例えば、次の宣言は同等です:

```swift
@ArrayBuilder var builderArray: [Int] {
    for i in 5...7 {
        100 + i
    }
}

var temporary: [[Int]] = []
for i in 5...7 {
    let partialResult = ArrayBuilder.buildExpression(100 + i)
    temporary.append(partialResult)
}
let manualArray = ArrayBuilder.buildArray(temporary)
```

* リザルトビルダに `buildFinalResult(_:)` メソッドがある場合、最終結果はそのメソッドの呼び出しになります。この変換は常に最後に行われます

変換の動作は一時変数の観点から説明されていますが、リザルトビルダを使用しても、コードの残りの部分から見える新しい宣言は実際には作成されません。

リザルトビルダが変換するコードで、`break`、`continue`、`defer`、`guard`、`return`、`while`、または `do-catch` を使用することはできません。

変換プロセスでは、コード内の宣言は変更されません。これにより、一時的な定数と変数を使用して、1 つずつ式を構築します。また、`throw` 文、コンパイル時の診断文、または `return` 文を含むクロージャも変更しません。

可能な限り、変換は合体します。例えば、式 `4 + 5 * 6` は、その関数への複数の呼び出しではなく、`buildExpression(4 + 5 * 6)` になります。同様に、ネストされた分岐文は、`buildEither` メソッドへの呼び出しの単一のバイナリツリーになります。

#### Custom Result-Builder Attributes

リザルトビルダ型を作成すると、同じ名前のカスタム属性が作成されます。その属性は、次の場所に適用できます:

* 関数宣言では、リザルトビルダは関数の本文を作成します
* get を含む変数またはサブスクリプトの宣言で、リザルトビルダは get の本文を作成します
* 関数宣言のパラメータでは、リザルトビルダは対応する引数として渡されるクロージャの本文を作成します

resultBuilder 属性を適用しても、ABI の互換性には影響しません。リザルトビルダ属性をパラメータに適用すると、その属性が関数のインターフェースの一部になり、ソースの互換性に影響を与える可能性があります。

### requires\_stored\_property\_inits

この属性をクラス宣言に適用すると、クラス内に格納されている全てのプロパティに、定義の一部としてデフォルト値を提供するように要求します。この属性は、`NSManagedObject` を継承する全てのクラスに対して推論されます。

### testable

モジュールのコードのテストを簡単にするために、この属性をインポート宣言に適用すると、アクセス制御へ変更を加えて、そのモジュールをインポートします。インポートされたモジュール内の `internal` 修飾子がマークされているエンティティは、`public` 修飾子で宣言されているかのようにインポートされます。`internal` または `public` 修飾子がマークされたクラスおよびクラスメンバは、`open` 修飾子で宣言されたかのようにインポートされます。インポートされたモジュールは、テストを有効にしてコンパイルする必要があります。

### UIApplicationMain

> 非推奨:  
> この属性は非推奨です。  
> [main](./language-reference/attributes.md#main)を代わりに使用してください。  
> Swift6では、この属性を使用するとエラーになります。

この属性をクラスに適用して、それが app delegate だということを示します。この属性を使用することは、`UIApplicationMain` 関数を呼び出し、このクラスの名前をデリゲートクラスの名前として渡すことと同じです。

この属性を使用しない場合は、[UIApplicationMain\(_:_:_:_:\)](https://developer.apple.com/documentation/uikit/1622933-uiapplicationmain)関数を呼び出すトップレベルのコードを `main.swift` ファイルに指定します。例えば、アプリが `UIApplication` の独自のサブクラスをメインクラスとして使用している場合、この属性を使用する代わりに `UIApplicationMain(_:_:_:_:)` 関数を呼び出します。

実行可能ファイルを作成するためにコンパイルする Swift コードには、[Top-Level Code\(トップレベルコード\)](../language-reference/declarations.md#top-level-code)で説明されているように、ただ 1 つのトップレベルのエントリポイントのみを含めます。

### <a id="unchecked">unchecked</a>

型宣言に適用されたプロトコルのリストの一部としてこの属性をあるプロトコルの型に適用した場合、そのプロトコルの要件の適用をオフにします。

サポートされているプロトコルは[`Sendable`](https://developer.apple.com/documentation/swift/sendable)のみです。

### usableFromInline

この属性を関数、メソッド、計算プロパティ、サブスクリプト、イニシャライザ、またはデイニシャライザの宣言に適用すると、宣言と同じモジュールで定義されているインライン化されたコードでそのシンボルを使用できるようにします。宣言には、`internal` 修飾子が必要です。`usedFromInline` がマークされた構造体またはクラスは、そのプロパティに `public` または `usableFromInline` の型のみを使用できます。`usedFromInline` がマークされた列挙型は、そのケースの Raw Value と関連値に対して、`public` または `usableFromInline` の型のみを使用できます。

`public` 修飾子と同様に、この属性はモジュールの公開インターフェイスの一部として宣言が公開されます。`public` とは異なり、コンパイラは、宣言のシンボルがエクスポートされている場合でも、`usableFromInline` がマークされた宣言をモジュール外のコードで参照することを許可しません。ただし、モジュール外のコードは、ランタイム時の動作を使用して宣言のシンボルとやり取りできることもあります。

`inlinable` 属性がマークされた宣言は、inlinable コードから暗黙的に使用できます。`inlinable` または `usableFromInline` のいずれかを `internal` 宣言に適用できますが、両方の属性を適用するとエラーです。

### warn\_unqualified\_access

この属性をトップレベルの関数、インスタンスメソッド、またはクラスまたは静的メソッドに適用すると、その関数またはメソッドがモジュール名、型名、インスタンス変数または定数などを修飾子なしで使用された場合に警告を出力します。この属性を使用して、同じスコープからアクセスできる同じ名前の関数間のあいまいさを回避できます。

例えば、Swift 標準ライブラリには、トップレベルの[min\(_:_:\)](https://developer.apple.com/documentation/swift/1538339-min/)関数と、同等の要素を持つ `Sequence` の `min()` メソッドの両方が含まれています。`Sequence` メソッドは `warn_unqualified_access` 属性で宣言されており、`Sequence` extension 内からどちらかを使用しようとするときの混乱を減らすのに役立ちます。

### Declaration Attributes Used by Interface Builder

Interface Builder 属性は、Xcode と同期するために InterfaceBuilder によって使用される宣言属性です。Swift は、次の Interface Builder 属性を提供します: `IBAction`、`IBSegueAction`、`IBOutlet`、`IBDesignable`、および `IBInspectable`。これらの属性は、Objective-C の対応する属性と概念的に同じです。

`IBOutlet` 属性と `IBInspectable` 属性はクラスのプロパティ宣言に適用できます。`IBAction` 属性と `IBSegueAction` 属性はクラスのメソッド宣言に適用でき、`IBDesignable` 属性はクラス宣言に適用できます。

`IBAction`、`IBSegueAction`、`IBOutlet`、`IBDesignable`、または `IBInspectable` 属性を適用すると、`objc` 属性も暗黙的に追加されます。

## 型属性\(Type Attributes\)

型属性は型にのみ適用できます。

### autoclosure

この属性を適用すると、引数のないクロージャで式を自動的にラップすることにより、式の評価を遅らせます。関数またはメソッド宣言のパラメータに、引数を受け取らず式の型の値を返す関数型を適用できます。`autoclosure` 属性の使用方法の例については、[AutoClosures\(自動クロージャ\)](../language-guide/closures.md#autoclosures)と[Function Type\(関数型\)](../language-reference/types.md#types-function-type)を参照ください。

### convention

呼び出し規約を示すために、この属性を関数の型に適用します。

`convention` 属性は、常に次のいずれかの引数で表示されます:

* `swift` 引数は、Swift 関数の参照を示します。これは、Swift の関数値の標準的な呼び出し規約です
* `block` 引数は、Objective-C 互換のブロック参照を示します。関数値は、ブロックオブジェクトへの参照として表されます。ブロックオブジェクトは、その呼び出し関数をオブジェクト内に埋め込む `id` 互換の Objective-C オブジェクトへの参照です。呼び出し関数は C 言語の呼び出し規約を使用します
* `c` 引数は、C 言語関数の参照を示します。関数値はコンテキストを持たず、C 言語の呼び出し規約を使用します

いくつかの例外を除いて、他の呼び出し規約の関数が必要な場合は、任意の呼び出し規約の関数を使用できます。非ジェネリックグローバル関数、ローカル変数をキャプチャしないローカル関数、またはローカル変数をキャプチャしないクロージャは、C 言語の呼び出し規約に変換できます。他の Swift 関数は C 言語の呼び出し規約に変換できません。Objective-C の block 呼び出し規約を持つ関数は、C 言語の呼び出し規約に変換できません。

### escaping

この属性を関数またはメソッド宣言のパラメータの型に適用すると、パラメータの値を後で実行するために保持されることがあることを示します。これは、値が呼び出し側の生存期間を超えて存続できることを意味します。`escaping` 属性を持つ関数型パラメータは、プロパティまたはメソッドに対して `self` を明示的に使用する必要があります。`escaping` 属性の使用方法の例については、[Escaping Closures\(エスケープクロージャ\)](../language-guide/closures.md#escaping-closures)を参照ください。

### <a id="sendable">Sendable</a>

この属性を関数の型に適用して、関数またはクロージャが `Sendable` なことを示します。この属性を関数型に適用することは、非関数型を [`Sendable`](https://developer.apple.com/documentation/swift/sendable) プロトコルに準拠させることと同じ意味です。

この属性は、関数またはクロージャが `Sendable` の値を期待するコンテキストで使用され、 `Sendable` の要件を満たしている場合、その関数またはクロージャは `Sendable` だと推論されます。

`Sendable` な関数型は、対応する `Sendable` ではない関数型のサブタイプです。

## Switch Case Attributes \(Switch ケース属性\)

switch の case 属性は、switch 文のケースにのみ適用できます。

### unknown

この属性を switch ケースに適用すると、コードのコンパイル時にわかっているどの列挙ケースとも一致することがない可能性を示します。`unknown` 属性の使用方法の例については、[Switching Over Future Enumeration Cases\(列挙型の将来のケースのスイッチング\)](../language-reference/statements.md#switching-over-future-enumeration-cases)を参照ください。

> Grammar of an attribute:
>
> *attribute* → **`@`** *attribute-name* *attribute-argument-clause*_?_ \
> *attribute-name* → *identifier* \
> *attribute-argument-clause* → **`(`** *balanced-tokens*_?_ **`)`** \
> *attributes* → *attribute* *attributes*_?_
>
> *balanced-tokens* → *balanced-token* *balanced-tokens*_?_ \
> *balanced-token* → **`(`** *balanced-tokens*_?_ **`)`** \
> *balanced-token* → **`[`** *balanced-tokens*_?_ **`]`** \
> *balanced-token* → **`{`** *balanced-tokens*_?_ **`}`** \
> *balanced-token* → 任意の識別子、キーワード、リテラル、または演算子 \
> *balanced-token* → **`(`**,  **`)`**,  **`[`**,  **`]`**,  **`{`**, または  **`}`** を除く任意の句読点