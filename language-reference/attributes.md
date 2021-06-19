# Attributes \(属性\)

最終更新日:

Swift には、宣言に適用される属性と型に適用される属性の 2 種類があります。属性は、宣言または型に関する追加情報を提供します。たとえば、関数宣言の `discardableResult` 属性は、関数は値を返しますが、戻り値が使用されていない場合、コンパイラは警告を生成しないことを示します。

`@` 記号に続けて属性の名前と、属性が受け入れる引数を書き込むことにより、属性を指定します。

![属性](./../.gitbook/assets/attributes.png)

一部の宣言属性は、属性とそれが特定の宣言にどのように適用されるかについての詳細情報を指定する引数を受け入れます。これらの attribute arguments は括弧で囲まれ、その形式は属する属性によって定義されています。

## Declaration Attributes\(宣言属性\)

宣言属性は宣言にのみ適用できます。

### available

この属性を適用して、特定の Swift 言語バージョンまたは特定のプラットフォームとオペレーティングシステムのバージョンに関連する宣言のライフサイクルを示します。

`available` 属性は、常に 2 つ以上のカンマ区切りの属性引数のリストを伴います。これらの引数は、次のプラットフォーム名または言語名のいずれかで始まります:

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
* `swift`

アスタリスク(`*`)を使用して、上記の全てのプラットフォーム名で宣言が使用可能なことを示すこともできます。Swift のバージョン番号を使用して availability を指定する `available` 属性では、アスタリスクを使用できません。

残りの引数は任意の順序で表示でき、重要なマイルストーンなど、宣言のライフサイクルに関する追加情報を指定できます。

* `unavailable` 引数は、指定されたプラットフォームで宣言が使用できないことを示します。この引数は、Swift バージョンの availability を指定する場合は使用できません

* `introduced` 引数は、宣言が導入された特定のプラットフォームまたは言語の最初のバージョンを示します。形式は次のとおりです:

![introduced引数](./../.gitbook/assets/introduced.png)

version number は、ピリオド(`.`)で区切られた 1〜3 個の正の整数で構成されます。

* `deprecated` 引数は、宣言が非推奨になった特定のプラットフォームまたは言語の最初のバージョンを示します。形式は次のとおりです:

![deprecated引数](./../.gitbook/assets/deprecated.png)

任意の version number は、ピリオド(`.`)で区切られた 1〜3 個の正の整数で構成されます。バージョン番号を省略すると、deprecated がいつ発生したかについての情報を提供せずに、宣言が現在 deprecated になっていることを示します。バージョン番号を省略する場合は、コロン(`:`)も省略してください。

* `obsoleted` 引数は、宣言が廃止された特定のプラットフォームまたは言語の最初のバージョンを示します。宣言が廃止されると、指定されたプラットフォームまたは言語から削除され、使用できなくなります。形式は次のとおりです:

![obsoleted引数](./../.gitbook/assets/obsoleted.png)

version number は、ピリオド(`.`)で区切られた 1〜3 個の正の整数で構成されます。

* `message` 引数は、廃止または deprecated された宣言の使用した際に、コンパイラが表示する警告またはエラーのテキストメッセージを提供します。形式は次のとおりです:

![message引数](./../.gitbook/assets/message.png)

message は文字列リテラルで構成されます。

* `renamed` 引数は、名前が変更された宣言の新しい名前を示すテキストメッセージを提供します。コンパイラは、名前が変更された宣言の使用に対してエラーを発行する際に、新しい名前を表示します。形式は次のとおりです:

![renamed引数](./../.gitbook/assets/renamed.png)

new name は文字列リテラルで構成されます。

下記に示すように、`renamed` 引数と `unavailable` 引数を使用してフレームワークまたはライブラリのリリース間で宣言の名前が変更されたことを示すために、`available` 属性をタイプエイリアス宣言に適用することができます。この組み合わせにより、宣言の名前が変更されたというコンパイルエラーが発生します。

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

1 つの宣言に複数の `available` 属性を適用して、様々なプラットフォームおよび様々なバージョンの Swift での宣言の availability を指定できます。属性が現在のターゲットと一致しないプラットフォームまたは言語バージョンを指定している場合、`available` 属性が適用される宣言は無視されます。複数の使用可能な属性を使用する場合、効果的な可用性は、プラットフォームと Swift の可用性の組み合わせです。

`available` 属性がプラットフォームまたは言語名の引数に加えて `introduced` 引数を指定するだけの場合は、代わりに次の省略構文を使用できます:

![available属性](./../.gitbook/assets/available.png)

`available` 属性の省略構文は、複数のプラットフォームの availability を簡潔に表しています。2 つの形式は機能的に同等ですが、可能な限り省略形が推奨されます。

```swift
@available(iOS 10.0, macOS 10.12, *)
class MyClass {
    // クラス定義
}
```

Swift バージョン番号を使用して availability を指定する `available` 属性は、宣言のプラットフォームの availability を追加で指定することはできません。代わりに、個別の `available` 属性を使用して、Swift バージョンの availability と 1 つ以上のプラットフォームの availability を指定します。

```swift
@available(swift 3.0.2)
@available(macOS 10.12, *)
struct MyStruct {
    // 構造体定義
}
```

### discardableResult

この属性を関数またはメソッドの宣言に適用すると、値を返す関数またはメソッドがその結果を使用せずに呼び出されたときのコンパイラの警告を抑制します。

### dynamicCallable

この属性をクラス、構造体、列挙型、またはプロトコルに適用すると、型のインスタンスを呼び出し可能な関数として扱います。この型は、`dynamicallyCall(withArguments:)` メソッド、`dynamicallyCall(withKeywordArguments:)` メソッド、またはその両方を実装する必要があります。

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
// "Get Swift help on forums.swift.org"

dial(8, 6, 7, 5, 3, 0, 9)
// "Unrecognized number"

// 基になるメソッドを直接呼び出します
dial.dynamicallyCall(withArguments: [4, 1, 1])
```

`dynamicallyCall(withArguments:)` メソッドの宣言には、上記の例の `[Int]` のように、p[ExpressibleByArrayLiteral](https://developer.apple.com/documentation/swift/expressiblebyarrayliteral)プロトコルに準拠する単一の引数が必要です。戻り値の型は任意の型にすることができます。

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

`dynamicallyCall(withKeywordArguments:)` メソッドの宣言には、[ExpressibleByDictionaryLiteral](https://developer.apple.com/documentation/swift/expressiblebydictionaryliteral)プロトコルに準拠する単一の引数が必要で、戻り値の型は任意の型にすることができます。引数の[Key](https://developer.apple.com/documentation/swift/expressiblebydictionaryliteral/2294108-key)は[ExpressibleByStringLiteral](https://developer.apple.com/documentation/swift/expressiblebystringliteral)の必要があります。前の例では、引数型として[KeyValuePairs](https://developer.apple.com/documentation/swift/keyvaluepairs)を使用しているため、呼び出し元は重複する引数ラベルを含めることができます。つまり、`a` と `b` は、`repeat` の呼び出しに複数回表示されます。

両方の `dynamiclyCall` メソッドを実装する場合、メソッド呼び出しにキーワード引数が含まれていると、`dynamicallyCall(withKeywordArguments:)` が呼び出されます。他の全ての場合、`dynamicallyCall(withArguments:)` が呼び出されます。

動的に呼び出すことができるインスタンスは、`dynamicCall` メソッドの実装の 1 つで指定した型と一致する引数と戻り値を使用してのみ呼び出すことができます。次の例の呼び出しは、`KeyValuePairs <String、String>` を受け取る `dynamicallyCall(withArguments:)` の実装がないため、コンパイルできません。

```swift
repeatLabels(a: "four") // エラー
```

### dynamicMemberLookup

この属性をクラス、構造体、列挙型、またはプロトコルに適用すると、実行時にメンバを名前で検索できるようになります。型は `subscript(dynamicMember:)`subscript を実装する必要があります。

明示的なメンバ式では、指定されたメンバに対応する宣言がない場合、式は型の `subscript(dynamicMember:)`subscript の呼び出しとして解釈され、メンバに関する情報を引数として渡します。subscript は、KeyPath またはメンバ名のいずれかで引数を受け取ることができます。両方の subscript を実装する場合、KeyPath 引数を取る subscript が使用されます。

`subscript(dynamicMember:)` の実装は、[KeyPath](https://developer.apple.com/documentation/swift/keypath)、[WritableKeyPath](https://developer.apple.com/documentation/swift/writablekeypath)、または [ReferenceWritableKeyPath](https://developer.apple.com/documentation/swift/referencewritablekeypath)の引数を使用して KeyPath を受け取ることができます。[ExpressibleByStringLiteral](https://developer.apple.com/documentation/swift/expressiblebystringliteral)プロトコル(ほとんどの場合、`String`)に準拠する型の引数を使用して、メンバ名を受け入れることができます。subscript の戻り値の型は任意の型にすることができます。

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
// "325"

// 基になる subscript を直接呼び出します
let equivalent = s[dynamicMember: "someDynamicMember"]
print(dynamic == equivalent)
// "true"
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

### frozen

この属性を構造体または列挙型宣言に適用すると、型に加えることができる変更の種類を制限します。この属性は、library evolution mode でコンパイルする場合にのみ許可されます。ライブラリの将来のバージョンでは、列挙型のケースまたは構造体の格納インスタンスプロパティを追加、削除、または並べ替えて宣言を変更することはできません。これらの変更は nonfrozen 型で許可されますが、frozen 型の ABI 互換性が損なわれます。

> NOTE  
> コンパイラが library evolution mode でない場合、全ての構造体と列挙型は暗黙的に frozen にされ、この属性は無視されます。

library evolution mode では、nonfrozen の構造体や列挙型のメンバと相互作用するコードは、ライブラリの将来のバージョンでその型のメンバの一部が追加、削除、または並べ替えられた場合でも、再コンパイルせずに作業を継続できるようにコンパイルされます。コンパイラは、実行時に情報を検索したり、間接層を追加したりするなどの手法を使用して、これを可能にします。構造体または列挙型を frozen でマークすると、パフォーマンスを向上させるためのこの柔軟性が失われます。ライブラリの将来のバージョンでは、型に限定的な変更しか加えることができませんが、コンパイラは、型のメンバとやり取りするコードに追加の最適化を行うことができます。

frozen 型、frozen 構造体の格納プロパティの型、および frozen 列挙ケースの関連値は、public か、`usableFromInline` 属性がマークされている必要があります。frozen 構造体のプロパティはプロパティオブザーバを持つことができず、格納インスタンスプロパティの初期値を提供する式は、[inlinable](#inlinable) で説明されているように inlinable 関数と同じ制限に従う必要があります。

コマンドラインで library evolution mode を有効にするには、`-enable-library-evolution` オプションを Swift コンパイラに渡します。Xcode で有効にするには、[Xcode Help](https://help.apple.com/xcode/mac/current/#/dev04b3a04ba)の説明に従って、「Build Libraries for Distribution」のビルド設定(`BUILD_LIBRARY_FOR_DISTRIBUTION`)を Yes に設定します。

frozen 列挙型の switch 文は、[Switching Over Future Enumeration Cases(列挙型の将来のケースのスイッチング)](./attributes.md#switching-over-future-enumeration-cases列挙型の将来のケースのスイッチング)で説明されているように、デフォルトのケースを必要としません。frozen 列挙型を切り替えるときに `default` または `@unknown default` のケースを含めると、そのコードは実行されないため、警告が生成されます。

### GKInspectable

この属性を適用して、独自の GameplayKit コンポーネントプロパティを SpriteKit エディタ UI に公開します。この属性を適用すると、`objc` 属性も暗黙的に追加されます。

### inlinable

この属性を関数、メソッド、計算プロパティ、subscript、convenience イニシャライザ、またはデイニシャライザ宣言に適用すると、モジュールのパブリックインターフェイスの一部としてその宣言の実装を公開します。コンパイラは、inlinable シンボルへの呼び出しを、呼び出し側のシンボルの実装のコピーに置き換えることができます。

inlinable コードは、任意のモジュールで宣言された `public` シンボルとやり取りし、`usableFromInline` 属性がマークされた同じモジュールで宣言された `internal` シンボルとやり取りできます。inline 化できないコードは、`private` シンボルまたは fileprivate シンボルとやり取りできません。

この属性は、関数内にネストされている宣言や、`fileprivate` 宣言または `private` 宣言には適用できません。inlinable 関数内で定義された関数とクロージャは、この属性をマークすることはできませんが、暗黙的に inlinable です。

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

`main` 属性の要件を説明する別の方法は、この属性を書き込む型が、次の架空のプロトコルに準拠する型と同じ要件を満たさなければならないということです。

```swift
protocol ProvidesMain {
    static func main() throws
}
```

実行可能ファイルを作成するためにコンパイルする Swift コードには、[Top-Level Code](./declarations.md#top-Level-Codeトップレベルコード)で説明されているように、最大で 1 つのトップレベルのエントリポイントを含めることができます。

### nonobjc

この属性をメソッド、プロパティ、subscript、またはイニシャライザ宣言に適用すると、暗黙の `objc` 属性を抑制します。`nonobjc` 属性は、Objective-C で宣言を表すことができる場合でも、Objective-C コードで宣言を使用できないようにするようコンパイラに指示します。

この属性を extension に適用すると、`objc` 属性が明示的にマークされていないその extension の全てのメンバに適用するのと同じ効果があります。

`nonobjc` 属性を使用して、`objc` 属性がマークされたクラスのブリッジメソッドの循環性を解決し、`objc` 属性がマークされたクラスのメソッドとイニシャライザのオーバーロードを許可します。

`nonobjc` 属性がマークされたメソッドは、`objc` 属性がマークされたメソッドをオーバーライドできません。ただし、`objc` 属性がマークされたメソッドは、`nonobjc` 属性がマークされたメソッドをオーバーライドできます。同様に、`nonobjc` 属性がマークされたメソッドは、`objc` 属性がマークされたメソッドのプロトコル要件を満たすことができません。

### NSApplicationMain

この属性をクラスに適用すると、それがアプリケーションデリゲートなことを示します。この属性を使用することは、`NSApplicationMain(_:_:)` 関数を呼び出すことと同じです。

この属性を使用しない場合は、次のように `NSApplicationMain(_:_:)` 関数を呼び出すトップレベルのコードを `main.swift` ファイルに指定します。

```swift
import AppKit
NSApplicationMain(CommandLine.argc, CommandLine.unsafeArgv)
```

実行可能ファイルを作成するためにコンパイルする Swift コードには、[Top-Level Code](./declarations.md#top-Level-Codeトップレベルコード)で説明されているように、最大で 1 つのトップレベルのエントリポイントを含めることができます。

### NSCopying

この属性をクラスの格納変数プロパティに適用します。この属性により、プロパティの set は、プロパティ自体の値ではなく、`copyWithZone(_:)` メソッドによって返されるプロパティの値のコピーと同期されます。プロパティの型は、`NSCopying` プロトコルに準拠している必要があります。

`NSCopying` 属性は、Objective-C のコピープロパティ属性と同じように動作します。

### NSManaged

この属性を `NSManagedObject` を継承するクラスのインスタンスメソッドまたは格納変数プロパティに適用して、関連付けられたエンティティの記述に基づいて、CoreData が実行時にその実装を動的に提供することを示します。`NSManaged` 属性がマークされたプロパティの場合、CoreData は実行時にストレージも提供します。この属性を適用すると、`objc` 属性も暗黙的に追加されます。

### objc

この属性は Objective-C で使用することができる全ての宣言に適用します。例えば、ネストされていないクラス、プロトコル、(整数の RawValue 型の)非ジェネリック列挙型、クラスのプロパティとメソッド(get と set を含む)、プロトコル、およびオプショナルのプロトコル、イニシャライザ、および subscript。`objc` 属性は、Objective-C コードで宣言を使用できることをコンパイラに通知します。

この属性を extension に適用すると、`nonobjc` 属性が明示的にマークされていないその extension の全てのメンバに適用するのと同じ効果があります。

コンパイラは、Objective-C で定義されたクラスのサブクラスに `objc` 属性を暗黙的に追加します。ただし、サブクラスはジェネリックにはできず、ジェネリッククラスから継承してはなりません。これらの条件を満たすサブクラスに `objc` 属性を明示的に追加して、下記で説明するように、その Objective-C 名を指定できます。`objc` 属性がマークされているプロトコルは、`objc` 属性がマークされていないプロトコルから継承できません。

objc 属性は、次の場合にも暗黙的に追加されます:

* 宣言はサブクラスのオーバーライドで、スーパークラスの宣言には `objc` 属性があります
* 宣言は、`objc` 属性を持つプロトコルの要件を満たしています
* 宣言には、`IBAction`、`IBSegueAction`、`IBOutlet`、`IBDesignable`、`IBInspectable`、`NSManaged`、または `GKInspectable` 属性があります

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
> `objc` 属性の引数は、その宣言のランタイム名を変更することもできます。`NSClassFromString` などの Objective-C ランタイムとやり取りする関数を呼び出すとき、およびアプリの `Info.plist` ファイルでクラス名を指定するときに、ランタイム名を使用します。引数を渡して名前を指定すると、その名前が Objective-C コードの名前およびランタイム名として使用されます。引数を省略すると、Objective-C コードで使用される名前は Swift コードの名前と一致し、ランタイム名は通常の Swift コンパイラの名前修飾規則に従います。

### objcMembers

この属性をクラス宣言に適用すると、クラスの全ての Objective-C に互換性のあるメンバ、その extension、そのサブクラス、およびそのサブクラスの全ての extension に `objc` 属性を暗黙的に追加します。

ほとんどのコードでは、代わりに `objc` 属性を使用して、必要な宣言のみを公開する必要があります。多くの宣言を公開する必要がある場合は、`objc` 属性を持つ extension でグループ化できます。`objcMembers` 属性は、Objective-C ランタイムのリフレクション機能を多用するライブラリにとって便利です。必要のないときに `objc` 属性を適用すると、バイナリサイズが大きくなり、パフォーマンスに悪影響を与える可能性があります。

### propertyWrapper

### resultBuilder

#### Result-Building Methods

#### Result Transformations

#### Custom Result-Builder Attributes

### requires_stored_property_inits

### testable

### UIApplicationMain

### usableFromInline

### warn_unqualified_access

### Declaration Attributes Used by Interface Builder

## Type Attributes

### autoclosure

### convention

### escaping

## Switch Case Attributes

### unknown
