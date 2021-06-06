# Advanced Operators

最終更新日:

[Basic Operators](./basic-operators.md)で説明されている演算子に加えて、Swift は、より複雑な値操作を実行するいくつかの高度な演算子を提供します。これらには、C 言語および Objective-C でおなじみの全てのビット単位およびビットシフト演算子が含まれます。

C 言語の算術演算子とは異なり、Swift の算術演算子はデフォルトでオーバーフローしません。オーバーフロー動作はトラップされ、エラーとして報告されます。オーバーフロー動作を選択するには、オーバーフロー加算演算子(`&+`)など、デフォルトでオーバーフローする Swift の 2 番目の算術演算子セットを使用します。これらのオーバーフロー演算子は全て、アンパサンド(`&`)で始まります。

独自の構造体、クラス、および列挙型を定義する場合、これらの独自型に標準の Swift 演算子の独自の実装を提供すると便利な場合があります。Swift では、簡単にこれらの演算子のカスタマイズ実装を提供し、作成した型ごとにそれらの動作を正確に決めることができます。

事前定義された演算子に限定されません。Swift では、独自の優先順位と結合規則を使用して、独自の前置、中置、後置、および代入演算子を自由に定義できます。これらの演算子は、事前定義の演算子と同様にコードで使用および適用でき、定義した独自の演算子をサポートするように既存の型を拡張することもできます。

## Bitwise Operators(ビット演算子)

ビットごとの演算子を使用すると、データ構造内の個々の生データビットを操作できます。これらは、グラフィクスプログラミングやデバイスドライバーの作成など、低レベルのプログラミングでよく使用されます。ビットごとの演算子は、独自のプロトコルを介した通信用データのエンコードおよびデコードなど、外部ソースからの生データを操作する場合にも役立ちます。

Swift は、下記で説明するように、C 言語に見られる全てのビット演算子をサポートしています。

### Bitwise NOT Operator(ビットNOT演算子)

ビット NOT 演算子(`~`)は、数値の全てのビットを反転します。

![ビットNOT演算子](./../.gitbook/assets/bitwiseNOT_2x.png)

ビット NOT 演算子は前置演算子で、空白を含めずに演算対象の値の直前に置きます。

```swift
let initialBits: UInt8 = 0b00001111
let invertedBits = ~initialBits  // 11110000 と等しい
```

`UInt8` 整数には 8 ビットの、`0` から `255` までの任意の値を格納できます。この例では、`UInt8` 整数をバイナリ値 `00001111` で初期化し、最初の 4 ビットが `0` に設定され、2 番目の 4 ビットが `1` に設定されています。これは、10 進数の `15` に相当します。

次に、ビット NOT 演算子を使用して、`invertedBits` と呼ばれる新しい定数を作成します。これは `initialBits` と同じですが、全てのビットが反転されています。ゼロは 1 になり、1 はゼロになります。`invertedBits` の値は `11110000` で、符号なし 10 進数の `240` に相当します。

### Bitwise AND Operator(ビットAND演算子)

ビット AND 演算子(`&`)は、2 つの数値のビットを合成します。両方の入力数値のビットが 1 の場合にのみ、ビットが 1 になり、新しい数値を返します:

![ビットAND演算子](./../.gitbook/assets/bitwiseAND_2x.png)

下記の例では、`firstSixBits` と `lastSixBits` の値は両方とも、1 に等しい 4 つの中間ビットを持っています。ビット AND 演算子はそれらを組み合わせて、符号なし 10 進数の `60` に等しい数値 `00111100` を作成します。

```swift
let firstSixBits: UInt8 = 0b11111100
let lastSixBits: UInt8  = 0b00111111
let middleFourBits = firstSixBits & lastSixBits  // 00111100 と等しい
```

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