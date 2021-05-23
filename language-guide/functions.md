# Functions

最終更新日:

関数(*Functions*)は、特定のタスクを実行する独立したコードの塊です。何をするものなのかを特定するために名前を与え、必要な時にタスクを実行するためにこの関数が呼び出される時にこの名前が使われます。

Swift の統一された関数シンタックは、シンプルな C 言語スタイルの引数名のない関数から Objective-C スタイルの個々の引数に名前とラベルを付けた関数までを表現するに十分な柔軟性を持っています。関数の呼び出しをシンプルにするために、引数にデフォルト値を提供できたり、関数の実行が完了すると引数として渡した値が変更する `in-out` 引数も渡すことができます。

全ての関数は、引数や戻り値の型で構成された自身の型を持っています。他の Swift の型と同じようにこの型を使用することができます。このことによって他の関数の引数に関数を渡したり、関数から関数を戻すことも簡単にできます。また、関数の中で、ネストした関数を作成することで、独自のスコープを作成して機能をカプセル化することもできます。

## Defining and Calling Functions

関数を定義する時、選択的に名前と型を持つ、パラメータと呼ばれる 1 つ以上のインプットを定義することができます。また、関数の完了後に呼び出し側に戻す、戻り値と呼ばれるアウトプットの値の型も選択的に定義できます。

全ての関数は、関数が実行するタスクを説明する、関数名(*function name*)を持っています。関数を使うには、関数名と(引数と呼ばれる)インプット値と一緒に呼びます。引数には関数の型に合致した値を渡します。関数の引数は、関数の引数のリストと同じ順序で常に提供しなければなりません。

下記の例の関数は、`greet(person:)` という関数で、名前が示す通り、人の名前を引数に受け取り、その人への挨拶を返します。この関数では、`person` という `String` 型の引数と、その人に対する挨拶を含めた `String` 型の戻り値をします。

```swift
func greet(person: String) -> String {
    let greeting = "Hello, " + person + "!"
    return greeting
}
```

この情報はすべて、関数の定義にまとめられ、関数の定義の前に `func` キーワードが付けられます。関数の戻り値の型は、戻り矢印 `->(ハイフンの後に直角括弧が続く)` で示し、その後に返す型名が続きます。

定義は、関数が何をするか、何を受け取ることを期待するか、そしてそれが行われたときに何を返すかを説明します。 この定義により、コード内の他の場所から関数を誤解を生むことなく呼び出すことができます。

```swift
print(greet(person: "Anna"))
// Prints "Hello, Anna!"
print(greet(person: "Brian"))
// Prints "Hello, Brian!"
```

`greet（person:)` 関数を呼び出すには、`greet（person: "Anna"）` のように、`person` 引数ラベルの後に `String` 型の値を渡します。 この関数は `String` 型の値を返すため、上記のように、`greet（person:)` を `print（_:separator:terminator:)` 関数の呼び出しでラップして、その文字列を出力し、その戻り値を確認できます。

> NOTE  
> `print（_:separator:terminator:)` 関数には最初の引数のラベルがなく、他の引数にはデフォルト値があるため引数を渡しても渡さなくても呼び出し可能です。関数のシンタックスの種類関しては[Function Argument Labels and Parameter Names](#function-argument-labels-and-parameter-names)と[Default Parameter Values](#default-parameter-values)に記載しています。

`greet(person:)` 関数の本文は、`greeting` と呼ばれる新しい `String` 型の定数を定義し、それを単純な挨拶メッセージに設定することから始まります。 このあいさつは、`return` キーワードを使用して関数から返されます。 `return greeting` という行で、関数は実行を終了し、`greeting` の現在の値を返します。

この関数の本文を短くするために、メッセージの作成と `return` 文を 1 行にまとめることができます。

```swift
func greetAgain(person: String) -> String {
    return "Hello again, " + person + "!"
}
print(greetAgain(person: "Anna"))
// Prints "Hello again, Anna!"
```

## Function Parameters and Return Values

Swift では、関数の引数と戻り値は非常に柔軟です。 名前のない単一のパラメーターを持つ単純な効用関数から、表現力豊かな引数名とさまざまな引数オプションを持つ複雑な関数まで、あらゆるものを定義できます。

### Functions Without Parameters

入力引数は関数に必須ではありません。 入力引数のない関数は次のとおりです。この関数は、呼び出されるたびに常に同じ `String` 型のメッセージを返します:

```swift
func sayHelloWorld() -> String {
    return "hello, world"
}
print(sayHelloWorld())
// Prints "hello, world"
```

関数は、引数を受け取りませんが、関数名の後に括弧が必要です。関数が呼び出されると、関数名の後に空の括弧のペア(`()`)も書きます。

### Functions With Multiple Parameters

関数は、カンマ(`,`)区切りで複数の引数を持つことができます。

この関数は、人の名前と、その人がすでに挨拶されているかどうかを引数として受け取り、その人に適切な挨拶を返します:

```swift
func greet(person: String, alreadyGreeted: Bool) -> String {
    if alreadyGreeted {
        return greetAgain(person: person)
    } else {
        return greet(person: person)
    }
}
print(greet(person: "Tim", alreadyGreeted: true))
// Prints "Hello again, Tim!"
```

`person` というラベルの付いた `String` 型の値と、括弧(`()`) 内にカンマ(`,`)で区切られた `alreadyGreeted` というラベルの付いた `Bool` 型の値の両方を渡して `great（person:alreadyGreeted:)` 関数を呼び出しています。この関数は、
前のセクションで示した `greet(person:)` 関数とは異なることに注意してください。 どちらも `greet` という関数名ですが、`greet(person:alreadyGreeted:)` は 2 つの引数を取り、`greet(person:)` 関数は 1 つしか取りません。

### Functions Without Return Values

戻り値の型を定義することは必須ではありません。`greet(person:)` バージョンは文字列値を返すのではなく出力します。

```swift
func greet(person: String) {
    print("Hello, \(person)!")
}
greet(person: "Dave")
// Prints "Hello, Dave!"
```

値を返す必要がないため、関数の定義には戻り値を示す矢印(`->`)や戻り値の型は含まれていません。

> NOTE  
> 厳密に言えば、`greet(person:)` は、戻り値が定義されていなくても、値を返してます。 戻り型が定義されていない関数は、`Void` 型の特別な値を返します。 これは単に `（）` と書く空のタプルです。

関数の戻り値は、呼び出されたときに無視できます:

```swift
func printAndCount(string: String) -> Int {
    print(string)
    return string.count
}
func printWithoutCounting(string: String) {
    let _ = printAndCount(string: string)
}
printAndCount(string: "hello, world")
// prints "hello, world" and returns a value of 12
printWithoutCounting(string: "hello, world")
// prints "hello, world" but doesn't return a value
```

最初の関数 `printAndCount(string:)` は文字列を出力し、その文字数を `Int` として返します。 2 番目の関数 `printWithoutCounting(string:)` は、最初の関数を呼び出しますが、その戻り値を無視します。 2 番目の関数が呼び出されても、メッセージは最初の関数によって出力されますが、戻り値は使われません。

> NOTE  
> 戻り値は無視できますが、常に関数が値を返すことは示さなければなりません。 戻り型が定義されている関数では、値を返さずに関数を使うことはできません。そうしようとすると、コンパイルエラーが発生します。

### Functions with Multiple Return Values

タプル型を関数の戻り型として使用して、1 つの複合戻り値として複数の値を返すことができます。

以下の例では、`minMax(array:)` という関数を定義しています。この関数は、`Int` 値の配列内の最小数と最大数を検索します。

```swift
func minMax(array: [Int]) -> (min: Int, max: Int) {
    var currentMin = array[0]
    var currentMax = array[0]
    for value in array[1..<array.count] {
        if value < currentMin {
            currentMin = value
        } else if value > currentMax {
            currentMax = value
        }
    }
    return (currentMin, currentMax)
}
```

`minMax(array:)` 関数は、2 つの `Int` 値を含むタプルを返します。 これらの値には `min` と `max` のラベルが付いているため、関数の戻り値を使うときに名前でアクセスできます。

`minMax(array:)` 関数の本体は、`currentMin` および `currentMax` と呼ばれる 2 つの変数を、配列の最初の整数値に設定することから始まります。 次に、この関数は配列内の残りの値を繰り返し処理し、各値をチェックして、それぞれ `currentMin` と `currentMax` の値よりも小さいか大きいかを確認します。 最後に、全体の最小値と最大値が 2 つの `Int` 値のタプルとして返されます。

タプルのそれぞれの値は、関数の戻り型の一部として名前が付けられているため、ドット(`.`)シンタックスでアクセスして、見つかった最小値と最大値を取得できます。

```swift
let bounds = minMax(array: [8, -6, 2, 109, 3, 71])
print("min is \(bounds.min) and max is \(bounds.max)")
// Prints "min is -6 and max is 109"
```

タプルの各値は、関数の戻り値の型で名前がすでに指定されているため、関数からタプルが返される時点で名前を付ける必要がないことに注意してください。

#### Optional Tuple Return Types

---

関数から返されるタプル型が、「値が存在しない」可能性がある場合は、optional のタプル戻り値の型を使用して、タプル全体が `nil` になる可能性があることを示すことができます。`(Int, Int)?` や `(String, Int, Bool)?` のように、タプル型の閉じ括弧(`)`)の後に疑問符(`?`)を配置して、optional のタプルの戻り値の型を記述します。

> NOTE  
> `(Int, Int)?` などの optional のタプル型は `（Int？、Int？）` などの optional の型を含むタプルとは異なります。 optional の型を使用すると、タプル内の個々の値だけでなく、タプル全体が optional になります。

上記の `minMax(array:)` 関数は、2 つの `Int` 値を含むタプルを返します。 ただし、この関数は、渡された配列に対して安全性チェックを行なっていません。 `array` 引数に空の配列が含まれている場合、上記で定義した `minMax(array:)` 関数は、`array [0]` にアクセスしようとしたときに実行時エラーが発生します。

空の配列を安全に処理するには、optional のタプルの戻り値の型を指定して、配列が空の場合は `nil` を返します。

```swift
func minMax(array: [Int]) -> (min: Int, max: Int)? {
    if array.isEmpty { return nil }
    var currentMin = array[0]
    var currentMax = array[0]
    for value in array[1..<array.count] {
        if value < currentMin {
            currentMin = value
        } else if value > currentMax {
            currentMax = value
        }
    }
    return (currentMin, currentMax)
}
```

オプショナルバインディングを使用して、このバージョンの `minMax(array:)` 関数が、実際のタプル値を返すか `nil` を返すかを確認できます:

```swift
if let bounds = minMax(array: [8, -6, 2, 109, 3, 71]) {
    print("min is \(bounds.min) and max is \(bounds.max)")
}
// Prints "min is -6 and max is 109"
```

### Functions With an Implicit Return

関数の本文全体が、単一の式の場合は、関数は暗黙的にその式の結果を返します。例えば、下記の両方の関数の動作は同じです:

```swift
func greeting(for person: String) -> String {
    "Hello, " + person + "!"
}
print(greeting(for: "Dave"))
// Prints "Hello, Dave!"

func anotherGreeting(for person: String) -> String {
    return "Hello, " + person + "!"
}
print(anotherGreeting(for: "Dave"))
// Prints "Hello, Dave!"
```

`greeting(for:)` 関数の定義全体は、戻り値の挨拶文です。つまり、この短い形式を使用できます。`anotherGreeting(for:)` 関数は、より長い式を持つ関数と同様に `return` キーワードを使用して、同じ挨拶文を返しています。 1 つの戻り行として記述した関数は、`return` を省略できます。

[Shorthand Getter Declaration](./properties.md#shorthand-getter-declaration)でも見ますが、プロパティゲッタ(*property getter*)でもは暗黙的な戻り値を使うことができます。

> NOTE  
> 暗黙の戻り値を使ったコードは、何かしらの値を返す必要があります。 例えば、 `fatalError("Oh no!")` または `print（13）` を暗黙の戻り値として使用することはできません。

## Function Argument Labels and Parameter Names

各関数の引数には、引数ラベルと引数名の両方持つことができます。引数ラベルは、関数を呼び出すときに使用されます。各引数は、その前に書かれた引数ラベルを使って関数が呼び出されます。引数名は関数の内部で使用されます。デフォルトでは、引数は引数名を引数ラベルとして使用します。

```swift
func someFunction(firstParameterName: Int, secondParameterName: Int) {
    // 関数の本文では、`firstParameterName`と`secondParameterName`は
    // 最初と 2 番目の引数の値を参照します。
}
someFunction(firstParameterName: 1, secondParameterName: 2)
```

全ての引数には一意の名前を付ける必要があります。複数の引数に同じ引数ラベルを付けることはできますが、一意の引数ラベルを使用すると、コードが読みやすくなります。

### Specifying Argument Labels

### Omitting Argument Labels

### Default Parameter Values

### Variadic Parameters

### In-Out Parameters

## Function Types

### Using Function Types

### Function Types as Parameter Types

### Function Types as Return Types

## Nested Functions
