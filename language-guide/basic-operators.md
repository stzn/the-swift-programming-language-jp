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
// a is now equal to 10
```

代入の右側が複数の値を持つタプル(tuple)の場合、一度に複数の定数や変数に分けて代入することができます。

```swift
let (x, y) = (1, 2)
// x is equal to 1, and y is equal to 2
```

C 言語や Objective-C の代入演算子と異なり、Swift の代入演算子は値を返しません。次のようなことはできません。

```swift
if x = y {
    // This isn't valid, because x = y doesn't return a value.
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
1 + 2       // equals 3
5 - 3       // equals 2
2 * 3       // equals 6
10.0 / 2.5  // equals 4.0
```

C 言語や Objective-C と異なり、Swift はデフォルトでオーバーフローはできません。`a &+ b`のようにオーバーフロー演算子(`&-`)を使ってオーバーフローさせることができます。詳細は[Overflow Operators](./advanced-operators.md#overflow-operators)を参照ください。

加算演算子は`String`の連結もすることができます。

```swift
"hello, " + "world"  // equals "hello, world"
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
9 % 4    // equals 1
```

`a % b`の解答を決めるために、`%`演算子は次の方程式を計算して`reminder`を返します。

`a = (b x some multiplier) + remainder`

`some multiplier`には`a`に収まる`b`の倍数の最大数が入ります。

`9`と`4`をこの方程式に当てはめるとこうなります:

`9 = (4 x 2) + 1`

`a`を負の値にした場合も見てみましょう。

```swift
-9 % 4   // equals -1
```

`-9`と`4`をこの方程式に当てはめるとこうなります:

`-9 = (4 x -2) + -1`

`-1`という剰余値を得られます。

`b`の位置に負の値を入れても、その負の記号は無視されます。`a % b`と`a % -b`は常に同じ結果が得られます。

### Unary Minus Operator

数値の記号は、前置の`-`を使って切り替えることができます。これは単項マイナス演算子と呼ばれます。

```swift
let three = 3
let minusThree = -three       // minusThree equals -3
let plusThree = -minusThree   // plusThree equals 3, or "minus minus three"
```

単項マイナス演算子(`-`)は、操作する値の直前にスペースなしで付けます。

### Unary Plus Operator

単項プラス演算子(`+`)は、何も変えず操作する値を返します。

```swift
let minusSix = -6
let alsoMinusSix = +minusSix  // alsoMinusSix equals -6
```

単項プラス演算子は何もしませんが、負の値に単項マイナス演算子使っている場合、その対称として正の値に単項プラス演算子を使うこともできます。

## Compound Assignment Operators

## Comparison Operators

## Ternary Conditional Operator

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
