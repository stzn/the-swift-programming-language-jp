# 高度な演算子\(Advanced Operators\)

最終更新日: 2021/7/1

[Basic Operators\(基本演算子\)](basic-operators.md)で説明されている演算子に加えて、Swift は、より複雑な値操作を実行するいくつかの高度な演算子を提供します。これらには、C 言語および Objective-C でおなじみの全てのビット演算子およびビットシフト演算子が含まれます。

C 言語の算術演算子とは異なり、Swift の算術演算子はデフォルトでオーバーフローしません。オーバーフローはトラップされ、エラーとして報告されます。オーバーフローするには、オーバーフロー加算演算子\(`&+`\)など、デフォルトでオーバーフローする Swift の第 2 の算術演算子を使用します。これらのオーバーフロー演算子は全て、アンパサンド\(`&`\)で始まります。

独自の構造体、クラス、および列挙型を定義する場合、標準の Swift 演算子の実装にこれらの独自型の実装を提供すると便利な場合があります。Swift では、簡単にこれらの演算子のカスタマイズ実装を提供し、作成した型ごとにそれらの動作を正確に決めることができます。

事前定義された演算子に限定されません。Swift では、独自の優先順位と結合規則を使用して、独自の前置、中置、後置、および代入演算子を自由に定義できます。これらの演算子は、事前定義の演算子と同様にコードで使用および適用でき、定義した独自の演算子をサポートするように既存の型を拡張することもできます。

## ビット演算子\(Bitwise Operators\)

ビット演算子を使用すると、データ構造内の個々のデータのビットを操作できます。これらは、グラフィックスプログラミングやデバイスドライバの作成など、低レベルのプログラミングでよく使用されます。ビット演算子は、独自のプロトコルを介した通信用データのエンコードおよびデコードなど、外部ソースからのデータを操作する場合にも役立ちます。

Swift は、下記で説明するように、C 言語の全てのビット演算子をサポートしています。

### ビット論理否定演算子\(Bitwise NOT Operator\)

ビット論理否定演算子\(`~`\)は、数値の全てのビットを反転します。

![&#x30D3;&#x30C3;&#x30C8;&#x8AD6;&#x7406;&#x5426;&#x5B9A;&#x6F14;&#x7B97;&#x5B50;](../.gitbook/assets/bitwiseNOT_2x.png)

ビット論理否定演算子は前置演算子で、空白を含めずに演算対象の値の直前に置きます。

```swift
let initialBits: UInt8 = 0b00001111
let invertedBits = ~initialBits  // 11110000 と等しい
```

`UInt8` 整数には 8 ビットの、`0` から `255` までの任意の値を格納できます。この例では、`UInt8` 整数を 2 進数値 `00001111` で初期化し、最初の 4 ビットが `0` に設定され、2 番目の 4 ビットが `1` に設定されています。これは、10 進数の `15` に相当します。

次に、ビット論理否定演算子を使用して、`invertedBits` と呼ばれる新しい定数を作成します。これは `initialBits` と同じですが、全てのビットが反転されます。`0` は `1` になり、`1` は `0` になります。`invertedBits` の値は `11110000` で、符号なし 10 進数の `240` に相当します。

### ビット論理積演算子\(Bitwise AND Operator\)

ビット論理積演算子\(`&`\)は、2 つの数値のビットを合成します。両方の入力数値のビットが `1` の場合にのみ、ビットが `1` になり、新しい数値を返します:

![&#x30D3;&#x30C3;&#x30C8;&#x8AD6;&#x7406;&#x7A4D;&#x6F14;&#x7B97;&#x5B50;](../.gitbook/assets/bitwiseAND_2x.png)

下記の例では、`firstSixBits` と `lastSixBits` の値は両方とも、4 つの中間ビットに `1` が設定されています。ビット論理積演算子はそれらを組み合わせて、符号なし 10 進数の `60` に相当する数値 `00111100` を作成します。

```swift
let firstSixBits: UInt8 = 0b11111100
let lastSixBits: UInt8  = 0b00111111
let middleFourBits = firstSixBits & lastSixBits  // 00111100 と等しい
```

### ビット論理和演算子\(Bitwise OR Operator\)

ビット論理和演算子\(`|`\)は、2 つの数値のビットを比較します。演算子は、いずれかの入力数値のビットが `1` に等しい場合、ビットは `1` になり、新しい数値を返します:

![&#x30D3;&#x30C3;&#x30C8;&#x8AD6;&#x7406;&#x548C;&#x6F14;&#x7B97;&#x5B50;](../.gitbook/assets/bitwiseOR_2x.png)

下記の例では、`someBits` と `moreBits` の値の異なるビットに `1` が設定されています。ビット論理和演算子はそれらを組み合わせて、符号なし 10 進数の `254` に相当する `11111110` という数値を作成します。

```swift
let someBits: UInt8 = 0b10110010
let moreBits: UInt8 = 0b01011110
let combinedbits = someBits | moreBits  // 11111110 と等しい
```

### ビット排他的論理和演算子\(Bitwise XOR Operator\)

ビット排他的論理和演算子\(`^`\)は、2 つの数値のビットを比較します。演算子は、入力ビットが異なる場合はビットに `1` が設定され、入力ビットが同じ場合は `0` が設定される新しい数値を返します:

![&#x30D3;&#x30C3;&#x30C8;&#x6392;&#x4ED6;&#x7684;&#x8AD6;&#x7406;&#x548C;&#x6F14;&#x7B97;&#x5B50;](../.gitbook/assets/bitwiseXOR_2x.png)

下記の例では、`firstBits` と `otherBits` の値はそれぞれ、他の場所にはないビットに `1` が設定されています。ビット排他的論理和演算子は、これらのビット両方に `1` を設定します。`firstBits` および `otherBits` の他の全てのビットは値が一致しているので、`0` が設定されます。

```swift
let firstBits: UInt8 = 0b00010100
let otherBits: UInt8 = 0b00000101
let outputBits = firstBits ^ otherBits  // 00010001 と等しい
```

### <a id="bitwise-left-and-right-shift-operators">ビット左右シフト演算子\(Bitwise Left and Right Shift Operators\)</a>

ビット左シフト演算子\(`<<`\)およびビット右シフト演算子\(`>>`\)は、下記で定義されている規則に従って、数値内の全てのビットを特定の桁数だけ左または右に移動します。

ビット左シフトと右シフトには、整数を 2 倍に乗算または除算する効果があります。整数のビットを 1 桁左にシフトするとその値は 2 倍になり、1 桁右にシフトすると値は半分になります。

#### 符号なし整数のシフト演算\(Shifting Behavior for Unsigned Integers\)

符号なし整数のビットシフト動作は次のとおりです:

1. 既存のビットは、要求された桁数だけ左または右に移動します
2. 整数のストレージの境界を超えて移動したビットは全て破棄されます
3. 元のビットを左または右に移動した後に残ったスペースに `0` が挿入されます

このアプローチは、_論理シフト_として知られています。

下の図は、`11111111 << 1`\(`11111111` を 1 桁左にずらしたもの\)と `11111111 >> 1`\(`11111111` を右に 1 桁ずらしたもの\)の結果を示しています。青色の数字はシフトされ、灰色の数字は破棄され、オレンジ色のには 0 が挿入されます。

![&#x7B26;&#x53F7;&#x306A;&#x3057;&#x6574;&#x6570;&#x306E;&#x8AD6;&#x7406;&#x30B7;&#x30D5;&#x30C8;](../.gitbook/assets/bitshiftUnsigned_2x.png)

Swift コードでのビットシフトの様子は次のとおりです:

```swift
let shiftBits: UInt8 = 4   // 2進数内の 00000100
shiftBits << 1             // 00001000
shiftBits << 2             // 00010000
shiftBits << 5             // 10000000
shiftBits << 6             // 00000000
shiftBits >> 2             // 00000001
```

ビットシフトを使用して、他のデータ型内の値をエンコードおよびデコードできます:

```swift
let pink: UInt32 = 0xCC6699
let redComponent = (pink & 0xFF0000) >> 16    // redComponent は 0xCC または 204
let greenComponent = (pink & 0x00FF00) >> 8   // greenComponent は 0x66 または 102
let blueComponent = pink & 0x0000FF           // blueComponent は 0x99 または 153
```

この例では、`pink` という `UInt32` 定数を使用して、ピンク色の CSS の値を格納します。CSS カラー値 `#CC6699` は、Swift の 16 進数で `0xCC6699` になります。次に、この色は、ビット論理積演算子\(`&`\)とビット右シフト演算子\(`>>`\)によって、赤\(`CC`\)、緑\(`66`\)、および青\(`99`\)のコンポーネントに分解されます。

赤のコンポーネントは、数値 `0xCC6699` と `0xFF0000` の間でビット論理積演算子を使用することによって取得されます。`0xFF0000` の 0 は、`0xCC6699` の 2 番目と 3 番目のバイトを効果的に「マスク」し、`6699` を無視して、結果として `0xCC0000` を残します。

次に、この番号が 16 桁右にシフトされます\(`>> 16`\)。16 進数の文字の各ペアは 8 ビットを使用するため、16 桁右に移動すると、`0xCC0000` が `0x0000CC` に変換されます。これは、10 進数 `204` に相当する、 `0xCC` が得られます。

同様に、緑のコンポーネントは、数値 `0xCC6699` と `0x00FF00` の間でビット論理積演算子を使用することによって取得され、`0x006600` の出力値が得られます。次に、この出力値が 8 桁右にシフトされ、10 進数 `102` に相当する、`0x66` が得られます。

最後に、青のコンポーネントは、数値 `0xCC6699` と `0x0000FF` の間でビット論理積演算子を使用することによって取得され、`0x000099` の出力値が得られます。`0x000099` はすでに 10 進数 `153` に相当し、 `0x99` に等しいため、この値は右にシフトせずに使用されます。

#### 符号付き整数のシフト演算\(Shifting Behavior for Signed Integers\)

符号付き整数のシフト演算は、符号なし整数よりも符号付き整数の方が複雑です。これは、符号付き整数が 2 進数で表現される方法に依ります。\(下記の例は、簡単にするために 8 ビットの符号付き整数に基づいていますが、同じ原則が任意のサイズの符号付き整数にも適用されます\)

符号付き整数は、最初のビット\(_符号ビット_と呼ばれます\)を使用して、整数が正か負かを示します。符号ビット `0` は正を意味し、符号ビット `1` は負を意味します。

残りのビット\(_値ビット_と呼ばれます\)には、実際の値が格納されています。正の数は、符号なし整数の場合とまったく同じ方法で、`0` から数えて格納されます。`Int8` 内からビットで数値 `4` を表す方法は次のとおりです:

![&#x7B26;&#x53F7;&#x4ED8;&#x304D;&#x6574;&#x6570; 4](../.gitbook/assets/bitshiftSignedFour_2x.png)

符号ビットは `0`\(「正」を意味します\)で、7 つの値ビットは 2 進数表記で書かれた数字の `4` です。

ただし、負の数は別の方法で格納されます。それらは、`2` から `n` のべき乗の絶対値を引くことによって保持されます。ここで、`n` は値のビット数です。8 ビットの数値には 7 つの値ビットがあるため、これは 2 の 7 乗、つまり `128` を意味します。

`Int8` 内からビットで数値 `-4` を表す方法は次のとおりです:

![&#x7B26;&#x53F7;&#x4ED8;&#x304D;&#x6574;&#x6570; -4](../.gitbook/assets/bitshiftSignedMinusFour_2x.png)

今回は、符号ビットは `1`\(「負」を意味する\)で、7 つの値ビットは `124` の 2 進数値\(`128 - 4`\)です:

![&#x7B26;&#x53F7;&#x4ED8;&#x304D;&#x6574;&#x6570; -4 &#x306E;&#x5024;&#x30D3;&#x30C3;&#x30C8;](../.gitbook/assets/bitshiftSignedMinusFourValue_2x.png)

負の数のこのエンコーディングは、2 の_補数表現_と呼ばれています。負の数を表すのに珍しい方法に見えるかもしれませんが、いくつかの利点があります。

まず、\(符号ビットを含む\)8 ビット全てに標準の 2 進数加算を実行し、完了したら 8 ビットに収まらないものは全て破棄するだけで、`-1` を `-4` に加算できます:

![&#x7B26;&#x53F7;&#x4ED8;&#x304D;&#x6574;&#x6570;&#x306E;&#x52A0;&#x7B97;](../.gitbook/assets/bitshiftSignedAddition_2x.png)

次に、2 の補数表現で、負の数のビットを正の数のように左右にシフトし、左にシフトするたびにビットを 2 倍し、右にシフトするたびにビットを半分にします。これを実現するために、符号付き整数を右にシフトするときに追加の規則が使用されます。符号付き整数を右にシフトするときは、符号なし整数と同じ規則を適用しますが、左側の空のビットを 0 で埋めるのではなく、符号ビットで埋めます。

![&#x7B26;&#x53F7;&#x4ED8;&#x304D;&#x6574;&#x6570;&#x306E;&#x30B7;&#x30D5;&#x30C8;](../.gitbook/assets/bitshiftSigned_2x.png)

このアクションにより、符号付き整数が右にシフトされた後も同じ符号を持つようになります。これは_算術シフト_と呼ばれています。

正の数と負の数は特殊な方法で保持されるため、どちらかを右にシフトすると `0` に近づきます。このシフト中でも符号ビットを保つということは、値が `0` に近づいても負の整数は負の値のままになることを意味します。

## <a id="overflow-operators">オーバーフロー演算子\(Overflow Operators\)</a>

その値を保持できない整数の定数または変数に数値を挿入しようとすると、Swift はデフォルトでは無効な値は作成せず、エラーを報告します。この動作により、大きすぎたり小さすぎたりする数値を処理するときの安全性がさらに高まります。

例えば、`Int16` 整数型は、`-32768` から `32767` までの任意の符号付き整数を保持できます。`Int16` の定数または変数に範囲外の数値を設定しようとすると、エラーが発生します。

```swift
var potentialOverflow = Int16.max
// potentialOverflow は 32767 に等しく、これは Int16 が保持できる最大値です
potentialOverflow += 1
// エラーが発生します
```

値が大きすぎたり小さすぎたりした場合のエラー処理を提供すると、境界値条件をコーディングする際の柔軟性が大幅に向上します。

ただし、オーバーフロー条件で使用可能なビット数を切り捨てる必要がある場合は、エラーをトリガするのではなく、この動作を選択できます。Swift は、整数計算のオーバーフロー動作を選択する 3 つの算術_オーバーフロー演算子_を提供します。これらの演算子は全てアンパサンド\(`&`\)で始まります。

* オーバーフロー加算\(`&+`\)
* オーバーフロー減算\(`&-`\)
* オーバーフロー乗算\(`&*`\)

### <a id="value-overflow">値のオーバーフロー\(Value Overflow\)</a>

数値は、正の方向と負の方向の両方でオーバーフローする可能性があります。

オーバーフロー加算演算子\(`&+`\)を使用して、符号なし整数を正の方向にオーバーフローさせた場合の例を次に示します:

```swift
var unsignedOverflow = UInt8.max
// unsignedOverflow は 255 に等しく、これは UInt8 が保持できる最大値です
unsignedOverflow = unsignedOverflow &+ 1
// unsignedOverflow は現在 0 に等しい
```

変数 `unsignedOverflow` は、`UInt8` が保持できる最大値\(`255`、または 2 進数で `11111111`\)で初期化されます。次に、オーバーフロー加算演算子\(`&+`\)を使用して 1 ずつインクリメントします。これにより、`UInt8` が保持できるサイズを超えてその 2 進数表現がプッシュされ、下の図に示すように、境界を超えてオーバーフローします。オーバーフロー加算後に `UInt8` の境界内に残っている値は `00000000`、つまり `0` です:

![&#x30AA;&#x30FC;&#x30D0;&#x30FC;&#x30D5;&#x30ED;&#x30FC;&#x52A0;&#x7B97;&#x6F14;&#x7B97;&#x5B50; 11111111 &#x306B; 1 &#x3092;&#x52A0;&#x7B97;](../.gitbook/assets/overflowAddition_2x.png)

符号なし整数が負の方向にオーバーフローできる場合にも、同様のことが起こります。オーバーフロー減算演算子\(`&-`\)を使用した例を次に示します:

```swift
var unsignedOverflow = UInt8.min
// unsignedOverflow は 0 に等しく、UInt8 が保持できる最小値です
unsignedOverflow = unsignedOverflow &- 1
// unsignedOverflow は 255 になりました
```

`UInt8` が保持できる最小値は `0`、つまり 2 進数で `00000000` です。オーバーフロー減算演算子\(`&-`\)を使用して `00000000` から `1` を減算すると、数値はオーバーフローして `11111111`、つまり 10 進数で `255` に巻き戻ります。

![&#x30AA;&#x30FC;&#x30D0;&#x30FC;&#x30D5;&#x30ED;&#x30FC;&#x6E1B;&#x7B97;&#x6F14;&#x7B97;&#x5B50; 00000000 &#x304B;&#x3089; 1 &#x3092;&#x6E1B;&#x7B97;](../.gitbook/assets/overflowUnsignedSubtraction_2x.png)

オーバーフローは、符号付き整数でも発生します。符号付き整数の加算と減算は全てビット単位で実行され、[Bitwise Left and Right Shift Operators\(ビット左右シフト演算子\)](advanced-operators.md#bitwise-left-and-right-shift-operators)で説明されているように、加算または減算される数値の一部として符号ビットが含まれます。

```swift
var signedOverflow = Int8.min
// signedOverflow は -128 に等しく、これは Int8 が保持できる最小値です
signedOverflow = signedOverflow &- 1
// signedOverflow は 127 になりました
```

`Int8` が保持できる最小値は `-128`、つまり 2 進数で `10000000` です。オーバーフロー演算子を使用してこの 2 進数から `1` を減算すると、`01111111` の 2 進数値が得られます。これは、符号ビットを切り替えて、`Int8` が保持できる正の最大値 `127` を返します。

![&#x7B26;&#x53F7;&#x4ED8;&#x304D;&#x6574;&#x6570;&#x306E;&#x30AA;&#x30FC;&#x30D0;&#x30FC;&#x30D5;&#x30ED;&#x30FC;&#x6E1B;&#x7B97;&#x6F14;&#x7B97;&#x5B50; 10000000 &#x304B;&#x3089; 1 &#x3092;&#x6E1B;&#x7B97;\`](../.gitbook/assets/overflowSignedSubtraction_2x.png)

符号付き整数と符号なし整数の両方で、正の方向のオーバーフローは有効な整数値の最大値から最小値に巻き戻り、負の方向のオーバーフローは最小値から最大値に巻き戻ります。

## [Precedence and Associativity\(優先順位と結合規則\)](advanced-operators.md) <a id="#precedence-and-associativity"></a>

演算子の_優先順位_により、一部の演算子は他の演算子よりも優先されます。これらの演算子は最初に適用されます。

演算子の_結合規則_は、同じ優先順位の演算子をグループ化する方法\(左からグループ化するか、右からグループ化するか\)を定義します。「それらは左側の表現のグループに所属する」または「それらは右側の表現のグループに所属する」という意味だと考えてください。

複合式が計算される順序を決めるときは、各演算子の優先順位と結合規則を考慮することが重要です。例えば、演算子の優先順位は、次の式が `17` に等しい理由を説明しています。

```swift
2 + 3 % 4 * 5
// これは 17 に等しい
```

左から右に厳密に読むと、式は次のように計算されると思われます。

* `2` 足す `3` は `5` に等しい
* `5` 割る `4` の余りは `1` に等しい
* `1` かける `5` は `5` に等しい

ただし、実際の答えは `5` ではなく `17` です。優先順位の高い演算子は、優先順位の低い演算子の前に評価されます。Swift では、C 言語と同様に、剰余演算子\(`%`\)と乗算演算子\(`*`\)が加算演算子\(`+`\)よりも優先されます。その結果、加算される前に両方が評価されます。

ただし、剰余と乗算の優先順位は互いに同じです。使用する正確な評価順序を決定するには、それらの結合規則も考慮する必要があります。剰余と乗算はどちらも、左側の式にグループ化されています。これは、式のこれらの部分の左から始めて、暗黙の括弧\(`()`\)を追加するものだと考えてください。

```swift
2 + ((3 % 4) * 5)
```

`(3 % 4)` は `3` なので、これは次と等価です。

```swift
2 + (3 * 5)
```

`(3 * 5)` は `15` なので、これは次と等価です。

```swift
2 + 15
```

この計算により、最終的な答えは `17` になります。

演算子の優先順位グループと結合規則の完全なリストを含む、Swift 標準ライブラリが提供する演算子については、[Operator Declarations\(演算子宣言\)](https://developer.apple.com/documentation/swift/swift_standard_library/operator_declarations)を参照ください。

> NOTE  
> Swift の演算子の優先順位と結合規則は、C 言語や Objective-C のものよりもシンプルで結果が予測しやすくなっています。ただし、これは、C 言語ベースの言語とまったく同じではないことを意味します。既存のコードを Swift に移植するときは、演算子の作用が意図したとおりに動作するかを確認してください。

## <a id="advanced-operators-operator-methods">演算子メソッド\(Operator Methods\)</a>

クラスと構造体は、既存の演算子に独自の実装を提供できます。これは、既存の演算子のオーバーロードとして呼ばれています。

下記の例は、独自の構造体に算術加算演算子\(`+`\)を実装する方法を示しています。算術加算演算子は、2 つのターゲットで動作するため二項演算子で、これら 2 つのターゲットの間に現れるため、_中置_と呼ばれます。

この例では、2 次元位置ベクトル `(x, y)` の `Vector2D` 構造体を定義し、その後に `Vector2D` 構造体のインスタンスを加算する演算子メソッドの定義が続きます。

```swift
struct Vector2D {
    var x = 0.0, y = 0.0
}

extension Vector2D {
    static func + (left: Vector2D, right: Vector2D) -> Vector2D {
        return Vector2D(x: left.x + right.x, y: left.y + right.y)
    }
}
```

演算子メソッドは、`Vector2D` の型メソッドとして定義され、オーバーロードされる演算子\(`+`\)に一致するメソッド名を使用します。加算はベクトルの本質的な動作の一部ではないため、型メソッドは、`Vector2D` のメインの宣言ではなく、extension で定義されています。算術加算演算子は二項演算子のため、この演算子メソッドは `Vector2D` 型の 2 つの入力パラメータを受け取り、同じく `Vector2D` 型の 1 つの出力値を返します。

この実装では、`+` 演算子の左側と右側にある `Vector2D` インスタンスを表すために、入力パラメータに `left` および `right` という名前が付けられています。このメソッドは、新しい `Vector2D` インスタンスを返します。このインスタンスの `x` および `y` プロパティは、2 つの `Vector2D` インスタンスの `x` プロパティと `y` プロパティの合計で初期化されています。

型メソッドは、既存の `Vector2D` インスタンス間の中置演算子として使用できます。

```swift
let vector = Vector2D(x: 3.0, y: 1.0)
let anotherVector = Vector2D(x: 2.0, y: 4.0)
let combinedVector = vector + anotherVector
// combinedVector は、値が (5.0, 5.0) の Vector2D インスタンスです。
```

この例では、下記に示すように、ベクトル `(3.0, 1.0)` と `(2.0, 4.0)` を加算してベクトル `(5.0, 5.0)` を作成します:

![Vector2D&#x306E;&#x52A0;&#x7B97;](../.gitbook/assets/vectorAddition_2x.png)

### <a id="prefix-and-postfix-operators">前置、後置演算子\(Prefix and Postfix Operators\)</a>

上記の例は、2 進数の中置演算子の独自実装を示しています。クラスと構造体は、単項演算子の実装を提供することもできます。単項演算子は、単一のターゲットで動作します。ターゲットの前にある場合はプレフィックス\(`-a` など\)で、ターゲットの後にある場合は後置演算子\(`b!` など\)です。

演算子メソッドを宣言するときに、`func` キーワードの前に `prefix` または `postfix` 修飾子を記述して、_前置_または_後置_単項演算子を実装します。

```swift
extension Vector2D {
    static prefix func - (vector: Vector2D) -> Vector2D {
        return Vector2D(x: -vector.x, y: -vector.y)
    }
}
```

上記の例では、`Vector2D` インスタンスに単項減算演算子\(`-a`\)を実装しています。単項減算演算子は前置演算子のため、このメソッドは `prefix` 修飾子で修飾する必要があります。

シンプルな数値の場合、単項減算演算子は正の数値を負の同等の数値に変換し、その逆も同様です。`Vector2D` インスタンスの対応する実装は、`x` プロパティと `y` プロパティの両方でこの操作を実行します:

```swift
let positive = Vector2D(x: 3.0, y: 4.0)
let negative = -positive
//負の値は (-3.0, -4.0) の Vector2D インスタンスです
let alsoPositive = -negative
// alsoPositive は、値が (3.0, 4.0) の Vector2D インスタンスです。
```

### [Compound Assignment Operators\(複合代入演算子\)](advanced-operators.md) <a id="advanced-operators-compound-assignment-operators"></a>

_複合代入演算子_は、代入\(`=`\)と別の演算子を組み合わせます。例えば、加算代入演算子\(`+=`\)は、加算と代入を 1 つの演算に結合します。パラメータの値は演算子メソッド内から直接変更されるため、複合代入演算子の左側の入力パラメータ型を `inout` としてマークします。

下記の例では、Vector2D インスタンスの加算代入演算子メソッドを実装しています:

```swift
extension Vector2D {
    static func += (left: inout Vector2D, right: Vector2D) {
        left = left + right
    }
}
```

加算演算子は以前に定義されているため、ここで加算プロセスを再実装する必要はありません。代わりに、加算代入演算子メソッドは既存の加算演算子メソッドを利用し、それを使用して左の値を左の値に右の値を足します:

```swift
var original = Vector2D(x: 1.0, y: 2.0)
let vectorToAdd = Vector2D(x: 3.0, y: 4.0)
original += vectorToAdd
// 元の値は (4.0, 6.0) になりました
```

> NOTE  
> デフォルトの代入演算子\(`=`\)をオーバーロードすることはできません。複合代入演算子のみをオーバーロードできます。同様に、三項条件演算子\(`a ? b : c`\)はオーバーロードできません。

### <a id="equivalence-operators">等価演算子\(Equivalence Operators\)</a>

デフォルトでは、独自のクラスと構造体には、等価演算子\(`==`\)および不等価演算子\(`!=`\)として知られる等価演算子の実装がありません。通常は `==` 演算子を実装し、標準ライブラリの `==` 演算子の結果を否定する `!=` 演算子のデフォルト実装を使用します。`==` 演算子を実装するには 2 つの方法があります。自分で実装するか、多くの型の場合、Swift に実装を合成するよう依頼することができます。どちらの場合も、標準ライブラリの `Equatable` プロトコルに準拠を追加します。

`==` 演算子の実装は、他の中置演算子を実装するのと同じ方法で提供します:

```swift
extension Vector2D: Equatable {
    static func == (left: Vector2D, right: Vector2D) -> Bool {
        return (left.x == right.x) && (left.y == right.y)
    }
}
```

上記の例では、`==` 演算子を実装して、2 つの `Vector2D` インスタンスが同等の値を持つかどうかを確認しています。`Vector2D` では、「等しい」を「両方のインスタンスが同じ `x` 値と `y` 値を持つ」ことを意味すると考えるのが理にかなっているおり、`==` 演算子の実装で使用されているロジックです。

この演算子を使用して、2 つの `Vector2D` インスタンスが同等かどうかを確認できるようになりました:

```swift
let twoThree = Vector2D(x: 2.0, y: 3.0)
let anotherTwoThree = Vector2D(x: 2.0, y: 3.0)
if twoThree == anotherTwoThree {
    print("These two vectors are equivalent.")
}
// These two vectors are equivalent.
```

多くのシンプルなケースでは、Swift の等価演算子のデフォルト実装を使用できます\([Adopting a Protocol Using a Synthesized Implementation\(デフォルト実装を使用したプロトコル準拠\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide-gaido/protocols#adopting-a-protocol-using-a-synthesized-implementation)で説明されています\)。

## <a id="custom-operators">カスタム演算子\(Custom Operators\)</a>

Swift が提供する標準演算子に加えて、独自の演算子を宣言して実装できます。カスタム演算子の定義に使用できる文字のリストについては、[Operators\(演算子\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/lexical-structure#operators)を参照ください。

新しい演算子は、`operator` キーワードを使用してグローバルレベルで宣言され、`prefix`、`infix` または `postfix` 修飾子でマークされます。

```swift
prefix operator +++
```

上記の例では、`+++` という新しいプレフィックス演算子を定義しています。この演算子は Swift では既存の意味を持たないため、`Vector2D` インスタンスを操作する特定のコンテキストで、独自のカスタムの意味が与えられます。この例では、`+++` は新しい「_プレフィックスダブリング_」演算子として扱われます。前に定義した加算代入演算子を使用してベクトルにそれ自体を追加することにより、`Vector2D` インスタンスの `x` 値と `y` 値を 2 倍にします。`+++` 演算子を実装するには、次のように `+++` という型メソッドを `Vector2D` に追加します。

```swift
extension Vector2D {
    static prefix func +++ (vector: inout Vector2D) -> Vector2D {
        vector += vector
        return vector
    }
}

var toBeDoubled = Vector2D(x: 1.0, y: 4.0)
let afterDoubling = +++toBeDoubled
// toBeDoubled の値は (2.0, 8.0) になりました
// afterDoubling も (2.0, 8.0) です
```

### <a id="precedence-for-custom-infix-operators">カスタム中置演算子の優先順位\(Precedence for Custom Infix Operators\)</a>

カスタム中置演算子は、それぞれある_優先順位グループ_に属します。優先順位グループは、他の中置演算子に対する演算子の優先順位および結合規則を指定します。これらの特性が、中置演算子と他の中置演算子との間でどのように影響するかについては、[Precedence and Associativity\(優先順位と結合規則\)](advanced-operators.md#precedence-and-associativity)を参照ください。

優先順位グループに明示的に配置されていないカスタム中置演算子には、三項条件演算子の優先順位よりも 1 つ高い優先順位を持つデフォルトの優先順位グループが与えられます。

次の例では、優先順位グループ `AdditionPrecedence` に属する `+-` という新しいカスタム中置演算子を定義しています:

```swift
infix operator +-: AdditionPrecedence
extension Vector2D {
    static func +- (left: Vector2D, right: Vector2D) -> Vector2D {
        return Vector2D(x: left.x + right.x, y: left.y - right.y)
    }
}
let firstVector = Vector2D(x: 1.0, y: 2.0)
let secondVector = Vector2D(x: 3.0, y: 4.0)
let plusMinusVector = firstVector +- secondVector
// plusMinusVector は、値が (4.0, -2.0) の Vector2D インスタンスです
```

この演算子は、2 つのベクトルの `x` 値を加算し、最初のベクトルから 2 番目のベクトルの `y` 値を減算します。これは本質的に「加算」演算子のため、`+` や `-` などの加算中置演算子と同じ優先順位グループが与えられています。演算子の優先順位グループと結合規則設定の完全なリストを含む、Swift 標準ライブラリによって提供される演算子については、[Operator Declarations\(演算子宣言\)](https://developer.apple.com/documentation/swift/swift_standard_library/operator_declarations)を参照ください。優先順位グループの詳細と、独自の演算子と優先順位グループを定義するための構文については、[Operator Declarations\(演算子の宣言\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/declarations#operator-declaration)を参照ください。

> NOTE  
> 前置または後置演算子を定義するときは、優先順位を指定しません。ただし、前置と後置演算子の両方を同じオペランドに適用すると、後置演算子が最初に適用されます。

## <a id="result-builders">リザルトビルダ\(Result Builders\)</a>

_リザルトビルダ_は、リストやツリーなどのネストデータを自然な宣言的な方法で作成するための構文を追加する、ユーザ定義の型です。リザルトビルダを使用するコードには、条件または繰り返しのデータを処理するために、`if` や `for` などの通常の Swift 構文を含めることができます。

下記のコードは、星とテキストを使用して 1 行に文字列を描画するためのいくつかの型を定義します。

```swift
protocol Drawable {
    func draw() -> String
}
struct Line: Drawable {
    var elements: [Drawable]
    func draw() -> String {
        return elements.map { $0.draw() }.joined(separator: "")
    }
}
struct Text: Drawable {
    var content: String
    init(_ content: String) { self.content = content }
    func draw() -> String { return content }
}
struct Space: Drawable {
    func draw() -> String { return " " }
}
struct Stars: Drawable {
    var length: Int
    func draw() -> String { return String(repeating: "*", count: length) }
}
struct AllCaps: Drawable {
    var content: Drawable
    func draw() -> String { return content.draw().uppercased() }
}
```

`Drawable` プロトコルは、線や形状などの描画可能なものの要件を定義します: 型は `draw()` メソッドを実装する必要があります。 `Line` 構造体は 1 行の描画を表し、ほとんどの描画のトップレベルのコンテナとして機能します。`Line` を描画するために、構造体は各コンポーネントの `draw()` を呼び出し、結果の文字列を単一の文字列に連結します。`Text` 構造体は、文字列をラップして描画の一部にします。`AllCaps` 構造体は、別の描画をラップしてテキストを大文字に変換します。

イニシャライザを呼び出すことで、これらの型の描画を作成できます:

```swift
let name: String? = "Ravi Patel"
let manualDrawing = Line(elements: [
    Stars(length: 3),
    Text("Hello"),
    Space(),
    AllCaps(content: Text((name ?? "World") + "!")),
    Stars(length: 2),
    ])
print(manualDrawing.draw())
// "***Hello RAVI PATEL!**"
```

このコードは機能しますが、少し扱いにづらく感じます。`AllCaps` の後の深くネストされた括弧は読みにくく、`name` が `nil` の場合に「World」を使用するフォールバックロジックは、`??` を使用してインラインで実行する必要があります。これは、より複雑なものに対しては困難です。描画の一部を構成するために `switch` または `for` ループを含める必要があっても、方法はありません。リザルトビルダを使用すると、このようなコードを書き換えて、通常の Swift のコードのように見せることができます。

リザルトビルダを定義するには、型宣言に `@resultBuilder` 属性を記述します。例えば、次のコードは `DrawingBuilder` というリザルトビルダを定義します。これにより、宣言的な構文を使用して描画できます:

```swift
@resultBuilder
struct DrawingBuilder {
    static func buildBlock(_ components: Drawable...) -> Drawable {
        return Line(elements: components)
    }
    static func buildEither(first: Drawable) -> Drawable {
        return first
    }
    static func buildEither(second: Drawable) -> Drawable {
        return second
    }
}
```

`DrawingBuilder` 構造体は、リザルトビルダ構文の一部を実装する 3 つのメソッドを定義します。`buildBlock(_:)` メソッドは、コードのブロックに一連の行を書き込むためのサポートを追加します。そのブロック内のコンポーネントを `Line` に結合します。 `buildEither(first:)` および `buildEither(second:)` メソッドは、`if-else` のサポートを追加します。

`@DrawingBuilding` を関数のパラメータに適用すると、関数に渡されたクロージャを、リザルトビルダがそのクロージャから作成する値に変換します。例えば:

```swift
func draw(@DrawingBuilder content: () -> Drawable) -> Drawable {
    return content()
}
func caps(@DrawingBuilder content: () -> Drawable) -> Drawable {
    return AllCaps(content: content())
}

func makeGreeting(for name: String? = nil) -> Drawable {
    let greeting = draw {
        Stars(length: 3)
        Text("Hello")
        Space()
        caps {
            if let name = name {
                Text(name + "!")
            } else {
                Text("World!")
            }
        }
        Stars(length: 2)
    }
    return greeting
}
let genericGreeting = makeGreeting()
print(genericGreeting.draw())
// "***Hello WORLD!**"

let personalGreeting = makeGreeting(for: "Ravi Patel")
print(personalGreeting.draw())
// "***Hello RAVI PATEL!**"
```

`makeGreeting(for:)` 関数は `name` パラメータを受け取り、それを使用してパーソナライズされた挨拶を描画します。`draw(_:)` 関数と `caps(_:)` 関数はどちらも、`@DrawingBuilder` 属性でマークされた単一のクロージャを引数として受け取ります。これらの関数を呼び出すときは、`DrawingBuilder` が定義する特別な構文を使用します。Swift は、描画の宣言的な記述を `DrawingBuilder` のメソッドへの一連の呼び出しに変換し、関数の引数として渡される値を構築します。例えば、Swift は `caps(_:)` の呼び出しを次のようなコードに変換します:

```swift
let capsDrawing = caps {
    let partialDrawing: Drawable
    if let name = name {
        let text = Text(name + "!")
        partialDrawing = DrawingBuilder.buildEither(first: text)
    } else {
        let text = Text("World!")
        partialDrawing = DrawingBuilder.buildEither(second: text)
    }
    return partialDrawing
}
```

Swift は `if-else` ブロックを `buildEither(first:)` および `buildEither(second:)` メソッドの呼び出しに変換します。これらのメソッドを自身で呼び出すことはありませんが、`DrawingBuilder` 構文を使用すると、変換の結果を表示して、 Swift がコードをどのように変換するかを簡単に確認できます。

特別な描画構文での `for` ループをサポートするには、`buildArray(_:)` メソッドを追加します。

```swift
extension DrawingBuilder {
    static func buildArray(_ components: [Drawable]) -> Drawable {
        return Line(elements: components)
    }
}
let manyStars = draw {
    Text("Stars:")
    for length in 1...3 {
        Space()
        Stars(length: length)
    }
}
```

上記のコードでは、`for` ループが描画の配列を作成し、`buildArray(_:)` メソッドがその配列を `Line` に変換します。

Swift が builder 構文を builder 型のメソッドの呼び出しに変換する方法の完全なリストについては、[resultBuilder](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/attributes#resultbuilder)を参照ください。

