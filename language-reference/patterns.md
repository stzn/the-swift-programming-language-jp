# Patterns \(パターン\)

最終更新日:

パターンは、単一の値または複合値の構造を表します。例えば、タプル `(1、2)` の構造は、2 つの要素のカンマ区切り(`,`)のリストです。パターンは特定の値ではなく値の構造を表すため、様々な値と一致させることができます。例えば、パターン `(x、y)` は、タプル `(1、2)` およびその他の 2 つの要素のタプルと一致します。パターンを値と照合することに加えて、複合値の一部または全てを抽出し、各部分を定数または変数名にバインドできます。

Swift には、2 つの基本的な種類のパターンがあります: 任意の種類の値に正常に一致するパターンと、実行時に指定された値に一致しない可能性のあるパターンです。

最初の種類のパターンは、シンプルな変数、定数、およびオプショナルバインディングの値を破棄するために使用されます。これらには、ワイルドカードパターン、識別子パターン、およびそれらを含む値バインディングまたはタプルパターンが含まれます。これらのパターンの型注釈を指定して、特定の型の値にのみ一致するようにパターンを制約できます。

2 番目の種類のパターンは、完全なパターンマッチングに使用されます。この場合、照合しようとしている値が実行時に存在しない可能性があります。これらには、列挙型のケースパターン、オプショナルパターン、式パターン、および型キャストパターンが含まれます。これらのパターンは、`switch` 文の `case` ラベル、`do` 文の `catch` 句、または `if`、`while`、`guard`、または `for-in` 文の `case` 条件で使用します。

> GRAMMAR OF A PATTERN  
> pattern → [wildcard-pattern](https://docs.swift.org/swift-book/ReferenceManual/Patterns.html#grammar_wildcard-pattern)  [type-annotation](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_type-annotation)<sub>*opt*</sub>  
> pattern → [identifier-pattern](https://docs.swift.org/swift-book/ReferenceManual/Patterns.html#grammar_identifier-pattern)  [type-annotation](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_type-annotation)<sub>*opt*</sub>  
> pattern → [value-binding-pattern](https://docs.swift.org/swift-book/ReferenceManual/Patterns.html#grammar_value-binding-pattern)  
> pattern → [tuple-pattern](https://docs.swift.org/swift-book/ReferenceManual/Patterns.html#grammar_tuple-pattern)  [type-annotation](https://docs.swift.org/swift-book/ReferenceManual/Types.html#grammar_type-annotation)<sub>*opt*</sub>  
> pattern → [enum-case-pattern](https://docs.swift.org/swift-book/ReferenceManual/Patterns.html#grammar_enum-case-pattern)  
> pattern → [optional-pattern](https://docs.swift.org/swift-book/ReferenceManual/Patterns.html#grammar_optional-pattern)  
> pattern → [type-casting-pattern](https://docs.swift.org/swift-book/ReferenceManual/Patterns.html#grammar_type-casting-pattern)  
> pattern → [expression-pattern](https://docs.swift.org/swift-book/ReferenceManual/Patterns.html#grammar_expression-pattern)

## Wildcard Pattern\(ワイルドカードパターン\)

## Identifier Pattern\(識別子パターン\)

## Value-Binding Pattern\(値バインディング\)

## Tuple Pattern\(タプル\)

## Enumeration Case Pattern\(列挙型のケースパターン\)

## Optional Pattern\(オプショナルパターン\)

## Type-Casting Patterns\(式パターン\)

## Expression Pattern\(型キャストパターン\)
