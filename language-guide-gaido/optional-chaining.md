# オプショナルチェーン\(Optional Chaining\)

最終更新日: 2021/6/29

_オプショナルチェーン_は、`nil` になる可能性のあるオプショナルのプロパティ、メソッド、および subscript を照会して呼び出すためのプロセスです。オプショナルに値が含まれている場合、プロパティ、メソッド、または subscript の呼び出しは成功します。オプショナルが `nil` の場合、プロパティ、メソッド、または subscript の呼び出しは `nil` を返します。複数を一気にチェーンさせることができ、ある地点で `nil` が返ってきた場合、オプショナルチェーン全体が失敗します。

> NOTE  
> Swift のオプショナルチェーンは、Objective-C の `nil` のメッセージングに似ていますが、どの型にでも機能し、成功または失敗をチェックできます。

## <a id="optional-chaining-as-an-alternative-to-forced-unwrapping">強制アンラップの代替としてのオプショナルチェーン\(Optional Chaining as an Alternative to Forced Unwrapping\)</a>

`nil` ではない場合に呼び出したいオプショナルのプロパティ、メソッド、または subscript の後に疑問符\(`?`\)を配置して、オプショナルチェーンを指定します。これは、オプショナルの後に感嘆符\(`!`\)を配置して、その値を強制アンラップするのによく似ています。主な違いは、オプショナルが `nil` の場合、オプショナルチェーンが失敗するのに対し、強制アンラップは実行時エラーを引き起こすことです。

オプショナルチェーンが `nil` 値で呼び出される可能性があるということを反映するために、照会しているプロパティ、メソッド、または subscript が非オプショナルの値を返した場合でも、オプショナルチェーンの呼び出しの結果は、常にオプショナルになります。このオプショナルの戻り値を使用して、オプショナルチェーンが成功したか\(返されたオプショナルに値が含まれているか\)、`nil` が存在して失敗したか\(返されたオプショナルが `nil` か\)を確認できます。

具体的には、オプショナルチェーンを介してアクセスされた場合、オプショナルチェーンの結果は、期待される戻り値と同じ型ですが、オプショナルでラップされています。`Int` を返すプロパティは `Int?` を返します。

次のいくつかのコードスニペットは、オプショナルチェーンが強制アンラップとどのように異なるかを示しており、成功したかどうかを確認できます。

まず、`Person` と `Residence` という 2 つのクラスが定義されています:

```swift
class Person {
    var residence: Residence?
}

class Residence {
    var numberOfRooms = 1
}
```

`Residence` インスタンスには、`numberOfRooms` という `Int` プロパティがあり、デフォルト値は `1` です。`Person` インスタンスには、`Residence?` 型の `residence` プロパティがあります。

新しい `Person` インスタンスを作成する場合、その `residence` プロパティはオプショナルなため、デフォルトで `nil` で初期化されています。下記のコードでは、`john` の `residence` プロパティは `nil` です。

```swift
let john = Person()
```

この人の `residence` の `numberOfRooms` プロパティにアクセスしようとすると、`residence` の後に感嘆符\(`!`\)を置いてその値を強制アンラップすると、`residence` に値がないため、実行時エラーが発生します。

```swift
let roomCount = john.residence!.numberOfRooms
// 実行時エラーが起きます
```

上記のコードは、`john.residence` の値が `nil` 以外の場合に成功し、`roomCount` に適切な部屋数を含む `Int` 値を設定します。ただし、上に示したように、このコードは、`residence` が `nil` の場合、常に実行時エラーを引き起こします。

オプショナルチェーンは、`numberOfRooms` の値にアクセスする別の方法を提供します。オプショナルチェーンを使用するには、感嘆符\(`!`\)の代わりに疑問符\(`?`\)を使用します:

```swift
if let roomCount = john.residence?.numberOfRooms {
    print("John's residence has \(roomCount) room(s).")
} else {
    print("Unable to retrieve the number of rooms.")
}
// "Unable to retrieve the number of rooms."
```

これは、オプショナルの `residence` プロパティを「チェーン」し、`residence` が存在する場合は `numberOfRooms` の値を取得するように Swift に指示します。

`numberOfRooms` へのアクセスは失敗する可能性があるため、オプショナルチェーンは `Int?` 型または"オプショナルの `Int`"の値を返します。上記の例のように、`residence` が `nil` の場合、`numberOfRooms` にアクセスできなかったことを反映するために、このオプショナルの `Int` も `nil` になります。オプショナルの `Int` は、整数をアンラップし、オプショナルバインディングを介してオプショナルではない値を `roomCount` 定数に割り当てます。

`numberOfRooms` がオプショナルではない `Int` でも、結果は `Int?` になることに注目してください。オプショナルチェーンを介して照会されるため、`numberOfRooms` の呼び出しは常に `Int` の代わりに `Int?` を返します。

`Residence` インスタンスを `john.residence` に割り当てて、`nil` 値を持たないようにすることができます。

```swift
john.residence = Residence()
```

`john.residence` には、`nil` ではなく `Residence` インスタンスが含まれるようになりました。以前と同じオプショナルチェーンを使用して `numberOfRooms` にアクセスすると、`numberOfRooms` のデフォルト値 `1` を含んだ `Int?` が返されます:

```swift
if let roomCount = john.residence?.numberOfRooms {
    print("John's residence has \(roomCount) room(s).")
} else {
    print("Unable to retrieve the number of rooms.")
}
// "John's residence has 1 room(s)."
```

## オプショナルチェーンモデルのクラスの定義\(Defining Model Classes for Optional Chaining\)

1 階層以上の深さのプロパティ、メソッド、および subscript の呼び出しにもオプショナルチェーンを使用できます。これにより、複雑なモデル内の関連する型のサブプロパティまで掘り下げて、それらのプロパティ、メソッド、および subscript にアクセスできるかどうかを確認できます。

下記のコードスニペットでは、複数階層のオプショナルチェーンの例を含む、後々のいくつかの例で使用する 4 つのモデルクラスを定義します。これらのクラスは、関連するプロパティ、メソッド、および subscript とともに `Room` および `Address` クラスを追加することで、上記の `Person` と `Residence` モデルを拡張します。

`Person` クラスは、以前と同じ方法で定義されています:

```swift
class Person {
    var residence: Residence?
}
```

`Residence` クラスは以前よりも複雑になっています。今回、`Residence` クラスは、`[Room]` 型の空の配列で初期化される `rooms` と呼ばれる変数プロパティを定義します:

```swift
class Residence {
    var rooms = [Room]()
    var numberOfRooms: Int {
        return rooms.count
    }
    subscript(i: Int) -> Room {
        get {
            return rooms[i]
        }
        set {
            rooms[i] = newValue
        }
    }
    func printNumberOfRooms() {
        print("The number of rooms is \(numberOfRooms)")
    }
    var address: Address?
}
```

このバージョンの `Residence` は `Room` インスタンスの配列を保存するため、その `numberOfRooms` プロパティは、格納プロパティではなく、計算プロパティとして実装されています。`numberOfRooms` 計算プロパティは、単に `rooms` 配列の `count` プロパティの値を返します。

`rooms` 配列にアクセスするためのショートカットとして、このバージョンの `Residence` は、`rooms` 配列内の要求されたインデックスにある部屋へのアクセスを提供する読み書き可能な subscript を提供します。

このバージョンの `Residence` は、`printNumberOfRooms` と呼ばれるメソッドも提供します。このメソッドは、単に住居の部屋数を出力します。

最後に、`Residence` は、`Address?` 型の `address` というオプショナルのプロパティを定義します。このプロパティの `Address` クラス型は下記で定義されています。

`rooms` 配列に使用される `Room` クラスは、`name` という 1 つのプロパティと、そのプロパティを適切な部屋名に設定するイニシャライザを持つシンプルなクラスです:

```swift
class Room {
    let name: String
    init(name: String) { self.name = name }
}
```

このモデルの最後のクラスは `Address` と呼ばれます。このクラスには、`String?` 型の 3 つのオプショナルプロパティがあります。最初の 2 つのプロパティ `buildingName` と `buildingNumber` は、特定の建物を住所の一部として識別する別の方法です。3 番目のプロパティ `street` は、その住所の通りに名前を付けるために使用されます。

```swift
class Address {
    var buildingName: String?
    var buildingNumber: String?
    var street: String?
    func buildingIdentifier() -> String? {
        if let buildingNumber = buildingNumber, let street = street {
            return "\(buildingNumber) \(street)"
        } else if buildingName != nil {
            return buildingName
        } else {
            return nil
        }
    }
}
```

`Address` クラスはまた、`String?` の戻り値の型を持つ `buildingIdentifier()` というメソッドも提供します。このメソッドは、住所のプロパティをチェックし、`buildingName` に値がある場合は `buildingName` を返し、`street` と `buildingNumber` の両方に値がある場合は 2 つを繋げた値を返し、それ以外は `nil` を返します

## <a id="accessing-properties-through-optional-chaining">オプショナルチェーンを通したプロパティへのアクセス\(Accessing Properties Through Optional Chaining\)</a>

[Optional Chaining as an Alternative to Forced Unwrapping\(強制アンラップの代替としてのオプショナルチェーン\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide-gaido/optional-chaining#optional-chaining-as-an-alternative-to-forced-unwrapping)で示されているように、オプショナルチェーンを使用してオプショナルのプロパティにアクセスし、アクセスが成功したかどうかを確認できます。

上で定義したクラスを使用して新しい `Person` インスタンスを作成し、以前と同じように `numberOfRooms` プロパティにアクセスしてみます:

```swift
let john = Person()
if let roomCount = john.residence?.numberOfRooms {
    print("John's residence has \(roomCount) room(s).")
} else {
    print("Unable to retrieve the number of rooms.")
}
// "Unable to retrieve the number of rooms."
```

`john.residence` が `nil` のため、このオプショナルチェーンは以前と同じように失敗します。

オプショナルチェーンを通じてプロパティの値を設定することもできます:

```swift
let someAddress = Address()
someAddress.buildingNumber = "29"
someAddress.street = "Acacia Road"
john.residence?.address = someAddress
```

この例では、`john.residence` が現在 `nil` のため、`john.residence` の `address` のプロパティの設定に失敗します。

値の割り当てはオプショナルチェーンの一部です。つまり、`=` 演算子の右側のコードはどれも評価されません。前の例では、定数へのアクセスには副作用がないため、`someAddress` が評価されないことを確認するのは簡単ではありません。下記のリストは同じ割り当てを行いますが、関数を使用して住所を作成します。この関数は、値を返す前に `"Function was called."` と出力します。これにより、`=` 演算子の右側が評価されたかどうかを確認できます:

```swift
func createAddress() -> Address {
    print("Function was called.")

    let someAddress = Address()
    someAddress.buildingNumber = "29"
    someAddress.street = "Acacia Road"

    return someAddress
}
john.residence?.address = createAddress()
```

何も出力されないため、`createAddress()` 関数が呼び出されていないことがわかります。

## <a id="calling-methods-through-optional-chaining">オプショナルチェーンを通したメソッドの呼び出し\(Calling Methods Through Optional Chaining\)</a>

オプショナルチェーンを使用して、オプショナルの値でメソッドを呼び出し、そのメソッドの呼び出しが成功したかどうかを確認できます。そのメソッドが戻り値を定義していなくても、これを行うことができます。

`Residence` クラスの `printNumberOfRooms()` メソッドは、`numberOfRooms` の現在の値を出力します。メソッドの外観は次のとおりです:

```swift
func printNumberOfRooms() {
    print("The number of rooms is \(numberOfRooms)")
}
```

このメソッドは戻り値の型を指定しません。ただし、[Functions Without Return Values\(戻り値のない関数\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide-gaido/functions#functions-without-return-values)で説明されているように、戻り値の型のない関数とメソッドには、暗黙的な戻り値の型 `Void` があります。これは、`()` の値、または空のタプルを返すことを意味します。

オプショナルチェーンを使用してオプショナルの値でこのメソッドを呼び出す場合、メソッドの戻り値の型は `Void` ではなく `Void?` になります。これにより、メソッド自体が戻り値を定義していなくても、`if` 文を使用して、`printNumberOfRooms()` メソッドを呼び出すことができたかどうかを確認できます。`printNumberOfRooms` の呼び出しからの戻り値を `nil` と比較して、メソッド呼び出しが成功したかどうかを確認します:

```swift
if john.residence?.printNumberOfRooms() != nil {
    print("It was possible to print the number of rooms.")
} else {
    print("It was not possible to print the number of rooms.")
}
// "It was not possible to print the number of rooms."
```

オプショナルチェーンによってプロパティを設定しようとする場合も同様です。[Accessing Properties Through Optional Chaining\(オプショナルチェーンを通したプロパティへのアクセス\)](optional-chaining.md#accessing-properties-through-optional-chaining)の上記の例では、`residence` プロパティが `nil` にもかかわらず、`john.residence` のアドレス値を設定しようとしています。オプショナルチェーンによってプロパティを設定しようとすると、`Void?` 型の値が返されます。これにより、`nil` と比較して、プロパティが正常に設定されたかどうかを確認できます:

```swift
if (john.residence?.address = someAddress) != nil {
    print("It was possible to set the address.")
} else {
    print("It was not possible to set the address.")
}
// "It was not possible to set the address."
```

## オプショナルチェーンを通したsubscriptへのアクセス\(Accessing Subscripts Through Optional Chaining\)

オプショナルチェーンを使用して、subscript の値を get/set し、その subscript の呼び出しが成功したかどうかを確認できます。

> NOTE  
> オプショナルチェーンを通じてオプショナルの subscript にアクセスする場合、疑問符は subscript の大括弧の後\(`]`\)ではなく_前_\(`[`\)に置きます。オプショナルチェーンの場合、疑問符\(`?`\)は、常に式のオプショナル部分の直後に続きます。

下記の例では、`Residence` クラスで定義された subscript を使用して、`john.residence` プロパティの `rooms` 配列内の最初の部屋の名前を取得しようとしています。`john.residence` は現在 `nil` のため、subscript の呼び出しは失敗します。

```swift
if let firstRoomName = john.residence?[0].name {
    print("The first room name is \(firstRoomName).")
} else {
    print("Unable to retrieve the first room name.")
}
// "Unable to retrieve the first room name."
```

`john.residence` がオプショナルの値のため、疑問符\(`?`\)は、`john.residence` の直後、subscript の括弧\(`[`\)の_前に_置かれます。

同様に、オプショナルチェーンを使用して、subscript を介して新しい値の設定を試みることができます:

```swift
john.residence?[0] = Room(name: "Bathroom")
```

この subscript 設定の試みも現在、`residence` は `nil` のため失敗します。

`john.residence` に `Residence` インスタンスを作成して割り当て、その `rooms` 配列に 1 つ以上の `Room` インスタンスを設定すると、`Residence` の subscript を使用して、オプショナルチェーンによって `rooms` 配列内の項目にアクセスできます:

```swift
let johnsHouse = Residence()
johnsHouse.rooms.append(Room(name: "Living Room"))
johnsHouse.rooms.append(Room(name: "Kitchen"))
john.residence = johnsHouse

if let firstRoomName = john.residence?[0].name {
    print("The first room name is \(firstRoomName).")
} else {
    print("Unable to retrieve the first room name.")
}
// "The first room name is Living Room."
```

### <a id="accessing-subscripts-of-optional-type">オプショナル型のsubscriptへのアクセス\(Accessing Subscripts of Optional Type\)</a>

subscript がオプショナル型の値 \(Swift の `Dictionary` 型のキーの subscript など\) を返す場合、そのオプショナルの戻り値をチェーンさせるために、subscript の閉じ括弧\(`]`\)の_後に_疑問符\(`?`\)を置きます:

```swift
var testScores = ["Dave": [86, 82, 84], "Bev": [79, 94, 81]]
testScores["Dave"]?[0] = 91
testScores["Bev"]?[0] += 1
testScores["Brian"]?[0] = 72
// "Dave" 配列 は [91, 82, 84] で "Bev" 配列 は [80, 94, 81]
```

上記の例では、`String` 型のキーを `Int` 型の配列にマップする `testScores` という辞書を定義しています。この例では、オプショナルチェーンを使用して、`"Dave"` 配列の最初の項目を `91` に設定し、`"Bev"` 配列の最初の項目を `1` ずつ増やし、`"Brian"` キーの配列の最初のアイテムを設定しようとします。`testScores` 辞書には `"Dave"` と `"Bev"` のキーが含まれているため、最初の 2 つの呼び出しは成功します。しかし、`testScores` 辞書に `"Brian"` のキーが含まれていないため、3 番目の呼び出しは失敗します。

## <a id="linking-multiple-levels-of-chaining">複数階層のチェーンへのリンク\(Linking Multiple Levels of Chaining\)</a>

オプショナルチェーンの複数の階層を一気にリンクして、モデル内のより深いプロパティ、メソッド、および subscript まで掘り下げることができます。ただし、複数階層のオプショナルチェーンは、戻り値に階層分のオプショナルを追加しません。

別の言い方をすると:

* 取得しようとしている型がオプショナルではない場合、オプショナルチェーンによりオプショナルになります
* 取得しようとしている型がすでにオプショナルの場合、チェーンしてもそれ以上のオプショナルにはなりません

したがって:

* オプショナルチェーンを通じて `Int` の値を取得しようとすると、チェーンの階層数に関係なく、常に `Int?` が返されます
* 同様に、オプショナルチェーンを通じて `Int?` の値を取得しようとすると、チェーンの階層数に関係なく、常に `Int?` が返されます

下記の例では、`john` の `residence` プロパティの `address` プロパティの `street` プロパティにアクセスしようとしています。ここでは、`residence` と `address` のプロパティに 2 階層のオプショナルチェーンを使用しています。どちらもオプショナルの型です:

```swift
if let johnsStreet = john.residence?.address?.street {
    print("John's street name is \(johnsStreet).")
} else {
    print("Unable to retrieve the address.")
}
// "Unable to retrieve the address."
```

`john.residence` の値には現在 `Residence` インスタンスが含まれています。ただし、`john.residence.address` の値は現在 `nil` です。このため、`john.residence?.address?.street` への呼び出しは失敗します。

上記の例では、`street` プロパティの値を取得しようとしていることに注目してください。このプロパティの型は `String?` です。したがって、2 階層分のオプショナルチェーンが適用されていますが、`john.residence?.address?.street` の戻り値も `String?` です。

`john.residence.address` の値として `Address` インスタンスを設定し、住所の `street` プロパティに値を設定すると、複数階層のオプショナルチェーンを通じて `street` プロパティの値にアクセスできます:

```swift
let johnsAddress = Address()
johnsAddress.buildingName = "The Larches"
johnsAddress.street = "Laurel Street"
john.residence?.address = johnsAddress

if let johnsStreet = john.residence?.address?.street {
    print("John's street name is \(johnsStreet).")
} else {
    print("Unable to retrieve the address.")
}
// "John's street name is Laurel Street."
```

この例では、`john.residence` の値に現在 `Residence` インスタンスが含まれているため、`john.residence` の `address` プロパティを設定する試みは成功します。

## オプショナルの戻り値を持つメソッドのチェーン\(Chaining on Methods with Optional Return Values\)

前の例は、オプショナルチェーンを通じてオプショナルの型のプロパティの値を取得する方法を示しています。また、オプショナルチェーンを使用して、オプショナル型の値を返すメソッドを呼び出し、必要に応じてそのメソッドの戻り値をチェーンすることもできます。

下記の例では、オプショナルチェーンを通じて `Address` クラスの `buildingIdentifier()` メソッドを呼び出します。このメソッドは、`String?` 型の値を返します。上で説明したように、オプショナルチェーン後のこのメソッド呼び出しの最終的な戻り値の型も `String?` です。

```swift
if let buildingIdentifier = john.residence?.address?.buildingIdentifier() {
    print("John's building identifier is \(buildingIdentifier).")
}
// "John's building identifier is The Larches."
```

このメソッドの戻り値に対してさらにオプショナルチェーンを実行する場合は、メソッドの括弧\(`)`\)の_後に_疑問符\(`?`\)を配置します。

```swift
if let beginsWithThe =
    john.residence?.address?.buildingIdentifier()?.hasPrefix("The") {
    if beginsWithThe {
        print("John's building identifier begins with \"The\".")
    } else {
        print("John's building identifier doesn't begin with \"The\".")
    }
}
// "John's building identifier begins with "The"."
```

> NOTE  
> 上記の例では、チェーンするオプショナルの値が `buildingIdentifier()` メソッド自体ではなく、`buildingIdentifier()` メソッドの戻り値のため、括弧の_後に_疑問符を配置します。

