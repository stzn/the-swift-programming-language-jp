# Declarations \(宣言\)

最終更新日:

宣言はプログラムに新しい名前または構文を紹介します。例えば、宣言を使用して、変数と定数を導入し、列挙型、構造体、クラス、およびプロトコルの型を定義するために、関数とメソッドを導入します。宣言を使用して、既存の名前付き型の動作を拡張し、他の場所で宣言されているプログラムにシンボルをインポートすることもできます。

Swift では、ほとんどの宣言は、宣言されているのと同時に実装または初期化されているという意味でも定義と同等です。そうは言っても、プロトコルはメンバを実装していないため、ほとんどのプロトコルメンバは宣言だけです。便宜上、この区別は重要ではないため、宣言という用語は宣言と定義の両方をカバーします。

> GRAMMAR OF A DECLARATION  
> declaration → [import-declaration](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_import-declaration)  
> declaration → [constant-declaration](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_constant-declaration)  
> declaration → [variable-declaration](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_variable-declaration)  
> declaration → [typealias-declaration](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_typealias-declaration)  
> declaration → [function-declaration](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_function-declaration)  
> declaration → [enum-declaration](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_enum-declaration)  
> declaration → [struct-declaration](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_struct-declaration)  
> declaration → [class-declaration](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_class-declaration)  
> declaration → [protocol-declaration](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_protocol-declaration)  
> declaration → [initializer-declaration](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_initializer-declaration)  
> declaration → [deinitializer-declaration](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_deinitializer-declaration)  
> declaration → [extension-declaration](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_extension-declaration)  
> declaration → [subscript-declaration](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_subscript-declaration)  
> declaration → [operator-declaration](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_operator-declaration)  
> declaration → [precedence-group-declaration](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_precedence-group-declaration)  
> declarations → [declaration](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_declaration)  [declarations](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_declarations)<sub>*opt*</sub>

## Top-Level Code(トップレベルコード)

Swift ソースファイルのトップレベルコードは、0 個以上の文、宣言、式で構成されています。デフォルトでは、ソースファイルのトップレベルで宣言されている変数、定数、およびその他の名前付き宣言は、同じモジュールの全てのソースファイル内のコードからアクセスできます。[Access Control Levels](#access-control-levelsアクセス制御レベル)で説明されているように、宣言をアクセスレベル修飾子でマークすることで、このデフォルトの動作をオーバーライドできます。

トップレベルコードには 2 種類あります: トップレベルの宣言と実行可能トップレベルコードです。トップレベル宣言は宣言のみで構成され、全ての Swift ソースファイルで許可されています。実行可能トップレベルコードには、宣言だけでなく、文と式が含まれており、プログラムのトップレベルのエントリポイントとしてのみ使用できます。

コードがファイルやモジュールに編成されている方法に関係なく、実行可能にするためにコンパイルした Swift のコードは、トップレベルのエントリポイントをマークする次の方法の 1 つを含めることはできます: `main` 属性、`NSApplicationMain` 属性、`UIApplicationMain` 属性、`main.swift` ファイル、またはトップレベルの実行可能コードを含むファイルです。

> GRAMMAR OF A TOP-LEVEL DECLARATION  
> top-level-declaration → [statements](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar_statements)<sub>*opt*</sub>

## Code Blocks(コードブロック)

コードブロックは、文をグループ化するための様々な宣言および制御構造によって使用されます。形式は次のとおりです:

![コードブロック](./../.gitbook/assets/code_blocks.png)

コードブロック内の文には、宣言、式、およびその他の文が含まれており、ソースコード内の出現順に実行されます。

> GRAMMAR OF A CODE BLOCK  
> code-block → `{` [statements](https://docs.swift.org/swift-book/ReferenceManual/Statements.html#grammar_statements)<sub>*opt*</sub> `}`

## Import Declaration(インポート宣言)

インポート宣言を使用すると、現在のファイルの外部で宣言されているシンボルにアクセスできます。基本形式はモジュール全体をインポートします。`import` キーワードとそれに続くモジュール名で構成されます:

![Import Declaration](./../.gitbook/assets/import_declaration.png)

シンボルのインポートをより細かく制限するために、モジュールまたはサブモジュール内の特定のサブモジュールまたは宣言を指定できます。この詳細な形式を使用する場合は、インポートされたシンボル（それを宣言するモジュールではなく）だけが現在のスコープで利用可能になります。

![Import Declaration2](./../.gitbook/assets/import_declaration2.png)

> GRAMMAR OF AN IMPORT DECLARATION  
> import-declaration → [attributes](https://docs.swift.org/swift-book/ReferenceManual/Attributes.html#grammar_attributes)<sub>*opt*</sub> `import` [import-kind](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_import-kind) <sub>*opt*</sub> [import-path](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_import-path)  
> import-kind → `typealias` \|  `struct` \|  `class` \|  `enum` \|  `protocol` \|  `let` \|  `var` \|  `func`  
> import-path → [import-path-identifier](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_import-path-identifier) \|  [import-path-identifier](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_import-path-identifier)  `.` [import-path](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_import-path)  
> import-path-identifier → [identifier](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_identifier) \|  [operator](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_operator)

## Constant Declaration(定数宣言)

定数宣言では、プログラムに定数の名前付き値を導入します。定数宣言は `let` キーワードを使用して宣言されており、形式は次のとおりです。

![Constant Declaration](./../.gitbook/assets/constant_declaration.png)

定数宣言は、定数名とイニシャライザ式の値の間の不変のバインディングを定義します。定数の値が設定されたら、変更できません。つまり、定数がクラスオブジェクトで初期化されている場合、オブジェクト自体は変更できますが、定数名とそれが参照するオブジェクトの間のバインディングは変更できません。

定数がグローバルスコープで宣言されている場合は、値を使って初期化する必要があります。関数またはメソッドのコンテキストで定数宣言が登場すると、その値が初めて読み込まれる前に値が設定されることが保証されている限り、後で初期化できます。コンパイラが定数の値が読み取られないことをわかっている場合、必ずしも定数は値を設定する必要はありません。クラスまたは構造体宣言のコンテキストで定数宣言が登場した場合は、定数プロパティと見なされます。定数宣言は計算プロパティではなく、get または set を持っていません。

定数宣言の定数名がタプルの場合、タプル内の各項目の名前は、イニシャライザ式で対応する値にバインドされます。

```swift
let (firstNumber, secondNumber) = (10, 42)
```

この例では、`firstNumber` は値 `10` の名前付き定数で、`secondNumber` は `42` の名前付き定数です。両方の定数は独立して使用できます。

```swift
print("The first number is \(firstNumber).")
// "The first number is 10."
print("The second number is \(secondNumber).")
// "The second number is 42."
```

型注釈(`:type`)は、[Type Inference](./types.md#type-inference型推論)で説明されているように、定数名の型を推論できる場合は、省略可能です。

定数型プロパティを宣言するには、宣言に `static` 修飾子をマークします。クラスの定数型プロパティは常に暗黙的に `final` です。サブクラスによるオーバーライドを許可または禁止するために、`class` または `final` 修飾子をマークすることはできません。型プロパティは、[Type Properties](./../language-guide/properties.md#type-properties型プロパティ)で説明されています。

定数の詳細およびそれらを使用するときのガイダンスについては、[Constants and Variables](./../language-guide/the-basics.md#constants-and-variables定数と変数)と[Stored Properties](./../language-guide/properties.md#stored-properties格納プロパティ)を参照ください。

> GRAMMAR OF A CONSTANT DECLARATION  
> constant-declaration → [attributes](https://docs.swift.org/swift-book/ReferenceManual/Attributes.html#grammar_attributes)<sub>*opt*</sub> [declaration-modifiers](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_declaration-modifiers)<sub>*opt*</sub> `let` [pattern-initializer-list](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_pattern-initializer-list)  
> pattern-initializer-list → [pattern-initializer](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_pattern-initializer) \|  [pattern-initializer](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_pattern-initializer)  `,` [pattern-initializer-list](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_pattern-initializer-list)  
> pattern-initializer → [pattern](https://docs.swift.org/swift-book/ReferenceManual/Patterns.html#grammar_pattern)  [initializer](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_initializer)<sub>*opt*</sub>  
> initializer → `=` [expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_expression)

## Variable Declaration(変数宣言)

変数宣言では、プログラムに変数の名前付き値を導入し、`var` キーワードを使用して宣言されています。

変数宣言には、格納/計算変数や格納/計算プロパティ、格納変数オブザーバやプロパティオブザーバ、および静的変数プロパティなど、様々な種類の名前付きで可変な値を宣言するいくつかの形式があります。適切な形式は、変数が宣言されているスコープと宣言する予定の変数の種類によって異なります。

> NOTE  
> [Protocol Property Declaration](#protocol-property-declarationプロトコルプロパティ宣言)で説明されているように、プロトコル宣言のコンテキストでプロパティを宣言することもできます。

[Overriding](./../language-guide/inheritance.md#overridingオーバーライド)で説明されているように、サブクラスのプロパティ宣言に `override` 宣言修飾子でマークすることで、サブクラス内でプロパティをオーバーライドできます。

### Stored Variables and Stored Variable Properties(格納変数、格納可変プロパティ)

次の形式は、格納変数や格納可変プロパティを宣言します。

![格納変数、格納可変プロパティ](./../.gitbook/assets/variable_declaration.png)

この形式の変数宣言は、グローバルスコープ、関数のローカルスコープ、またはクラスまたは構造体宣言のコンテキストで定義できます。このフォームの変数宣言が、グローバルスコープまたは関数のローカルスコープで宣言されている場合は、格納変数(*stored variable*)と呼ばれます。クラスまたは構造体宣言のコンテキストで宣言されている場合は、格納可変プロパティ(*stored variable property*)と呼ばれます。

イニシャライザ式はプロトコル宣言に定義できませんが、他の全てのコンテキストでは、イニシャライザ式は省略可能です。つまり、イニシャライザ式が存在しない場合、変数宣言に明示的な型注釈(`: type`)を含める必要があります。

定数宣言と同様に、変数名がタプルの場合、タプル内の各項目の名前は、イニシャライザ式の対応する値にバインドされます。

それらの名前が示唆するように、格納変数または格納可変プロパティの値がメモリに格納されています。

### Computed Variables and Computed Properties(計算変数、計算変数プロパティ)

次の形式は、計算変数または計算プロパティを宣言します。

![計算変数、計算変数プロパティ](./../.gitbook/assets/computed_variables_and_computed_properties.png)

この形式の変数宣言は、グローバルスコープ、関数のローカルスコープ、またはクラス、構造体、列挙型、または extension のコンテキストで定義できます。この形式の変数宣言がグローバルスコープまたは関数のローカルスコープで宣言されている場合は、計算変数(*computed variable*)と呼ばれます。クラス、構造体、または extension のコンテキストで宣言されている場合は、それは計算プロパティ(*computed property*)と呼びます。

get は値を読み取るために使用され、set は値を書き込むために使用されます。set 句は省略可能で、get のみが必要な場合は、[Read-Only Computed Properties](./../language-guide/properties.md#read-Only-computed-properties読み取り専用計算プロパティ)で説明されているように、両方の句を省略し、シンプルに要求された値を直接返すことができます。しかし、set 句を指定した場合は、get 句も提供する必要があります。

setter name とそれを囲む括弧は省略可能です。setter name を指定した場合は、set への引数の名前として使用されます。setter name を指定しない場合は、[Shorthand Setter Declaration](./../language-guide/properties.md#shorthand-setter-declaration短縮setプロパティ宣言)で説明されているように、set へのデフォルトの引数名は `newValue` です。

格納変数や格納可変プロパティとは異なり、計算変数または計算プロパティの値はメモリに格納されません。

詳細や計算プロパティの例は、[Computed Properties](./../language-guide/properties.md#computed-properties計算プロパティ)を参照ください。

### Stored Variable Observers and Property Observers(格納変数オブザーバとプロパティオブザーバ)

格納変数またはプロパティを `willSet` や `didSet` オブザーバと一緒に宣言することもできます。オブザーバで宣言された格納変数またはプロパティの形式は次のとおりです:

![格納変数オブザーバとプロパティオブザーバ](./../.gitbook/assets/stored_variable_observers_and_property_observers.png)

この形式の変数宣言は、グローバルスコープ、関数のローカルスコープ、またはクラスまたは構造体宣言のコンテキストで定義できます。この形式の変数宣言がグローバルスコープまたは関数のローカルスコープで宣言されている場合、オブザーバは格納変数オブザーバ(*stored variable observer*)と呼ばれます。クラスまたは構造体宣言のコンテキストで宣言されている場合、オブザーバはプロパティオブザーバ(*property observer*)と呼びます。

任意の格納プロパティにプロパティオブザーバを追加できます。[Overriding Property Observers](./../language-guide/inheritance.md#overriding-property-observersプロパティオブザーバのオーバーライド)で説明されているように、サブクラス内でプロパティをオーバーライドすることで、継承したプロパティ(格納または計算)にプロパティオブサーバを追加することもできます。

イニシャライザ式は、クラスまたは構造体宣言のコンテキストでは省略可能ですが、他の場所では必須です。型注釈は、型がイニシャライザ式から推論できる場合は省略可能です。この式は、プロパティの値を初めて読むときに評価されます。プロパティの値を読み込む前にプロパティの初期値を上書きする場合、この式はプロパティに初めて書き込まれる前に評価されます。

`willSet` および `didSet` オブザーバは、変数またはプロパティの値が設定されるときに、その値を監視(そして適切に対応する）方法を提供します。変数またはプロパティが最初に初期化される場合は、オブザーバは呼び出されません。代わりに、それらは初期化以外で値が設定されている場合にのみ呼び出されます。

`willSet` オブザーバは、変数またはプロパティの値が設定される直前に呼び出されます。新しい値が定数として `willSet` オブザーバに渡され、したがって `willSet` 句の実装では変更できません。`didSet` オブザーバは、新しい値が設定された直後に呼び出されます。`willSet` オブザーバとは対照的に、それにアクセスする必要がある場合は、変数またはプロパティの古い値が `didSet` オブザーバーに渡されます。つまり、自身の `didSet` オブザーバ句内で変数またはプロパティに値を割り当てた場合、`willSet` オブザーバに渡された値を置き換えます。

`willSet` および `didSet` 句内の setter name とそれを囲む括弧内は省略可能です。setter name を指定した場合は、`willSet` と `didSet` オブザーバの引数名として使用されます。setter name を指定しない場合は、`willSet` オブザーバのデフォルトの引数名が `newValue` で、`didSet` オブザーバのデフォルトの引数名は `oldValue` です。

`didSet` 句は、`willSet` 句を指定した場合は省略可能です。同様に、`didSet` 句を指定するときは、`willSet` 句は省略可能です。

`didSet` オブザーバの本文が古い値を参照している場合、get はオブザーバの前に呼び出され、古い値を使用できます。それ以外の場合は、スーパークラスの get を呼び出さずに新しい値が格納されます。下記の例は、スーパークラスで定義され、オブザーバを追加するためにそのサブクラスでオーバーライドされた計算プロパティを示しています:

```swift
class Superclass {
    private var xValue = 12
    var x: Int {
        get { print("Getter was called"); return xValue }
        set { print("Setter was called"); xValue = newValue }
    }
}

// このサブクラスは オブザーバ の oldValue を参照していません
// SuperClass の get は、値を出力するために一度だけ呼び出されます。
class New: Superclass {
    override var x: Int {
        didSet { print("New value \(x)") }
    }
}
let new = New()
new.x = 100
// "Setter was called"
// "Getter was called"
// "New value 100"

// このサブクラスはそのオブザーバの oldValue を参照しているので、スーパークラスの
// get はセッターの前に一度呼び出され、また値を出力します
class NewAndOld: Superclass {
    override var x: Int {
        didSet { print("Old value \(oldValue) - new value \(x)") }
    }
}
let newAndOld = NewAndOld()
newAndOld.x = 200
// "Getter was called"
// "Setter was called"
// "Getter was called"
// "Old value 12 - new value 200"
```

より詳細な情報やプロパティオブザーバの使用方法の例は、[Property Observers](./../language-guide/properties.md#property-observersプロパティオブザーバ)を参照ください。

### Type Variable Properties(型変数プロパティ)

型変数プロパティを宣言するには、宣言を `static` 修飾子をマークします。クラスは、サブクラスがスーパークラスの実装をオーバーライドできるようにするには、代わりに `class` 修飾子を使用して型計算プロパティをマークすることができます。 型プロパティは、[Type Properties](./../language-guide/properties.md#type-properties型プロパティ)で説明されています。

> GRAMMAR OF A VARIABLE DECLARATION  
> variable-declaration → [variable-declaration-head](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_variable-declaration-head)  [pattern-initializer-list](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_pattern-initializer-list)  
> variable-declaration → [variable-declaration-head](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_variable-declaration-head)  [variable-name](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_variable-name)  [type-annotation](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_type-annotation)  [code-block](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_code-block)  
> variable-declaration → [variable-declaration-head](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_variable-declaration-head)  [variable-name](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_variable-name)  [type-annotation](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_type-annotation)  [getter-setter-block](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_getter-setter-block)  
> variable-declaration → [variable-declaration-head](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_variable-declaration-head)  [variable-name](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_variable-name)  [type-annotation](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_type-annotation)  [getter-setter-keyword-block](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_getter-setter-keyword-block)  
> variable-declaration → [variable-declaration-head](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_variable-declaration-head)  [variable-name](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_variable-name)  [initializer](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_initializer)  [willSet-didSet-block](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_willSet-didSet-block)  
> variable-declaration → [variable-declaration-head](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_variable-declaration-head)  [variable-name](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_variable-name)  [type-annotation](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_type-annotation)  [initializer](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_initializer)<sub>*opt*</sub> [willSet-didSet-block](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_willSet-didSet-block)  
> variable-declaration-head → [attributes](https://docs.swift.org/swift-book/ReferenceManual/Attributes.html#grammar_attributes)<sub>*opt*</sub> [declaration-modifiers](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_declaration-modifiers)<sub>*opt*</sub> `var`  
> variable-name → [identifier](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_identifier)  
> getter-setter-block → [code-block](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_code-block)  
> getter-setter-block → `{` [getter-clause](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_getter-clause)  [setter-clause](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_setter-clause)<sub>*opt*</sub> `}`  
> getter-setter-block → `{` [setter-clause](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_setter-clause)  [getter-clause](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_getter-clause)  `}`  
> getter-clause → [attributes](https://docs.swift.org/swift-book/ReferenceManual/Attributes.html#grammar_attributes)<sub>*opt*</sub> [mutation-modifier](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_mutation-modifier)<sub>*opt*</sub> `get` [code-block](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_code-block)  
> setter-clause → [attributes](https://docs.swift.org/swift-book/ReferenceManual/Attributes.html#grammar_attributes)<sub>*opt*</sub> [mutation-modifier](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_mutation-modifier)<sub>*opt*</sub> `set` [setter-name](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_setter-name)<sub>*opt*</sub> [code-block](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_code-block)  
> setter-name → `(` [identifier](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_identifier)  `)`  
> getter-setter-keyword-block → `{` [getter-keyword-clause](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_getter-keyword-clause)  [setter-keyword-clause](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_setter-keyword-clause)<sub>*opt*</sub> `}`  
> getter-setter-keyword-block → `{` [setter-keyword-clause](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_setter-keyword-clause)  [getter-keyword-clause](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_getter-keyword-clause)  `}`  
> getter-keyword-clause → [attributes](https://docs.swift.org/swift-book/ReferenceManual/Attributes.html#grammar_attributes)<sub>*opt*</sub> [mutation-modifier](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_mutation-modifier)<sub>*opt*</sub> `get`  
> setter-keyword-clause → [attributes](https://docs.swift.org/swift-book/ReferenceManual/Attributes.html#grammar_attributes)<sub>*opt*</sub> [mutation-modifier](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_mutation-modifier)<sub>*opt*</sub> `set`  
> willSet-didSet-block → `{` [willSet-clause](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_willSet-clause)  [didSet-clause](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_didSet-clause)<sub>*opt*</sub> `}`  
> willSet-didSet-block → `{` [didSet-clause](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_didSet-clause)  [willSet-clause](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_willSet-clause)<sub>*opt*</sub> `}`  
> willSet-clause → [attributes](https://docs.swift.org/swift-book/ReferenceManual/Attributes.html#grammar_attributes)<sub>*opt*</sub> `willSet` [setter-name](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_setter-name)<sub>*opt*</sub> [code-block](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_code-block)  
> didSet-clause → [attributes](https://docs.swift.org/swift-book/ReferenceManual/Attributes.html#grammar_attributes)<sub>*opt*</sub> `didSet` [setter-name](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_setter-name)<sub>*opt*</sub> [code-block](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_code-block)

## Type Alias Declaration(タイプエイリアス宣言)

タイプエイリアス宣言では、既存の型に別名を導入します。タイプエイリアス宣言は、`typealias` キーワードを使用して宣言され、形式は次のとおりです:

![タイプエイリアス宣言](./../.gitbook/assets/type_alias_declaration.png)

タイプエイリアスが宣言された後、プログラム内の既存の型の代わりに注釈名を使用できます。既存の型は、名前付き型または複合型にすることができます。タイプエイリアスは新しい型を作成しません。それらは単に名前が既存の型を参照することを可能にします。

タイプエイリアス宣言は、ジェネリック引数を使用して既存のジェネリック型に名前を付けることができます。タイプエイリアスは、既存の型のジェネリック引数の一部または全部に具体的な型を提供できます。例えば:

```swift
typealias StringDictionary<Value> = Dictionary<String, Value>

// 以下の辞書は同じ型を持っています
var dictionary1: StringDictionary<Int> = [:]
var dictionary2: Dictionary<String, Int> = [:]
```

タイプエイリアスがジェネリック引数で宣言されると、それらの引数の制約は既存の型のジェネリック引数の制約と正確に一致する必要があります。例えば:

```swift
typealias DictionaryOfInts<Key: Hashable> = Dictionary<Key, Int>
```

タイプエイリアスと既存の型を互換的に使用できるため、タイプエイリアスは追加のジェネリック制約を導入できません。

タイプエイリアスは、宣言から全てのジェネリック引数を省略することで、既存の型のジェネリック引数を転送できます。例えば、ここで宣言されている `Diccionario` 型の別名は、`Dictionary` と同じジェネリック引数と制約を持ちます。

```swift
typealias Diccionario = Dictionary
```

プロトコル宣言の内部では、タイプエイリアスは頻繁に使用される型に短くて便利な名前を付けることができます。例えば:

```swift
protocol Sequence {
    associatedtype Iterator: IteratorProtocol
    typealias Element = Iterator.Element
}

func sum<T: Sequence>(_ sequence: T) -> Int where T.Element == Int {
    // ...
}
```

このタイプエイリアスがないと、`sum` 関数は関連型を `T.Element` ではなく `T.Iterator.Element` として参照する必要があります。

[Protocol Associated Type Declaration](#protocol-associated-type-declarationプロトコル関連型宣言)も参照ください。

> GRAMMAR OF A TYPE ALIAS DECLARATION  
> typealias-declaration → [attributes](https://docs.swift.org/swift-book/ReferenceManual/Attributes.html#grammar_attributes)<sub>*opt*</sub> [access-level-modifier](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_access-level-modifier)<sub>*opt*</sub> `typealias` [typealias-name](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_typealias-name)  [generic-parameter-clause](https://docs.swift.org/swift-book/ReferenceManual/GenericParametersAndArguments.html#grammar_generic-parameter-clause)<sub>*opt*</sub> [typealias-assignment](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_typealias-assignment)  
> typealias-name → [identifier](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_identifier)  
> typealias-assignment → `=` [type](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_type)

## Function Declaration(関数宣言)

関数宣言では、プログラムに関数またはメソッドを導入します。クラス、構造体、列挙型、またはプロトコルのコンテキストで宣言されている関数はメソッドと呼ばれます。関数宣言は `func` キーワードを使用して宣言され、形式は次のとおりです:

![関数宣言](./../.gitbook/assets/function_declaration.png)

関数に戻り値の型 `Void` がある場合は、次のように戻り値の型を省略できます:

![関数宣言戻り値の型Void](./../.gitbook/assets/function_declaration2.png)

各引数の型を含める必要があります。推論できません。引数型の前に `inout` を書くと、その関数の範囲内で引数を変更できます。In-Out 引数については、下記の[In-Out Parameters](#in-out-parametersIn-Out引数)で詳細に説明されています。

文が単一の式のみを含む関数宣言は、その式の値を返すことが明らかです。この暗黙的なリターン構文は、式の型と関数の戻り型が `Void` ではなく、ケースを持たない `Never` のような列挙型ではない場合にのみ使用できます。

関数は、関数の戻り値の型としてタプル型を使用して複数の値を返すことができます。

関数定義は他の関数宣言の内部に登場することがあります。この種の関数は入れ子関数(*nested functio*)と呼ばれます。

入れ子関数は、In-Out 引数のように決してエスケープしない、または非エスケープ関数引数として渡されることが保証されている値をキャプチャする場合は、非エスケープです。それ以外の場合、入れ子関数はエスケープ関数です。

入れ子関数については、[Nested Functions](./../language-guide/functions.md#nested-Functions入れ子関数)を参照ください。

### Parameter Names

### In-Out Parameters(In-Out引数)

### Special Kinds of Parameters

### Special Kinds of Methods

### Methods with Special Names

### Throwing Functions and Methods

### Rethrowing Functions and Methods

### Asynchronous Functions and Methods

### Functions that Never Return

## Enumeration Declaration

### Enumerations with Cases of Any Type

#### Enumerations with Indirection

---

### Enumerations with Cases of a Raw-Value Type

### Accessing Enumeration Cases

## Structure Declaration

## Class Declaration

## Actor Declaration

## Protocol Declaration

### Protocol Property Declaration(プロトコルプロパティ宣言)

### Protocol Method Declaration

### Protocol Initializer Declaration

### Protocol Subscript Declaration

### Protocol Associated Type Declaration(プロトコル関連型宣言)

## Initializer Declaration

### Failable Initializers

## Deinitializer Declaration

## Extension Declaration

### Conditional Conformance

#### Overridden Requirements Aren’t Used in Some Generic Contexts

---

### Protocol Conformance Must Not Be Redundant

#### Resolving Explicit Redundancy

---

#### Resolving Implicit Redundancy

---

## Subscript Declaration

### Type Subscript Declarations

## Operator Declaration

## Precedence Group Declaration

## Declaration Modifiers

## Access Control Levels(アクセス制御レベル)
