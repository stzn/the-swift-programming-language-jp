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

### Unary Minus Operator

### Unary Plus Operator

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
