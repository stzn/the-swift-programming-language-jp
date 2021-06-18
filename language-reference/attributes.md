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

アスタリスク(`*`)を使用して、上記のすべてのプラットフォーム名で宣言が使用可能なことを示すこともできます。Swift のバージョン番号を使用して availability を指定する `available` 属性では、アスタリスクを使用できません。

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

### dynamicCallable

### dynamicMemberLookup

### frozen

### GKInspectable

### inlinable

### main

### nonobjc

### NSApplicationMain

### NSCopying

### NSManaged

### objc

### objcMembers

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
