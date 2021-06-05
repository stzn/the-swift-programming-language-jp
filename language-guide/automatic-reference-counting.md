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

上記の例では、ARC は、作成した新しい `Person` インスタンスへの参照の数を追跡し、不要になったらその `Person` インスタンスの割り当てを解除できます。

ただし、クラスインスタンスの強参照がゼロにならないコードを書いてしまう可能性があります。これは、2 つのクラスインスタンスが互いに強参照を保持している場合に発生する可能性があります。これは、強参照循環(*Strong Reference Cycles*)と呼ばれています。

クラス間の関係の一部を強参照ではなく、弱参照または非所有参照として定義することにより、強参照循環を解決します。このプロセスは、[Resolving Strong Reference Cycles Between Class Instances](#resolving-strong-reference-cycles-between-class-instancesクラスインスタンス間の強参照循環の解消)で説明されています。ただし、強参照循環を解決する方法を学ぶ前に、そのようなサイクルがどのように発生するかを理解しておくと役に立ちます。

強参照循環が偶発的に発生する例を次に示します。この例では、`Person` と `Apartment` という 2 つのクラスを定義しており、アパートのブロックとその住人をモデル化しています。

```swift
class Person {
    let name: String
    init(name: String) { self.name = name }
    var apartment: Apartment?
    deinit { print("\(name) is being deinitialized") }
}

class Apartment {
    let unit: String
    init(unit: String) { self.unit = unit }
    var tenant: Person?
    deinit { print("Apartment \(unit) is being deinitialized") }
}
```

全ての `Person` インスタンスには、`String` 型の `name` プロパティと、最初は `nil` のオプショナルの `apartment` プロパティがあります。人は常にアパートを所有しているとは限らないため、`apartment` のプロパティはオプショナルです。

同様に、全ての `Apartment` インスタンスには `String` 型の `unit` プロパティがあり、最初は `nil` のオプショナルの `tenant` プロパティがあります。アパートには常にテナントがいるとは限らないため、`tenant` プロパティはオプショナルです。

これらのクラスは両方とも、そのクラスのインスタンスがメモリから割り当て解除されているという事実を出力する `deinit` も定義しています。これにより、`Person` と `Apartment` のインスタンスが期待どおりに割り当て解除されているかどうかを確認できます。

この次のコードスニペットは、`john` と `unit4A` と呼ばれるオプショナルの型の 2 つの変数を定義してます。これらは、下記の特定の `Apartment` および `Person` インスタンスに設定されています。これらの変数は両方とも、オプショナルなため、初期値は `nil` です:

```swift
var john: Person?
var unit4A: Apartment?
```

特定の `Person` インスタンスと `Apartment` インスタンスを作成し、これらの新しいインスタンスを `john` 変数と `unit4A` 変数に割り当てることができます:

```swift
john = Person(name: "John Appleseed")
unit4A = Apartment(unit: "4A")
```

これら 2 つのインスタンスを作成して割り当てた後、強参照がどのように見えるかを次に示します。`john` 変数には新しい `Person` インスタンスへの強参照があり、`unit4A` 変数には新しい `Apartment` インスタンスへの強参照があります:

![強参照循環1](./../.gitbook/assets/referenceCycle01_2x.png)

2 つのインスタンスをリンクして、その人にアパートを持ち、そのアパートにテナントがあるようにすることができます。感嘆符(`!`)は、オプショナルの `john` および `unit4A` 変数内に格納されているインスタンスをアンラップしてアクセスするために使用されるため、これらのインスタンスのプロパティを設定できます:

```swift
john!.apartment = unit4A
unit4A!.tenant = john
```

2 つのインスタンスをリンクした後、強参照がどのように見えるかを次に示します。

![強参照循環2](./../.gitbook/assets/referenceCycle02_2x.png)

残念ながら、これら 2 つのインスタンスをリンクすると、それらの間に強参照循環が作成されます。`Person` インスタンスは `Apartment` インスタンスへの強参照を持ち、`Apartment` インスタンスは `Person` インスタンスへの強参照を持つようになりました。したがって、`john` 変数と `unit4A` 変数によって保持されている強参照を解除しても、参照カウントはゼロにはならず、インスタンスは ARC によって割り当て解除されません。

```swift
john = nil
unit4A = nil
```

これら 2 つの変数を `nil` に設定したときには、どちらのデイニシャライザも呼び出されなかったことに注目してください。強参照循環により、`Person` インスタンスと `Apartment` インスタンスの割り当てが解除されることがなくなり、アプリでメモリリークが発生します。

`john` 変数と `unit4A` 変数を `nil` に設定した後、強参照がどのように見えるかを次に示します。

![強参照循環3](./../.gitbook/assets/referenceCycle03_2x.png)

`Person` インスタンスと `Apartment` インスタンス間の強参照は残り、なくなることはありません。

## Resolving Strong Reference Cycles Between Class Instances(クラスインスタンス間の強参照循環の解消)

Swift は、クラス型のプロパティを操作するときに強参照循環を解決する 2 つの方法を提供します。弱参照と非所有参照です。

弱参照と非所有参照により、参照循環の 1 つのインスタンスは、強参照を維持することなく、他のインスタンスを参照できます。その後、インスタンスは強参照循環を作成せずに相互に参照できます。

他のインスタンスの有効期間が短い場合、つまり、他のインスタンスの割り当てを最初に解除できる場合は、弱参照を使用します。上記の `Apartment` の例では、アパートがその存続期間のある時点でテナントを持たないことが適切なため、この場合、弱参照が参照循環を断ち切る適切な方法です。対照的に、他のインスタンスの存続期間が同じか、それよりも長い場合は、非所有参照を使用します。

### Weak References(弱参照)

弱参照(*weak reference*)は、参照するインスタンスを強く保持しない参照で、ARC に参照されたインスタンスが破棄されません。この動作により、参照が強参照循環の一部になるのを防ぎます。プロパティまたは変数宣言の前に `weak` キーワードを配置することにより、弱参照を示します。

弱参照は参照するインスタンスを強く保持しないため、弱参照が参照している間にそのインスタンスの割り当てが解除される可能性があります。したがって、ARC は、参照するインスタンスの割り当てが解除されると、自動的に弱参照を `nil` に設定します。また、弱参照は実行時に値を `nil` に変更できるようにする必要があるため、常にオプショナル型の定数ではなく変数として宣言されます。

他のオプショナルの値と同様に、弱参照内の値の存在を確認できます。存在しない無効なインスタンスへの参照で終わることはありません。

> NOTE  
> ARC が弱参照を `nil` に設定した場合、プロパティオブザーバは呼び出されません。

下記の例は、上記の `Person` と `Apartment` の例と同じですが、1 つの重要な違いがあります。今回は、`Apartment` 型の `tenant` プロパティが弱参照として宣言されています:

```swift
class Person {
    let name: String
    init(name: String) { self.name = name }
    var apartment: Apartment?
    deinit { print("\(name) is being deinitialized") }
}

class Apartment {
    let unit: String
    init(unit: String) { self.unit = unit }
    weak var tenant: Person?
    deinit { print("Apartment \(unit) is being deinitialized") }
}
```

2 つの変数(`john` と `unit4A`)からの強参照と、2 つのインスタンス間のリンクは、以前と同じように作成されます。

```swift
var john: Person?
var unit4A: Apartment?

john = Person(name: "John Appleseed")
unit4A = Apartment(unit: "4A")

john!.apartment = unit4A
unit4A!.tenant = john
```

2 つのインスタンスをリンクしたので、参照は次のようになります:

![弱参照1](./../.gitbook/assets/weakReference01_2x.png)

`Person` インスタンスは依然として `Apartment` インスタンスへの強参照を持っていますが、`Apartment` インスタンスは `Person` インスタンスへの弱参照を持っています。これは、`john` 変数を `nil` に設定してその強参照を解除すると、`Person` インスタンスへの強参照がなくなることを意味します。

```swift
john = nil
// "John Appleseed is being deinitialized"
```

`Person` インスタンスへの強参照がなくなったため、割り当てが解除され、`tenant` プロパティが `nil` に設定されます:

![弱参照2](./../.gitbook/assets/weakReference02_2x.png)

`Apartment` インスタンスへの唯一の強参照は、`unit4A` 変数からです。その強参照がなくなると、`Apartment` インスタンスへの強参照はなくなります。

```swift
unit4A = nil
// "Apartment 4A is being deinitialized"
```

`Apartment` インスタンスへの強参照がなくなったため、この割り当ても解除されます:

![弱参照3](./../.gitbook/assets/weakReference03_2x.png)

> NOTE  
> ガベージ コレクションを使用するシステムでは、メモリプレッシャーによってガベージコレクションがトリガーされた場合にのみ、
> 強参照を持たないオブジェクトの割り当てが解除されるため、シンプルなキャッシュメカニズムを実装するために弱いポインタが使用されることがあります。
> ただし、ARC では、最後の強参照が削除されるとすぐに値の割り当てが解除されるため、弱参照はそのような目的には適していません。

### Unowned References(非所有参照)

### Unowned Optional References(非所有オプショナル参照)

### Unowned References and Implicitly Unwrapped Optional Properties(非所有参照と暗黙的にアンラップしたオプショナルプロパティ)

## Strong Reference Cycles for Closures(クロージャの強参照循環)

## Resolving Strong Reference Cycles for Closures(クロージャの強参照循環の解消)

## Defining a Capture List(キャプチャリストの定義)

## Weak and Unowned References(弱参照と非所有参照)
