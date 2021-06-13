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

変数宣言には、格納/計算変数や格納/計算プロパティ、プロパティオブザーバ、および静的変数プロパティなど、様々な種類の名前付きで可変な値を宣言するいくつかの形式があります。適切な形式は、変数が宣言されているスコープと宣言する予定の変数の種類によって異なります。

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

この形式の変数宣言は、グローバルスコープ、関数のローカルスコープ、またはクラス、構造体、列挙型、または extension のコンテキストで定義できます。この形式の変数宣言がグローバルスコープまたは関数のローカルスコープで宣言されている場合は、計算変数(*computed variable*)と呼ばれます。クラス、構造体、または extension のコンテキストで宣言されている場合は、それは計算プロパティ(*computed property*)と呼ばれています。

get は値を読み取るために使用され、set は値を書き込むために使用されます。set 句は省略可能で、get のみが必要な場合は、[Read-Only Computed Properties](./../language-guide/properties.md#read-Only-computed-properties読み取り専用計算プロパティ)で説明されているように、両方の句を省略し、シンプルに要求された値を直接返すことができます。しかし、set 句を指定した場合は、get 句も提供する必要があります。

setter name とそれを囲む括弧は省略可能です。setter name を指定した場合は、set への引数の名前として使用されます。setter name を指定しない場合は、[Shorthand Setter Declaration](./../language-guide/properties.md#shorthand-setter-declaration短縮setプロパティ宣言)で説明されているように、set へのデフォルトの引数名は `newValue` です。

格納変数や格納可変プロパティとは異なり、計算変数または計算プロパティの値はメモリに格納されません。

詳細や計算プロパティの例は、[Computed Properties](./../language-guide/properties.md#computed-properties計算プロパティ)を参照ください。

### Stored Variable Observers and Property Observers

### Type Variable Properties

## Type Alias Declaration

## Function Declaration

### Parameter Names

### In-Out Parameters

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

### Protocol イニシャライザ Declaration

### Protocol Subscript Declaration

### Protocol Associated Type Declaration

## イニシャライザ Declaration

### Failable イニシャライザs

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
