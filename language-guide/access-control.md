# アクセスコントロール\(Access Control\)

最終更新日: 2021/6/30  
原文: https://docs.swift.org/swift-book/LanguageGuide/AccessControl.html

アクセス制御は、他のソースファイルやモジュールのコードから一部のコードへのアクセスを制限します。この機能を使用すると、コードの実装の詳細を隠し、そのコードにアクセスして使用するために望ましいインターフェイスを指定できます。

個々の型\(クラス、構造体、および列挙型\)、およびそれらの型に属するプロパティ、メソッド、イニシャライザ、およびサブスクリプトに特定のアクセスレベルを割り当てることができます。グローバル定数、変数、および関数と同様に、プロトコルも特定のコンテキストに制限できます。

様々なレベルのアクセス制御を提供することに加えて、Swift は、一般的なシナリオのデフォルトのアクセスレベルを提供することにより、明示的にアクセスレベルを指定する手間を減らします。実際、単一ターゲットのアプリを作成している場合は、明示的なアクセスレベルを全く指定する必要がない場合もあります。

> NOTE  
> アクセス制御を適用できるコードの様々な側面\(プロパティ、型、関数など\)は、簡潔に記述するために、以下のセクションでは「エンティティ」と呼びます。

## モジュールとソースファイル\(Modules and Source Files\)

Swift のアクセス制御モデルは、モジュールとソースファイルの概念に基づいています。

モジュールは、コード配布の単位で、単体で構築および配布され、`import` キーワードを使用して別のモジュールによってインポートできるフレームワークまたはアプリケーションです。

Xcode の各ビルドターゲット\(アプリバンドルやフレームワークなど\) は、Swift では個別のモジュールとして扱われます。アプリのコードの様々な側面を独立したフレームワークとしてグループ化する場合\(おそらく、そのコードを複数のアプリケーションでカプセル化して再利用する場合\)、そのフレームワーク内で定義するものは全て、アプリ内でインポートされて使用されたり、別のフレームワーク内で使用されているときに、別のモジュールの一部になります。

ソースファイルは、モジュール内の Swift のソースコードファイルです\(実際には、アプリまたはフレームワーク内の単一のファイル\)。個別のソースファイルで個々の型を定義するのが一般的ですが、1 つのソースファイルに複数の型や関数などの定義を含めることができます。

## [アクセスレベル\(Access Levels\)](access-control.md) <a id="access-levels"></a>

Swift は、コード内のエンティティに 5 つの異なるアクセスレベルを提供します。これらのアクセスレベルは、エンティティが定義されているソースファイルに関連しており、ソースファイルが属するモジュールにも関連しています。

* _open_ アクセスと _public_ アクセスにより、エンティティを定義モジュールの任意のソースファイル内で使用できるほか、定義モジュールをインポートする別のモジュールのソースファイルでもエンティティを使用できます。フレームワークの公開インターフェイスを指定するときは、通常、open アクセスまたは public アクセスを使用します。open アクセスと public アクセスの違いについては、以下で説明します
* _internal_ アクセスにより、エンティティを定義するモジュールの任意のソースファイル内で使用できますが、そのモジュールの外部のソースファイルでは使用できません。通常、アプリまたはフレームワークの内部構造を定義するときは、internal アクセスを使用します
* _fileprivate_ アクセスは、エンティティの使用をそれ自体を定義するソースファイルに制限します。特定の機能の実装の詳細がファイル全体で使用されている場合、それらの詳細を隠すには、fileprivate アクセスを使用します
* _private_ アクセスは、エンティティの使用を、それを囲む宣言と、同じファイル内にあるその宣言の extension に制限します。private アクセスを使用して、特定の機能の実装の詳細が単一の宣言内でのみ使用される場合、それらの詳細を隠します

open アクセスは最も高い\(制限の緩い\)アクセスレベルで、private アクセスは最も低い\(制限の厳しい\)アクセスレベルです

[Subclassing\(サブクラス\)](access-control.md#subclassing)でも説明されているように、open アクセスはクラスとクラスメンバにのみ適用され、モジュール外のコードがサブクラス化およびオーバーライドできる点で public アクセスとは異なります。クラスを `open` として明示的にマークすることは、そのクラスをスーパークラスとして使用している他のモジュールからのコードの影響を考慮し、それに応じてクラスのコードを設計していることを示します。

### アクセスレベルの指針\(Guiding Principle of Access Levels\)

Swift のアクセスレベルは、全体の指針に従います: より低いアクセスレベルを持つ別のエンティティを定義することはできません。

例えば:

* public 変数が使用されている全ての場所で型を使用できるとは限らないため、internal、fileprivate、または private 型を public 変数として定義することはできません
* 関数は、その構成要素の型が周囲のコードで使用できない状況で使用される可能性があるため、関数はそのパラメータの型と戻り値の型よりも高いアクセスレベルを持つことはできません

言語の様々な側面に対するこの指針の具体的な意味については、下記で詳しく説明します。

### [デフォルトのアクセスレベル\(Default Access Levels\)](access-control.md) <a id="default-access-levels"></a>

自身で明示的なアクセスレベルを指定しない場合、コード内の全てのエンティティ\(この章の後半で説明するいくつかの特定の例外を除く\)には、デフォルトのアクセスレベル internal が設定されています。その結果、多くの場合、コードで明示的なアクセスレベルを指定する必要はありません。

### 単一ターゲットアプリのアクセスレベル\(Access Levels for Single-Target Apps\)

シンプルな単一ターゲットのアプリを作成する場合、アプリのコードは一般的にアプリ内で自己完結でき、アプリのモジュールの外部で使用できるようにする必要はありません。internal のデフォルトのアクセスレベルは、既にこの要件を満たしています。したがって、独自のアクセスレベルを指定する必要はありません。ただし、アプリのモジュール内の他のコードから実装の詳細を隠すために、コードの一部を fileprivate または private としてマークしたい場合もあるかもしれません。

### フレームワークのアクセスレベル\(Access Levels for Frameworks\)

フレームワークを開発するときは、公開インターフェイスを open または public としてマークして、フレームワークをインポートするアプリなどの他のモジュールからアクセスできるようにします。この公開インターフェースは、フレームワークのアプリケーションプログラミングインターフェース\(または API\) です。

> NOTE  
> フレームワークの内部実装の詳細は、引き続きデフォルトの internal アクセスレベルを使用するか、フレームワークの内部コードを他の部分から隠したい場合は private または fileprivate としてマークすることができます。エンティティをフレームワークの API の一部にする場合にのみ、エンティティを open または public としてマークする必要があります。

### [単体テストターゲットのアクセスレベル\(Access Levels for Unit Test Targets\)](access-control.md) <a id="access-levels-for-unit-test-targets"></a>

単体テストターゲットを使用してアプリを作成する場合、アプリのコードをテストするには、そのモジュールで使用できるようにする必要があります。デフォルトでは、open または public としてマークされたエンティティのみが他のモジュールにアクセスできます。ただし、プロダクションモジュールのインポート宣言を `@testable` 属性でマークし、テストを有効にしてそのプロダクションモジュールをコンパイルすると、単体テストターゲットは任意の internal エンティティにアクセスできます。

## アクセス制御構文\(Access Control Syntax\)

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

特に指定がない限り、デフォルトのアクセスレベルは、[Default Access Levels\(デフォルトのアクセスレベル\)](access-control.md#default-access-levels)で説明されているように、internal です。これは、`SomeInternalClass` と `someInternalConstant` が明示的なアクセスレベル修飾子なしで記述でき、internal アクセスレベルを持つことを意味します。

```swift
class SomeInternalClass {}  // 暗黙的に internal
let someInternalConstant = 0  // 暗黙的に internal
```

## [独自型\(Custom Types\)](access-control.md) <a id="custom-types"></a>

独自型に明示的なアクセスレベルを指定する場合は、型を定義する時点で指定します。新しい型は、アクセスレベルが許可する場所ではどこでも使用できます。例えば、fileprivate クラスを定義する場合、そのクラスは、fileprivate クラスが定義されているソースファイル内で、プロパティの型、または関数パラメータまたは戻り値の型としてのみ使用できます。

型のアクセス制御レベルは、その型のメンバ\(そのプロパティ、メソッド、イニシャライザ、およびサブスクリプト\)のデフォルトのアクセスレベルにも影響します。型のアクセスレベルを private または fileprivate として定義すると、そのメンバのデフォルトのアクセスレベルも private または fileprivate になります。型のアクセスレベルを internal または public として定義する場合\(またはアクセスレベルを明示的に指定せずに内部のデフォルトのアクセスレベルを使用する場合\)、型のメンバのデフォルトのアクセスレベルは internal になります。

> IMPORTANT  
> public 型のメンバは、デフォルトで public ではなく、internal のアクセスレベルが設定されます。メンバを public にする場合は、明示的にそのようにマークする必要があります。これにより、型の API を選択的に公開することができ、型の内部動作を誤って API として公開することを回避できます。

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

### タプル型\(Tuple Types\)

タプル型のアクセスレベルは、そのタプルで使用される全ての型の中で最も制限の厳しいアクセスレベルになります。例えば、1 つは internal、もう 1 つは private の 2 つの異なる型からタプルを構成する場合、その複合タプル型のアクセスレベルは private になります。

> NOTE  
> タプル型には、クラス、構造体、列挙型、および関数のような独立した定義がありません。タプル型のアクセスレベルは、タプル型を構成する型から自動的に決定され、明示的に指定することはできません。

### 関数型\(Function Types\)

関数型のアクセスレベルは、関数のパラメータ型と戻り値の型の最も制限の厳しいアクセスレベルになります。関数のアクセスレベルがコンテキストのデフォルト値と一致しない場合は、関数の定義にアクセスレベルを明示的に指定する必要があります。

下記の例では、関数自体に特定のアクセスレベル修飾子を指定せずに、`someFunction()` というグローバル関数を定義しています。この関数のデフォルトのアクセスレベルは「internal」だと思われるかもしれませんが、そうではありません。実際、 `someFunction()` は下記の場合コンパイルできません。

```swift
func someFunction() -> (SomeInternalClass, SomePrivateClass) {
    // ここに関数の実装が入ります
}
```

関数の戻り値の型は、上記の[Custom Types\(独自型\)](access-control.md#custom-types)で定義された 2 つの独自クラスで構成されるタプル型です。これらのクラスの 1 つは internal として定義され、もう 1 つは private として定義されます。したがって、複合タプル型の全体的なアクセスレベルは private です\(タプルの構成型の最も厳しいアクセスレベル\)。

関数の戻り値の型は private なため、関数宣言が有効になるように、関数の全体的なアクセスレベルを `private` 修飾子でマークする必要があります。

```swift
private func someFunction() -> (SomeInternalClass, SomePrivateClass) {
    // ここに関数の実装が入ります
}
```

`public` または `internal` 修飾子で `someFunction()` の定義をマークしたり、関数の public または internal ユーザが関数の戻り値の型で使用する private クラスへアクセスできない可能性があるため、internal のデフォルト設定を使用できません。

### 列挙型\(Enumeration Types\)

列挙型の個々のケースは、それらが属する列挙型と同じアクセスレベルを自動的に受け取ります。個々の列挙ケースに異なるアクセスレベルを指定することはできません。

下記の例では、`CompassPoint` 列挙型に public が明示的なアクセスレベルとして設定されています。したがって、列挙ケースの `north`、 `south`、 `east`、 および `west` にも public が設定されています。

```swift
public enum CompassPoint {
    case north
    case south
    case east
    case west
}
```

#### Raw Valueと関連値\(Raw Values and Associated Values\)

列挙型定義の Raw Value または関連値に使用される型には、少なくとも列挙型のアクセスレベルと同じレベルのアクセスレベルが必要です。例えば、private 型を internal アクセスレベルの列挙型の Raw Value 型として使用することはできません。

### ネスト型\(Nested Types\)

ネスト型のアクセスレベルは、ラップされている型が public でない限り、ラップされている型と同じです。public 型内のネスト型には、internal が自動で設定されています。public 型内のネスト型を public で使用できるようにする場合は、ネスト型を public として明示的に宣言する必要があります。

## [サブクラス\(Subclassing\)](access-control.md) <a id="subclassing"></a>

現在のアクセスレベルのコンテキストで使用でき、同じモジュールで定義されている任意のクラスをサブクラス化できます。また、別のモジュールで定義されている open クラスをサブクラス化することもできます。サブクラスは、そのスーパークラスよりも制限の緩いアクセスレベルを設定することはできません。例えば、internal スーパークラスから public サブクラスを作成することはできません。

さらに、同じモジュールで定義されているクラスの場合、特定のアクセスレベルコンテキストでアクセス可能な任意のクラスのメンバ\(メソッド、プロパティ、イニシャライザ、またはサブスクリプト\)をオーバーライドできます。別のモジュールで定義されているクラスの場合、open クラスのメンバをオーバーライドできます。

オーバーライドにより、継承されたクラスメンバは、そのスーパークラスのバージョンよりもアクセスしやすくなります。下記の例では、クラス `A` は `someMethod()` と呼ばれる fileprivate メソッドを持つ public クラスです。クラス `B` は `A` のサブクラスで、アクセスレベルが「internal」に制限されています。それにもかかわらず、クラス `B` は、`someMethod()` の元の実装よりも制限の緩い「internal」のアクセスレベルで `someMethod()` のオーバーライドを提供しています。

```swift
public class A {
    fileprivate func someMethod() {}
}

internal class B: A {
    override internal func someMethod() {}
}
```

スーパークラスのメンバへの呼び出しが許可されたアクセスレベルコンテキスト内で行われる限り、サブクラスのメンバがそれよりも制限の厳しいアクセスレベルを持つスーパークラスのメンバを呼び出すことも可能です\(つまり、スーパークラスと同じソースファイル内で fileprivate メンバ呼び出しや、スーパークラスと同じモジュール内の internal メンバの呼び出しなど\):

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

スーパークラス `A` とサブクラス `B` は同じソースファイルで定義されているため、`someMethod()` の B 実装で `super.someMethod()` を呼び出すことは妥当です。

## 定数、変数、プロパティ、サブスクリプト\(Constants, Variables, Properties, and Subscripts\)

定数、変数、またはプロパティは、その型よりも制限の厳しいアクセスレベルにすることはできません。例えば、public プロパティに private 型を使用することはできません。同様に、サブスクリプトを、そのインデックスの型または戻り値の型よりも制限の緩いアクセスレベルにすることはできません。

定数、変数、プロパティ、またはサブスクリプトが private 型を使用する場合、定数、変数、プロパティ、またはサブスクリプトも private としてマークする必要があります。

```swift
private var privateInstance = SomePrivateClass()
```

### [get と set\(Getters and Setters\)](access-control.md) <a id="getters-and-setters"></a>

定数、変数、プロパティ、およびサブスクリプトの get と set は、それらが属する定数、変数、プロパティ、またはサブスクリプトと同じアクセスレベルを自動的に受け取ります。

その変数、プロパティ、またはサブスクリプトの読み取り/書き込みスコープを制限するために、対応する get よりも制限の厳しいアクセスレベルを set に与えることができます。`var` または `subscript` の前に `fileprivate(set)`、`private(set)`、または `internal(set)` を記述して、より制限の厳しいアクセスレベルを割り当てます。

> NOTE  
> このルールは、格納プロパティと計算プロパティにも適用されます。格納プロパティに明示的な get と set を作成しなくても、Swift は暗黙的な get と set を合成して、格納プロパティのバッキングストレージへのアクセスを提供します。`fileprivate(set)`、`private(set)`、および `internal(set)` を使用して、格納プロパティの明示的な set の場合とまったく同じ方法で、合成された set のアクセスレベルを変更します。

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

`TrackedString` 構造体は、`value` と呼ばれる文字列の格納プロパティを定義し、初期値は "" \(空の文字列\) です。この構造体は、`numberOfEdits` という整数格納プロパティも定義しています。これは、値が変更された回数を追跡するために使用されています。この変更追跡は、`value` プロパティに `didSet` プロパティオブザーバを使用して実装され、`value` プロパティに新しい値に設定される度に `numberOfEdits` を増加します。

`TrackedString` 構造体と `value` プロパティは明示的なアクセスレベル修飾子を提供していないため、両方ともデフォルトの internal アクセスレベルを受け取ります。ただし、`numberOfEdits` プロパティのアクセスレベルは `private(set)` 修飾子でマークされており、get には引き続きデフォルトのアクセスレベル internal が設定され、プロパティは `TrackedString` 構造体の一部のコード内からのみ設定可能だということを示しています。これにより、`TrackedString` は `numberOfEdits` プロパティを内部的に変更できますが、構造体の定義の外部で使用される場合は、プロパティを読み取り専用プロパティとして表示できます。

`TrackedString` インスタンスを作成し、その文字列値を数回変更すると、`numberOfEdits` プロパティの値が変更の数に一致するように更新されていることがわかります。

```swift
var stringToEdit = TrackedString()
stringToEdit.value = "This string will be tracked."
stringToEdit.value += " This edit will increment numberOfEdits."
stringToEdit.value += " So will this one."
print("変更回数は \(stringToEdit.numberOfEdits)")
// 変更回数は 3
```

`numberOfEdits` プロパティの現在の値へ別のソースファイルからアクセスすることはできますが、別のソースファイルからプロパティを変更することはできません。この制限により、`TrackedString` 編集追跡機能の実装の詳細が保護されつつ、その機能の便利な一側面へのアクセスは提供することができています。

必要に応じて、get と set の両方に明示的なアクセスレベルを割り当てることができることに注目してください。下記の例は、構造体が public の明示的なアクセスレベルで定義されている `TrackedString` 構造体のバージョンを示しています。したがって、構造体のメンバ\(`numberOfEdits` プロパティを含む\) には、デフォルトで internal アクセスレベルが設定されています。`public` と `private(set)` のアクセスレベル修飾子を組み合わせることで、構造体の `numberOfEdits` プロパティの get を public にし、その set を private にすることができます。

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

## 初期化\(Initializers\)

独自イニシャライザには、初期化する型よりも制限の厳しいのアクセスレベルを割り当てることができます。唯一の例外は、必須イニシャライザ\([Required Initializers\(必須イニシャライザ\)](../language-guide/initialization.md#required-initializers)で定義\) です。必須イニシャライザは、それが属するクラスと同じアクセスレベルを持っている必要があります。

関数とメソッドのパラメータと同様に、イニシャライザのパラメータの型は、イニシャライザのアクセスレベルよりも制限の厳しいレベルにすることはできません。

### デフォルトイニシャライザ\(Default Initializers\)

[Default Initializers\(デフォルトイニシャライザ\)](../language-guide/initialization.md#default-initializers)で説明されているように、Swift は、全てのプロパティにデフォルト値を提供している構造体または基本クラスに 1 つもイニシャライザを提供しない引数なしの_デフォルトイニシャライザ_を自動的に提供します。

デフォルトイニシャライザは、その型が public として定義されていない限り、初期化する型と同じアクセスレベルを持ちます。public として定義されている型の場合、デフォルトのイニシャライザは internal と見なされます。public 型を別のモジュールで使用するときに引数なしのイニシャライザで初期化できるようにする場合は、型の定義で public 型の引数なしのイニシャライザを明示的に提供する必要があります。

### 構造体のデフォルトのメンバワイズイニシャライザ\(Default Memberwise Initializers for Structure Types\)

構造体の格納プロパティのいずれかが private の場合、構造体のデフォルトのメンバワイズイニシャライザは private と見なされます。同様に、構造体の格納プロパティのいずれかが fileprivate の場合、イニシャライザは fileprivate です。それ以外の場合、イニシャライザのアクセスレベルは internal です。

上記のデフォルトイニシャライザと同様に、public 構造体を別のモジュールで使用するときにメンバワイズイニシャライザで初期化できるようにする場合は、型の定義に public のメンバワイズイニシャライザを自分で提供する必要があります。

## プロトコル\(Protocols\)

プロトコル型に明示的なアクセスレベルを割り当てる場合は、プロトコルを定義する時点で割り当てます。これにより、特定のアクセスコンテキスト内でのみ準拠できるプロトコルを作成できます。

プロトコル定義内の各要件のアクセスレベルは、プロトコルと同じアクセスレベルに自動的に設定されます。プロトコルの要件をプロトコルとは異なるアクセスレベルに設定することはできません。これにより、プロトコルに準拠する全ての型で、プロトコルの全ての要件に確実にアクセスできます。

> NOTE  
> public プロトコルを定義する場合、プロトコルの要件は、実装時にそれらの要件に対する public アクセスレベルを必要とします。この動作は、他の型のように、型の定義が public のメンバのアクセスレベルが internal に暗黙的になる動作とは異なります。

### プロトコル継承\(Protocol Inheritance\)

既存のプロトコルから継承する新しいプロトコルを定義する場合、新しいプロトコルは、継承元のプロトコルと同じアクセスレベルにしか設定できません。例えば、internal プロトコルから継承する public プロトコルを作成することはできません。

### プロトコル準拠\(Protocol Conformance\)

型は、型自体よりもアクセスレベルの厳しいプロトコルに準拠できます。例えば、他のモジュールで使用できる public 型を定義できますが、internal プロトコルへ準拠した実装は internal プロトコルの定義モジュール内でのみ使用できます。

型が特定のプロトコルに準拠するコンテキストは、型のアクセスレベルとプロトコルのアクセスレベルの内、最も厳しい制限になります。例えば、型が public で、準拠するプロトコルが internal の場合、そのプロトコルに準拠した型の実装は internal です。

プロトコルに準拠するように型を作成または拡張する場合、各プロトコル要件の型の実装は、そのプロトコル要件と少なくとも同じアクセスレベルだということを確認する必要があります。例えば、public 型が internal プロトコルに準拠する場合、各プロトコル要件の実装は少なくとも internal にする必要があります。

> NOTE  
> Swift では、Objective-C と同様に、プロトコル準拠はグローバルです。同じプログラム内で 2 つの異なる方法で同じ型が 1 つのプロトコルに準拠することはできません。

## 拡張\(Extensions\)

クラス、構造体、または列挙型が使用可能な任意のコンテキストで、クラス、構造体、または列挙型を拡張できます。extension に追加された全ての型のメンバは、拡張される元の型で宣言された型のメンバと同じデフォルトのアクセスレベルを持ちます。public 型または internal 型を拡張する場合、追加する新しい型のメンバのデフォルトのアクセスレベルは internal です。fileprivate 型を拡張する場合、追加する新しい型のメンバには、デフォルトで fileprivate のアクセスレベルが設定されます。private 型を拡張する場合、追加する新しい型のメンバのデフォルトのアクセスレベルは private です。

または、明示的なアクセスレベル修飾子 \(たとえば、`private`\) で extension をマークして、extension 内で定義されている全てのメンバに新しいデフォルトのアクセスレベルを設定することもできます。この新しいデフォルトは、extension 内で引き続き個々の型のメンバでオーバーライドできます。

extension を使用してプロトコル準拠を追加している場合、その extension に明示的なアクセスレベル修飾子を提供することはできません。代わりに、プロトコル独自のアクセスレベルが、extension 内の各プロトコル要件の実装にデフォルトのアクセスレベルとして提供されます。

### extension の private メンバ\(Private Members in Extensions\)

拡張するクラス、構造体、または列挙型と同じファイルにある extension は、コードが元の型の宣言の一部として記述されているかのように動作します。その結果、次のことができます。

* 元の宣言で private メンバを宣言している場合、同じファイル内の extension からそのメンバにアクセスできます
* 1 つの extension で private メンバを宣言している場合、同じファイル内の別の extension からそのメンバにアクセスできます
* extension で private メンバを宣言している場合、同じファイル内の元の宣言からそのメンバにアクセスできます

この動作は、型に private エンティティがあるかどうかに関係なく、同じ方法で extension を使用してコードを整理できることを意味します。例えば、次のシンプルなプロトコルがあるとします:

```swift
protocol SomeProtocol {
    func doSomething()
}
```

extension を使用して、次のようにプロトコル準拠も追加できます:

```swift
struct SomeStruct {
    private var privateVariable = 12
}

extension SomeStruct: SomeProtocol {
    func doSomething() {
        print(privateVariable)
    }
}
```

## ジェネリクス\(Generics\)

ジェネリック型またはジェネリック関数のアクセスレベルは、ジェネリック型またはジェネリック関数自体のアクセスレベルと、その型パラメータの型制約のアクセスレベルの内で最も厳しいレベルになります。

## タイプエイリアス\(Type Aliases\)

定義する全てのタイプエイリアスは、アクセス制御の目的で別個の型として扱われます。タイプエイリアスは、エイリアスする型のアクセスレベルより制限の緩いアクセスレベルを設定することができます。例えば、private タイプエイリアスは private、fileprivate、internal、public、または open 型のエイリアスを作成できますが、public タイプエイリアスは internal、fileprivate 、または private 型のエイリアスを作成できません。

> NOTE  
> この規則は、プロトコル準拠を満たすために使用される関連型のタイプエイリアスにも適用されます。

