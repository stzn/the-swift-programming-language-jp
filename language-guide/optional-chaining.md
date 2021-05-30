# Optional Chaining(オプショナルの連鎖)

最終更新日:

オプショナルの連鎖(*optional chaining*)は、現在 `nil` な可能性のある optional のプロパティ、メソッド、および subscript を照会して呼び出すためのプロセスです。optional に値が含まれている場合、プロパティ、メソッド、または subscript の呼び出しは成功します。optional が `nil` の場合、プロパティ、メソッド、または subscript の呼び出しは `nil` を返します。複数のクエリを一緒に連鎖させることができ、連鎖内のリンクが `nil` の場合、連鎖全体が失敗します。

> NOTE  
> Swift のオプショナルの連鎖は、Objective-C の `nil` のメッセージングに似ていますが、どの型にでも機能し、成功または失敗をチェックできます。

## Optional Chaining as an Alternative to Forced Unwrapping(強制アンラップの代替としてのオプショナル連鎖)

optional が `nil` 以外の場合、プロパティ、メソッド、または subscript を呼び出す optional の値の後に疑問符(`?`)を配置して、optional の連鎖を指定します。これは、optional の値の後に感嘆符(`!`)を配置して、その値を強制アンラップするのとよく似ています。主な違いは、optional が `nil` の場合、optional の連鎖が失敗するのに対し、強制アンラップは実行時エラーを引き起こすことです。

optional の連鎖が `nil` 値で呼び出される可能性があるということを反映するために、クエリしているプロパティ、メソッド、または subscript が非 optional の値を返した場合でも、optional の連鎖呼び出しの結果は常に optional になります。この optional の戻り値を使用して、optional の連鎖が成功したか(返された optional に値が含まれているか)、連鎖内の `nil` で失敗したか(返された optional が `nil` か)を確認できます。

具体的には、optional の連鎖を介してアクセスされた場合、optional の連鎖の結果は、期待される戻り値と同じ型ですが、optional でラップされています。通常は `Int` を返すプロパティは `Int?` を返します。

次のいくつかのコードスニペットは、optional の連鎖が強制アンラップとどのように異なるかを示しており、成功したかどうかを確認できます。

まず、`Person` と `Residence` という 2 つのクラスが定義されます:

```swift
class Person {
    var residence: Residence?
}

class Residence {
    var numberOfRooms = 1
}
```

`Residence` インスタンスには、`numberOfRooms` という単一の `Int` プロパティがあり、デフォルト値は `1` です。`Person` インスタンスには、`Residence?` 型の `residence` プロパティがあります。

新しい `Person` インスタンスを作成する場合、その `residence` プロパティは optional なため、デフォルトで `nil` で初期化されます。下記のコードでは、`john` の `residence` プロパティは `nil` です。

```swift
let john = Person()
```

この人の `residence` の `numberOfRooms` プロパティにアクセスしようとすると、`residence` の後に感嘆符(`!`)を置いてその値を強制アンラップすると、`residence` に値がないため、実行時エラーが発生します。

```swift
let roomCount = john.residence!.numberOfRooms
// 実行時エラーが起きます
```

上記のコードは、`john.residence` の値が `nil` 以外の場合に成功し、`roomCount` に適切な部屋数を含む `Int` 値に設定します。ただし、上に示したように、このコードは、`residence` が `nil` の場合、常に実行時エラーを引き起こします。

optional の連鎖は、`numberOfRooms` の値にアクセスする別の方法を提供します。 optional の連鎖を使用するには、感嘆符(`!`)の代わりに疑問符(`?`)を使用します:

```swift
if let roomCount = john.residence?.numberOfRooms {
    print("John's residence has \(roomCount) room(s).")
} else {
    print("Unable to retrieve the number of rooms.")
}
// "Unable to retrieve the number of rooms."
```

これは、optional の `residence` プロパティを「連鎖」し、`residence` が存在する場合は `numberOfRooms` の値を取得するように Swift に指示します。

`numberOfRooms` へのアクセスは失敗する可能性があるため、optional の連鎖は `Int?` 型または"optional `Int`"の値を返します。上記の例のように、`residence` が `nil` の場合、`numberOfRooms` にアクセスできなかったことを反映するために、この optional の `Int` も `nil` になります。optional の `Int` は、整数をアンラップし、optional バインディングを介して optional ではない値を `roomCount` 定数に割り当てます。

`numberOfRooms` が optional ではない `Int` の場合でも、同じ挙動なことに注意してください。optional の連鎖を介してクエリが実行されるということは、`numberOfRooms` の呼び出しは常に `Int` の代わりに `Int?` を返します。

`Residence` インスタンスを `john.residence` に割り当てて、`nil` 値を持たないようにすることができます。

```swift
john.residence = Residence()
```

`john.residence` には、`nil` ではなく実際の `Residence` インスタンスが含まれるようになりました。以前と同じ optional の連鎖を使用して `numberOfRooms` にアクセスすると、デフォルトの `numberOfRooms` 値 `1` を含んだ `Int?` が返されます。

```swift
if let roomCount = john.residence?.numberOfRooms {
    print("John's residence has \(roomCount) room(s).")
} else {
    print("Unable to retrieve the number of rooms.")
}
// "John's residence has 1 room(s)."
```

## Defining Model Classes for Optional Chaining(オプショナル連鎖モデルのクラスの定義)

## Accessing Properties Through Optional Chaining(オプショナル連鎖を通したプロパティへのアクセス)

## Calling Methods Through Optional Chaining(オプショナル連鎖を通したメソッドの呼び出し)

## Accessing Subscripts Through Optional Chaining(オプショナル連鎖を通したsubscriptへのアクセス)

### Accessing Subscripts of Optional Type(optional型のsubscriptへのアクセス)

## Linking Multiple Levels of Chaining(複数レベルの連鎖のリンク)

## Chaining on Methods with Optional Return Values(optionalの戻り値を持つメソッドの連鎖)