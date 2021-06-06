# Access Control

最終更新日:

アクセス制御は、他のソースファイルやモジュールのコードからコードの一部へのアクセスを制限します。この機能を使用すると、コードの実装の詳細を非表示にし、そのコードにアクセスして使用するための望ましいインターフェイスを指定できます。

個々の型(クラス、構造体、および列挙型)、およびそれらの型に属するプロパティ、メソッド、イニシャライザ、および subscript に特定のアクセスレベルを割り当てることができます。プロトコルは、グローバル定数、変数、および関数と同様に、特定のコンテキストに制限できます。

様々なレベルのアクセス制御を提供することに加えて、Swift は、一般的なシナリオのデフォルトのアクセスレベルを提供することにより、明示的なアクセス制御レベルを指定する手間を減らします。実際、単一のターゲットアプリを作成している場合は、明示的なアクセス制御レベルをまったく指定する必要がない場合もあります。

> NOTE  
> アクセス制御を適用できるコードの様々な側面(プロパティ、型、関数など)は、簡潔にするために、以下のセクションでは「エンティティ」と呼びます。

## Modules and Source Files(モジュールとソースファイル)

Swift のアクセス制御モデルは、モジュールとソースファイルの概念に基づいています。

モジュールは、コード配布の単位で、単体で構築および出荷され、`import` キーワードを使用して別のモジュールによってインポートできるフレームワークまたはアプリケーションです。

Xcode の各ビルドターゲット(アプリバンドルやフレームワークなど) は、Swift では個別のモジュールとして扱われます。アプリのコードの様々な側面を独立したフレームワークとしてグループ化する場合(おそらく、そのコードを複数のアプリケーションでカプセル化して再利用する場合)、そのフレームワーク内で定義するものは全て、アプリ内でインポートされて使用される場合や、別のフレームワーク内で使用されている場合に、別のモジュールの一部になります。

ソースファイルは、モジュール内の Swift ソースコードファイルです(実際には、アプリまたはフレームワーク内の単一のファイル)。個別のソースファイルで個々の型を定義するのが一般的ですが、1 つのソースファイルに複数の型や関数などの定義を含めることができます。

## Access Levels(アクセスレベル)

Swift は、コード内のエンティティに 5 つの異なるアクセスレベルを提供します。これらのアクセスレベルは、エンティティが定義されているソースファイルに関連しており、ソースファイルが属するモジュールにも関連しています。

* open アクセスと public アクセスにより、エンティティを定義モジュールの任意のソースファイル内で使用できるほか、定義モジュールをインポートする別のモジュールのソースファイルでもエンティティを使用できます。フレームワークへの public インターフェイスを指定するときは、通常、open アクセスまたは public アクセスを使用します。open アクセスと public アクセスの違いについては、以下で説明します
* internal アクセスにより、エンティティを定義するモジュールの任意のソースファイル内で使用できますが、そのモジュールの外部のソースファイルでは使用できません。通常、アプリまたはフレームワークの内部構造を定義するときは、internal アクセスを使用します
* fileprivate アクセスは、エンティティの使用をそれ自体を定義するソースファイルに制限します。特定の機能の実装の詳細がファイル全体で使用されている場合、それらの詳細を隠すには、fileprivate アクセスを使用します
* private アクセスは、エンティティの使用を、それを囲む宣言と、同じファイル内にあるその宣言の extension に制限します。private アクセスを使用して、特定の機能の実装の詳細が単一の宣言内でのみ使用される場合、それらの詳細を隠します

open アクセスは最も高い(制限の少ない)アクセスレベルで、private アクセスは最も低い(最も制限の厳しい)アクセスレベルです

[Subclassing](#subclassingサブクラス)でも話されているように、open アクセスはクラスとクラスメンバにのみ適用され、モジュール外のコードがサブクラス化およびオーバーライドできる点で public アクセスとは異なります。クラスを `open` として明示的にマークすることは、そのクラスをスーパークラスとして使用している他のモジュールからのコードの影響を考慮し、それに応じてクラスのコードを設計していることを示します。

### Guiding Principle of Access Levels(アクセスレベルの指針)

Swift のアクセスレベルは、全体の指針に従います: より低い(より制限の厳しい)アクセスレベルを持つ別のエンティティに関してエンティティを定義することはできません。

例えば:

* public 変数が使用されている全ての場所で型を使用できるとは限らないため、public 変数を internal、fileprivate、または private 型として定義することはできません
* 関数は、その構成要素の型が周囲のコードで使用できない状況で使用される可能性があるため、関数はその引数の型と戻り値の型よりも高いアクセスレベルを持つことはできません

言語の様々な側面に対するこの指針の具体的な意味については、下記で詳しく説明します。

### Default Access Levels(デフォルトのアクセスレベル)

自分で明示的なアクセスレベルを指定しない場合、コード内の全てのエンティティ(この章の後半で説明するいくつかの特定の例外を除く)には、デフォルトのアクセスレベル internal が設定されています。その結果、多くの場合、コードで明示的なアクセスレベルを指定する必要はありません。

### Access Levels for Single-Target Apps(単一ターゲットアプリのアクセスレベル)

シンプルな単一ターゲットのアプリを作成する場合、アプリのコードは一般的にアプリ内で自己完結でき、アプリのモジュールの外部で使用できるようにする必要はありません。internal のデフォルトのアクセスレベルは、既にこの要件に一致しています。したがって、独自のアクセスレベルを指定する必要はありません。ただし、アプリのモジュール内の他のコードから実装の詳細を隠すために、コードの一部を fileprivate または private としてマークしたい場合もあります。

### Access Levels for Frameworks(フレームワークのアクセスレベル)

フレームワークを開発するときは、そのフレームワークへの public インターフェイスを open または public としてマークして、フレームワークをインポートするアプリなどの他のモジュールから表示およびアクセスできるようにします。この public インターフェースは、フレームワークのアプリケーションプログラミングインターフェース(または API) です。

> NOTE  
> フレームワークの内部実装の詳細は、引き続きデフォルトの internal アクセスレベルを使用するか、フレームワークの内部コードを他の部分から隠したい場合は private または fileprivate としてマークすることができます。エンティティをフレームワークの API の一部にする場合にのみ、エンティティを open または public としてマークする必要があります。

### Access Levels for Unit Test Targets(単体テストターゲットのアクセスレベル)

単体テストターゲットを使用してアプリを作成する場合、アプリのコードをテストするには、そのモジュールで使用できるようにする必要があります。デフォルトでは、open または public としてマークされたエンティティのみが他のモジュールにアクセスできます。ただし、製品モジュールのインポート宣言を `@testable` 属性でマークし、テストを有効にしてその製品モジュールをコンパイルすると、単体テストターゲットは任意の内部エンティティにアクセスできます。

## Access Control Syntax(アクセス制御構文)

エンティティの宣言の先頭に `open`、`public`、`internal`、`fileprivate`、または `private` 修飾子を配置して、エンティティのアクセスレベルを定義します。

```swift
public class SomePublicClass {}
internal class SomeInternalClass {}
fileprivate class SomeFilePrivateClass {}
private class SomePrivateClass {}

public var somePublicVariable = 0
internal let someInternalConstant = 0
fileprivate func someFilePrivateFunction() {}
private func somePrivateFunction() {}
```

特に指定がない限り、デフォルトのアクセスレベルは、[Default Access Levels](#default-access-levelsデフォルトのアクセスレベル)で説明されているように、internal です。これは、`SomeInternalClass` と `someInternalConstant` が明示的なアクセスレベル修飾子なしで記述でき、internal アクセスレベルを持つことを意味します。

```swift
class SomeInternalClass {}  // 暗黙的に internal
let someInternalConstant = 0  // 暗黙的に internal
```

## Custom Types(独自型)

独自型に明示的なアクセスレベルを指定する場合は、型を定義する時点で指定します。新しい型は、アクセスレベルが許可する場所ではどこでも使用できます。例えば、fileprivate クラスを定義する場合、そのクラスは、fileprivate クラスが定義されているソースファイル内で、プロパティの型、または関数引数または戻り値の型としてのみ使用できます。

型のアクセス制御レベルは、その型のメンバ(そのプロパティ、メソッド、イニシャライザ、および subscript)のデフォルトのアクセスレベルにも影響します。型のアクセスレベルを private または fileprivate として定義すると、そのメンバのデフォルトのアクセスレベルも private または fileprivate になります。型のアクセスレベルを internal または public として定義する場合(またはアクセスレベルを明示的に指定せずに内部の既定のアクセスレベルを使用する場合)、型のメンバの既定のアクセスレベルは internal になります。

> IMPORTANT  
> public 型のメンバは、public ではなく、internal のアクセスレベルがデフォルト設定されています。メンバを public にする場合は、明示的にそのようにマークする必要があります。この要件により、型の API を選択的に公開することができ、型の内部動作を誤って API として公開することを回避できます。

```swift
public class SomePublicClass {                  // 明示的な public クラス
    public var somePublicProperty = 0            // 明示的な public クラス メンバ
    var someInternalProperty = 0                 // 暗黙的な internal クラス メンバ
    fileprivate func someFilePrivateMethod() {}  // 明示的な file-private クラス メンバ
    private func somePrivateMethod() {}          // 明示的な private クラス メンバ
}

class SomeInternalClass {                       // 暗黙的な internal クラス
    var someInternalProperty = 0                 // 暗黙的な internal クラスメンバ
    fileprivate func someFilePrivateMethod() {}  // 明示的な file-private クラスメンバ
    private func somePrivateMethod() {}          // 明示的な private クラスメンバ
}

fileprivate class SomeFilePrivateClass {        // 明示的な file-privateクラス
    func someFilePrivateMethod() {}              // 暗黙的な file-private クラスメンバ
    private func somePrivateMethod() {}          // 明示的な private クラスメンバ
}

private class SomePrivateClass {                // 明示的な private クラス
    func somePrivateMethod() {}                  // 暗黙的な private クラスメンバ
}
```

### Tuple Types(タプル型)

タプル型のアクセスレベルは、そのタプルで使用される全ての型の中で最も制限の厳しいアクセスレベルになります。例えば、1 つは internal、もう 1 つは private の 2 つの異なる型からタプルを構成する場合、その複合タプル型のアクセスレベルは private になります。

> NOTE  
> タプル型には、クラス、構造体、列挙型、および関数のような独立した定義がありません。タプル型のアクセスレベルは、タプル型を構成する型から自動的に決定され、明示的に指定することはできません。

### Function Types(関数型)

関数型のアクセスレベルは、関数の引数型と戻り型の最も制限の厳しいアクセスレベルとして計算されます。関数の計算されたアクセスレベルがコンテキストの既定値と一致しない場合は、関数の定義にアクセスレベルを明示的に指定する必要があります。

下記の例では、関数自体に特定のアクセスレベル修飾子を指定せずに、`someFunction()` というグローバル関数を定義しています。この関数のデフォルトのアクセスレベルは「internal」だと思われるかもしれませんが、そうではありません。実際、 `someFunction()` は下記のようにコンパイルされません。

```swift
func someFunction() -> (SomeInternalClass, SomePrivateClass) {
    // ここに関数の実装が入ります
}
```

関数の戻り値の型は、上記の[Custom Types](#custom-types独自型)で定義された 2 つの独自クラスで構成されるタプル型です。これらのクラスの 1 つは internal として定義され、もう 1 つは private として定義されます。したがって、複合タプル型の全体的なアクセスレベルは private です(タプルの構成型の最小アクセスレベル)。

関数の戻り値の型は private なため、関数宣言が有効になるように、関数の全体的なアクセスレベルを `private` 修飾子でマークする必要があります。

```swift
private func someFunction() -> (SomeInternalClass, SomePrivateClass) {
    // ここに関数の実装が入ります
}
```

`public` または `internal` 修飾子で `someFunction()` の定義をマークしたり、関数の public または internal ユーザが関数の戻り値の型で使用する private クラスへアクセスできない可能性があるため、internal のデフォルト設定を使用できません。

### Enumeration Types(列挙型)

列挙型の個々のケースは、それらが属する列挙型と同じアクセスレベルを自動的に受け取ります。個々の列挙型ケースに異なるアクセスレベルを指定することはできません。

下記の例では、`CompassPoint` 列挙型に public の明示的なアクセスレベルが設定されています。したがって、列挙型ケースの `north`、 `south`、 `east`、 および `west` にも public のアクセスレベルがあります。

```swift
public enum CompassPoint {
    case north
    case south
    case east
    case west
}
```

#### Raw Values and Associated Values(Raw Valueと関連値)

---

列挙型定義の Raw Value または関連値に使用される型には、少なくとも列挙型のアクセスレベルと同じレベルのアクセスレベルが必要です。例えば、private 型を internal アクセスレベルの列挙型の Raw Value 型として使用することはできません。

### Nested Types(ネストされた型)

ネストされた型のアクセスレベルは、ラップされている型が public でない限り、ラップされている型と同じです。public 型内のネストされた型には、internal が自動で設定されています。public 型内のネストされた型を public に使用できるようにする場合は、ネストされた型を public として明示的に宣言する必要があります。

## Subclassing(サブクラス)

現在のアクセスコンテキストでアクセスでき、同じモジュールで定義されている任意のクラスをサブクラス化できます。別のモジュールで定義されている open クラスをサブクラス化することもできます。サブクラスは、そのスーパークラスよりも高いアクセスレベルを持つことはできません。例えば、internal スーパークラスから public サブクラスを作成することはできません。

さらに、同じモジュールで定義されているクラスの場合、特定のアクセスコンテキストでアクセス可能な任意のクラスのメンバ(メソッド、プロパティ、イニシャライザ、または subscript) をオーバーライドできます。別のモジュールで定義されているクラスの場合、open クラスのメンバをオーバーライドできます。

オーバーライドにより、継承されたクラスメンバは、そのスーパークラスのバージョンよりもアクセスしやすくなります。下記の例では、クラス `A` は `someMethod()` と呼ばれる fileprivate メソッドを持つ public クラスです。クラス `B` は `A` のサブクラスで、アクセスレベルが「internal」に制限されています。それにもかかわらず、クラス `B` は、`someMethod()` の元の実装よりも高い「internal」のアクセスレベルで `someMethod()` のオーバーライドを提供します。

```swift
public class A {
    fileprivate func someMethod() {}
}

internal class B: A {
    override internal func someMethod() {}
}
```

スーパークラスのメンバへの呼び出しが許可されたアクセスレベルコンテキスト内で行われる限り、サブクラスのメンバがそれよりも低いアクセスレベルを持つスーパークラスのメンバを呼び出すことも可能です(つまり、スーパークラスと同じソースファイル内で fileprivate メンバ呼び出しや、スーパークラスと同じモジュール内の internal メンバー呼び出し):

```swift
public class A {
    fileprivate func someMethod() {}
}

internal class B: A {
    override internal func someMethod() {
        super.someMethod()
    }
}
```

スーパークラス `A` とサブクラス `B` は同じソースファイルで定義されているため、`someMethod()` の B 実装で `super.someMethod()` を呼び出すことは有効です。

## Constants, Variables, Properties, and Subscripts(定数、変数、プロパティ、subscript)

定数、変数、またはプロパティは、その型よりも低いアクセスレベルにすることはできません。例えば、public プロパティを private 型で記述することはできません。同様に、subscript は、そのインデックス型または戻り型よりも低いアクセスレベルにすることはできません。

定数、変数、プロパティ、または subscript が private 型を使用する場合、定数、変数、プロパティ、または subscript も private としてマークする必要があります。

```swift
private var privateInstance = SomePrivateClass()
```

### Getters and Setters(get と set)

定数、変数、プロパティ、および subscript の get と set は、それらが属する定数、変数、プロパティ、または subscript と同じアクセスレベルを自動的に受け取ります。

その変数、プロパティ、または subscript の読み取り/書き込みスコープを制限するために、対応する get よりも低いアクセス レベルを set に与えることができます。`var` または `subscript` の前に `fileprivate(set)`、`private(set)`、または `internal(set)` を記述して、より低いアクセスレベルを割り当てます。

> NOTE  
> このルールは、格納プロパティと計算プロパティに適用されます。格納プロパティに明示的な get と set を作成しなくても、Swift は暗黙的な get と set を合成して、格納プロパティのバッキングストレージへのアクセスを提供します。`fileprivate(set)`、`private(set)`、および `internal(set)` を使用して、格納プロパティの明示的な set の場合とまったく同じ方法で、合成された set のアクセス レベルを変更します。

下記の例では、文字列プロパティが変更された回数を追跡する `TrackedString` という構造体を定義しています:

```swift
struct TrackedString {
    private(set) var numberOfEdits = 0
    var value: String = "" {
        didSet {
            numberOfEdits += 1
        }
    }
}
```

`TrackedString` 構造体は、`value` と呼ばれる文字列の格納プロパティを定義し、初期値は "" (空の文字列) です。この構造体は、`numberOfEdits` という整数格納プロパティも定義しています。これは、値が変更された回数を追跡するために使用されています。この変更追跡は、`value` プロパティに `didSet` プロパティオブザーバを使用して実装され、`value` プロパティに新しい値に設定される度に `numberOfEdits` を増加します。

`TrackedString` 構造体と `value` プロパティは明示的なアクセスレベル修飾子を提供していないため、両方ともデフォルトの internal アクセスレベルを受け取ります。ただし、`numberOfEdits` プロパティのアクセスレベルは `private(set)` 修飾子でマークされており、プロパティの get には引き続きデフォルトのアクセスレベル internal が設定され、プロパティは `TrackedString` 構造体の一部のコード内からのみ設定可能なことを示しています。これにより、`TrackedString` は `numberOfEdits` プロパティを内部的に変更できますが、構造体の定義の外部で使用される場合は、プロパティを読み取り専用プロパティとして表示できます。

`TrackedString` インスタンスを作成し、その文字列値を数回変更すると、`numberOfEdits` プロパティの値が変更の数に一致するように更新されていることがわかります。

```swift
var stringToEdit = TrackedString()
stringToEdit.value = "This string will be tracked."
stringToEdit.value += " This edit will increment numberOfEdits."
stringToEdit.value += " So will this one."
print("The number of edits is \(stringToEdit.numberOfEdits)")
// "The number of edits is 3"
```

`numberOfEdits` プロパティの現在の値へ別のソースファイルからアクセスすることはできますが、別のソースファイルからプロパティを変更することはできません。この制限により、`TrackedString` 編集追跡機能の実装の詳細が保護されつつ、その機能の便利な一側面へのアクセスは提供することができています。

必要に応じて、get と set の両方に明示的なアクセスレベルを割り当てることができることに注目してください。下記の例は、構造体が public の明示的なアクセスレベルで定義されている `TrackedString` 構造体のバージョンを示しています。したがって、構造体のメンバー(`numberOfEdits` プロパティを含む) には、デフォルトで internal アクセスレベルが設定されています。`public` と `private(set)` のアクセスレベル修飾子を組み合わせることで、構造体の `numberOfEdits` プロパティの get を public にし、そのプロパティの set を private にすることができます。

```swift
public struct TrackedString {
    public private(set) var numberOfEdits = 0
    public var value: String = "" {
        didSet {
            numberOfEdits += 1
        }
    }
    public init() {}
}
```

## Initializers(初期化)

### Default Initializers(デフォルトイニシャライザ)

### Default Memberwise Initializers for Structure Types(構造体のデフォルトのメンバごとのイニシャライザ)

## Protocols(プロトコル)

### Protocol Inheritance(プロトコル継承)

### Protocol Conformance(プロトコル準拠)

## Extensions(拡張)

### Private Members in Extensions(extensionのprivateメンバ)

## Generics(ジェネリクス)

## Type Aliases(型エイリアス)
