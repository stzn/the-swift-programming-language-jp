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

## Top-Level Code

## Code Blocks

## Import Declaration

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

## Access Control Levels
