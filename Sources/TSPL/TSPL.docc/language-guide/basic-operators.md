# 基本演算子\(Basic Operators\)

<!--最終更新日: 2022/12/3  -->
<!--原文: https://docs.swift.org/swift-book/LanguageGuide/BasicOperators.html-->

代入、算術、比較、およびブール演算を行う。

_演算子_は、値のチェックや変更、合成するために使われる特殊な記号や用語です。例えば、加算演算子\(`+`\)は `let i = 1 + 2` のように 2 つの数値を足します。論理 AND 演算子\(`&&`\)は、`if enteredDoorCode && passedRetinaScan` のように 2 つのブール値を組み合わせます。

Swift は、C 言語のような他の言語でおそらく見たことがある演算子をサポートするのに加え、よく起きるコーディング中のエラーを防ぐために改良を加えています。代入演算子\(`=`\)を、本当は等価演算子\(`==`\)を使おうと思っていた所へ間違って使えなくするために、値を返しません。算術演算子\(`+`, `-`, `*`, `/`, `%` など\)は、保持している値の型の許容範囲を超えて予期せぬ結果が起きることを防ぐために、オーバーフローを検知して発生しないようにしています。Swift のオーバーフロー演算子を使用することで、オーバーフローに対する挙動を変えることもできます。詳細は[Overflow Operators\(オーバーフロー演算子\)](../language-guide/advanced-operators.md#overflow-operators)に記載しています。

また、C 言語にはなかった `a..<b` や `a...b` のような値の範囲を簡単に表すための範囲演算子も提供しています。

この章では、Swift の基本的な演算子について記載します。[Advanced Operators\(高度な演算子\)](advanced-operators.md)ではより応用的な演算子をカバーしており、独自に作成できるカスタム演算子や、独自で定義した型へ Swift が用意している標準の演算子を実装する方法を記載しています。

## 用語\(Terminology\)

演算子は単項\(`unary`\)、二項\(`binary`\)、三項\(`ternary`\)があります。

* 単項演算子は、`-a` のような単一のターゲットで動作します。前置\(`prefix`\)演算子は `!b` のようにターゲットの直前に置かれ、後置\(`postfix`\)演算子は `c!` のようにターゲットの直後に置かれます
* 二項演算子は、`2 + 3` のように 2 つの値をターゲットで動作します。これは 2 つのターゲットの間に置かれるので中置\(`infix`\)演算子です
* 三項演算子は、3 つの値をターゲットとした演算子です。C 言語と同様に、Swift は 1 つの三項演算子、三項条件演算子しかありません\(`a ? b : c`\)

演算子が影響を与える値は、オペランドと呼ばれます。`1 + 2` という式は、`+` が中置演算子で `1` と `2` がオペランドです。

## 代入演算子\(Assignment Operator\)

_代入演算子_は、例えば `a = b` とすると、`a` の値を `b` の値で初期化、または更新します。

```swift
let b = 10
var a = 5
a = b
// a は 10 と等しい
```

代入の右側が複数の値を持つタプルの場合、一度に複数の定数や変数に代入することができます。

```swift
let (x, y) = (1, 2)
// x は 1、 y は 2 と等しい
```

C 言語や Objective-C の代入演算子と異なり、Swift の代入演算子は値を返しません。そのため次のようなことはできません。

```swift
if x = y {
    // x = yは値を返さないのでエラーになる。
}
```

この特徴から代入演算子\(`=`\)を等価演算子\(`==`\)と間違えて使用することを防げます。上記の例では、`if x = y` はコンパイルエラーになります。

## 算術演算子\(Arithmetic Operators\)

Swift は全ての数値型に対して 4 つの基本的な_算術演算子_をサポートしています。

* 加算\(`+`\)
* 減算\(`-`\)
* 乗算\(`*`\)
* 割算\(`/`\)

```swift
1 + 2       // 3
5 - 3       // 2
2 * 3       // 6
10.0 / 2.5  // 4.0
```

C 言語や Objective-C と異なり、Swift はデフォルトでオーバーフローはできません。`a &+ b` のようにオーバーフロー演算子\(`&-`\)を使用してオーバーフローさせることができます。詳細は[Overflow Operators\(オーバーフロー演算子\)](../language-guide/advanced-operators.md#overflow-operators)を参照ください。

加算演算子は `String` の連結もすることができます。

```swift
"hello, " + "world"  // "hello, world"
```

### 剰余演算子\(Remainder Operator\)

`a % b` のような剰余演算子は、`a` の中に `b` がどのくらい含まれているのかを計算し、その\(_剰余_と呼ばれる\)余った値を返します。

> NOTE  
> 剰余演算子\(`%`\)は、他の言語ではモジュロ演算子とも呼ばれています。しかし、厳密に言うと、負数に対するSwiftの挙動は、モジュロ演算ではなく剰余演算を行います。

剰余演算子がどのように機能するかの例を見てみましょう。`9 % 4` を計算すると、まず `9` の中にいくつ `4` が含まれているかの結果を出します。

![remainderinteger](../assets/remainderinteger_2x.png)

`9` の中には 2 つの `4` があり、剰余は `1` です\(オレンジ色の部分\)。

Swift では、下記のように書きます。

```swift
9 % 4    // 1
```

`a % b` へ解答するために、`%` 演算子は次の方程式を計算して `reminder` を返します。

`a = (b x some multiplier) + remainder`

`some multiplier` には `a` に収まる `b` の倍数の最大数が入ります。

`9` と `4` をこの方程式に当てはめるとこうなります:

`9 = (4 x 2) + 1`

`a` を負数にした場合も見てみましょう。

```swift
-9 % 4   // -1
```

`-9` と `4` をこの方程式に当てはめるとこうなります:

`-9 = (4 x -2) + -1`

`-1` という剰余値を得られます。

`b` の位置に負数を入れても、その負の記号は無視されます。つまり、`a % b` と `a % -b` は常に同じ結果が得られます。

### 単項減算演算子\(Unary Minus Operator\)

数値の記号は、前置の `-` を使用して切り替えることができます。これは_単項減算演算子_と呼ばれます。

```swift
let three = 3
let minusThree = -three       // minusThree は -3
let plusThree = -minusThree   // plusThree は 3("マイナスマイナス3")
```

単項減算演算子\(`-`\)は、操作する値の直前にスペースなしで付けます。

### 単項加算演算子\(Unary Plus Operator\)

_単項加算演算子_\(`+`\)は、何も変えずに操作した値を返します。

```swift
let minusSix = -6
let alsoMinusSix = +minusSix  // alsoMinusSix は -6
```

単項加算演算子は何もしませんが、負数に単項減算演算子を使用している場合、その対称として正数に単項加算演算子を使用することもできます。

## 複合代入演算子\(Compound Assignment Operators\)

C 言語と同様に、Swift の_複合代入演算子_は、代入演算子\(`=`\)と他の演算子を組み合わせることができます。1 つの例として、加算代入演算子\(`+=`\)があります:

```swift
var a = 1
a += 2
// a は 3 と等しい
```

`a += 2` は `a = a + 2` のショートカットです。効率的に、加算と代入が 1 つの操作に合成されて、同時に行われます。

> NOTE  
> 複合代入演算子は値を返しません。例えば、`let b = a += 2` と書くことはできません。

Swift の標準ライブラリで提供している演算子については、[Operator Declarations\(演算子宣言\)](https://developer.apple.com/documentation/swift/swift_standard_library/operator_declarations)を参照ください。

## <a id="comparison-operators">比較演算子\(Comparison Operators\)</a>

Swift は、次の比較演算子をサポートしています。

* 等価演算子\(`a == b`\)
* 不等価演算子\(`a != b`\)
* より大きい\(`a > b`\)
* より小さい\(`a < b`\)
* 等しいかそれ以上\(`a >= b`\)
* 等しいかそれ以下\(`a <= b`\)

> NOTE  
> Swift では、2つの恒等作用素\(`===` と `!==`\)も提供しています。これは2つのオブジェクトが同じインスタンスを参照しているかどうかの確認ができます。より詳細は、[Identity Operators\(恒等作用素\)](../language-guide/structures-and-classes.md#identity-operators)を参照ください。

それぞれの比較演算子は、そのステートメントが `true` かどうかのブール値を返します。

```swift
1 == 1   // 1 は 1 と等しいなので true
2 != 1   // 2 は 1 と等しくないので true
2 > 1    // 2 は 1 とより大きいので true
1 < 2    // 1 は 2 とより小さいので true
1 >= 1   // 1 は 1 とより大きいまたは等しいので true
2 <= 1   // 1 は 1 とより小さくないかつ等しくないので false
```

比較演算子は、`if` などの条件文でよく使われます。

```swift
let name = "world"
if name == "world" {
    print("こんにちは, world")
} else {
    print("ごめんなさい \(name), あなただと気づきませんでした")
}
// name は "world" と等しいので、 こんにちは, world が出力される。
```

`if` については、[Control Flow\(制御フロー\)](control-flow.md)を参照ください。同じ型、同じ数の値を持ったタプル同士の比較もできます。タプルは左から右へと順番に値を比較します。それぞれの値の比較結果が全体の結果に反映されます。全て等しい場合、タプル同士は等しいと見なされます。例えば:

```swift
(1, "zebra") < (2, "apple")   // 1 は 2 より小さいので true。"zebra" と "apple" は比較しない
(3, "apple") < (3, "bird")    // 3 と 3 は同じで、"apple" は "bird" はより小さいので true
(4, "dog") == (4, "dog")      // 4 と 4、"dog" と "dog" は等しいので true
```

上記の例では、最初の行では左から右へと比較をしています。`1` は `2` より小さいので、`(1, "zebra")` はタプルの他の値がどんな値でも `(2, "apple")` よりも小さいと判定されます。既に最初の値同士でこのタプルの比較は完了しているので、`"zebra"` と `"apple"` の比較は関係ありません。しかし、最初の値が同じ場合、2 番目の値同士で比較されます。上記の 2, 3 行目がこれに当たります。

タプルは、個々の要素の値にその演算子が適応できる場合に比較をすることができます。例えば、下記のコードでは、2 つの `(String, Int)` のタプルを比較していますが、`String` も `Int` も `<` で比較できるので、タプル自身も比較ができます。反対に、`(String, Bool)` は、`Bool` が `<` に使えないため、タプルの比較ができません。

```swift
("blue", -1) < ("purple", 1)        // OK true と評価されます
("blue", false) < ("purple", true)  // Error < はブール値の比較をできません
```

> NOTE  
> Swift の標準ライブラリは 要素数が 7 つ以下のタプルの演算子を提供しています。7 つ以上の要素数のタプルへは自身で実装しなければなりません。

## <a id="basic-operator-ternary-conditional-operator">三項条件演算子\(Ternary Conditional Operator\)</a>

_三項条件演算子_は、`question ? answer1 : answer2` という形式の、3 つの部分を持った特別な演算子です。これは、`question` が `true` か `false` かを基に 2 つの式のどちらかを評価するショートカットです。`question` が `true` ならば、`answer1` が評価され、`false` だと `answer2` が評価されます。

三項条件演算子は下記のコードのショートカットです。

```swift
if question {
    answer1
} else {
    answer2
}
```

下記の例は、table の行の高さを計算しています。ヘッダーがある場合、行の高さは内容の高さよりも 50 ポイント高く、ない場合は、20 ポイント高くします。

```swift
let contentHeight = 40
let hasHeader = true
let rowHeight = contentHeight + (hasHeader ? 50 : 20)
// rowHeight は 90
```

上記のコードは下記のショートカットです。

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

最初の例では三項条件演算子を使用して 1 行で `rowHeight` を設定でき、2 つ目の例よりもより簡潔になっています。

三項条件演算子は 2 つの式のどちらが使われるのかを決める効率的なショートカットです。しかし、三項条件演算子にも注意が必要です。あまり使いすぎると可読性を損なう場合もあります。複数の三項条件演算子を 1 つのステートメントに含めることは避けましょう。

## <a id="nilcoalescing-operator">nil 合体演算子\(Nil-Coalescing Operator\)</a>

`(a ?? b)` の _nil 合体演算子_は、オプショナルの `a` にもし値が存在すれば `a` をアンラップし、`a` が `nil` の場合は `b` をデフォルトとして返します。式 `a` は常にオプショナル型です。式 `b` は `a` が内包する値の型と一致していなけばなりません。

nil 合体演算子は下記のコードのショートカットです。

```swift
a != nil ? a! : b
```

上記のコードは、三項条件演算子を使用して、`a` が `nil` ではない場合は、`a` が内包する値へアクセスするために強制アンラップ\(`a!`\)し、それ以外は `b` を返します。nil 合体演算子は条件チェックとアンラップを 1 つにした、簡潔で読みやすく、より洗練された方法です。

> NOTE  
> `a` が `nil` ではない場合、`b` は評価されません。これは_短絡評価_と呼ばれます。

下記の例では、デフォルトのカラー名かユーザ定義のオプショナルなカラー名を選択するために、nil 合体演算子を使用しています。

```swift
let defaultColorName = "red"
var userDefinedColorName: String?   // defaults は nil

var colorNameToUse = userDefinedColorName ?? defaultColorName
// userDefinedColorName が nil なので、 colorNameToUse にデフォルトの"red"が設定されます
```

`userDefinedColorName` は、オプショナルの `String` として定義され、デフォルトは `nil` です。`userDefinedColorName` はオプショナル型なので、その値を検討するために、nil 合体演算子を使用することができます。上記の例では、`colorNameToUse` という `String` 型の変数の初期値を決めるために nil 合体演算子を使用しています。`userDefinedColorName` は `nil` なので、式 `userDefinedColorName ?? defaultColorName` は、`defaultColorName`、つまり `"red"` を返します。

もし `userDefinedColorName` に `nil` 以外が代入されていて nil 合体演算子でチェックを行った場合、`userDefinedColorName` が内包する値がデフォルトの代わりに使われます:

```swift
userDefinedColorName = "green"
colorNameToUse = userDefinedColorName ?? defaultColorName
// userDefinedColorName は nil ではないので、 colorNameToUse の "green" が設定されます
```

## <a id="range-operators">範囲演算子\(Range Operators\)</a>

Swift は、値の範囲を表すショートカットとして複数の_範囲演算子_を提供しています。

### 閉範囲演算子\(Closed Range Operator\)

`a...b` は、`a` から `b` までの連続した、`a` も `b` も含んだ範囲を定義します。`a` を `b` よりも大きい値にしてはいけません。

閉範囲演算子は、必要な全ての値を範囲に含め、繰り返し処理を行うときに有用です。例えば `for-in` ループでは:

```swift
for index in 1...5 {
    print("\(index) × 5 is \(index * 5)")
}
// 1 × 5 は 5
// 2 × 5 は 10
// 3 × 5 は 15
// 4 × 5 は 20
// 5 × 5 は 25
```

`for-in` ループについては、[Control Flow\(制御フロー\)](control-flow.md)を参照ください。

### <a id="halfopen-range-operator">半開範囲演算子\(Half-Open Range Operator\)</a>

`a..<b` のような_半開範囲演算子_は、`a` から `b` まで連続しているものの `b` は含まない範囲を定義します。`a` を `b` よりも大きい値にしてはいけません。`a` と `b` が等しい場合、範囲は空になります。

半開範囲演算子は、配列などの 0 ベースのリストで特に有用です。例えば、リストの要素数に順番にアクセスする場合などあります:

```swift
let names = ["Anna", "Alex", "Brian", "Jack"]
let count = names.count
for i in 0..<count {
    print("人物 \(i + 1) の名前は \(names[i])")
}
// 人物 1 の名前は Anna
// 人物 2 の名前は Alex
// 人物 3 の名前は Brian
// 人物 4 の名前は Jack
```

この配列は 4 つの要素が含まれていますが、`0..<count` は半開範囲演算子なので `3`\(配列の最後のインデックス\)までしかカウントしないことに注目してください。配列については、[Arrays\(配列\)](../language-guide/collection-types.md#arrays)を参照ください。

### 片側範囲演算子\(One-Sided Ranges\)

閉範囲演算子は、1 つの方向へ可能な限り続く範囲に対して違った形式で書くことができます。例えば、ある配列のインデックス 2 から配列の最後までの要素を含んだ範囲などがこれに当たります。これらの場合では、範囲演算子の片方の値を省略できます。この種類の範囲は、片方しか値を持たないので、_片側範囲_と呼ばれています。例えば:

```swift
for name in names[2...] {
    print(name)
}
// Brian
// Jack

for name in names[...2] {
    print(name)
}
// Anna
// Alex
// Brian
```

最後の値のみを書くことで、片側範囲の形式で半開範囲演算子を書くことができます。両側の値を書いたときと同様に、記載した最後の値は範囲に含まれません。例えば:

```swift
for name in names[..<2] {
    print(name)
}
// Anna
// Alex
```

片側範囲は、サブスクリプト以外の他のコンテキストでも使用することができます。最初の値を省略した片側範囲を繰り返し処理に利用することはできません。どこから繰り返しを始めて良いかがわからないからです。一方で、最後の値を省略した片側範囲を繰り返し処理に利用することは_できます_。しかし、範囲は無限に続くので、ループの中で終了条件を明示的に設定していることを確かめましょう。片側範囲にある値が含まれているかどうかのチェックもできます。下記に例を示します:

```swift
let range = ...5
range.contains(7)   // false
range.contains(4)   // true
range.contains(-1)  // true
```

## 論理演算子\(Logical Operators\)

_論理演算子_は、ブール論理値の `true` と `false` を変更したり、組み合わせたりします。Swift では、C ベースの言語にもある 3 つの標準の論理演算子をサポートしています。

* 否定 NOT \(`!a`\)
* 論理積 AND \(`a && b`\)
* 論理和 OR \(`a || b`\)

### 論理否定演算子\(Logical NOT Operator\)

`!a` のような_論理否定演算子_は、`true` を `false` に、`false` を `true` にブール値を反転させます。

論理否定演算子は、操作する値の直前にスペースなしで付けます。次の例では、「`a` ではない」と読み取れます。

```swift
let allowedEntry = false
if !allowedEntry {
    print("アクセス拒否")
}
// アクセス拒否
```

`if !allowedEntry` は「もし許可されたエントリでなければ」と読み取れます。次の行は、「もし許可されたエントリでなければ」が `true` だった場合のみ実行されます。つまり、`if allowedEntry` は `false` です。

この例では、Bool 型の定数と変数の名前を注意深く選択していることで、コードの可読性や簡潔さを保つことができてます。\(二重否定や複合式を避けるなど\)

### 論理積演算子\(Logical AND Operator\)

`a && b` のような_論理積演算子_は、両方の値が `true` の場合に、全体の式も `true` になる論理式を作ります。

もしどちらかが `false` ならば、全体の式は `false` になります。実際、最初の値が `false` ならば、既に全体の式が `false` だと分かっているため、次の値は評価されません。これは短絡評価と呼ばれています。

この例は、2 つのブール値を検討し、両方の値が `true` の場合のみ、アクセスを許可します。

```swift
let enteredDoorCode = true
let passedRetinaScan = false
if enteredDoorCode && passedRetinaScan {
    print("こんにちは!")
} else {
    print("アクセス拒否")
}
// アクセス拒否
```

### 論理和演算子\(Logical OR Operator\)

`a || b` のような_論理和演算子_は、2 つの連続したバーティカルバー\(`||`\)を使った中置演算子です。片方の値が `true` の場合に、全体の式も `true` になる論理式を作ります。

論理積と同様に、論理和は式を検討するときに短絡評価を使います。論理和の左側が `true` ならば、既に全体の式も `true` と分かっているので、右側は評価されません。

下記の例では、最初の `Bool` 値\(`hasDoorKey`\)は `false` ですが、2 番目の値\(`knowsOverridePassword`\)は `true` です。片方が `true` なので、全体の式も `true` になり、アクセスは許可されます:

```swift
let hasDoorKey = false
let knowsOverridePassword = true
if hasDoorKey || knowsOverridePassword {
    print("こんにちは!")
} else {
    print("アクセス拒否")
}
// こんにちは!
```

### 論理演算子の合成\(Combining Logical Operators\)

より長く様々な条件の複合式を作成するために、複数の論理演算子を組み合わせることができます。

```swift
if enteredDoorCode && passedRetinaScan || hasDoorKey || knowsOverridePassword {
    print("こんにちは!")
} else {
    print("アクセス拒否")
}
// こんにちは!
```

この例では、複数の `&&` と `||` を使用して複合式を作っています。`&&` と `||` は 2 つの値しか操作することができないため、実際には 3 つの小さい式を繋げています。この例は下記のように読み取れます:

「もし正しいドア番号に入って、網膜スキャンを通過し、正しいドアキーを持っているまたは緊急の上書きパスワードを知っている場合に、アクセスすることができます」

`enteredDoorCode` と `passedRetinaScan` と `hasDoorKey` の値を基に、最初の 2 つの部分式は `false` です。しかし、緊急の上書きパスワードを知っているため、全体の式としては `true` と評価されます。

> NOTE  
> Swift の `&&` と `||` は左結合です。つまり、複数の論理演算子を持つ複合式の場合は、一番左の部分式から評価されます。

### 明示的な括弧\(Explicit Parentheses\)

厳密には必要ありませんが、複雑な式の意図を分かりやすくするために、括弧\(`()`\)を使用すると有用なときがあります。ドアへのアクセスの例では、意図を明確にするために、複合式の最初の部分に括弧を追加すると役に立ちます。

```swift
if (enteredDoorCode && passedRetinaScan) || hasDoorKey || knowsOverridePassword {
    print("こんにちは!")
} else {
    print("アクセス拒否")
}
// こんにちは!
```

この括弧は、最初の 2 つの値が、全体の論理の中で別の状態を持つことを明確にしています。複合式の出力結果は変わりませんが、全体の意図は読み手により明確になります。可読性は簡潔さよりも常に優先されます。意図を明確にすることができるならば、括弧を使いましょう。

