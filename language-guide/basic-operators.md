# Basic Operators

最終更新日:

演算子(*operator*)は、値のチェックや変更、合成するために使われる特殊なシンボルや用語です。例えば、加算演算子(`+`)は`let i = 1 + 2`のように 2 つの数値を足します。論理 AND 演算子(&&)は、`if enteredDoorCode && passedRetinaScan`のように 2 つのブール値を組み合わせます。

Swift は、C 言語のような他の言語でおそらく見たことがある演算子をサポートしていて、よくあるコーディング中のエラーを排除するために、さらにそれらに改良を加えています。代入演算子(`=`)は、本当は等価演算子(`==`)を使おうと思っていた時に間違って使えないようにするために、値を返さないようにしています。算術演算子(`+`, `-`, `*`, `/`, `%`など)は、保持している値の型で許容される値の範囲を超えて予期せぬ結果を招くことを防ぐために、オーバーフローを検知して発生しないようにしています。Swift のオーバーフロー演算子を使うことで、オーバーフローに対する挙動を変えることもできます。詳細は[Overflow Operators](./advanced-operators.md#overflow-operators)に記載しています。

また、C 言語にはなかった`a..<b`や`a...b`のような値の範囲を簡単に表すための範囲演算子も提供しています。

この章では、Swift の基本的な演算子について記載します。[Advanced Operators](./advanced-operators.md)ではより応用的な演算子をカバーしており、独自に作成できるカスタム演算子や、独自で定義した型へ Swift が用意している標準の演算子をどう実装するのかなどを記載しています。

## Terminology

演算子は単項(`unary`)、二項(`binary`)、三項(`ternary`)があります。

* 単項演算子は、`-a`のような 1 つの値を対象(オペランド)にした演算子です。前置(prefix)演算子は`!b`のように対象の直前に置かれ、後置(postfix)演算子は`c!`のように対象の直後に置かれます
* 二項演算子は、`2 + 3`のように 2 つの値を対象にした演算子です。これは 2 つの対象の間に置かれるので中置(`infix`)演算子でもあります
* 三項演算子は、3 つの値を対象とした演算子です。C 言語と同様に、Swift は 1 つの三項演算子、三項条件演算子しかありません(`a ? b : c`)

演算子が影響を与える値をオペランド(*operands*)と呼ばれます。`1 + 2`という式は、`+`が二項演算子で、`1`と`2`がオペランドです。

## Assignment Operator

代入演算子(*assignment operator*)は例えば`a = b`とすると、`a`の値を`b`の値で初期化、または更新します。

```swift
let b = 10
var a = 5
a = b
// a は 10 と等しい
```

代入の右側が複数の値を持つタプル(tuple)の場合、一度に複数の定数や変数に分けて代入することができます。

```swift
let (x, y) = (1, 2)
// x は 1、 y は 2 と等しい
```

C 言語や Objective-C の代入演算子と異なり、Swift の代入演算子は値を返しません。次のようなことはできません。

```swift
if x = y {
    // x = yは値を返さないのでエラーになる。
}
```

この特徴が代入演算子(=)を等価演算子(==)と間違えて使ってしまうことを防ぐことができます。Swift が`if x = y`をコンパイルエラーにしてくれます。

## Arithmetic Operators

Swift は全ての数値型の 4 つの基本的な算術演算子(*arithmetic operators*)をサポートしています。

* 加算(`+`)
* 減算(`-`)
* 乗算(`*`)
* 割算(`/`)

```swift
1 + 2       // 3
5 - 3       // 2
2 * 3       // 6
10.0 / 2.5  // 4.0
```

C 言語や Objective-C と異なり、Swift はデフォルトでオーバーフローはできません。`a &+ b`のようにオーバーフロー演算子(`&-`)を使ってオーバーフローさせることができます。詳細は[Overflow Operators](./advanced-operators.md#overflow-operators)を参照ください。

加算演算子は`String`の連結もすることができます。

```swift
"hello, " + "world"  // "hello, world"
```

### Remainder Operator

`a % b`のような剰余演算子は、`a`の中に`b`がどのくらい含まれているのかを計算し、その余った値(剰余)を返します。

> NOTE  
> 剰余演算子(%)は、他の言語ではモジュロ演算子とも呼ばれています。しかし、厳密に言うと、負の数値へのSwiftの挙動は、モジュロ演算ではなく、剰余演算を行います。

剰余演算子がどのように機能するかの例を見てみましょう。`9 % 4`を計算すると、まず`9`の中にいくつ`4`が含まれているかの結果を出します。

![remainderinteger](../.gitbook/assets/remainderinteger_2x.png)

`9`の中には 2 つの`4`があり、剰余は`1`です(オレンジ色の部分)。

Swift では、下記のように書きます。

```swift
9 % 4    // 1
```

`a % b`の解答を決めるために、`%`演算子は次の方程式を計算して`reminder`を返します。

`a = (b x some multiplier) + remainder`

`some multiplier`には`a`に収まる`b`の倍数の最大数が入ります。

`9`と`4`をこの方程式に当てはめるとこうなります:

`9 = (4 x 2) + 1`

`a`を負の値にした場合も見てみましょう。

```swift
-9 % 4   // -1
```

`-9`と`4`をこの方程式に当てはめるとこうなります:

`-9 = (4 x -2) + -1`

`-1`という剰余値を得られます。

`b`の位置に負の値を入れても、その負の記号は無視されます。`a % b`と`a % -b`は常に同じ結果が得られます。

### Unary Minus Operator

数値の記号は、前置の`-`を使って切り替えることができます。これは単項マイナス演算子と呼ばれます。

```swift
let three = 3
let minusThree = -three       // minusThree は -3
let plusThree = -minusThree   // plusThree は 3、もしくは "マイナスマイナス3"
```

単項マイナス演算子(`-`)は、操作する値の直前にスペースなしで付けます。

### Unary Plus Operator

単項プラス演算子(`+`)は、何も変えず操作する値を返します。

```swift
let minusSix = -6
let alsoMinusSix = +minusSix  // alsoMinusSix は -6
```

単項プラス演算子は何もしませんが、負の値に単項マイナス演算子使っている場合、その対称として正の値に単項プラス演算子を使うこともできます。

## Compound Assignment Operators

C 言語と同様に、Swift の複合代入演算子は代入演算子(`=`)と他の演算子を組み合わせることができます。1 つの例として、加算代入演算子(`+=`)があります:

```swift
var a = 1
a += 2
// a は 3 と等しい
```

`a += 2`は`a = a + 2`の短縮版です。効率的に、加算と代入が 1 つの操作に合成されて、同時に行われます。

> NOTE  
> 複合代入演算子は値を返しません。例えば、`let b = a += 2`と書くことはできません。

Swift の標準ライブラリで提供している演算子については、[Operator Declarations](https://developer.apple.com/documentation/swift/swift_standard_library/operator_declarations)を参照ください。

## Comparison Operators

Swift は、次の比較演算子をサポートしています。

* 等価演算子(`a == b`)
* 不等価演算子(`a != b`)
* より大きい(`a > b`)
* より小さい(`a < b`)
* 等しいかそれ以上(`a >= b`)
* 等しいかそれ以下(`a <= b`)

> NOTE  
> Swift では、2つの恒等作用素(`===`と`!==`)も提供しています。これは2つのオブジェクトが同じインスタンスを参照しているかどうかの確認ができます。より詳細は、[Identity Operators](./structures-and-classes.md#identity-operators.md)を参照ください。

それぞれの比較演算子は、その宣言が`true`かどうかのブール値を返します。

```swift
1 == 1   // 1 は 1 と等しいなので true
2 != 1   // 2 は 1 と等しくないので true
2 > 1    // 2 は 1 とより大きいので true
1 < 2    // 1 は 2 とより小さいので true
1 >= 1   // 1 は 1 とより大きいまたは等しいので true
2 <= 1   // 1 は 1 とより小さくないかつ等しくないので false
```

比較演算子は、`if`などの条件文でよく使われます。

```swift
let name = "world"
if name == "world" {
    print("hello, world")
} else {
    print("I'm sorry \(name), but I don't recognize you")
}
// name は "world" と等しいので、"hello, world" が出力される。
```

`if`については、[Control FLow](./control-flow.md)を参照ください。
同じ型、同じ数の値を持ったタプル(tuple)同士の比較もできます。タプルは左から右へと順番に値を比較します。それぞれの値の比較結果が全体の結果に反映されます。全て等しい場合、タプル同士は等しいと見なされます。例えば:

```swift
(1, "zebra") < (2, "apple")   // 1 は 2 より小さいので true。"zebra" と "apple" は比較しない
(3, "apple") < (3, "bird")    // 3 と 3 は同じで、"apple" は "bird" はより小さいので true
(4, "dog") == (4, "dog")      // 3 と 3、"dog" と "dog" は等しいので true
```

上記の例では、最初の行では左から右へと比較をしています。`1`は`2`より小さいので、`(1, "zebra")`はタプルは他の値がどんな値でも`(2, "apple")`よりも小さいと判定されます。既に最初の値同士でこのタプルの比較は完了しているので、`"zebra"`と`"apple"`の比較は関係ではありません。しかし、最初の値が同じ場合、2 番目の値同士で比較されます。上記の 2, 3 行目で見られます。

タプルは、個々の要素の値がその演算子に適応できる場合に、比較をすることができます。例えば、下記のコードでは、2 つの`(String, Int)`のタプルを比較していますが、`String`も`Int`も`<`で比較できるので、タプル自身も比較ができます。反対に、`(String, Bool)`は、`Bool`が`<`に使えないため、タプルの比較ができません。

```swift
("blue", -1) < ("purple", 1)        // OK true と評価されます
("blue", false) < ("purple", true)  // Error < はブール値の比較をできない
```

> NOTE  
> Swift の標準ライブラリは 7 つより少ない要素数のタプルの演算子を提供しています。 7 つ以上の要素数のタプルへの演算子は自身で実装しなければなりません。

## Ternary Conditional Operator

三項条件演算子(*ternary conditional operator*)は、`question ? answer1 : answer2`という形式の、3 つの部分を持った特別な演算子です。これは、`question`が`true`か`false`かを基に 2 つの式のどちらかを評価する短縮的な書き方です。`question`が`true`ならば、`answer1`が評価され、`false`だと`answer2`が評価されます。

三項条件演算子は下記のコードの短縮版です。

```swift
if question {
    answer1
} else {
    answer2
}
```

こちらの例は、table の行の高さを計算しています。ヘッダーがある場合の行の高さは内容の高さよりも 50 ポイント高く、ない場合は内容の高さよりも 20 ポイント高くします。

```swift
let contentHeight = 40
let hasHeader = true
let rowHeight = contentHeight + (hasHeader ? 50 : 20)
// rowHeight は 90
```

上記のコードは下記の短縮版です。

```swift
let contentHeight = 40
let hasHeader = true
let rowHeight: Int
if hasHeader {
    rowHeight = contentHeight + 50
} else {
    rowHeight = contentHeight + 20
}
// rowHeight は 90
```

最初の例では三項条件演算子を使って 1 行で`rowHeight`を設定でき、
2 つ目の例よりもより簡潔になっています。

三項条件演算子は 2 つの式のどちらが使われるのかを決める効率的な短縮方法です。しかし、三項条件演算子にも注意が必要です。あまり使いすぎると可動性がを損なう場合もあります。複数の三項条件演算子を 1 つのステートメントに含めることは避けましょう。

## Nil-Coalescing Operator

## Range Operators

### Closed Range Operator

### Half-Open Range Operator

### One-Sided Ranges

## Logical Operators

### Logical NOT Operator

### Logical AND Operator

### Logical OR Operator

### Combining Logical Operators

### Explicit Parentheses
