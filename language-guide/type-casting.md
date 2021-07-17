# 型キャスト\(Type Casting\)

最終更新日: 2021/6/29

_型キャスト_は、インスタンスの型をチェックする、またはそのインスタンスを独自のクラス階層のスーパークラスまたはサブクラスとして扱う方法です。

Swift での型キャストは、`is` および `as` 演算子で実装されます。これらの 2 つの演算子は、値の型をチェックしたり、値を別の型にキャストしたりするためのシンプルで表現力豊かな方法を提供します。

[Checking for Protocol Conformance\(プロトコル準拠チェック\)](../language-guide/protocols.md#checking-for-protocol-conformance)で説明されているように、型キャストを使用して、型がプロトコルに準拠しているかどうかを確認することもできます。

## 型キャストのためのクラス階層の定義\(Defining a Class Hierarchy for Type Casting\)

クラスとサブクラスの階層で型キャストを使用して、特定のクラスインスタンスの型を確認し、そのインスタンスを同じ階層内の別のクラスにキャストできます。下記の 3 つのコードスニペットは、今後の型キャストの例で使用するために、クラスの階層とそれらのクラスのインスタンスを含む配列を定義しています。

最初のスニペットは、`MediaItem` という新しい基本クラスを定義します。このクラスは、デジタルメディアライブラリに表示されるあらゆる種類のアイテムの基本的な機能を提供します。具体的には、`String` 型の `name` プロパティと `init(name:)` イニシャライザを宣言しています。\(全ての映画と曲を含む全てのメディアアイテムに名前があると想定しています\)

```swift
class MediaItem {
    var name: String
    init(name: String) {
        self.name = name
    }
}
```

次のスニペットは、`MediaItem` の 2 つのサブクラスを定義します。最初のサブクラスの `Movie` は、映画または映画に関する追加情報をカプセル化します。これは、対応するイニシャライザを使用して、基本クラスの `MediaItem` の上に `director` プロパティを追加します。2 番目のサブクラスの `Song` は、基本クラスの上に `artist` プロパティとイニシャライザを追加します:

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

最後のスニペットは、2 つの `Movie` インスタンスと 3 つの `Song` インスタンスを含む `library` という定数配列を作成します。`library` の配列の型は、初期化時に使用する配列リテラルの内容から推論できます。Swift の型チェックは、`Movie` と `Song` が `MediaItem` という共通のスーパークラスを持っていることを推論できるため、`library` 配列が `[MediaItem]` 型だと推論できます。

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

`library` に保存されているアイテムは、依然として裏側では `Movie` および `Song` インスタンスです。ただし、この配列の内容を反復処理すると、返されるアイテムは `Movie` や `Song` ではなく `MediaItem` になります。それらを本来の型として使用するには、型を確認するか、下記で説明するように別の型に_ダウンキャスト_する必要があります。

## 型チェック\(Checking Type\)

インスタンスが特定のサブクラス型かどうかを確認するには、型チェック演算子\(`is`\)を使用します。型チェック演算子は、インスタンスがそのサブクラス型の場合は `true` を返し、そうでない場合は `false` を返します。

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

この例では、`library` 配列内の全てのアイテムを繰り返します。`for-in` ループの各ループでは、`item` 定数に配列内の次の `MediaItem` を設定します。

`item is Movie` は、現在の `MediaItem` が `Movie` インスタンスの場合は `true` を返し、そうでない場合は `false` を返します。同様に、`item is Song` は、アイテムが `Song` インスタンスかどうかをチェックします。`for-in` ループの最後で、`movieCount` と `songCount` の値には、各型の `MediaItem` インスタンスがいくつ見つかったかをカウントしています。

## <a id="downcasting">ダウンキャスト\(Downcasting\)</a>

特定のクラス型の定数または変数は、実際には裏側でサブクラスのインスタンスを参照している場合があります。そのような場合は、型キャスト演算子 \(`as?` または `as!`\) を使用してサブクラス型への_ダウンキャスト_を試みることができます。

ダウンキャストは失敗する可能性があるため、型キャスト演算子には 2 つの異なる形式があります。条件付き形式の `as?` は、ダウンキャストしようとしている型のオプショナルの値を返します。強制形式の `as!` は、ダウンキャストと強制アンラップを同時に試みます。

ダウンキャストが成功するかどうかわからない場合は、条件付き形式の型キャスト演算子 \(`as?`\) を使用してください。この形式の演算子は常にオプショナルの値を返し、ダウンキャストが不可能な場合、値は `nil` になります。これにより、ダウンキャストが成功したかどうかを確認できます。

強制形式の型キャスト演算子 \(`as!`\) は、ダウンキャストが常に成功することが確実な場合にのみ使用してください。不適切なクラス型にダウンキャストしようとすると、実行時エラーを引き起こしします。

下記の例では、`library` 内の各 `MediaItem` を繰り返し処理し、各アイテムの適切な説明を出力します。これを行うには、`MediaItem` としてだけでなく、実際の `Movie` または `Song` として各アイテムにアクセスする必要があります。これは、説明で使用する `Movie` または `Song` の `director` または `artist` のプロパティにアクセスできるようにするために必要です。

この例では、配列内の各アイテムが `Movie` または `Song` のいずれかの可能性があります。各項目に使用する実際のクラスは事前にわからないため、条件付き形式の型キャスト演算子\(`as?`\)を使用して、ループの中で毎回ダウンキャストしてチェックすることが適切です:

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

ライブラリ配列内の `Song` インスタンスに適用すると、`Movie` へのダウンキャストが失敗します。これに対処するために、上記の例では、オプショナルバインディングを使用して、オプショナルの `Movie` に実際に値が含まれているかどうか\(つまり、ダウンキャストが成功したかどうか\)を確認します。このオプショナルバインディング、"`if let movie = item as? Movie`"は、次のように読むことができます:

「Movie として item アイテムにアクセスしてみてください。成功した場合は、返されたオプショナルの `Movie` を `movie` という新しい一時定数に設定します」

ダウンキャストが成功すると、`Movie` のプロパティを使用して、その `Movie` インスタンスの `director` の名前を含む説明が出力されます。同様に、`Song` インスタンスをチェックし、ライブラリで `Song` が見つかる度に `artist` 名を含む適切な説明を出力します。

> NOTE  
> キャストは実際にインスタンスを変更したり、その内部の値を変更したりしません。基になるインスタンスは同じままです。キャストした型のインスタンスとして扱われ、アクセスされるだけです。

## <a id="type-casting-for-any-and-anyobject">Any および AnyObject の型キャスト\(Type Casting for Any and AnyObject\)</a>

Swift は、型を特定できない型を操作するための 2 つの特別な型を提供しています。

* `Any` は、関数型を含むあらゆる型のインスタンスを表すことができます
* `AnyObject` は、任意のクラス型のインスタンスを表すことができます

`Any` および `AnyObject` は、それらが提供する動作と機能が明らかに必要な場合にのみ使用してください。常にコードで使用しようとしている型を特定することをお勧めします。

`Any` を使用して、関数型と非クラス型を含む様々な型の組み合わせを処理する例を次に示します。この例では、`Any` 型の値を格納できる `things` という配列を作成します:

```swift
var things = [Any]()

things.append(0)
things.append(0.0)
things.append(42)
things.append(3.14159)
things.append("hello")
things.append((3.0, 5.0))
things.append(Movie(name: "Ghostbusters", director: "Ivan Reitman"))
things.append({ (name: String) -> String in "Hello, \(name)" })
```

`things` 配列には、2 つの `Int`、2 つの `Double`、1 つの `String`、`(Double、Double)` 型のタプル、映画"Ghostbusters"、および `String` 値を受け取り、別の `String` 値を返すクロージャ式、が含まれます。

`Any` 型または `AnyObject` 型なことがわかっている定数または変数の特定の型を検出するには、`switch` 文のケースで `is` または `as` パターンを使用できます。下記の例では、`things` 配列内の項目を繰り返し処理し、`switch` 文を使用して各項目の型を照会します。`switch` 文のいくつかのケースは、一致した値を指定された型の定数にバインドして、その値を出力できるようにしています:

```swift
for thing in things {
    switch thing {
    case 0 as Int:
        print("zero as an Int")
    case 0 as Double:
        print("zero as a Double")
    case let someInt as Int:
        print("an integer value of \(someInt)")
    case let someDouble as Double where someDouble > 0:
        print("a positive double value of \(someDouble)")
    case is Double:
        print("some other double value that I don't want to print")
    case let someString as String:
        print("a string value of \"\(someString)\"")
    case let (x, y) as (Double, Double):
        print("an (x, y) point at \(x), \(y)")
    case let movie as Movie:
        print("a movie called \(movie.name), dir. \(movie.director)")
    case let stringConverter as (String) -> String:
        print(stringConverter("Michael"))
    default:
        print("something else")
    }
}

// zero as an Int
// zero as a Double
// an integer value of 42
// a positive double value of 3.14159
// a string value of "hello"
// an (x, y) point at 3.0, 5.0
// a movie called Ghostbusters, dir. Ivan Reitman
// Hello, Michael
```

> NOTE  
> Any 型は、オプショナルの型を含む任意の型の値を表します。`Any` 型の値が期待されているところにオプショナル値を使用すると、Swift は警告を出します。本当にオプショナルの値を `Any` 値として使用する必要がある場合は、下記に示すように、`as` 演算子を使用して、オプショナルを `Any` に明示的にキャストします。
>
> ```swift
> let optionalNumber: Int? = 3
> things.append(optionalNumber)        // Warning
> things.append(optionalNumber as Any) // No warning
> ```

