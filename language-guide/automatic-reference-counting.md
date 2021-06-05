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

弱参照と同様に、非所有参照は、参照するインスタンスを強く保持しません。ただし、弱参照とは異なり、非所有参照は、他のインスタンスの存続期間が同じか、存続期間が長い場合に使用します。プロパティまたは変数の宣言の前に `unowned` キーワードを配置することにより、非所有参照を示します。弱参照とは異なり、非所有参照は常に値を持つことが期待されます。その結果、値を `unowned` としてマークしてもオプショナルにはなりません。また、ARC は非所有参照の値を nil に設定することはありません。

> NOTE  
> 参照が常に割​​り当て解除されていないインスタンスを参照していることが確実な場合にのみ、非所有参照を使用してください。

インスタンスの割り当てが解除された後に非所有参照の値にアクセスしようとすると、実行時エラーが発生します。
次の例では、2 つのクラス、`Customer` と `CreditCard` を定義しています。これらのクラスは、銀行の顧客と、その顧客のクレジットカードの候補をモデル化しています。これら 2 つのクラスはそれぞれ、もう一方のクラスのインスタンスをプロパティとして保存します。この関係は、強参照循環を作成する可能性があります。

`Customer` と `CreditCard` の関係は、上記の弱参照の例に見られる `Apartment` と `Person` の関係とは少し異なります。このデータモデルでは、顧客はクレジットカードを持っている場合と持っていない場合がありますが、クレジットカードは常に顧客に関連付けられます。`CreditCard` インスタンスは、それが参照する顧客より長生きすることはありません。これを表すために、`Customer` クラスにはオプショナルのカードプロパティがありますが、`CreditCard` クラスには所有されていない(オプショナルではない)顧客プロパティがあります。

さらに、新しい `CreditCard` インスタンスは、数値と顧客インスタンスを独自の `CreditCard` イニシャライザに渡すことによってのみ作成できます。これにより、`CreditCard` インスタンスの作成時に、`CreditCard` インスタンスに常に顧客インスタンスが関連付けられます。

クレジットカードには常に顧客がいるため、強参照循環を避けるために、その顧客プロパティを非所有参照として定義します:

```swift
class Customer {
    let name: String
    var card: CreditCard?
    init(name: String) {
        self.name = name
    }
    deinit { print("\(name) is being deinitialized") }
}

class CreditCard {
    let number: UInt64
    unowned let customer: Customer
    init(number: UInt64, customer: Customer) {
        self.number = number
        self.customer = customer
    }
    deinit { print("Card #\(number) is being deinitialized") }
}
```

> NOTE  
> `CreditCard` クラスの `number` プロパティは、`Int` ではなく `UInt64` の型で定義され、`number` プロパティの容量が 32 ビットと 64 ビットの両方のシステムで 16 桁のカード番号を格納するのに十分な大きさであることを確認します。

この次のコードスニペットは、`john` というオプショナルの `Customer` 変数を定義します。この変数は、特定の顧客への参照を保存するために使用されます。この変数はオプショナルのため、初期値は `nil` です:

```swift
var john: Customer?
```

これで `Customer` インスタンスを作成し、それを使用して新しい `CreditCard` インスタンスを初期化し、その顧客の `card` プロパティとして割り当てることができます:

```swift
john = Customer(name: "John Appleseed")
john!.card = CreditCard(number: 1234_5678_9012_3456, customer: john!)
```

2 つのインスタンスをリンクしたので、参照は次のようになります:

![非所有参照1](./../.gitbook/assets/unownedReference01_2x.png)

`Customer` インスタンスには、`CreditCard` インスタンスへの強参照があり、`CreditCard` インスタンスには、`Customer` インスタンスへの非所有参照があります。

`customer` は非所有参照のため、`john` 変数によって保持されている強参照を解除すると、`Customer` インスタンスへの強参照はなくなります:

![非所有参照2](./../.gitbook/assets/unownedReference02_2x.png)

`Customer` インスタンスへの強参照がなくなったため、割り当てが解除されます。これが発生すると、`CreditCard` インスタンスへの強参照はなくなり、割り当ても解除されます。

```swift
john = nil
// "John Appleseed is being deinitialized"
// "Card #1234567890123456 is being deinitialized"
```

上記の最後のコードスニペットは、`john` 変数が `nil` に設定された後、`Customer` インスタンスと `CreditCard` インスタンスのデイニシャライザプログラムが両方とも"deinitialized"メッセージを出力することを示しています。

> NOTE  
> 上記の例は、安全な非所有参照の使用方法を示しています。Swift は、パフォーマンス上の理由などで、ランタイムの安全性チェックを無効にする必要がある場合に、安全でない非所有参照も提供します。全ての安全でない操作と同様、そのコードの安全性をチェックする責任はあなたにあります。
>
> `unowned(unsafe)` と書くことで、安全でない非所有参照を示します。参照しているインスタンスの割り当てが解除された後で、所有されていない安全でない参照にアクセスしようとすると、プログラムはインスタンスがかつて存在していたメモリアドレスにアクセスしようとしますが、これは安全ではありません。

### Unowned Optional References(オプショナル非所有参照)

クラスへのオプショナルの参照を unowned としてマークできます。ARC 所有権モデルに関しては、オプショナルの非所有参照と弱参照の両方を同じコンテキストで使用できます。違いは、オプショナルの非所有参照を使用する場合、それが常に有効なオブジェクトを参照するのか、`nil` に設定されていることを確認する責任があることです。

学校の特定の学科が提供するコースを追跡する例を次に示します:

```swift
class Department {
    var name: String
    var courses: [Course]
    init(name: String) {
        self.name = name
        self.courses = []
    }
}

class Course {
    var name: String
    unowned var department: Department
    unowned var nextCourse: Course?
    init(name: String, in department: Department) {
        self.name = name
        self.department = department
        self.nextCourse = nil
    }
}
```

`Department` は、学科が提供する各コースへの強参照を維持します。ARC 所有権モデルでは、学科がそのコースを所有します。`Course` には非所有参照が 2 つあります。1 つは学科へ、もう 1 つは学生が受講する必要がある次のコースです。コースはこれらのオブジェクトのいずれも所有しません。全てのコースはいくつかの部門の一部のため、`department` プロパティはオプショナルではありません。ただし、一部のコースには推奨される次のコースがないため、`nextCourse` プロパティはオプショナルです。

これらのクラスの使用例を次に示します:

```swift
let department = Department(name: "Horticulture")

let intro = Course(name: "Survey of Plants", in: department)
let intermediate = Course(name: "Growing Common Herbs", in: department)
let advanced = Course(name: "Caring for Tropical Plants", in: department)

intro.nextCourse = intermediate
intermediate.nextCourse = advanced
department.courses = [intro, intermediate, advanced]
```

上記のコードは、学科とその 3 つのコースを作成します。入門コースと中級コースの両方には、`nextCourse` プロパティに保存された次のコースの提案があり、このコースを完了した後に学生が受講する必要があるコースへのオプショナルの非所有参照が維持されます。

![オプショナル非所有参照](./../.gitbook/assets/unownedOptionalReference_2x.png)

オプショナル非所有参照は、ラップするクラスのインスタンスを強く保持しないため、ARC によるインスタンスの割り当て解除を妨げません。これは、オプショナル非所有参照が `nil` にできることを除いて、ARC での非所有参照と同じように動作します。

オプショナル非所有参照と同様に、`nextCourse` が常に割り当てが解除されていないコースを参照するようにする必要があります。この場合、例えば、`department.courses` からコースを削除するときに、他のコースが持つ可能性のあるそのコースへの参照も全て削除する必要があります。

> NOTE  
> オプショナル値の基になる型は `Optional` です。これは、Swift 標準ライブラリの列挙型です。`Optional` は、値型を `unowned` でマークできないという規則の例外です。
>
> クラスをラップするオプショナルは参照カウントを使用しないため、オプショナルへの強参照を維持する必要はありません。

### Unowned References and Implicitly Unwrapped Optional Properties(非所有参照と暗黙的にアンラップしたオプショナルプロパティ)

上記の弱参照と非所有参照の例は、強参照循環を断ち切る必要がある、より一般的な 2 つのシナリオをカバーしています。

`Person` と `Apartment` の例は、両方とも `nil` にすることができる 2 つのプロパティが、強参照循環を引き起こす可能性がある状況を示しています。このシナリオは、弱参照で解決するのが最善です。

`Customer` と `CreditCard` の例は、`nil` にできる 1 つのプロパティと `nil` にできない別のプロパティが、強参照循環を引き起こす可能性がある状況を示しています。このシナリオは、非所有参照で解決するのが最善です。

ただし、3 番目のシナリオでは、両方のプロパティに常に値が必要で、初期化が完了すると、どちらのプロパティも `nil` にはできません。このシナリオでは、一方のクラスの unowned プロパティと、もう一方のクラスの暗黙的にアンラップされたオプショナルプロパティと組み合わせると便利です。

これにより、初期化が完了すると、参照循環を回避しながら、両方のプロパティに直接(オプショナルのアンラップなしで)アクセスできます。このセクションでは、そのような関係を設定する方法を示します。

下記の例では、`Country` と `City` の 2 つのクラスが定義されており、それぞれが他のクラスのインスタンスをプロパティとして保持しています。このデータモデルでは、全ての国には常に首都が必要で、全ての都市は常に国に属している必要があります。これを表すために、`Country` クラスには `capitalCity` プロパティがあり、`City` クラスには `country` プロパティがあります:

```swift
class Country {
    let name: String
    var capitalCity: City!
    init(name: String, capitalName: String) {
        self.name = name
        self.capitalCity = City(name: capitalName, country: self)
    }
}

class City {
    let name: String
    unowned let country: Country
    init(name: String, country: Country) {
        self.name = name
        self.country = country
    }
}
```

2 つのクラス間の相互依存関係を設定するために、`City` のイニシャライザは `Country` インスタンスを取得し、このインスタンスを `country` プロパティに格納します。

`City` のイニシャライザは、`Country` のイニシャライザ内から呼び出されます。ただし、[Two-Phase Initialization](./initialization.md#two-phase-initializationイニシャライザの2フェーズ)で説明されているように、新しい `Country` インスタンスが完全に初期化されるまで、`Country` のイニシャライザは `self` を `City` イニシャライザに渡すことはできません。

この要件に対処するには、`Country` の `capitalCity` プロパティを、型注釈の最後に感嘆符で示される、暗黙的にアンラップされたオプショナルプロパティ(`City!`)として宣言しています。これは、他のオプショナルと同様に、`capitalCity` プロパティのデフォルト値は `nil` ですが、[Implicitly Unwrapped Optionals](./the-basics.md#implicitly-unwrapped-optionals暗黙アンラップオプショナル)で説明されているように、その値をアンラップする必要なくアクセスできることを意味します。

`capitalCity` にはデフォルトの `nil` 値があるため、新しい `Country` インスタンスは、`Country` インスタンスがイニシャライザ内で `name` プロパティを設定するとすぐに完全に初期化されたと見なされます。これは、`name` プロパティが設定されるとすぐに、`Country` イニシャライザが暗黙的な `self` プロパティの参照の受け渡しを開始できることを意味します。したがって、`Country` イニシャライザが独自の `capitalCity` プロパティを設定している場合、`Country` イニシャライザは `City` イニシャライザの引数の 1 つとして `self` を渡すことができます。

これで全て、強参照循環を作成せずに単一の文で `Country` と `City` のインスタンスを作成できることを意味します。また、オプショナルの値をアンラップするために感嘆符を使用する必要なく、`capitalCity` プロパティに直接アクセスできます:

```swift
var country = Country(name: "Canada", capitalName: "Ottawa")
print("\(country.name)'s capital city is called \(country.capitalCity.name)")
// "Canada's capital city is called Ottawa"
```

上記の例では、暗黙的にアンラップされたオプショナルを使用することは、2 フェーズのクラスイニシャライザの要件が全て満たされることを意味します。`capitalCity` プロパティは、初期化が完了すると、強参照循環を回避しつつ、オプショナルではない値のように使用およびアクセスできます。

## Strong Reference Cycles for Closures(クロージャの強参照循環)

2 つのクラス インスタンスプロパティが互いに強参照を保持している場合に、強参照循環がどのように作成されるかを上で説明しました。また、弱参照と非所有参照を使用して、これらの強参照循環を破る方法についても説明しました。

クラスインスタンスのプロパティにクロージャを割り当て、そのクロージャの本文がインスタンスをキャプチャした場合にも、強参照循環が発生する可能性があります。このキャプチャは、クロージャの本文がインスタンスのプロパティ(`self.someProperty` など) にアクセスした場合、またはクロージャがインスタンスのメソッド(`self.someMethod()` など)を呼び出した場合に発生する可能性があります。いずれの場合も、これらのアクセスにより、クロージャが `self` を「キャプチャ」し、強参照循環が作成されます。

この強参照循環が発生するのは、クロージャがクラスと同様に参照型のためです。プロパティにクロージャを割り当てると、そのクロージャへの参照が割り当てられます。本質的に、これは上記と同じ問題です。2 つの強参照がお互いを生かしています。ただし、2 つのクラスインスタンスではなく、今回はクラスインスタンスとクロージャがお互いを生かしています。

Swift は、クロージャキャプチャリスト(*closure capture list*)と呼ばれるこの問題に対するエレガントなソリューションを提供します。ただし、クロージャキャプチャリストを使用して強参照循環を中断する方法を学ぶ前に、そのような循環がどのように発生するかを理解しておくと役立ちます。

下記の例は、`self` を参照するクロージャを使用する場合に強参照循環を作成する方法を示しています。この例では、`HTMLElement` というクラスを定義しています。これは、HTML ドキュメント内の個々の要素にシンプルなモデルを提供します:

```swift
class HTMLElement {

    let name: String
    let text: String?

    lazy var asHTML: () -> String = {
        if let text = self.text {
            return "<\(self.name)>\(text)</\(self.name)>"
        } else {
            return "<\(self.name) />"
        }
    }

    init(name: String, text: String? = nil) {
        self.name = name
        self.text = text
    }

    deinit {
        print("\(name) is being deinitialized")
    }

}
```

`HTMLElement` クラスは、見出し要素の場合は `"h1"`、段落要素の場合は `"p"`、改行要素の場合は `"br"` など、要素の名前を示す `name` プロパティを定義します。`HTMLElement` は、オプショナルの `text` プロパティも定義します。このプロパティは、その HTML 要素内でレンダリングされるテキストを表す文字列に設定できます。

これら 2 つのシンプルなプロパティに加えて、`HTMLElement` クラスは `asHTML` と呼ばれる遅延プロパティを定義します。このプロパティは、`name` と `text` を HTML 文字列フラグメントに結合するクロージャを参照します。`asHTML` プロパティの型は `() -> String`、または「引数を受け取らず、`String` 値を返す関数」です。

デフォルトでは、`asHTML` プロパティには、HTML タグの文字列表現を返すクロージャが割り当てられます。このタグには、(存在する場合)オプショナルの `text` 値が含まれます。`text` が存在しない場合、テキストコンテンツは含まれません。段落要素の場合、クロージャは `text` プロパティが `"some text"` または `nil` かどうかに応じて、`"<p>some text</p>"` または `"<p />"` を返します。

`asHTML` プロパティには名前が付けられ、インスタンスメソッドのように使用されます。ただし、`asHTML` はインスタンスメソッドではなくクロージャプロパティのため、特定の HTML 要素の HTML レンダリングを変更する場合は、`asHTML` プロパティのデフォルト値を独自のクロージャに置き換えることができます。

例えば、表現が空の HTML タグを返さないようにするために、`asHTML` プロパティは、`text` プロパティが `nil` の場合にデフォルトで何らかのテキストになるクロージャに設定できます。

```swift
let heading = HTMLElement(name: "h1")
let defaultText = "some default text"
heading.asHTML = {
    return "<\(heading.name)>\(heading.text ?? defaultText)</\(heading.name)>"
}
print(heading.asHTML())
// "<h1>some default text</h1>"
```

> NOTE  
> `asHTML` プロパティは遅延プロパティとして宣言されています。これは、要素が実際にHTML 出力ターゲットの文字列値の一部としてレンダリングされる必要がある場合にのみ必要になるためです。`asHTML` が遅延プロパティであるということは、デフォルトクロージャ内で `self` を参照できることを意味します。これは、初期化が完了し、`self` が存在することがわかるまで遅延プロパティにアクセスできないためです。

`HTMLElement` クラスは単一のイニシャライザを提供します。このイニシャライザは、新しい要素を初期化するための `name` 引数と(必要に応じて)`text` 引数を受け取ります。このクラスは、`HTMLElement` インスタンスの割り当てが解除されたときに表示するメッセージを出力するデイニシャライザも定義しています。

`HTMLElement` クラスを使用して新しいインスタンスを作成および出力する方法は次のとおりです:

```swift
var paragraph: HTMLElement? = HTMLElement(name: "p", text: "hello, world")
print(paragraph!.asHTML())
// "<p>hello, world</p>"
```

> NOTE  
> 上記の `paragraph` 変数はオプショナルの `HTMLElement` として定義されているため、下記では `nil` に設定して、強参照循環の存在を示すことができます。

残念ながら、上記の `HTMLElement` クラスは、`HTMLElement` インスタンスと、デフォルトの `asHTML` 値に使用されるクロージャとの間に強参照循環を作成します。サイクルは次のようになります:

![クロージャ強参照循環1](./../.gitbook/assets/closureReferenceCycle01_2x.png)

インスタンスの `asHTML` プロパティは、そのクロージャへの強参照を保持します。ただし、クロージャは(`self.name` および `self.text` を参照する方法として)本文内の `self` を参照するため、クロージャは `self` をキャプチャします。つまり、クロージャは `HTMLElement` インスタンスへの強参照を保持します。2 つの間に強参照循環が作成されます。(クロージャでの値のキャプチャの詳細については、[Capturing Values](./closures.md#capturing-values値のキャプチャ)を参照してください)

> NOTE  
> クロージャは `self` を複数回参照していますが、`HTMLElement` インスタンスへの強参照を 1 つだけキャプチャします。

`paragraph` 変数を `nil` に設定し、`HTMLElement` インスタンスへの強参照を解除すると、強参照循環のため、`HTMLElement` インスタンスもそのクロージャも割り当てを解除できません。

```swift
paragraph = nil
```

`HTMLElement` のデイニシャライザのメッセージは出力されないことに注目してください。これは、`HTMLElement` インスタンスが割り当て解除されていないことを示しています。

## Resolving Strong Reference Cycles for Closures(クロージャの強参照循環の解消)

クロージャの定義の一部としてキャプチャリストを定義することで、クロージャとクラスインスタンス間の強参照循環を解決します。キャプチャリストは、クロージャの本文内で 1 つ以上の参照型をキャプチャするときに使用するルールを定義します。2 つのクラスインスタンス間の強参照循環と同様に、キャプチャされた各参照は、強参照ではなく、弱参照または非所有参照と宣言します。弱いか所有されていないかの適切な選択は、コード間の様々な関係によって異なります。

> NOTE  
> Swift では、クロージャ内で `self` のメンバを参照するときは常に、(`someProperty` または `someMethod()` だけでなく) `self.someProperty` または `self.someMethod()` を記述する必要があります。これは、思わず `self` をキャプチャする可能性があることを思い出すのに役立ちます。

## Defining a Capture List(キャプチャリストの定義)

キャプチャリストの各アイテムは、クラスインスタンス(`self` など)への参照、または何らかの値で初期化された変数(`delegate = self.delegate` など)を使用した、`weak` または `unowned` キーワードのペアです。これらのペアは、カンマで区切られた 1 組の角括弧内(`[]`)に記述されます。

キャプチャリストをクロージャの引数リストと(提供されている場合は)戻り値の型の前に配置します。

```swift
lazy var someClosure = {
    [unowned self, weak delegate = self.delegate]
    (index: Int, stringToProcess: String) -> String in
     // クロージャ本文がここに来ます
}
```

コンテキストから推論できるため、クロージャが引数リストまたは戻り値の型を指定しない場合は、キャプチャリストをクロージャの最初に置き、その後に `in` キーワードを置きます。

```swift
lazy var someClosure = {
    [unowned self, weak delegate = self.delegate] in
     // クロージャ本文がここに来ます
}
```

## Weak and Unowned References(弱参照と非所有参照)

クロージャとキャプチャするインスタンスが常に相互に参照し、常に同時に割り当てが解除される場合は、クロージャ内のキャプチャを非所有参照として定義します。

逆に、キャプチャされた参照が将来のある時点でゼロになる可能性がある場合は、キャプチャを弱参照として定義します。弱参照は常にオプショナル型で、参照するインスタンスの割り当てが解除されると、自動的に `nil` になります。これにより、クロージャの本文内に存在するかどうかを確認できます。

> NOTE  
> キャプチャされた参照が `nil` にならない場合は、弱参照ではなく、常に非所有参照としてキャプチャする必要があります。

非所有参照は、上記のクロージャの強参照循環の `HTMLElement` の例の強参照循環を解決するために使用する適切なキャプチャ方法です。循環を回避するための `HTMLElement` クラスの作成方法は次のとおりです:

```swift
class HTMLElement {

    let name: String
    let text: String?

    lazy var asHTML: () -> String = {
        [unowned self] in
        if let text = self.text {
            return "<\(self.name)>\(text)</\(self.name)>"
        } else {
            return "<\(self.name) />"
        }
    }

    init(name: String, text: String? = nil) {
        self.name = name
        self.text = text
    }

    deinit {
        print("\(name) is being deinitialized")
    }
}
```

この `HTMLElement` の実装は、`asHTML` クロージャ内にキャプチャリストが追加されていることを除けば、前の実装と同じです。この場合、キャプチャリストは `[unowned self]` で、「強参照ではなく、非所有参照として `self` をキャプチャする」という意味です。

先ほどと同じように `HTMLElement` インスタンスを作成して出力できます。

```swift
var paragraph: HTMLElement? = HTMLElement(name: "p", text: "hello, world")
print(paragraph!.asHTML())
// "<p>hello, world</p>"
```

キャプチャリストを配置した場合の参照は次のようになります：

![クロージャ強参照循環2](./../.gitbook/assets/closureReferenceCycle02_2x.png)

今回は、クロージャによる `self` のキャプチャは非所有参照で、キャプチャした `HTMLElement` インスタンスを強く保持しません。`paragraph` 変数からの強参照を `nil` に設定すると、`HTMLElement` インスタンスの割り当てが解除されます。これは、下記の例のデイニシャライザメッセージの出力からもわかります:

```swift
paragraph = nil
// "p is being deinitialized"
```

キャプチャリストの詳細については、[Capture Lists](./../language-reference/expressions.md#capture-listsキャプチャリスト)を参照ください。
