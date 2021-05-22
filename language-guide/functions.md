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



### Functions Without Parameters

### Functions With Multiple Parameters

### Functions Without Return Values

### Functions with Multiple Return Values

#### Optional Tuple Return Types

### Functions With an Implicit Return

## Function Argument Labels and Parameter Names

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
