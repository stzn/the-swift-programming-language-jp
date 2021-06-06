# Advanced Operators

最終更新日:

[Basic Operators](./basic-operators.md)で説明されている演算子に加えて、Swift は、より複雑な値操作を実行するいくつかの高度な演算子を提供します。これらには、C 言語および Objective-C でおなじみの全てのビット単位およびビットシフト演算子が含まれます。

C 言語の算術演算子とは異なり、Swift の算術演算子はデフォルトでオーバーフローしません。オーバーフロー動作はトラップされ、エラーとして報告されます。オーバーフロー動作を選択するには、オーバーフロー加算演算子(`&+`)など、デフォルトでオーバーフローする Swift の 2 番目の算術演算子セットを使用します。これらのオーバーフロー演算子は全て、アンパサンド(`&`)で始まります。

独自の構造体、クラス、および列挙型を定義する場合、これらの独自型に標準の Swift 演算子の独自の実装を提供すると便利な場合があります。Swift では、簡単にこれらの演算子のカスタマイズ実装を提供し、作成した型ごとにそれらの動作を正確に決めることができます。

事前定義された演算子に限定されません。Swift では、独自の優先順位と結合規則を使用して、独自の前置、中置、後置、および代入演算子を自由に定義できます。これらの演算子は、事前定義の演算子と同様にコードで使用および適用でき、定義した独自の演算子をサポートするように既存の型を拡張することもできます。

## Bitwise Operators(ビット演算子)

### Bitwise NOT Operator(ビットNOT演算子)

### Bitwise AND Operator(ビットAND演算子)

### Bitwise OR Operator(ビットOR演算子)

### Bitwise XOR Operator(ビットXOR演算子)

### Bitwise Left and Right Shift Operators(ビット左右シフト演算子)

#### Shifting Behavior for Unsigned Integers(符号なし整数のシフト演算)

#### Shifting Behavior for Signed Integers(符号あり整数のシフト演算)

## Overflow Operators(オーバーフロー演算子)

### Value Overflow(値のオーバーフロー)

## Precedence and Associativity(優先順位と結合規則)

## Operator Methods(演算子メソッド)

### Prefix and Postfix Operators(前置、後置演算子)

### Compound Assignment Operators(合成代入演算子)

### Equivalence Operators(比較演算子)

## Custom Operators(カスタム演算子)

### Precedence for Custom Infix Operators(カスタム中置演算子の優先順位)

## Result Builders(リザルトビルダー)