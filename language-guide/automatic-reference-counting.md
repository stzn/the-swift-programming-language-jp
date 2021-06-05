# Automatic Reference Counting

最終更新日:

Swift は自動参照カウント(以下*ARC*)を使用して、アプリのメモリ使用量を追跡および管理します。ほとんどの場合、これはメモリ管理が Swift が「ただ機能している」ことを意味し、メモリ管理について自分で考える必要はありません。ARC は、クラスインスタンスが不要になったときに、クラスインスタンスによって使用されていたメモリを自動的に解放します。

ただし、場合によっては、ARC がメモリを管理するために、コードの各部分間の関係についてより多くの情報を必要とすることがあります。この章では、これらの状況について説明し、ARC でアプリの全てのメモリを管理できるようにする方法を示します。Swift での ARC の使用は、[Transitioning to ARC Release Notes](https://developer.apple.com/library/archive/releasenotes/ObjectiveC/RN-TransitioningToARC/Introduction/Introduction.html)で説明されているアプローチと非常によく似ています。

参照カウントは、クラスインスタンスにのみ適用されます。構造体と列挙型は、参照型ではなく値型で、参照が格納および渡されることはありません。

## How ARC Works(ARCはどう機能するのか)

クラスの新しいインスタンスを作成するたびに、ARC はそのインスタンスに関する情報を格納するためにメモリの塊を割り当てます。このメモリには、インスタンスの型に関する情報と、そのインスタンスに関連付けられた格納プロパティの値が保持されます。

さらに、インスタンスが不要になった場合、ARC はそのインスタンスが使用していたメモリを解放し、代わりにメモリを他の目的に使用できるようにします。これにより、クラスインスタンスが不要になったときにメモリ内のスペースを占有しないようにします。

ただし、ARC がまだ使用中のインスタンスの割り当てを解除すると、そのインスタンスのプロパティにアクセスすることも、そのインスタンスのメソッドを呼び出すこともできなくなります。実際、インスタンスにアクセスしようとすると、アプリがクラッシュする可能性が高くなります。

インスタンスがまだ必要なときにインスタンスが消えないようにするために、ARC は各クラスインスタンスを現在参照しているプロパティ、定数、変数の数を追跡します。そのインスタンスへのアクティブな参照が少なくとも 1 つ存在する限り、ARC はインスタンスの割り当てを解除しません。

これを可能にするために、クラスインスタンスをプロパティ、定数、または変数に割り当てると、そのプロパティ、定数、または変数はインスタンスへの強参照を作成します。参照は、そのインスタンスをしっかりと保持し、その強参照が残っている限り、割り当てを解除できないため、「強い」参照と呼ばれています。

## ARC in Action(ARCの挙動)

ARC の仕組みの例を次に示します。この例は、`name` という名前の定数格納プロパティを定義する `Person` という単純なクラスから始まります。

```swift
class Person {
    let name: String
    init(name: String) {
        self.name = name
        print("\(name) is being initialized")
    }
    deinit {
        print("\(name) is being deinitialized")
    }
}
```

`Person` クラスには、インスタンスの `name` プロパティを設定し、初期化が進行中なことを示すメッセージを出力するイニシャライザがあります。`Person` クラスには、インスタンスの割り当てが解除されたときにメッセージを出力するデイニシャライザもあります。

次のコードスニペットでは、`Person?` 型の 3 つの変数を定義しています。これらの変数は、後続のコードスニペットで新しい `Person` インスタンスへの複数の参照を設定するために使用されます。これらの変数はオプショナルの型(`Person?`、`Person` ではない)のため、`nil` で自動的に初期化され、現在 `Person` インスタンスを参照していません。

```swift
var reference1: Person?
var reference2: Person?
var reference3: Person?
```

新しい `Person` インスタンスを作成して、次の 3 つの変数のいずれかに割り当てることができます。

```swift
reference1 = Person(name: "John Appleseed")
// "John Appleseed is being initialized"
```

`Person` クラスのイニシャライザを呼び出した時点で、`"John Appleseed is being initialized"` というメッセージが出力されることに注目してください。これは、初期化が行われたことを確認します。

新しい `Person` インスタンスが `reference1` 変数に割り当てられているため、`reference1` から新しい `Person` インスタンスへの強参照があります。少なくとも 1 つの強参照があるため、ARC はこの `Person` がメモリに保持され、割り当てが解除されないようにします。

同じ `Person` インスタンスをさらに 2 つの変数に割り当てると、そのインスタンスへのさらに 2 つの強参照が確立されます。

```swift
reference2 = reference1
reference3 = reference1
```

現在、この単一の `Person` インスタンスへの 3 つの強参照があります。

2 つの変数に `nil` を代入してこれらの 2 つの強参照(元の参照を含む)を解除すると、1 つの強参照が残り、`Person` インスタンスの割り当てが解除されません。

```swift
reference1 = nil
reference2 = nil
```

ARC は、最後の 3 番目の強参照がなくなるまで、`Person` インスタンスの割り当てを解除しません。強参照がなくなった時点で、`Person` インスタンスを使用していないことが明らかになります。

```swift
reference3 = nil
// "John Appleseed is being deinitialized"
```

## Strong Reference Cycles Between Class Instances(クラスインスタンス間の強参照循環)

## Resolving Strong Reference Cycles Between Class Instances(クラスインスタンス間の強参照循環の解消)

### Weak References(弱参照)

### Unowned References(非所有参照)

### Unowned Optional References(非所有オプショナル参照)

### Unowned References and Implicitly Unwrapped Optional Properties(非所有参照と暗黙的にアンラップしたオプショナルプロパティ)

## Strong Reference Cycles for Closures(クロージャの強参照循環)

## Resolving Strong Reference Cycles for Closures(クロージャの強参照循環の解消)

## Defining a Capture List(キャプチャリストの定義)

## Weak and Unowned References(弱参照と非所有参照)
