# Type Casting

最終更新日:

型キャスト(*type casting*)は、インスタンスの型をチェックする方法、またはそのインスタンスを独自のクラス階層のどこか別のスーパークラスまたはサブクラスとして扱う方法です。

Swift での型キャストは、 `is` および `as` 演算子で実装されます。これらの 2 つの演算子は、値の型をチェックしたり、値を別の型にキャストしたりするためのシンプルで表現力豊かな方法を提供します。

[Checking for Protocol Conformance](./protocols.md#checking-for-protocol-conformanceプロトコル準拠のチェック)で説明されているように、型キャストを使用して、型がプロトコルに準拠しているかどうかを確認することもできます。

## Defining a Class Hierarchy for Type Casting(型キャストのためのクラス階層の定義)

クラスとサブクラスの階層で型キャストを使用して、特定のクラスインスタンスの型を確認し、そのインスタンスを同じ階層内の別のクラスにキャストできます。以下の 3 つのコードスニペットは、型キャストの例で使用するために、クラスの階層とそれらのクラスのインスタンスを含む配列を定義します。

最初のスニペットは、`MediaItem` という新しい基本クラスを定義します。このクラスは、デジタルメディアライブラリに表示されるあらゆる種類のアイテムに基本的な機能を提供します。具体的には、`String` 型の `name` プロパティと `init name` イニシャライザを宣言します。(全ての映画と曲を含む全てのメディアアイテムに名前があると想定されています)

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

## Checking Type(型チェック)

インスタンスが特定のサブクラス型かどうかを確認するには、型チェック演算子(`is`)を使用します。型チェック演算子は、インスタンスがそのサブクラス型の場合は `true` を返し、そうでない場合は `false` を返します。

下記の例では、`library` 配列内の `Movie` および `Song` インスタンスの数をカウントする 2 つの変数 `movieCount` と `songCount` を定義しています:

```swift
var movieCount = 0
var songCount = 0

for item in library {
    if item is Movie {
        movieCount += 1
    } else if item is Song {
        songCount += 1
    }
}

print("Media library contains \(movieCount) movies and \(songCount) songs")
// "Media library contains 2 movies and 3 songs"
```

この例では、`library` 配列内の全てのアイテムを繰り返します。各パスで、`for-in` ループは、`item` 定数を配列内の次の `MediaItem` に設定します。

`item is Movie` は、現在の `MediaItem` が `Movie` インスタンスの場合は `true` を返し、そうでない場合は `false` を返します。同様に、`item is Song` は、アイテムが `Song` インスタンスかどうかをチェックします。`for-in` ループの最後で、`movieCount` と `songCount` の値には、各型の `MediaItem` インスタンスがいくつ見つかったかのカウントが含まれます。

## Downcasting(ダウンキャスト)

特定のクラス型の定数または変数は、実際には裏側でサブクラスのインスタンスを参照している場合があります。そのような場合は、型キャスト演算子 (`as?` または `as!`) を使用してサブクラス型へのダウンキャスト(*downcasting*)を試みることができます。

ダウンキャストは失敗する可能性があるため、型キャスト演算子には 2 つの異なる形式があります。条件付き形式の `as?` は、ダウンキャストしようとしている型のオプショナルの値を返します。強制形式の `as!` は、ダウンキャストと強制アンラップを同時に試みます。

ダウンキャストが成功するかどうかわからない場合は、型キャスト演算子 (`as?`) の条件付き形式を使用してください。この形式の演算子は常にオプショナルの値を返し、ダウンキャストが不可能な場合、値は `nil` になります。これにより、ダウンキャストが成功したかどうかを確認できます。

型キャスト演算子の強制形式 (`as!`) は、ダウンキャストが常に成功することが確実な場合にのみ使用してください。不適切なクラス型にダウンキャストしようとすると、実行時エラーを引き起こしします。

下記の例では、`library` 内の各 `MediaItem` を繰り返し処理し、各アイテムの適切な説明を出力します。これを行うには、`MediaItem` としてだけでなく、実際の `Movie` または `Song` として各アイテムにアクセスする必要があります。これは、説明で使用する `Movie` または `Song` の `director` または `artist` のプロパティにアクセスできるようにするために必要です。

この例では、配列内の各アイテムが `Movie` または `Song` の可能性があります。各項目に使用する実際のクラスは事前にわからないため、条件付き形式の型キャスト演算子(`as?`)を使用して、ループの中で毎回ダウンキャストしてチェックすることが適切です:

```swift
for item in library {
    if let movie = item as? Movie {
        print("Movie: \(movie.name), dir. \(movie.director)")
    } else if let song = item as? Song {
        print("Song: \(song.name), by \(song.artist)")
    }
}

// Movie: Casablanca, dir. Michael Curtiz
// Song: Blue Suede Shoes, by Elvis Presley
// Movie: Citizen Kane, dir. Orson Welles
// Song: The One And Only, by Chesney Hawkes
// Song: Never Gonna Give You Up, by Rick Astley
```

この例では、まず現在のアイテムを `Movie` としてダウンキャストしようとしています。`item` は `MediaItem` インスタンスなので、それが実際は `Movie` インスタンスの可能性があります。同様に、それが `Song` インスタンスの可能性もあれば、単に基本クラスの `MediaItem` インスタンスの可能性もあります。このように不確かなため、`as?` 型キャスト演算子の形式でサブクラス型にダウンキャストしようとしたときに、オプショナルの値を返します。`item as? Movie` の結果は `Movie?` または"オプショナルの `Movie`"です。

ライブラリ配列内の `Song` インスタンスに適用すると、`Movie` へのダウンキャストが失敗します。これに対処するために、上記の例では、オプショナルバインディングを使用して、オプショナルの `Movie` に実際に値が含まれているかどうか(つまり、ダウンキャストが成功したかどうか)を確認します。このオプショナルバインディング、"`if let movie = item as? Movie`"は、次のように読むことができます:

「Movie として item アイテムにアクセスしてみてください。成功した場合は、返されたオプショナルの `Movie` を `movie` という新しい一時定数をに設定します」

ダウンキャストが成功すると、`Movie` のプロパティを使用して、その `Movie` インスタンスの `director` の名前を含む説明が出力されます。同様に、`Song` インスタンスをチェックし、ライブラリで `Song` ソングが見つかる度に `artist` 名を含む適切な説明を出力します。

> NOTE  
> キャストは実際にインスタンスを変更したり、その値を変更したりはしません。基になるインスタンスは同じままです。キャストした型のインスタンスとして扱われ、アクセスされるだけです。

## Type Casting for Any and AnyObject(AnyおよびAnyObject の型キャスト)
