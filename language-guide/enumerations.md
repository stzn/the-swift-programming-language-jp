# Enumerations

最終更新日:

列挙型(*enumerations*)は、関連する値のグループに共通の型を定義し、コード内で型安全にそれらの値を操作できるようにします。

C 言語に馴染みがあるれば、C 言語の列挙型が、関連する名前のグループを整数値のセットに割り当てることを知っていると思います。Swift の列挙型は、それよりもはるかに柔軟性があり、列挙型の各ケースに値を割り当てる必要はありません。列挙型ごとに（*raw value*と呼ばれる）値が与えられている場合、値は文字列、文字、または任意の整数型または浮動小数点型の値にすることができます。

あるいは、他の言語の `unions` や `variants` のように、それぞれの case で任意の型の関連値を格納できます。それぞれで適切な型の異なる値のセットで関連付けられた
case の共通セットをある列挙型の一部として定義できます。

Swift の列挙型は、それ自体がファーストクラスの型です。これらは、列挙型の現在値の追加情報を提供する計算プロパティや、列挙型が表す値に関わる機能を提供するインスタンスメソッドなど、従来は class でのみサポートされていた多くの機能を使うことができます。列挙型は、初期の case 値を指定するイニシャライザを定義することもできます。元の実装を超えて機能を拡張するように extension も使用することができます。また、プロトコルに準拠して標準機能を提供することもできます。

これらの機能の詳細については、[Properties](./properties.md), [Methods](./methods.md), [Initialization](./initialization.md), [Extensions](../language-guide/extensions.md)および[Protocols](./protocols.md)を参照ください。

## Enumeration Syntax

## Matching Enumeration Values with a Switch Statement

## Iterating over Enumeration Cases

## Associated Values

## Raw Values

### Implicitly Assigned Raw Values

### Initializing from a Raw Value

## Recursive Enumerations
