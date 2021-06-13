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

## Constant Declaration

## Variable Declaration

### Stored Variables and Stored Variable Properties

### Computed Variables and Computed Properties

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

### Protocol Property Declaration

### Protocol Method Declaration

### Protocol Initializer Declaration

### Protocol Subscript Declaration

### Protocol Associated Type Declaration

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
