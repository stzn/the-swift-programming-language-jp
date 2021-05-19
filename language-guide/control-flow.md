# Control Flow

最終更新日:

Swift は、様々な制御フロー(*Control Flow*)を提供しています。1 つのタスクを複数回行う `while` ループ、ある条件を基に異なる分岐のコードを実行する `if`,`guard`,`switch`、コードの他の地点へ実行フローを移動させる `break` や `continue` のような文があります。

また、配列や辞書、範囲、文字列や他のシーケンスに対して、簡単に繰り返し処理を行う `for-in` ループも提供しています。

Swift の `switch` 文は、C のような他の言語と比べてもかなり強力です。case は、範囲やタプル、特定の型へのキャストなど、様々なパターンに合致させることができます。`switch` の case に合致した値は、case の body 内で使用するために一時的な変数や定数に紐づけることができます。各ケースに `where` を使ってより複雑な条件を作ることもできます。

## For-In Loops

配列のアイテムや数値の範囲、文字列の文字などのシーケンスに対して繰り返し処理を行うために `for-in` ループを使います。

下記の例は、`for-in` ループを使って配列のアイテムに繰り返し処理を行なっています。

```swift
let names = ["Anna", "Alex", "Brian", "Jack"]
for name in names {
    print("Hello, \(name)!")
}
// Hello, Anna!
// Hello, Alex!
// Hello, Brian!
// Hello, Jack!
```

辞書のキーバリューペアにアクセスするためにも繰り返し処理を使うことができます。辞書を繰り返す際に、辞書内の個々のアイテムは `(key, value)` のタプルとして返され、`for-in` ループ内で使用するために、`(key, value)` のタプルの個々のメンバーは明示的に名付けられた定数に展開できます。下記のコードは、辞書のキーの配列を `animalName` という定数に、辞書のバリューは `legCount` という定数に展開されています。

```swift
let numberOfLegs = ["spider": 8, "ant": 6, "cat": 4]
for (animalName, legCount) in numberOfLegs {
    print("\(animalName)s have \(legCount) legs")
}
// cats have 4 legs
// ants have 6 legs
// spiders have 8 legs
```

`Dictionary` の定数は元々順序がなく、繰り返し処理をする時に、取得されるキーバリューの順序は保証されていません。配列や辞書に関しては、[Collection Types](./collection-types.md)を参照ください。

数値の範囲にも `for-in` ループを使うことができます。下記の例では、5 の倍数の最初のいくつかを出力しています。

```swift
for index in 1...5 {
    print("\(index) times 5 is \(index * 5)")
}
// 1 times 5 is 5
// 2 times 5 is 10
// 3 times 5 is 15
// 4 times 5 is 20
// 5 times 5 is 25
```

繰り返されているシーケンスは、Closed 範囲演算子(`...`)を使って最初と最後も含んだ `1` から `5` の数字の範囲です。ます。`index` の値は範囲の最初の値(`1`)が設定され、ループ内の文が実行されます。この例では、ループは 1 つの文(index とそれに対応する五の倍数の値を出力する文)のみを含んでいます。文が完了すると、`index` は範囲の次の値(`2`)に更新され、`print(_:separator:terminator:)` が再び出力されます。このプロセスは範囲の終わりに達するまで続きます。

上記の例では、`index` はそれぞれの繰り返しの最初に自動で設定される値です。`index` は使われる前に宣言する必要はありません。`let` をつける必要はなく、ループの中に含まれることで自動で宣言されます。

シーケンスで各値が必要ない場合、変数名の位置にアンダースコア(`_`)を付けることで無視できます。

```swift
let base = 3
let power = 10
var answer = 1
for _ in 1...power {
    answer *= base
}
print("\(base) to the power of \(power) is \(answer)")
// Prints "3 to the power of 10 is 59049"
```

上記の例では、1 つの数字に累乗した結果を計算しています。この例では `1` に `3`(つまり `3` の `0` 乗) を掛けることから始まり、`1` から `10` までの closed 範囲を使って、10 回計算を繰り返しています。この計算では、ループ内で範囲から取得できる値は必要ありません。シンプルに、範囲の数だけ処理を繰り返しています。ループの変数部に使われているアンダースコア(`_`)は、それぞれの値を無視し、ループ中に値を提供しません。

最初と最後の両方を含める closed 範囲を使う必要がない場合もあるかもしれません。時計の分針を思い浮かべてください。0 から始まる 60 個の目盛が必要です。最初の値を含める Half-open 範囲演算子(`..<`)を使いましょう。

範囲についての詳細は、[Range Operators](./basic-operators.md#range-operators)を参照ください。

```swift
let minutes = 60
for tickMark in 0..<minutes {
    // 毎分ごとに 60 回目盛を描きます
}
```

表示上ではもっと目盛を少なくしたいかもしれません。5 分ごとに 1 目盛にしてみましょう。`stride(from:to:by:)` 関数を使うと不要な目盛をスキップすることができます。

```swift
let minuteInterval = 5
for tickMark in stride(from: 0, to: minutes, by: minuteInterval) {
    // 5 分ごと (0, 5, 10, 15 ... 45, 50, 55) に目盛を描きます
}
```

Closed 範囲も利用可能で、その場合は `stride(from:through:by:)` を使います:

```swift
let hours = 12
let hourInterval = 3
for tickMark in stride(from: 3, through: hours, by: hourInterval) {
    // 3 時間ごと (3, 6, 9, 12) に目盛を描きます
}
```

## While Loops

### while

### Repeat-While

## Conditional Statements

### If

### Switch

#### No Implicit Fallthrough

#### Interval Matching

#### Tuples

#### Value Bindings

#### Where

#### Compound Cases

## Control Transfer Statements

### Continue

### Break

#### Break in a Loop Statement

#### Break in a Switch Statement

### Fallthrough

### Labeled Statements

## Early Exit

## Checking API Availability
