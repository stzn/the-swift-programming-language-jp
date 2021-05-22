# Control Flow

最終更新日:

Swift は、様々な制御フロー(*Control Flow*)を提供しています。1 つのタスクを複数回行う `while` ループ、ある条件を基に異なる分岐のコードを実行する `if`,`guard`,`switch`、コードの他の地点へ実行フローを移動させる `break` や `continue` のような文があります。

また、配列や辞書、範囲、文字列や他のシーケンスに対して、簡単に繰り返し処理を行う `for-in` ループも提供しています。

Swift の `switch` 文は、C のような他の言語と比べてもかなり強力です。case は、範囲やタプル、特定の型へのキャストなど、様々なパターンに合致させることができます。`switch` の case に合致した値は、case の body 内で使用するために変数や定数に紐づけることができます。各ケースに `where` を使ってより複雑な条件を作ることもできます。

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

`while` ループは条件が `false` になるまで内部の文を実行します。`while` ループは、最初の繰り返しが始まるまで、何回繰り返すのかがわからないような時に最も使われます。

* `while` は、ループ内の処理を実行する前に条件を評価します
* `repeat-while` は、ループ内の処理を実行した後に条件を評価します

### while

`while` は、条件を評価してからループを開始します。`true` ならば条件が `false` になるまでループ内の処理を繰り返します。

`while` ループの基本的な書き方は下記の通りです:

![While書き方](../.gitbook/assets/05_whileform.png)

下記の例は、「蛇とはしご(*Snakes and ladders*)」と呼ばれるシンプルなゲームの例です。

![蛇とはしご](./../.gitbook/assets/snakesandladders_2x.png)

ゲームのルールは下記のようです:

* ボードには 25 の枠があり、目的は枠 25 以上に到達することです
* プレイヤーはボードの左下を「0 枠」として、そこから始めます
* それぞれの番で、サイコロを振り、出た目分、画像のドットに従って水平に移動します
* 画像のはしごの下の枠に止まったら、はしごの上の枠に行きます
* 蛇の頭の枠に止まったら、蛇のしっぽの枠に行きます

このボードは、`Int` 型配列で表されます。サイズは配列の初期化や、勝者を決める条件に使用されている `finalSquare` という定数で決められます。プレイヤーは、「0 枠」から始まるので、`25` ではなく、`26` 個の 0 でボードは初期化されています。

```swift
let finalSquare = 25
var board = [Int](repeating: 0, count: finalSquare + 1)
```

いくつかの枠は蛇やはしごのために特別な値を設定します。はしごの枠はボードの上に移動させるので正の値を、蛇はボードの下に移動させるので負の値を設定します。

```swift
board[03] = +08; board[06] = +11; board[09] = +09; board[10] = +02
board[14] = -10; board[19] = -11; board[22] = -02; board[24] = -08
```

3 枠は 11 枠に移動します。これを表すには `board[03]` は `+08` で、(11 と 3 の差分の)数字の 8 に相応します。値と文を見てみると、単項加算演算子(`+i`)、単項減算演算子(`-i`)が明示的に使われ、10 以下の値は 0 が前に付いています。(厳密に 0`は必要ありませんが、より読みやすくできます)

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
print("Game over!")
```

この例では、サイコロを降るアクションにとてもシンプルな方法を使っています。 ランダムな数字を生成するのではなく、`0` で初期化された `diceRoll` から開始します。`while` ループが繰り返される度に、`diceRoll` は 1 つずつ加算され、値が大き過ぎないかをチェックしています。`7` になると、大き過ぎるため `1` にリセットされます。 つまり、`diceRoll` は `1, 2, 3, 4, 5, 6, 1, 2` のように繰り返されます。

サイコロを振った後、 `diceRoll` 分移動します。25 を超すこともありますが、その場合はゲーム終了です。これを実現するために、コードでは `square` が `board` 配列の `count` よりも小さいことをチェックしています。`square` が妥当ならば、`board[square]` を `square` に加算され、蛇やはしごの場合はプレイヤーはさらに移動します。

> NOTE  
> このチェックがないと、`board[square]` は `board` の範囲を超えてアクセスする可能性があり、実行時エラーが発生します。

現在の `while` ループが終了し、次のループを実行するかどうかを決めるために、ループの条件をチェックします。プレイヤーは 25 枠またはそれ以上に到達していた場合、ループ条件は `false` になり、ゲームは終了します。

今回のケースは、ループの開始時にどのくらいゲームが続くのかがわからないため、`while` ループが適切です。代わりに条件が満たされるまで繰り返し処理が実行されます。

### Repeat-While

`while` ループの別の形として、`repeat-while` があり、ループ条件を検証する前に、一度ループ内の処理を実行します。その後、条件が `false` になるまで、ループを繰り返します。

> NOTE  
> Swiftの `repeat-while` は、他の言語でいうところの `do-while` に当たります

下記が `repeat-while` ループ一般的な形式です。

![repeat-while](./../.gitbook/assets/05_repeatwhile.png)

ここで蛇とはしごの例を `while` の代わりに `repeat-while` を使って書き換えてみます。`finalSquare`、 `board`、`square` と `diceRoll` は `while` と全く同じ方法で初期化します。

```swift
let finalSquare = 25
var board = [Int](repeating: 0, count: finalSquare + 1)
board[03] = +08; board[06] = +11; board[09] = +09; board[10] = +02
board[14] = -10; board[19] = -11; board[22] = -02; board[24] = -08
var square = 0
var diceRoll = 0
```

このバージョンのゲームでは、ループの最初で、蛇かはしごかをチェックしています。プレイヤーを 25 枠に直接移動させるはしごはないので、はしごを上るだけで勝利することはできません。なので、ループの最初で蛇かはしごかをチェックするのは安全です。

ゲームの最初はにプレイヤーは「0 枠」にいます。`board[0]` は常に 0 で影響はありません。

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
print("Game over!")
```

蛇かはしごかのチェック後に、サイコロが振られ、プレイヤーは `diceRoll` 分の枠を移動します。そして現在のループを終了します。

ループの条件は前と同様(`while square < finalSquare`)ですが、今回はループを一度実行するまでは評価されません。今回の例では、`repeat-while` の構造は `while` 適しています。`repeat-while` では、`square += board[square]` は常に `while` 条件で `square` がまだボード上にいることを確かめた後で、すぐに実行されます。この挙動で、`while` の例では必要だった配列の範囲チェックは必要なくなります。

## Conditional Statements

特定の条件に基づいて、コードの異なるパートを実行するのはしばしば役に立ちます。エラーが起きた時に追加でコードを実行したいこともあるでしょう。値が大き過ぎたり、小さ過ぎたりしたらメッセージを表示したいかもしれません。こうすことで、コードを条件付き(*conditional*)にします。

Swift は、コードに条件分岐を追加する 2 つの方法、`if` 文と `switch` 文、を提供しています。一般的に、`if` 文は、ごく少数の可能性があるシンプルな条件を評価します。`switch` 文は、様々な可能性の組み合わせを持つ、より複雑な条件に適し、パターンマッチが適切な実行コードを選択する手助けになる状況で役に立ちます。

### If

最も単純な形式として、`if` 文は 1 つの `if` 条件です。条件が `true` の場合にのみ、内部の文が実行されます。

```swift
var temperatureInFahrenheit = 30
if temperatureInFahrenheit <= 32 {
    print("It's very cold. Consider wearing a scarf.")
}
// Prints "It's very cold. Consider wearing a scarf."
```

上記の例は、気温が華氏 32 度以下かどうかをチェックしています。もしそうならば、メッセージが出力されます。そうでないならば、メッセージは出力されません。コード実行は `if` 文の右中括弧(`}`)の後のコードが継続して実行します。

`if` 文は、条件が `false` の場合に `if` 文の代わりに実行する、`else` 句と呼ばれる文も提供できます。この文は `else` キーワードを使って書かれます。

```swift
temperatureInFahrenheit = 40
if temperatureInFahrenheit <= 32 {
    print("It's very cold. Consider wearing a scarf.")
} else {
    print("It's not that cold. Wear a t-shirt.")
}
// Prints "It's not that cold. Wear a t-shirt."
```

この 2 つの分岐の内の 1 つが必ず実行されます。気温は華氏 40 度に増加しているので、スカーフを巻くアドバイスをする程寒くはなく、`else` の分岐が実行されます。

さらに句を追加して、複数の `if` 文をつなげることもできます。

```swift
temperatureInFahrenheit = 90
if temperatureInFahrenheit <= 32 {
    print("It's very cold. Consider wearing a scarf.")
} else if temperatureInFahrenheit >= 86 {
    print("It's really warm. Don't forget to wear sunscreen.")
} else {
    print("It's not that cold. Wear a t-shirt.")
}
// Prints "It's really warm. Don't forget to wear sunscreen."
```

ここで、特定の暖かい温度に応答するために、追加の `if` 文が提供されています。最後の `else` 句で、暑過ぎず寒過ぎない温度全ての応答を出力します。

しかし、最後の `else` 句は必須ではなく、条件が絶対に必要でない場合は、除くこともできます。

```swift
temperatureInFahrenheit = 72
if temperatureInFahrenheit <= 32 {
    print("It's very cold. Consider wearing a scarf.")
} else if temperatureInFahrenheit >= 86 {
    print("It's really warm. Don't forget to wear sunscreen.")
}
```

この温度は、暑過ぎず寒過ぎない温度なので、`if` も `else if` も実行されず、何も出力しません。

### Switch

`switch` 文は複数の可能性に対してパターンマッチを使って比較を行い、値を検討します。そして、一致した最初のパターンのコードのブロックを実行します。`switch` 文は、複数の可能性がある状態に対して `if` 文の代わりに使うことができます。

最もシンプルな形式として、`switch` 文は同じ型の 1 つ以上の値を比較します。

![switch](./../.gitbook/assets/05_switchform.png)

全ての `switch` 文は、`case` キーワードで始まる複数の可能性があるケースで構成されます。特定の値の比較に加えて、さらに複雑なパターンマッチをケースに指定する様々な方法を提供しています。それらについては、この章で後ほど記載しています。

`if` 文の `body` と同じく、各 `case` はコードを実行する分岐です。`switch` 文はどの分岐を選択するべきかどうかを決めます。このプロセスは、検討中の値の切り替え(*switching*)と呼ばれます。

全ての `switch` 文は網羅的(*exhaustive*)です。つまり、全ての検討中の型の値は、`switch` ケースのどれか 1 つにマッチしなければなりません。特定のケースに当てはまらない可能性がある値を扱う場合、明示的にパターンに合致しないあらゆる値をカバーするデフォルトを定義することができます。このデフォルトケースは、`default` キーワードで示され、必ず最後に書かなければなりません。

下記の例では、`someCharacter` という 1 つの小文字検討する `switch` 文です。

```swift
let someCharacter: Character = "z"
switch someCharacter {
case "a":
    print("The first letter of the alphabet")
case "z":
    print("The last letter of the alphabet")
default:
    print("Some other character")
}
// Prints "The last letter of the alphabet"
```

`switch` 文の最初のケースは、英語アルファベットの最初の文字 `a` に合致し、2 番目のケースは最後の文字 `z`  `a` に合致します。全ての可能性がある文字をカバーしなければならないため、`a` と `z` 以外の全ての文字に対して `default` ケースを使っています。こうすることで全てのケースを網羅できています。

#### No Implicit Fallthrough

---

C 言語や Objective-C と異なり、Swift の `switch` 文はデフォルトでそれぞれのケースを上から下に通り抜けません。代わりに、最初に合致した `switch` ケースの実行が完了すると、明示的に `break` しなくても、全体の `switch` 文も完了します。こうすることで、C 言語の `switch` 文よりも、より安全に簡単に使えるようにしています。間違って 1 つ以上のケースを事項してしまうリスクを防ぎます。

> NOTE  
> `break` は必須ではありませんが、合致したけど何も実行しないような場合や、あるケースの中で最後まで実行される前にそのブロックを抜けたい場合に、`break` を使うことができます。詳細は、[Break in a Switch Statement](#break-in-a-switch-statement)を参照ください。

各ケースの `body` は少なくとも 1 つの文を実行しなければなりません。次のコードは最初のケースの `body` が空なので不正です。

```swift
let anotherCharacter: Character = "a"
switch anotherCharacter {
case "a": // 空の body は不正
case "A":
    print("The letter A")
default:
    print("Not the letter A")
}
// コンパイルエラーが発生します
```

C 言語の `switch` 文と異なり、`"a"` と `"A"` の両方に合致することはありません。`case "a":` は実行文が何も含まれていないため、コンパイルエラーが発生します。こうすることで、不意に 1 つのケースから他のケースに進むことを防ぎ、意図がより明確な安全なコードにすることができます。

`"a"` と `"A"` の両方に合致させたい場合は、カンマ区切り(`,`)で値を区切って、2 つの値を複合ケースに組み合わせます。

```swift
let anotherCharacter: Character = "a"
switch anotherCharacter {
case "a", "A":
    print("The letter A")
default:
    print("Not the letter A")
}
// Prints "The letter A"
```

可読性のために、複合ケースに複数行に分けて書くこともできます。複合ケースについての詳細は[Compound Cases](#compound-cases)を参照ください。

> NOTE  
> 特定のケースから次のケースの `body` を実行したい場合は、`fallthrough` キーワードを使います。[Fallthrough](#fallthrough)に記載しています。

#### Interval Matching

---

`switch` ケースの値には、範囲内に含まれるかどうかのチェックもできます。下記の例では、数値間隔を使用して、任意のサイズの数値の自然言語カウントを提供します。

```swift
let approximateCount = 62
let countedThings = "moons orbiting Saturn"
let naturalCount: String
switch approximateCount {
case 0:
    naturalCount = "no"
case 1..<5:
    naturalCount = "a few"
case 5..<12:
    naturalCount = "several"
case 12..<100:
    naturalCount = "dozens of"
case 100..<1000:
    naturalCount = "hundreds of"
default:
    naturalCount = "many"
}
print("There are \(naturalCount) \(countedThings).")
// Prints "There are dozens of moons orbiting Saturn."
```

上記の例では、`approximateCount` が `switch` 文で評価されています。それぞれのケースでは、1 つの数値または範囲で比較しています。`approximateCount` の値は、12 から 100 の間にあるので、`naturalCount` は `"dozens of"` に値が代入されます。実行後は `switch` 文から抜け出します。

#### Tuples

---

タプルを使って同じ `switch` 文の中で、複数の値を検証することができます。タプルの個々の要素は、異なる値や範囲に対して検証できます。逆に、アンダースコア(`_`)を使うと、ワイルドカードとしてどんな値にも合致されることができます。

下記の例では、`(Int, Int`)型のタプルとして `(x, y)` 座標を受け取り、グラフに分布しています。

```swift
let somePoint = (1, 1)
switch somePoint {
case (0, 0):
    print("\(somePoint) is at the origin")
case (_, 0):
    print("\(somePoint) is on the x-axis")
case (0, _):
    print("\(somePoint) is on the y-axis")
case (-2...2, -2...2):
    print("\(somePoint) is inside the box")
default:
    print("\(somePoint) is outside of the box")
}
// Prints "(1, 1) is inside the box"
```

![switch例のグラフ](./../.gitbook/assets/coordinategraphsimple_2x.png)

`switch` 文は、座標が原点 `(0、0)` にあるか、赤い x 軸にあるか、オレンジ色の y 軸にあるか、原点を中心とする青い 4x4 列のボックスの内側にあるか、ボックスの外側にあるかを判別します。

C 言語と異なり、Swift では複数のケースで同じ値を検証することができます。実際、この例では `(0、0)` は全ての 4 つのケースに当てはまります。複数に合致する場合は、常に最初に合致したケースが使われます。`(0、0)` は最初に `case (0, 0)` に合致するので、他のケースは無視します。

#### Value Bindings

---

`switch` ケースは、合致した値を、ケースの `body` で使える変数や定数に代入することができます。この挙動は、値を変数や定数にバインドするので、値バインディング(*value binding*)と呼ばれています。

下記の例では、`(Int, Int`)型のタプルとして `(x, y)` 座標を受け取り、グラフに分布しています。

```swift
let anotherPoint = (2, 0)
switch anotherPoint {
case (let x, 0):
    print("on the x-axis with an x value of \(x)")
case (0, let y):
    print("on the y-axis with a y value of \(y)")
case let (x, y):
    print("somewhere else at (\(x), \(y))")
}
// Prints "on the x-axis with an x value of 2"
```

![switchタプルの例のグラフ](./../.gitbook/assets/coordinategraphmedium_2x.png)

`switch` 文は、座標がが赤い x 軸上にあるか、オレンジ色の y 軸上にあるか、または他の場所（どちらの軸上にもない）にあるかを判別します。

この 3 つの `switch` ケースは、`anotherPoint` から 1 つまたは両方のタプルの値を、プレースホルダー定数 `x` と `y` として宣言しています。最初のケース `case (let x, 0)` は、`y` は 0 で、座標 `x` の値を定数 `x` に代入した任意の値に合致します。同様に、2 番目のケース `case (0, let y)` は、`x` は 0 で、座標 `y` の値を定数 `y` に代入した任意の値に合致します。

定数は、そのケースのブロック内で使用することができます。ここでは、座標の分類を出力するために使われています。

`switch` 文は、`default` のケースが必ず必要な訳ではありません。最後のケース `case let (x, y)` は、どんな値にも合致する 2 つのプレースホルダとして定数を宣言しています。`anotherPoint` は、常に 2 つの値を持つタプルなので、このケースで、先の 2 つのケース以外の全ての値に合致するため、`switch` 文で全ての値を網羅するための `default` ケースは不要です。

#### Where

---

`switch` 文は、追加条件として `where` 句を使うことができます。

下記の例では、`(x, y)` 座標を受け取り、グラフに分布しています。

```swift
let yetAnotherPoint = (1, -1)
switch yetAnotherPoint {
case let (x, y) where x == y:
    print("(\(x), \(y)) is on the line x == y")
case let (x, y) where x == -y:
    print("(\(x), \(y)) is on the line x == -y")
case let (x, y):
    print("(\(x), \(y)) is just some arbitrary point")
}
// Prints "(1, -1) is on the line x == -y"
```

![switch whereの例のグラフ](./../.gitbook/assets/coordinategraphcomplex_2x.png)

`switch` 文は、座標が `x == y` の場合は緑色の対角線上にあるか、`x == -y` の場合は紫色の対角線上にあるか、またはどちらでもないかを判別します。

この 3 つの `switch` ケースは、`yetAnotherPoint` から 1 つまたは両方のタプルの値を、プレースホルダー定数 `x` と `y` として宣言しています。これらの定数は `where` 句の一部で使われており、動的なフィルターを作成しています。`switch` ケースには、`point` の現在値が `where` の条件で `true` になった場合のみ合致します。

最後のケースは、先の 2 つのケース以外の全ての値に合致するため、`switch` 文で全ての値を網羅するための `default` ケースは不要です。

#### Compound Cases

---

各ケースをカンマ(`,`)で区切って `case` の後に書くことで、同じ `body` を共有する複数のケースを書くことができます。その中の 1 つのケースに合致した場合に、合致したと見なされます。ケースのリストが長くなるような場合は、複数行にまたがって書くことができます。例えば:

```swift
let someCharacter: Character = "e"
switch someCharacter {
case "a", "e", "i", "o", "u":
    print("\(someCharacter) is a vowel")
case "b", "c", "d", "f", "g", "h", "j", "k", "l", "m",
     "n", "p", "q", "r", "s", "t", "v", "w", "x", "y", "z":
    print("\(someCharacter) is a consonant")
default:
    print("\(someCharacter) isn't a vowel or a consonant")
}
// Prints "e is a vowel"
```

最初のケースは英語の小文字の母音全てに合致します。同様に、2 番目のケースは英語の子音全てに合致します。`default` は他の全ての文字に合致します。

複合ケースは、値バインディング(*value binding*)を含めることもできます。複合ケース内のすべてのパターンには、同じ値バインディングのセットが含まれている必要があり、各バインディングは、すべてのパターンから同じ型の値を取得する必要があります。こうすることで、複合ケース内のどのパターンでも、`body` 内でバインディングされた値にアクセスすることができ、型も常に同じことが保証できます。

```swift
let stillAnotherPoint = (9, 0)
switch stillAnotherPoint {
case (let distance, 0), (0, let distance):
    print("On an axis, \(distance) from the origin")
default:
    print("Not on an axis")
}
// Prints "On an axis, 9 from the origin"
```

上記の `case` では 2 つのパターンを含んでいます: `(let distance, 0)` は x 軸上にある点に合致し、 `(0, let distance)` は y 軸上にある点に合致します。どちらのパターンでも、`distance` へのバインディングが含まれており、どちらも数値です。つまり、`case` 内では常に `distance` にアクセスできます。

## Control Transfer Statements

制御転送文(*Control Transfer Statements*)は、コードの一部の制御を他の部分へ移動させることで、コードの実行順序を変更します。Swift は 5 つの制御転送文があります:

* `continue`
* `break`
* `fallthrough`
* `return`
* `throw`

`continue`、`break` と `fallthrough` は下記に記載しています。`return` は[Functions](./functions.md)、`throw` は[Propagating Errors Using Throwing Functions](./error-handling.md#propagating-errors-using-throwing-functions)に記載しています。

### Continue

### Break

#### Break in a Loop Statement

#### Break in a Switch Statement

### Fallthrough

### Labeled Statements

## Early Exit

## Checking API Availability
