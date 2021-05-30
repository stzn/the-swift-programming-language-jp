# Type Casting

最終更新日:

型キャスト(*type casting*)は、インスタンスの型をチェックする方法、またはそのインスタンスを独自のクラス階層のどこか別のスーパークラスまたはサブクラスとして扱う方法です。

Swift での型キャストは、 `is` および `as` 演算子で実装されます。これらの 2 つの演算子は、値の型をチェックしたり、値を別の型にキャストしたりするためのシンプルで表現力豊かな方法を提供します。

[Checking for Protocol Conformance](./protocols.md#checking-for-protocol-conformanceプロトコル準拠のチェック)で説明されているように、型キャストを使用して、型がプロトコルに準拠しているかどうかを確認することもできます。

## Defining a Class Hierarchy for Type Casting(型キャストのためのクラス階層の定義)

クラスとサブクラスの階層で型キャストを使用して、特定のクラスインスタンスの型を確認し、そのインスタンスを同じ階層内の別のクラスにキャストできます。以下の 3 つのコードスニペットは、型キャストの例で使用するために、クラスの階層とそれらのクラスのインスタンスを含む配列を定義します。

最初のスニペットは、`MediaItem` という新しい基本クラスを定義します。このクラスは、デジタルメディアライブラリに表示されるあらゆる種類のアイテムに基本的な機能を提供します。具体的には、`String` 型の `name` プロパティと `init name` イニシャライザを宣言します。(全ての映画と曲を含むすべてのメディアアイテムに名前があると想定されています)

```swift
class MediaItem {
    var name: String
    init(name: String) {
        self.name = name
    }
}
```

次のスニペットは、`MediaItem` の 2 つのサブクラスを定義します。最初のサブクラスの `Movie` は、映画または映画に関する追加情報をカプセル化します。これは、対応するイニシャライザを使用して、基本クラスの `MediaItem` 上に `director` プロパティを追加します。2 番目のサブクラスの `Song` は、基本クラスの上に `artist` プロパティとイニシャライザを追加します:

```swift
class Movie: MediaItem {
    var director: String
    init(name: String, director: String) {
        self.director = director
        super.init(name: name)
    }
}

class Song: MediaItem {
    var artist: String
    init(name: String, artist: String) {
        self.artist = artist
        super.init(name: name)
    }
}
```

最後のスニペットは、2 つの `Movie` インスタンスと 3 つの `Song` インスタンスを含む `library` という定数配列を作成します。`library` の配列の型は、配列リテラルの内容で初期化することで推論されます。Swift の型チェッカーは、`Movie` と `Song` が `MediaItem` の共通のスーパークラスを持っていることを推論できるため、`library` 配列の `[MediaItem]` の型を推論します。

```swift
let library = [
    Movie(name: "Casablanca", director: "Michael Curtiz"),
    Song(name: "Blue Suede Shoes", artist: "Elvis Presley"),
    Movie(name: "Citizen Kane", director: "Orson Welles"),
    Song(name: "The One And Only", artist: "Chesney Hawkes"),
    Song(name: "Never Gonna Give You Up", artist: "Rick Astley")
]
// "library" の型は [MediaItem] と推論されます
```

`library` に保存されているアイテムは、依然として裏側では `Movie` および `Song` インスタンスです。ただし、この配列の内容を反復処理すると、返されるアイテムは `Movie` や `Song` ではなく `MediaItem` になります。それらを本来の型として使用するには、型を確認するか、下記で説明するように別の型にダウンキャスト(*downcast*)する必要があります。

## Checking Type

## Downcasting

## Type Casting for Any and AnyObject