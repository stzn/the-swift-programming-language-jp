# 制御フロー\(Control Flow\)

最終更新日: 2025/6/21  
原文: https://docs.swift.org/swift-book/LanguageGuide/ControlFlow.html

分岐、ループ、および早期終了を使ってコードを構造化する。

Swift は、様々な制御フロー文を提供しています。1 つのタスクを複数回行う `while` ループ、ある条件を基に異なる分岐のコードを実行する `if`、`guard`、`switch` 文、コードの他の地点へ実行フローを移動させる `break` や `continue` のような文があります。

また、配列や辞書、範囲、文字列や他のシーケンスに対して、簡単にループ処理を行う `for-in` ループも提供しています。

Swift の `switch` 文は、C のような他の言語と比べてもかなり強力です。ケースは、範囲やタプル、特定の型へのキャストなど、様々なパターンに合致させることができます。また、`switch` のケースに合致した値は、ケースの本文内で使用するために変数や定数に紐づけることができます。各ケースに `where` を使用してより複雑な条件を作成することもできます。

## <a id="for-in-loops">For-Inループ\(For-In Loops\)</a>

配列のアイテムや数値の範囲、文字列の文字などのシーケンスに対してループ処理を行うために `for-in` ループを使います。

下記の例は、`for-in` ループを使用して配列のアイテムにループ処理を行なっています。

```swift
let names = ["Anna", "Alex", "Brian", "Jack"]
for name in names {
    print("こんにちは、 \(name)!")
}
// こんにちは、 Anna!
// こんにちは、 Alex!
// こんにちは、 Brian!
// こんにちは、 Jack!
```

辞書のキーバリューペアにアクセスするためにもループ処理を使用することができます。辞書を繰り返す際に、辞書内の個々のアイテムは `(key, value)` のタプルとして返され、`for-in` ループ内で使用するために、`(key, value)` のタプルの個々のメンバは明示的に命名した定数に展開できます。下記のコードは、辞書のキーは `animalName` 定数に、バリューは `legCount` 定数に展開されています。

```swift
let numberOfLegs = ["spider": 8, "ant": 6, "cat": 4]
for (animalName, legCount) in numberOfLegs {
    print("\(animalName) には \(legCount) 本の足があります")
}
// cat には 4 本の足があります
// ant には 6 本の足があります
// spider には 8 本の足があります
```

`Dictionary` の定数は元々順序がなく、ループ処理をするときに、取得されるキーバリューの順序も保証されていません。配列や辞書に関しては、[Collection Types\(コレクション型\)](collection-types.md)を参照ください。

数値の範囲にも `for-in` ループを使用することができます。下記の例では、5 の倍数の最初のいくつかを出力しています。

```swift
for index in 1...5 {
    print("\(index) × 5 は \(index * 5)")
}
// 1 × 5 は 5
// 2 × 5 は 10
// 3 × 5 は 15
// 4 × 5 は 20
// 5 × 5 は 25
```

繰り返されているシーケンスは、閉範囲演算子\(`...`\)を使用して最初と最後も含んだ `1` から `5` の数字の範囲です。`index` の値は、まず範囲の最初の値\(`1`\)が設定され、ループ内の文が実行されます。この例では、ループは 1 つの文\(`index` とそれに対応する 5 の倍数の値を出力する文\)のみを含んでいます。文が完了すると、`index` は範囲の次の値\(`2`\)に更新され、`print(_:separator:terminator:)` が再び呼ばれます。このプロセスは範囲の終わり\(`5`\)に達するまで続きます。

上記の例では、`index` は各ループの最初に自動で設定される値です。`index` を使用する前に宣言する必要はありません。`let` をつける必要もなく、ループに含めることで自動で宣言されます。

各ループの値が必要ない場合、変数名の位置にアンダースコア\(`_`\)を付けることで無視できます。

```swift
let base = 3
let power = 10
var answer = 1
for _ in 1...power {
    answer *= base
}
print("\(base) to the power of \(power) is \(answer)")
// 3 to the power of 10 is 59049
```

上記の例では、1 つの数字に累乗した結果を出力しています。この例では `1` に `3`\(つまり `3` の `0` 乗\)を掛けることから始まり、`1` から `10` までの閉範囲を使用して、10 回計算をループしています。この計算では、ループ内で範囲から取得できる値は必要ありません。シンプルに範囲の数だけ処理をループしています。ループの変数部に使われているアンダースコア\(`_`\)は、それぞれの値を無視し、ループ内に値を提供しません。

最初と最後の両方を含める閉範囲を使用する必要がない場合もあるかもしれません。時計の分針を思い浮かべてください。`0` から始まる `60` 個の目盛が必要です。最初の値を含める半開範囲演算子\(`..<`\)を使いましょう。

範囲についての詳細は、[Range Operators\(範囲演算子\)](../language-guide/basic-operators.md#range-operators)を参照ください。

```swift
let minutes = 60
for tickMark in 0..<minutes {
    // 毎分ごとに 60 回目盛を描きます
}
```

表示上ではもっと目盛を少なくしたいかもしれません。`5` 分ごとに `1` 目盛にしてみましょう。`stride(from:to:by:)` 関数を使用すると不要な目盛をスキップすることができます。

```swift
let minuteInterval = 5
for tickMark in stride(from: 0, to: minutes, by: minuteInterval) {
    // 5 分ごと (0, 5, 10, 15 ... 45, 50, 55) に目盛を描きます
}
```

閉範囲も利用可能で、その場合は `stride(from:through:by:)` を使います:

```swift
let hours = 12
let hourInterval = 3
for tickMark in stride(from: 3, through: hours, by: hourInterval) {
    // 3 時間ごと (3, 6, 9, 12) に目盛を描きます
}
```

## Whileループ\(While Loops\)

`while` ループは条件が `false` になるまで内部の文を実行します。最初のループが始まるまで、何回繰り返すのかがわからないようなときに最も使われます。

* `while` は、ループ内の処理を実行する前に条件を評価します
* `repeat-while` は、ループ内の処理を実行した後に条件を評価します

### while

`while` ループは、条件を評価してからループを開始します。`true` ならば条件が `false` になるまでループ内の処理を繰り返します。

`while` ループの基本的な書き方は下記の通りです:

```swift
while <#condition#> {
   <#statements#>
}
```

下記の例は、_蛇とはしご_と呼ばれるシンプルなゲームの例です。

![&#x86C7;&#x3068;&#x306F;&#x3057;&#x3054;](../assets/snakesandladders_2x.png)

ゲームのルールは下記のようです:

* ボードには 25 の枠があり、目的は枠 25 以上に到達することです
* プレイヤーはボードの左下を「枠 0」として、そこから始めます
* それぞれの番で、サイコロを振り、出た目の分、画像のドットに従って水平に移動します
* 画像のはしごの下の枠に止まったら、はしごの上の枠に行きます
* 蛇の頭の枠に止まったら、蛇のしっぽの枠に行きます

このボードは、`Int` 型配列で表されます。サイズは配列の初期化や、勝者を決める条件に使用されている `finalSquare` という定数で決められます。プレイヤーは、「枠 0」から始まるので、`25` ではなく、`26` 個の `0` でボードは初期化されています。

```swift
let finalSquare = 25
var board = [Int](repeating: 0, count: finalSquare + 1)
```

いくつかの枠は蛇やはしごのために特別な値を設定します。はしごの枠はボードの上に移動させるので正の値を、蛇はボードの下に移動させるので負の値を設定します。

```swift
board[03] = +08; board[06] = +11; board[09] = +09; board[10] = +02
board[14] = -10; board[19] = -11; board[22] = -02; board[24] = -08
```

枠 3 は枠 11 に移動します。これを表すには `board[03]` は `+08` で、\(`11` と `3` の差分の\)数字の `8` に相応します。値と文を見てみると、単項加算演算子\(`+i`\)、単項減算演算子\(`-i`\)が明示的に使われ、`10` 以下の値は `0` が前に付いています。\(厳密に `0` は必要ありませんが、より読みやすくできます\)

```swift
var square = 0
var diceRoll = 0
while square < finalSquare {
    // サイコロを振る
    diceRoll += 1
    if diceRoll == 7 { diceRoll = 1 }
    // 出た目分移動
    square += diceRoll
    if square < board.count {
        // まだどこかの枠にいる場合, 蛇かはしご分の移動をする
        square += board[square]
    }
}
print("ゲームオーバー!")
```

この例では、サイコロを振るアクションにとてもシンプルな方法を使用しています。ランダムな数字を生成するのではなく、`0` で初期化された `diceRoll` から開始します。`while` ループが繰り返される度に、`diceRoll` は 1 つずつ加算され、値が大きすぎないかをチェックしています。`7` になると、大きすぎるため `1` にリセットされます。つまり、`diceRoll` は `1, 2, 3, 4, 5, 6, 1, 2` のように繰り返されます。

サイコロを振った後、 `diceRoll` 分移動します。`25` を超すこともありますが、その場合はゲーム終了です。これを実現するために、コードでは `square` が `board` 配列の `count` よりも小さいことをチェックしています。`square` が妥当ならば、`board[square]` を `square` に加算し、蛇やはしごの場合はプレイヤーはさらに移動します。

> NOTE  
> このチェックがないと、`board[square]` は `board` の範囲を超えてアクセスする可能性があり、実行時エラーが発生します。

現在の `while` ループが終了し、次のループを実行するかどうかを決めるために、ループの条件をチェックします。プレイヤーは枠 25 またはそれ以上に到達していた場合、ループ条件は `false` になり、ゲームは終了します。

今回のケースでは、ループの開始時にどのくらいゲームが続くのかがわからないため、`while` ループが適切です。条件が満たされるまでループ処理が実行されます。

### Repeat-While

`while` ループの別の形として、`repeat-while` があり、ループ条件を検証する前に、一度ループ内の処理を実行します。その後、条件が `false` になるまで繰り返します。

> NOTE  
> Swiftの `repeat-while` は、他の言語でいうところの `do-while` に当たります

下記が `repeat-while` ループの一般的な形式です。

```swift
repeat {
   <#statements#>
} while <#condition#>
```

ここで蛇とはしごの例を `while` の代わりに `repeat-while` を使用して書き換えてみます。`finalSquare`、`board`、`square` と `diceRoll` は `while` と全く同じ方法で初期化します。

```swift
let finalSquare = 25
var board = [Int](repeating: 0, count: finalSquare + 1)
board[03] = +08; board[06] = +11; board[09] = +09; board[10] = +02
board[14] = -10; board[19] = -11; board[22] = -02; board[24] = -08
var square = 0
var diceRoll = 0
```

このバージョンのゲームでは、ループの最初で、蛇かはしごかをチェックしています。プレイヤーを枠 25 に直接移動させるはしごはないので、はしごを上るだけで勝利することはできません。なので、ループの最初で蛇かはしごかをチェックするのは安全です。

ゲームの最初、プレイヤーは「枠 0」にいます。`board[0]` は常に `0` で影響はありません。

```swift
repeat {
    // 蛇かはしご分移動する
    square += board[square]
    // サイコロを振る
    diceRoll += 1
    if diceRoll == 7 { diceRoll = 1 }
    // 出た目分移動
    square += diceRoll
} while square < finalSquare
print("ゲームオーバー!")
```

蛇かはしごかのチェック後に、サイコロが振られ、プレイヤーは `diceRoll` 分枠を移動します。そして現在のループを終了します。

ループの条件は前と同様\(`while square < finalSquare`\)ですが、今回はループを一度実行するまでは評価されません。今回の例では、`repeat-while` の構造は `while` 適よりしています。`repeat-while` では、`square += board[square]` は常に `while` 条件で `square` がまだボード上にいることを確かめた後にすぐ実行されます。この挙動で、`while` の例では必要だった配列の範囲チェックは必要なくなります。

## 条件文\(Conditional Statements\)

特定の条件に基づいてコードの異なる部分を実行すると役に立つことが多くあります。エラーが起きたときに追加でコードを実行したいこともあるでしょう。値が大きすぎたり、小さすぎたりしたらメッセージを表示したいかもしれません。この場合、コードを_条件付き_にすることで実現できます。

Swift は、コードに条件分岐を追加する 2 つの方法、`if` 文と `switch` 文、を提供しています。一般的に、`if` 文は、ごく少数の可能性があるシンプルな条件を評価するときに使います。`switch` 文は、様々な可能性の組み合わせを持つ、より複雑な条件に適し、パターンマッチが適切な実行コードを選択する手助けになる状況で役に立ちます。

### If

最もシンプルな形式の `if` 文は単一の 1 つの `if` 条件です。条件が `true` の場合にのみ、内部の文が実行されます。

```swift
var temperatureInFahrenheit = 30
if temperatureInFahrenheit <= 32 {
    print("とても寒いですね。マフラーを巻いたほうがいいでしょう。")
}
// とても寒いですね。マフラーを巻いたほうがいいでしょう。
```

上記の例は、気温が華氏 32 度以下かどうかをチェックしています。もしそうならばメッセージが出力されます。そうでないならばメッセージは出力されません。コードの実行は `if` 文の右中括弧\(`}`\)の後のコードが継続して実行します。

`if` 文は、条件が `false` の場合に `if` 文の代わりに実行する、`else` 句と呼ばれる文も提供できます。この文は `else` キーワードを使用して書かれます。

```swift
temperatureInFahrenheit = 40
if temperatureInFahrenheit <= 32 {
    print("とても寒いですね。マフラーを巻いたほうがいいでしょう。")
} else {
    print("そんなに寒くありません。Tシャツを着ましょう。")
}
// そんなに寒くありません。Tシャツを着ましょう。
```

この 2 つの分岐の内の 1 つが必ず実行されます。気温は華氏 40 度に増加しているので、スカーフを巻くアドバイスをする程寒くはなく、`else` の分岐が実行されます。

さらに句を追加して、複数の `if` 文をつなげることもできます。

```swift
temperatureInFahrenheit = 90
if temperatureInFahrenheit <= 32 {
    print("とても寒いですね。マフラーを巻いたほうがいいでしょう。")
} else if temperatureInFahrenheit >= 86 {
    print("とても暖かいですね。日焼け止めを忘れずにしましょう。")
} else {
    print("そんなに寒くありません。Tシャツを着ましょう。")
}
// とても暖かいですね。日焼け止めを忘れずにしましょう。
```

ここで、特定の暖かい温度に応答するために、追加の `if` 文が提供されています。最後の `else` 句で、暑すぎず寒すぎない温度に対する全ての応答を出力します。

しかし、最後の `else` 句は必須ではなく、条件が絶対に必要でない場合は、除くこともできます。

```swift
temperatureInFahrenheit = 72
if temperatureInFahrenheit <= 32 {
    print("とても寒いですね。マフラーを巻いたほうがいいでしょう。")
} else if temperatureInFahrenheit >= 86 {
    print("とても暖かいですね。日焼け止めを忘れずにしましょう。")
}
```

この温度は、暑すぎず寒すぎない温度なので、`if` も `else if` も実行されず、何も出力しません。

Swift は、値を設定するときに使用できる `if` の省略記法のスペルを提供します。例えば、以下のコードを考えてみましょう:

```swift
let temperatureInCelsius = 25
let weatherAdvice: String

if temperatureInCelsius <= 0 {
    weatherAdvice = "とても寒いですね。マフラーを巻いたほうがいいでしょう。"
} else if temperatureInCelsius >= 30 {
    weatherAdvice = "とても暖かいですね。日焼け止めを忘れずにしましょう。"
} else {
    weatherAdvice = "そんなに寒くありません。Tシャツを着ましょう。"
}

print(weatherAdvice)
// "そんなに寒くありません。Tシャツを着ましょう。"
```

ここでは、それぞれの分岐が `weatherAdvice` 定数の値を設定し、`if` 文の後で出力しています。

`if` 式と呼ばれる別の構文を使えば、このコードをより簡潔に書くことができます:

```swift
let weatherAdvice = if temperatureInCelsius <= 0 {
    "とても寒いですね。マフラーを巻いたほうがいいでしょう。"
} else if temperatureInCelsius >= 30 {
    "とても暖かいですね。日焼け止めを忘れずにしましょう。"
} else {
    "そんなに寒くありません。Tシャツを着ましょう。"
}

print(weatherAdvice)
// "そんなに寒くありません。Tシャツを着ましょう。"
```

この `if` 式のバージョンでは、各分岐に 1 つの値が含まれています。ある分岐の条件が真であれば、その分岐の値が `weatherAdvice` の割り当てられ、 `if` 式全体の値として使用されます。すべての `if` の分岐には対応する `else if` 分岐または `else` 分岐があり、どの条件が真であっても、必ずいずれかの分岐が一致し、`if` 式が常に値を生成します。

代入の構文は `if` 式の外側から始まるので、各分岐の中で `weatherAdvice =` を繰り返す必要はありません。その代わり、`if` 式の各分岐は `weatherAdvice` の 3 つの可能性のある値のうちの 1 つを生成し、代入後はその値を使用します。

`if` 式のすべての分岐には、同じ型の値を含む必要があります。Swift は各分岐の型を別々にチェックするので、複数の型で使用できる `nil` のような値は、Swift が `if` 式の型を自動的に決定することを妨げます。代わりに、あなたは明示的に型を指定する必要があります---例えば:

```swift
let freezeWarning: String? = if temperatureInCelsius <= 0 {
    "氷点下です。氷に注意しましょう!"
} else {
    nil
}
```

上のコードでは、`if` 式の一方の分岐に `String` 値が、もう一方の分岐に `nil` 値が設定されています。`nil` 値は任意のオプショナル型の値として使用できるため、 [型注釈\(Type Annotations\)](../language-guide/the-basics.md#type-annotations)にあるように、`freezeWarning` がオプショナルの文字列であることを明示的に記述する必要があります。
この型情報を提供する別の方法として、`freezeWarning` に明示的な型を提供する代わりに、`nil` に明示的な型を提供することができます：


```swift
let freezeWarning = if temperatureInCelsius <= 0 {
    "氷点下です。氷に注意しましょう！"
} else {
    nil as String?
}
```

`if` 式は、予期せぬ失敗に対して、エラーを投げたり、`fatalError(_:file:line:)` のように、戻り値を返さない関数を呼び出して対応することができます。例えば、次のようなものです:

```swift
let weatherAdvice = if temperatureInCelsius > 100 {
    throw TemperatureError.boiling
} else {
    "適度な気温ですね。"
}
```

この例では、`if` 式は予報気温が 100°C(水の沸点)より高いかどうかをチェックします。これほど高温になると、`if` 式はテキストの要約を返す代わりに、`.boiling` エラーを投げることになります。この `if` 式がエラーを投げる可能性があるにもかかわらず、その前に `try` を書いていません。エラーの扱いについては、[エラー処理\(Error Handling\)](../language-guide/error-handling.md)を参照してください。

上の例のように、`if` 式を代入の右辺で使うだけでなく、関数やクロージャが返す値として使うこともできます。

### Switch

`switch` 文は、複数の可能性に対してパターンマッチを使用して比較を行い、値を検討します。そして、一致した最初のパターンのコードのブロックを実行します。`switch` 文は、複数の可能性がある状態に対して `if` 文の代わりに使用することができます。

最もシンプルな形式として、`switch` 文は同じ型の 1 つ以上の値を比較します。

```swift
switch <#some value to consider#> {
case <#value 1#>:
    <#respond to value 1#>
case <#value 2#>,
    <#value 3#>:
    <#respond to value 2 or 3#>
default:
    <#otherwise, do something else#>
}
```

全ての `switch` 文は、`case` キーワードで始まる複数の可能性があるケースで構成されます。特定の値の比較に加えて、さらに複雑なパターンマッチをケースに指定するための様々な方法を提供しています。この章で後ほど記載しています。

`if` 文の本文と同じく、各ケースはコードを実行する分岐です。`switch` 文はどの分岐を選択するべきかどうかを決めます。このプロセスは、検証される値に基づいた切り替え\(_switching_\)と呼ばれます。

全ての `switch` 文は_網羅的_です。つまり、全ての検証される型の値は、`switch` ケースのどれか 1 つにマッチしなければなりません。特定のケースに当てはまらない可能性がある値を扱う場合、明示的にパターンに合致しないあらゆる値をカバーするデフォルトを定義することができます。このデフォルトケースは、`default` キーワードで示され、必ず最後に書かなければなりません。

下記の例では、`someCharacter` という 1 つの小文字を検証する `switch` 文です。

```swift
let someCharacter: Character = "z"
switch someCharacter {
case "a":
    print("ラテンアルファベットの最初の文字")
case "z":
    print("ラテンアルファベットの最後の文字")
default:
    print("その他の文字")
}
// ラテンアルファベットの最後の文字
```

`switch` 文の最初のケースは、英語アルファベットの最初の文字 `a` に合致し、2 番目のケースは最後の文字 `z` に合致します。全ての可能性がある文字をカバーしなければならないため、`a` と `z` 以外の全ての文字に対して `default` ケースを使用しています。こうすることで全てのケースを網羅できています。

`if` 文と同様に、`switch` 文にも式があります:

```swift
let anotherCharacter: Character = "a"
let message = switch anotherCharacter {
case "a":
    "ラテンアルファベットの最初の文字"
case "z":
    "ラテンアルファベットの最後の文字"
default:
    "その他の文字"
}

print(message)
// "ラテンアルファベットの最初の文字"
```

この例では、`switch` 式の各ケースには、そのケースが `anotherCharacter` にマッチしたときに使用される `message` の値が含まれています。`switch` は常に網羅的であるため、代入する値は必ず存在します。

`if` 式と同様に、与えられたケースに値を与える代わりに、エラーを投げたり、`fatalError(_:file:line:)` のように戻り値を返さない関数を呼び出すことができます。`switch` 式は、上の例のように代入の右辺で使ったり、関数やクロージャが返す値として使うことができます。

#### 暗黙的にfallthroughしない\(No Implicit Fallthrough\)

C 言語や Objective-C と異なり、Swift の `switch` 文は、デフォルトでは、それぞれのケースの下から次のケースに行くことはありません。その代わりに、最初に合致した `switch` ケースの実行が完了すると、明示的に `break` しなくても、全体の `switch` 文も完了します。こうすることで、C 言語の `switch` 文よりも、より安全に簡単に使えるようにしています。間違って 1 つ以上のケースを実行してしまうリスクを防ぎます。

> NOTE  
> `break` は必須ではありませんが、合致したけど何も実行しないような場合や、あるケースの中で最後まで実行される前にそのブロックを抜けたい場合に、`break` を使用することができます。詳細は、[Switch内でBreak\(Break in a Switch Statement\)](control-flow.md#break-in-a-switch-statement)
を参照ください。

各ケースの本文は少なくとも 1 つの文を実行しなければなりません。次のコードは最初のケースの本文が空なので不正です。

```swift
let anotherCharacter: Character = "a"
switch anotherCharacter {
case "a": // 空の 本文 は不正
case "A":
    print("文字 A")
default:
    print("A 以外の文字")
}
// コンパイルエラーが発生します
```

C 言語の `switch` 文と異なり、`"a"` と `"A"` の両方に合致することはありません。`case "a":` は実行文が何も含まれていないため、コンパイルエラーが発生します。こうすることで、不意に 1 つのケースから他のケースに進むことを防ぎ、意図がより明確な安全なコードにすることができます。

`"a"` と `"A"` の両方に合致させたい場合は、カンマ区切り\(`,`\)で値を区切って、2 つの値を 1 つのケースに合成します。

```swift
let anotherCharacter: Character = "a"
switch anotherCharacter {
case "a", "A":
    print("文字 A")
default:
    print("A 以外の文字")
}
// 文字 A
```

可読性のために、複合ケースを複数行に分けて書くこともできます。複合ケースについての詳細は[Compound Cases\(複合ケース\)](control-flow.md#compound-cases)を参照ください。

> NOTE  
> 特定のケースから次のケースの 本文 を実行したい場合は、`fallthrough` キーワードを使います。[Fallthrough](control-flow.md#fallthrough)に記載しています。

#### 範囲マッチング\(Interval Matching\)

`switch` ケースの値には、範囲内に含まれるかどうかのチェックもできます。下記の例では、数値の範囲を使用して、任意のサイズの数値の自然言語カウントを提供します。

```swift
let approximateCount = 62
let countedThings = "土星を回る月"
let naturalCount: String
switch approximateCount {
case 0:
    naturalCount = "まったくない"
case 1..<5:
    naturalCount = "少しある"
case 5..<12:
    naturalCount = "多少ある"
case 12..<100:
    naturalCount = "数多くある"
case 100..<1000:
    naturalCount = "たくさんある"
default:
    naturalCount = "膨大にある"
}
print("\(naturalCount) は \(countedThings)。")
// 土星を回る月 は 数多くある。
```

上記の例では、`approximateCount` が `switch` 文で評価されています。それぞれのケースでは、1 つの数値または範囲で比較しています。`approximateCount` の値は、12 から 100 の間にあるので、`naturalCount` には `"数多くある"` という値が代入されます。実行後は `switch` 文から抜け出します。

#### タプル\(Tuples\)

タプルを使用して同じ `switch` 文の中で、複数の値を検証することができます。タプルの個々の要素は、異なる値や範囲に対して検証できます。逆に、アンダースコア\(`_`\)を使用すると、ワイルドカードとしてどんな値にも合致させることができます。

下記の例では、`(Int, Int)` 型のタプルとして `(x, y)` 座標を受け取り、グラフに分布しています。

```swift
let somePoint = (1, 1)
switch somePoint {
case (0, 0):
    print("\(somePoint) は原点にあります")
case (_, 0):
    print("\(somePoint) は x 軸上にあります")
case (0, _):
    print("\(somePoint) は y 軸上にあります")
case (-2...2, -2...2):
    print("\(somePoint) はボックスの中にあります")
default:
    print("\(somePoint) はボックスの外にあります")
}
// (1, 1) はボックスの中にあります
```

![switch&#x6587; &#x5EA7;&#x6A19;&#x5206;&#x5E03;&#x56F3;](../assets/coordinategraphsimple_2x.png)

`switch` 文は、座標が原点 `(0、0)` にあるか、赤い x 軸上にあるか、緑の y 軸上にあるか、原点を中心とする青い 4x4 列のボックスの内側にあるか、ボックスの外側にあるかを判別します。

C 言語と異なり、Swift では複数のケースで同じ値を検証することができます。実際、この例では `(0、0)` は全ての 4 つのケースに当てはまります。複数に合致する場合は、常に最初に合致したケースが使われます。`(0、0)` は最初に `case (0, 0)` に合致するので、他のケースは無視します。

#### 値バインディング\(Value Bindings\)

`switch` ケースは、合致した値を、ケースの本文で使用するために、一時的な定数または変数に一致する 1 つまたは複数の値に名前を付けることができます。この挙動は、値を変数や定数にバインドするので、_値バインディング_と呼ばれています。

下記の例では、`(Int, Int)` 型のタプルとして `(x, y)` 座標を受け取り、グラフに分布しています。

```swift
let anotherPoint = (2, 0)
switch anotherPoint {
case (let x, 0):
    print("x 軸上の x の値が \(x) の点")
case (0, let y):
    print("y 軸上の y の値が \(y) の点")
case let (x, y):
    print("その他の (\(x), \(y)) の点")
}
// x 軸上の x の値が 2 の点
```

![switch&#x6587; &#x5EA7;&#x6A19;&#x5206;&#x5E03;&#x56F3;](../assets/coordinategraphmedium_2x.png)

`switch` 文は、座標が赤い x 軸上にあるか、緑の y 軸上にあるか、または他の場所にあるか\(どちらの軸上にもないか\)を判別します。

この 3 つの `switch` ケースは、`anotherPoint` から 1 つまたは両方のタプルの値を、プレースホルダ定数 `x` と `y` として宣言しています。最初の `case (let x, 0)` は、`y` は 0 で、座標 `x` の値を定数 `x` に代入した任意の値に合致します。同様に、2 番目の `case (0, let y)` は、`x` は 0 で、座標 `y` の値を定数 `y` に代入した任意の値に合致します。

定数は、そのケースのブロック内で使用することができます。ここでは、座標の分布を出力するために使われています。

`switch` 文は、`default` のケースが必ず必要な訳ではありません。最後の `case let (x, y)` は、どんな値にも合致する 2 つのプレースホルダとして定数を宣言しています。`anotherPoint` は、常に 2 つの値を持つタプルなので、このケースで、先の 2 つのケース以外の全ての値に合致するため、`switch` 文で全ての値を網羅するための `default` ケースは不要です。

#### Where

`switch` 文は、追加条件として `where` 句を使用することができます。

下記の例では、`(x, y)` 座標を受け取り、グラフに分布しています。

```swift
let yetAnotherPoint = (1, -1)
switch yetAnotherPoint {
case let (x, y) where x == y:
    print("(\(x), \(y)) は x == y の線上にあります")
case let (x, y) where x == -y:
    print("(\(x), \(y)) は x == -y の線上にあります")
case let (x, y):
    print("(\(x), \(y)) は単なる任意の点です")
}
// (1, -1) は x == -y 線上にあります
```

![switch&#x6587; where&#x53E5; &#x5EA7;&#x6A19;&#x5206;&#x5E03;&#x56F3;](../assets/coordinategraphcomplex_2x.png)

`switch` 文は、座標が `x == y` の場合は緑色の対角線上にあるか、`x == -y` の場合は紫色の対角線上にあるか、またはどちらでもないかを判別します。

この 3 つの `switch` ケースは、`yetAnotherPoint` から 1 つまたは両方のタプルの値を、プレースホルダ定数 `x` と `y` として宣言しています。これらの定数は `where` 句の一部で使われており、動的なフィルターを作成しています。`switch` ケースには、`point` の現在値が `where` の条件で `true` になった場合のみ合致します。

最後のケースは、先の 2 つのケース以外の全ての値に合致するため、`switch` 文で全ての値を網羅するための `default` ケースは不要です。

#### <a id="compound-cases">複合ケース\(Compound Cases\)</a>

各ケースをカンマ\(`,`\)で区切ってケースの後に書くことで、同じ本文を共有する複数のケースを書くことができます。その中の 1 つのケースに合致した場合に、合致したと見なされます。ケースのリストが長くなるような場合は、複数行にまたがって書くことができます。例えば:

```swift
let someCharacter: Character = "e"
switch someCharacter {
case "a", "e", "i", "o", "u":
    print("\(someCharacter) は母音です")
case "b", "c", "d", "f", "g", "h", "j", "k", "l", "m",
     "n", "p", "q", "r", "s", "t", "v", "w", "x", "y", "z":
    print("\(someCharacter) は子音です")
default:
    print("\(someCharacter) は母音でも子音でもありません")
}
// e は母音です
```

最初のケースは英語の小文字の母音全てに合致します。同様に、2 番目のケースは英語の子音全てに合致します。`default` は他の全ての文字に合致します。

複合ケースは、値バインディングを含めることもできます。複合ケース内の全てのパターンには、同じ値バインディングのセットが含まれている必要があり、各バインディングは、全てのパターンから同じ型の値を取得する必要があります。こうすることで、複合ケース内のどのパターンでも、本文内でバインディングされた値にアクセスすることができ、型も常に同じことが保証できます。

```swift
let stillAnotherPoint = (9, 0)
switch stillAnotherPoint {
case (let distance, 0), (0, let distance):
    print("x 軸上または y 軸上にあり、原点から \(distance) 離れている")
default:
    print("x 軸上または y 軸上にはない")
}
// x 軸上または y 軸上にあり、原点から 9 離れている
```

上記のケースでは 2 つのパターンを含んでいます: `(let distance, 0)` は x 軸上にある点に合致し、`(0, let distance)` は y 軸上にある点に合致します。どちらのパターンでも、`distance` へのバインディングが含まれており、どちらも数値です。つまり、ケース内では常に `distance` にアクセスできます。

## <a id="patterns">パターン\(Patterns\)</a>

これまでの例では、それぞれの `switch` の `case` に、その `case` にマッチする値を示すパターンが含まれていました。`if` 文の条件式としてもパターンを使うことができます。次のようになります。

```swift
let somePoint = (12, 100)
if case (let x, 100) = somePoint {
    print("y=100線上に点\(x)を発見")
}
// "y=100線上に点12を発見" と出力
```

このコードでは、`if` 文の条件式が `case` で始まっていますが、これは条件がブール値ではなくパターンであることを示しています。パターンがマッチした場合、`if` の条件は `true` と見なされ、`if` 文本体のコードが実行されます。`if case` の後に記述できるパターンは、`switch` の `case` で記述できるパターンと同じです。

`for-in` ループでは、コードに `case` を書かなくても、値をバインドするパターンを使って値の各部分に名前を付けることができます。

```swift
let points = [(10, 0), (30, -30), (-20, 0)]

for (x, y) in points {
    if y == 0 {
        print("x軸上に点\(x)を発見")
    }
}
// "x軸上に点10を発見" と出力
// "x軸上に点-20を発見" と出力
```

上記の `for-in` ループはタプルの配列をイテレートし、タプルの 1 番目と 2 番目の要素を定数 `x` と `y` にバインドします。ループ内の文では、点が x 軸上にあるかどうかをチェックする `if` 文のように、これらの定数を使用できます。このコードをより簡潔に書くには、`for-case-in` ループを使って値のバインドと条件を組み合わせます。以下のコードは、上記の `for-in` ループと同じ動作をします。

```swift
for case (let x, 0) in points {
    print("x軸上に点\(x)を発見")
}
// "x軸上に点10を発見" と出力
// "x軸上に点-20を発見" と出力
```

このコードでは、条件がパターンの一部として `for-case-in` ループに統合されています。`for-case-in` ループ内の文は、x 軸上の点に対してのみ実行されます。このコードは上記の `for-in` ループと同じ結果を生成しますが、コレクション内の特定の要素だけをイテレートするための、よりコンパクトな方法です。

`for-case-in` ループには、追加の条件をチェックするために `where` 節を含めることもできます。ループ内の文は、`where` 節が現在の要素に対して `true` である場合にのみ実行されます。例えば、次のようになります。

```swift
for case let (x, y) in points where x == y || x == -y  {
    print("原点を通る直線上に (\(x), \(y)) を発見")
}
// "原点を通る直線上に (30, -30) を発見" と出力
```

このコードは、タプルの 1 番目と 2 番目の要素を定数 `x` と `y` にバインドし、`where` 節でそれらの値をチェックします。`where` 節が `true` の場合、`for` ループの本体のコードが実行されます。そうでなければ、イテレーションは次の要素へと続きます。

パターンは値をバインドできるため、`if-case` 文と `for-case-in` ループは、[関連値\(Associated Values\)](./enumerations.md#associated-values) で説明されているように、関連値を持つ列挙型を扱うのに便利です。

## <a id="control-flow-control-transfer-statements">制御転送文\(Control Transfer Statements\)</a>

_制御転送文_は、コードの一部の制御を他の部分へ転送させることで、コードの実行順序を変更します。Swift には 5 つの制御転送文があります:

* `continue`
* `break`
* `fallthrough`
* `return`
* `throw`

`continue`、`break` と `fallthrough` は下記に記載しています。`return` は[Functions\(関数\)](functions.md)、`throw` は[Propagating Errors Using Throwing Functions](../language-guide/error-handling.md#propagating-errors-using-throwing-functions)に記載しています。

### Continue

`continue` 文は、各ループ内の実行を止めて、次のループの最初から処理を開始します。つまり、ループを抜けることなく「今のループ処理を完了します」と伝えます。

次の例は、全ての母音とスペースを小文字の文字列から除いて、暗号パズルフレーズを作成しています:

```swift
let puzzleInput = "great minds think alike"
var puzzleOutput = ""
let charactersToRemove: [Character] = ["a", "e", "i", "o", "u", " "]
for character in puzzleInput {
    if charactersToRemove.contains(character) {
        continue
    }
    puzzleOutput.append(character)
}
print(puzzleOutput)
// grtmndsthnklk
```

上記のコードは、母音かスペースの場合に即座に現在のループ処理を終了し、次のループ処理の開始に移動するように `continue` キーワードを使用しています。

### Break

`break` 文は、即座に全体の制御フローの実行を終了させます。`break` は、`switch` 文かループ文の内部で、他のケースよりも早期に処理の実行を終了させたいときに使います。

#### ループ内でBreak\(Break in a Loop Statement\)

ループ文の内部で使用するとき、`break` はループの実行を即座に終了し、ループの末尾の閉じ括弧\(`}`\)の後に制御フローを移します。ループ内の続きの処理は実行されず、次のループも開始されません。

#### <a id="break-in-a-switch-statement">Switch内でBreak\(Break in a Switch Statement\)</a>

`switch` 文の内部で使用するとき、`break` は `switch` の実行を即座に終了し、`switch` の末尾の閉じ括弧\(`}`\)の後に制御フローを移します。`switch` 文は網羅的で空のケースが許されないため、明示的に何もしないことを伝えるために、わざと合致させて無視するときに必要になります。無視したいケースの本文に `break` のみを書きます。そのケースに合致した場合、ケース内の `break` が `switch` 文全体の実行を即時に終了させます。

> NOTE  
> コメントだけを含めた `switch` ケースは、コンパイルエラーになります。コメントは文ではなく、`switch` ケースを無視することができません。`switch` ケースを無視するには `break` を常に使います。

次の例では、`Character` 型の値が、4 つの言語内の 1 つの番号記号に合致するかどうかを判定しています。簡潔にするために、複数の値を 1 つの `switch` ケースで網羅しています。

```swift
let numberSymbol: Character = "三"  // 中国語の数字 3
var possibleIntegerValue: Int?
switch numberSymbol {
case "1", "١", "一", "๑":
    possibleIntegerValue = 1
case "2", "٢", "二", "๒":
    possibleIntegerValue = 2
case "3", "٣", "三", "๓":
    possibleIntegerValue = 3
case "4", "٤", "四", "๔":
    possibleIntegerValue = 4
default:
    break
}
if let integerValue = possibleIntegerValue {
    print("\(numberSymbol) の整数値は \(integerValue) です。")
} else {
    print("\(numberSymbol) の整数値は見つかりませんでした。")
}
// 三 の整数値は 3 です。
```

この例では、`numberSymbol` が `1`〜`4` のラテン語、アラビア語、中国語、タイ語に合致するかどうかをチェックしています。合致した場合、各ケースは、`Int?` の `possibleIntegerValue` 変数に適切な数字を設定しています。

`switch` 文の実行完了後、この例では値が見つかったかどうかを判定するのに、オプショナルバインディングを使用しています。`possibleIntegerValue` 変数は、オプショナル型の性質上、`nil` が暗黙的に初期値になります。`switch` の最初の 4 つのケースで `possibleIntegerValue` に値が設定された場合のみ、オプショナルバインディングは成功します。

上記の例で `Character` 型の値を全部羅列することは現実的ではないので、`default` ケースで合致しない全ての値を処理します。`default` ケースでは何もする必要がないので、本文には `break` のみを記載しています。`default` ケースが合致すると、`break` 文は `switch` 文を終了させ、次の `if let` からコードを継続します。

### Fallthrough

Swift の `switch` 文は、各ケースの底から次のケースに通り抜けることはしません。つまり、最初に合致したケースが完了したらすぐに `switch` 文全体の実行が終了します。逆に、C 言語の `switch` 文では、各ケースへ通り抜けることを防ぐために、明示的に `break` を各ケースの末尾に書かなければなりません。Swift の `switch` 文は、デフォルトで通り抜けないことで、C 言語とは反対に、より簡潔で挙動が予測しやすくなっています。間違って複数のケースを実行することを防ぎます。

もし C 言語のような通り抜けが必要な場合は、`fallthrough` キーワードを使用してケースごとに設定することができます。下記の例では、`fallthrough` を使用して数値のテキストによる説明を作成します。

```swift
let integerToDescribe = 5
var description = "数字 \(integerToDescribe) は"
switch integerToDescribe {
case 2, 3, 5, 7, 11, 13, 17, 19:
    description += " 素数です。そして、"
    fallthrough
default:
    description += "整数です。"
}
print(description)
// 数字 5 は 素数です。そして、整数です。
```

この例では、`description` という新しい `String` 変数を宣言して、初期値を設定しています。この関数は `switch` 文で `integerToDescribe` を検証しています。`integerToDescribe` がリストの素数の 1 つの場合、この関数は `description` の末尾に、数字が素数だと説明したテキストを追加します。`default` ケースにも「通り抜ける」ように、`fallthrough` キーワードを使用しています。`default` ケースでは、説明の末尾に追加のテキストを追加して `switch` 文は完了します。数字の説明は、`print(_:separator:terminator:)` 関数を使用して出力されています。この例では、素数として `5` が適切に特定されています。

> NOTE  
> `fallthrough` キーワードは、前のケースから通り抜けて実行されたケースの条件をチェックしません。`fallthrough` キーワードは次のケース\(または `default` ケース\)のブロック内の文に直接移動し、コードを実行します。これは C 言語と同じ挙動です。

### <a id="labeled-statements">ラベル付き文\(Labeled Statements\)</a>

Swift では、より複雑な制御フローを作成するために、ループや条件文の中にさらにループや条件文をネストできます。しかし、`break` を使用して早期に実行を終了させることもできます。そのため、どの文を `break` するのかを明示すると役に立つことがあります。同様に、複数のネストしたループで、どのループに `continue` が影響するかを明示することも役に立ちます。

これを実現するために、ループや条件文にラベルを付けることができます。条件文では、`break` と_文ラベル_を使用して実行を終了できます。ループでは、`break` か `continue` と_文ラベル_を使用してラベル付き文の実行を終了または継続することができます。

ラベル付き文は、文の導入キーワードの前にコロン\(`:`\)を付けて同じ行にラベルを配置することで示せます。下記は、`while` ループの構文の例ですが、他の全てのループや `switch` にも同じ原則が当てはまります。

```swift
<#label name#>: while <#condition#> {
   <#statements#>
}
```

次の例は、この章の前で見てきた「蛇とはしご」にラベル付き `while` ループと `break` と `continue` 文を適用したバージョンです。今回は、追加のルールがあります。

* 勝つためには、ちょうど枠 25 に到達しなければなりません

サイコロの結果で枠 25 を超えた場合、ちょうど枠 25 に到達するまでサイコロを振らなければなりません。

ゲーム盤は、前と同じです。

![&#x86C7;&#x3068;&#x306F;&#x3057;&#x3054;](../assets/snakesandladders_2x.png)

`finalSquare`、`board`、`square` と `diceRoll` は前と同じように初期化されています。

```swift
let finalSquare = 25
var board = [Int](repeating: 0, count: finalSquare + 1)
board[03] = +08; board[06] = +11; board[09] = +09; board[10] = +02
board[14] = -10; board[19] = -11; board[22] = -02; board[24] = -08
var square = 0
var diceRoll = 0
```

今回のバージョンでは、`while` ループと `switch` 文をロジックの実装に使用しています。`while` ループは、`gameLoop` と呼ばれるラベルを付けて、これがゲームのメインのループだということを示しています。

`while` ループ条件は `while square != finalSquare` で、正確に枠 25 に到達しなければならないことを反映しています。

```swift
gameLoop: while square != finalSquare {
    diceRoll += 1
    if diceRoll == 7 { diceRoll = 1 }
    switch square + diceRoll {
    case finalSquare:
        // diceRoll が最後の枠に移動したらゲームは終了します
        break gameLoop
    case let newSquare where newSquare > finalSquare:
        // diceRoll が最後の枠を超えた場合はサイコロを再度振ります
        continue gameLoop
    default:
        // 妥当な移動であった場合、その値を反映します
        square += diceRoll
        square += board[square]
    }
}
print("ゲームオーバー!")
```

各ループの最初でサイコロを振ります。即座にプレイヤーが移動する前に、ループの移動結果を検証して、その移動ができるかどうかを判定しています:

* サイコロの結果でプレイヤーが最後の枠に移動したらゲームは終了します。`break gameLoop` は `while` ループの外側の最初の行へフローを移し、ゲームは終わります
* `diceRoll` が最後の枠を超えた場合は、その移動は不正で、プレイヤーは再度サイコロを振る必要があります。`continue gameLoop` で現在の `while` のループを終了し、次のループを開始します
* 他の全てのケースでは、サイコロの結果は妥当です。プレイヤーは `diceRoll` 分移動し、蛇かはしごかのチェックをします。ループが終了後、フローは `while` の条件文に戻り、次のループが必要かどうかを判定します

> NOTE  
> もし `break` が `gameLoop` ラベルを付けなかった場合、`while` ではなく `switch` から抜けます。`gameLoop` ラベルを使用することで、どの制御文から抜けるのかを明確にできます  
> 次のループに移動するのに `continue gameLoop` と `gameLoop` ラベルを使用する必要は厳密にはありません。ゲーム内にループは 1 つしかなく、どのループに `continue` が影響を与えるかが明確です。しかし、`gameLoop` ラベルを使用しても害はなく、そうすることで `break` のラベルと一貫性ができ、ロジックを読みやすく理解しやすくすることができます。

## <a id="early-exit">早期リターン\(Early Exit\)</a>

`guard` 文は `if` 文と同様に、式のブール値に応じて文を実行します。`guard` 文は、`guard` の後のコードを実行するために、`guard` の条件が `true` でなければならない場合に使います。`if` 文と異なり、`guard` 文は常に `else` 句が必要で、条件が `false` の場合に `else` 内が実行されます。

```swift
func greet(person: [String: String]) {
    guard let name = person["name"] else {
        return
    }

    print("こんにちは \(name)!")

    guard let location = person["location"] else {
        print("そちらのお天気は良いといいですね。")
        return
    }

    print("\(location) のお天気は良いといいですね。")
}

greet(person: ["name": "John"])
// こんにちは John!
// そちらのお天気は良いといいですね。
greet(person: ["name": "Jane", "location": "Cupertino"])
// こんにちは Jane!
// Cupertino のお天気は良いといいですね。
```

`guard` 文の条件を満たした場合、`guard` の閉じ括弧\(`}`\)以降のコードを続けて実行します。条件の一部としてオプショナルバインディングに使われている変数や定数は、`guard` の後のコードで利用できるようになります。

条件を満たさない場合、`else` 句の内部を実行します。この分岐内では、`guard` 文内のコードブロックを抜けるために、制御を他へ転送させなければなりません。これは、`return`、`break`、`continue` や `throw` といった制御転送文を使用することができます。あるいは、`fatalError(_:file:line:)` のように値を返さない関数やメソッドを呼びます。

`guard` 文を必須条件に使用することで、`if` 文で同じことを行うよりも可読性を改善させることができます。概ね `else` ブロックで包むことなくコードを書くことができたり、必須条件のすぐ下で条件を満たさないケースを処理することができます。

## <a id="deferred-actions">遅延アクション\(Deferred Actions\)</a>

`if` や `while` のように、コードの一部が実行されるかどうか、何回実行されるかを制御する制御フローの構成要素とは異なり、`defer` はコードの一部がいつ実行されるかを制御します。`defer` ブロックは、プログラムが現在のスコープの最後に到達したときに、後で実行されるコードを記述するために使用されます。たとえば、次のように使用します:

```swift
var score = 1
if score < 10 {
    defer {
        print(score)
    }
    score += 5
}
// "6"を出力
```

上の例では、`defer` ブロックの中のコードは、`if` 文の本文を抜ける前に実行されます。まず、`if` 文の中のコードが実行され、スコアが 5 増えます。そして、`if` 文のスコープを抜ける前に、`defer` のコードが実行され、`score` が表示されます。

`defer` の中のコードは、プログラムがそのスコープをどのように抜けるかにかかわらず、常に実行されます。これには、関数からの早期終了、`for` ループからの脱出、エラーのスローといったコードも含まれます。この動作は、メモリの確保と解放、低レベルのファイル記述子のオープンとクローズ、データベースのトランザクションの開始と終了など、2 つの動作が起こることを保証する必要がある操作に便利です。たとえば、次のコードは、コードの塊の中で 100 を足したり引いたりすることによって、スコアに一時的なボーナスを与えるものです:

```swift
var score = 3
if score < 100 {
    score += 100
    defer {
        score -= 100
    }
    // ボーナスでスコアを使用する他のコード
    print(score)
}
// "103"を出力
```

同じスコープに複数の `defer` ブロックを記述した場合、最初に記述したものが最後に実行されます。

```swift
if score < 10 {
    defer {
        print(score)
    }
    defer {
        print("スコアは:")
    }
    score += 5
}
// "スコアは:"を出力
// "6"を出力
```

プログラムの実行が停止した場合(たとえば、実行時エラーやクラッシュのため)、`defer` 内のコードは実行されません。しかし、`defer` 内のコードはエラーが発生した後に実行されます。エラー処理での `defer` の使用については、[クリーンアップアクションの指定\(Specifying Cleanup Actions\)](../language-guide/error-handling.md#クリーンアップアクションの指定specifying-cleanup-actions) を参照してください。


## <a id="checking-api-availability">APIアベイラビリティチェック\(Checking API Availability\)</a>

Swift は API のアベイラビリティチェックをビルトインでサポートしています。つまり、あるデプロイターゲットでは利用できない API を間違って使わないことが保証できます。

コンパイラはコード内で使われている全ての API がプロジェクトで特定したデプロイターゲットで使用可能かどうかを検証するために、SDK 内のアベイラビリティ情報を使います。利用できない API を使おうとしていた場合、コンパイル時にエラーを報告します。

使用する API が実行時に使用可能かどうかに応じて、`if` 文や `guard` 文で_アベイラビリティ条件_を使用して、コードのブロックを条件付きで実行します。コンパイラはこのコードブロックが使用可能かどうかを検証するときに、このアベイラビリティ条件からの情報を使います。

```swift
if #available(iOS 10, macOS 10.12, *) {
    // iOS 10以上またはmacOS 10.12以上で実行
} else {
    // それ以前のiOSとmacOSで実行
}
```

上記のアベイラビリティ条件は、iOS では、`if` 文が `iOS10` 以降でのみ実行されることを指定しています。`macOS` では、`macOS10.12` 以降のみになります。最後の引数 `*` は必須で、他のプラットフォームでは、`if` の本文がターゲットで指定された最小のデプロイメントターゲットで実行されることを示します。

基本的な形式として、アベイラビリティ条件はプラットフォームの名前とバージョンのリストを受け取ります。`iOS`、`macOS`、`watchOS`、`tvOS`、`visionOS` などをプラットフォームの名前として使います。全リストは[宣言属性\(Declaration Attributes\)](../language-reference/attributes.md)を参照ください。`iOS 8` や `macOS 10.10` のようなメジャーバージョンに加え、`iOS 11.2.6` や `macOS 10.13.3` のようにマイナーバージョンも指定できます。

```swift
if #available(<#platform name#> <#version#>, <#...#>, *) {
    <#statements to execute if the APIs are available#>
} else {
    <#fallback statements to execute if the APIs are unavailable#>
}
```

`guard` 文でアベイラビリティ条件を使用すると、そのコードブロック内の残りのコードに使用されるアベイラビリティ情報が絞り込まれます。

```swift
@available(macOS 10.12, *)
struct ColorPreference {
    var bestColor = "blue"
}

func chooseBestColor() -> String {
    guard #available(macOS 10.12, *) else {
        return "gray"
    }
    let colors = ColorPreference()
    return colors.bestColor
}
```

上記の例では、`ColorPreference` 構造体には `macOS10.12` 以降が必要です。 `chooseBestColor()` 関数は、アベイラビリティガードから始まります。プラットフォームのバージョンが古すぎて `ColorPreference` を使用できない場合は、常に利用可能な動作にフォールバックします。`guard` 文の後、`macOS10.12` 以降を必要とする API を使用できます。

`#available` に加えて、Swift は使用不可条件を使用した反対のチェックもサポートします。 例えば、次の 2 つのチェックは同じことを行います。

```swift
if #available(iOS 10, *) {
} else {
    // フォールバックコード
}

if #unavailable(iOS 10) {
    // フォールバックコード
}
```

`#unavailable` 形式を使用すると、フォールバックコードのみが含まれている場合に、コードが読みやすくなります。