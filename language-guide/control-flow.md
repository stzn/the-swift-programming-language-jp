# Control Flow(制御フロー)

最終更新日: 2021/5/22

Swift は、様々な制御フロー(*Control Flow*)を提供しています。1 つのタスクを複数回行う `while` ループ、ある条件を基に異なる分岐のコードを実行する `if`,`guard`,`switch`、コードの他の地点へ実行フローを移動させる `break` や `continue` のような文があります。

また、配列や辞書、範囲、文字列や他のシーケンスに対して、簡単にループ処理を行う `for-in` ループも提供しています。

Swift の `switch` 文は、C のような他の言語と比べてもかなり強力です。case は、範囲やタプル、特定の型へのキャストなど、様々なパターンに合致させることができます。また、`switch` の case に合致した値は、case の本文内で使用するために変数や定数に紐づけることができます。各 case に `where` を使用してより複雑な条件を作成することもできます。

## For-In Loops(For-Inループ）

配列のアイテムや数値の範囲、文字列の文字などのシーケンスに対してループ処理を行うために `for-in` ループを使います。

下記の例は、`for-in` ループを使用して配列のアイテムにループ処理を行なっています。

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

辞書のキーバリューペアにアクセスするためにもループ処理を使用することができます。辞書を繰り返す際に、辞書内の個々のアイテムは `(key, value)` のタプルとして返され、`for-in` ループ内で使用するために、`(key, value)` のタプルの個々のメンバーは明示的に命名した定数に展開できます。下記のコードは、辞書のキーの配列を `animalName` 定数に、辞書のバリューは `legCount` 定数に展開されています。

```swift
let numberOfLegs = ["spider": 8, "ant": 6, "cat": 4]
for (animalName, legCount) in numberOfLegs {
    print("\(animalName)s have \(legCount) legs")
}
// cats have 4 legs
// ants have 6 legs
// spiders have 8 legs
```

`Dictionary` の定数は元々順序がなく、ループ処理をする時に、取得されるキーバリューの順序も保証されていません。配列や辞書に関しては、[Collection Types](./collection-types.md)を参照ください。

数値の範囲にも `for-in` ループを使用することができます。下記の例では、5 の倍数の最初のいくつかを出力しています。

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

繰り返されているシーケンスは、Closed 範囲演算子（`...`)を使用して最初と最後も含んだ `1` から `5` の数字の範囲です。`index` の値は、まず範囲の最初の値（`1`)が設定され、ループ内の文が実行されます。この例では、ループは 1 つの文（index とそれに対応する 5 の倍数の値を出力する文）のみを含んでいます。文が完了すると、`index` は範囲の次の値（`2`)に更新され、`print(_:separator:terminator:)` が再び呼ばれます。このプロセスは範囲の終わり（`5`)に達するまで続きます。

上記の例では、`index` は各ループの最初に自動で設定される値です。`index` を使用する前に宣言する必要はありません。`let` をつける必要もなく、ループに含めることで自動で宣言されます。

各ループの値が必要ない場合、変数名の位置にアンダースコア（`_`)を付けることで無視できます。

```swift
let base = 3
let power = 10
var answer = 1
for _ in 1...power {
    answer *= base
}
print("\(base) to the power of \(power) is \(answer)")
// "3 to the power of 10 is 59049"
```

上記の例では、1 つの数字に累乗した結果を出力しています。この例では `1` に `3`(つまり `3` の `0` 乗）を掛けることから始まり、`1` から `10` までの Closed 範囲を使用して、10 回計算をループしています。この計算では、ループ内で範囲から取得できる値は必要ありません。シンプルに範囲の数だけ処理をループしています。ループの変数部に使われているアンダースコア（`_`)は、それぞれの値を無視し、ループ内に値を提供しません。

最初と最後の両方を含める Closed 範囲を使用する必要がない場合もあるかもしれません。時計の分針を思い浮かべてください。0 から始まる 60 個の目盛が必要です。最初の値を含める Half-open 範囲演算子（`..<`)を使いましょう。

範囲についての詳細は、[Range Operators](./basic-operators.md#range-operators範囲演算子)を参照ください。

```swift
let minutes = 60
for tickMark in 0..<minutes {
    // 毎分ごとに 60 回目盛を描きます
}
```

表示上ではもっと目盛を少なくしたいかもしれません。5 分ごとに 1 目盛にしてみましょう。`stride(from:to:by:)` 関数を使用すると不要な目盛をスキップすることができます。

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

## While Loops(Whileループ）

`while` ループは条件が `false` になるまで内部の文を実行します。最初のループが始まるまで、何回繰り返すのかがわからないような時に最も使われます。

* `while` は、ループ内の処理を実行する前に条件を評価します
* `repeat-while` は、ループ内の処理を実行した後に条件を評価します

### while

`while` ループは、条件を評価してからループを開始します。`true` ならば条件が `false` になるまでループ内の処理を繰り返します。

`while` ループの基本的な書き方は下記の通りです:

![While書き方](../.gitbook/assets/05_whileform.png)

下記の例は、「蛇とはしご（*Snakes and ladders*)」と呼ばれるシンプルなゲームの例です。

![蛇とはしご](./../.gitbook/assets/snakesandladders_2x.png)

ゲームのルールは下記のようです:

* ボードには 25 の枠があり、目的は枠 25 以上に到達することです
* プレイヤーはボードの左下を「枠 0」として、そこから始めます
* それぞれの番で、サイコロを振り、出た目分、画像のドットに従って水平に移動します
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

枠 3 は枠 11 に移動します。これを表すには `board[03]` は `+08` で、(`11` と `3` の差分の）数字の `8` に相応します。値と文を見てみると、単項加算演算子（`+i`)、単項減算演算子（`-i`)が明示的に使われ、`10` 以下の値は `0` が前に付いています。(厳密に `0` は必要ありませんが、より読みやすくできます）

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

この例では、サイコロを振るアクションにとてもシンプルな方法を使用しています。ランダムな数字を生成するのではなく、`0` で初期化された `diceRoll` から開始します。`while` ループが繰り返される度に、`diceRoll` は 1 つずつ加算され、値が大き過ぎないかをチェックしています。`7` になると、大き過ぎるため `1` にリセットされます。 つまり、`diceRoll` は `1, 2, 3, 4, 5, 6, 1, 2` のように繰り返されます。

サイコロを振った後、 `diceRoll` 分移動します。`25` を超すこともありますが、その場合はゲーム終了です。これを実現するために、コードでは `square` が `board` 配列の `count` よりも小さいことをチェックしています。`square` が妥当ならば、`board[square]` を `square` に加算され、蛇やはしごの場合はプレイヤーはさらに移動します。

> NOTE  
> このチェックがないと、`board[square]` は `board` の範囲を超えてアクセスする可能性があり、実行時エラーが発生します。

現在の `while` ループが終了し、次のループを実行するかどうかを決めるために、ループの条件をチェックします。プレイヤーは枠 25 またはそれ以上に到達していた場合、ループ条件は `false` になり、ゲームは終了します。

今回のケースでは、ループの開始時にどのくらいゲームが続くのかがわからないため、`while` ループが適切です。条件が満たされるまでループ処理が実行されます。

### Repeat-While

`while` ループの別の形として、`repeat-while` があり、ループ条件を検証する前に、一度ループ内の処理を実行します。その後、条件が `false` になるまで繰り返します。

> NOTE  
> Swiftの `repeat-while` は、他の言語でいうところの `do-while` に当たります

下記が `repeat-while` ループの一般的な形式です。

![repeat-while](./../.gitbook/assets/05_repeatwhile.png)

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
print("Game over!")
```

蛇かはしごかのチェック後に、サイコロが振られ、プレイヤーは `diceRoll` 分枠を移動します。そして現在のループを終了します。

ループの条件は前と同様（`while square < finalSquare`)ですが、今回はループを一度実行するまでは評価されません。今回の例では、`repeat-while` の構造は `while` 適しています。`repeat-while` では、`square += board[square]` は常に `while` 条件で `square` がまだボード上にいることを確かめた後にすぐ実行されます。この挙動で、`while` の例では必要だった配列の範囲チェックは必要なくなります。

## Conditional Statements(条件文）

特定の条件に基づいてコードの異なるパートを実行するのは役に立つことが多くあります。エラーが起きた時に追加でコードを実行したいこともあるでしょう。値が大き過ぎたり、小さ過ぎたりしたらメッセージを表示したいかもしれません。この場合、コードを条件付き（*conditional*)することで実現できます。

Swift は、コードに条件分岐を追加する 2 つの方法、`if` 文と `switch` 文、を提供しています。一般的に、`if` 文は、ごく少数の可能性があるシンプルな条件を評価する時に使います。`switch` 文は、様々な可能性の組み合わせを持つ、より複雑な条件に適し、パターンマッチが適切な実行コードを選択する手助けになる状況で役に立ちます。

### If

最も単純な形式として、`if` 文は 1 つの `if` 条件です。条件が `true` の場合にのみ、内部の文が実行されます。

```swift
var temperatureInFahrenheit = 30
if temperatureInFahrenheit <= 32 {
    print("It's very cold. Consider wearing a scarf.")
}
// "It's very cold. Consider wearing a scarf."
```

上記の例は、気温が華氏 32 度以下かどうかをチェックしています。もしそうならばメッセージが出力されます。そうでないならばメッセージは出力されません。コードの実行は `if` 文の右中括弧（`}`)の後のコードが継続して実行します。

`if` 文は、条件が `false` の場合に `if` 文の代わりに実行する、`else` 句と呼ばれる文も提供できます。この文は `else` キーワードを使用して書かれます。

```swift
temperatureInFahrenheit = 40
if temperatureInFahrenheit <= 32 {
    print("It's very cold. Consider wearing a scarf.")
} else {
    print("It's not that cold. Wear a t-shirt.")
}
// "It's not that cold. Wear a t-shirt."
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
// "It's really warm. Don't forget to wear sunscreen."
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

`switch` 文は、複数の可能性に対してパターンマッチを使用して比較を行い、値を検討します。そして、一致した最初のパターンのコードのブロックを実行します。`switch` 文は、複数の可能性がある状態に対して `if` 文の代わりに使用することができます。

最もシンプルな形式として、`switch` 文は同じ型の 1 つ以上の値を比較します。

![switch](./../.gitbook/assets/05_switchform.png)

全ての `switch` 文は、`case` キーワードで始まる複数の可能性がある case で構成されます。特定の値の比較に加えて、さらに複雑なパターンマッチを case に指定するための様々な方法を提供しています。この章で後ほど記載しています。

`if` 文の本文と同じく、各 case はコードを実行する分岐です。`switch` 文はどの分岐を選択するべきかどうかを決めます。このプロセスは、検証される値に基づいた切り替え（*switching*)と呼ばれます。

全ての `switch` 文は網羅的（*exhaustive*)です。つまり、全ての検証される型の値は、`switch` case のどれか 1 つにマッチしなければなりません。特定の case に当てはまらない可能性がある値を扱う場合、明示的にパターンに合致しないあらゆる値をカバーするデフォルトを定義することができます。このデフォルト case は、`default` キーワードで示され、必ず最後に書かなければなりません。

下記の例では、`someCharacter` という 1 つの小文字を検証する `switch` 文です。

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
// "The last letter of the alphabet"
```

`switch` 文の最初の case は、英語アルファベットの最初の文字 `a` に合致し、2 番目の case は最後の文字 `z` に合致します。全ての可能性がある文字をカバーしなければならないため、`a` と `z` 以外の全ての文字に対して `default` case を使用しています。こうすることで全ての case を網羅できています。

#### No Implicit Fallthrough(暗黙的にfallthroughしない）

---

C 言語や Objective-C と異なり、Swift の `switch` 文は、デフォルトでは、それぞれの case 下から次の case に行くことはありません。その代わりに、最初に合致した `switch` case の実行が完了すると、明示的に `break` しなくても、全体の `switch` 文も完了します。こうすることで、C 言語の `switch` 文よりも、より安全に簡単に使えるようにしています。間違って 1 つ以上の case を事項してしまうリスクを防ぎます。

> NOTE  
> `break` は必須ではありませんが、合致したけど何も実行しないような場合や、あるcaseの中で最後まで実行される前にそのブロックを抜けたい場合に、`break` を使用することができます。詳細は、[Break in a Switch Statement](#break-in-a-switch-statementswitch内でbreak)を参照ください。

各 case の本文は少なくとも 1 つの文を実行しなければなりません。次のコードは最初の case の本文が空なので不正です。

```swift
let anotherCharacter: Character = "a"
switch anotherCharacter {
case "a": // 空の 本文 は不正
case "A":
    print("The letter A")
default:
    print("Not the letter A")
}
// コンパイルエラーが発生します
```

C 言語の `switch` 文と異なり、`"a"` と `"A"` の両方に合致することはありません。`case "a":` は実行文が何も含まれていないため、コンパイルエラーが発生します。こうすることで、不意に 1 つの case から他の case に進むことを防ぎ、意図がより明確な安全なコードにすることができます。

`"a"` と `"A"` の両方に合致させたい場合は、カンマ区切り（`,`)で値を区切って、2 つの値を複合 case に組み合わせます。

```swift
let anotherCharacter: Character = "a"
switch anotherCharacter {
case "a", "A":
    print("The letter A")
default:
    print("Not the letter A")
}
// "The letter A"
```

可読性のために、複合 case を複数行に分けて書くこともできます。複合 case についての詳細は[Compound Cases](#compound-cases)を参照ください。

> NOTE  
> 特定のcaseから次のcaseの 本文 を実行したい場合は、`fallthrough` キーワードを使います。[Fallthrough](#fallthrough)に記載しています。

#### Interval Matching(範囲マッチング）

---

`switch` case の値には、範囲内に含まれるかどうかのチェックもできます。下記の例では、数値の範囲を使用して、任意のサイズの数値の自然言語カウントを提供します。

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
// "There are dozens of moons orbiting Saturn."
```

上記の例では、`approximateCount` が `switch` 文で評価されています。それぞれの case では、1 つの数値または範囲で比較しています。`approximateCount` の値は、12 から 100 の間にあるので、`naturalCount` は `"dozens of"` に値が代入されます。実行後は `switch` 文から抜け出します。

#### Tuples(タプル）

---

タプルを使用して同じ `switch` 文の中で、複数の値を検証することができます。タプルの個々の要素は、異なる値や範囲に対して検証できます。逆に、アンダースコア（`_`)を使用すると、ワイルドカードとしてどんな値にも合致させることができます。

下記の例では、`(Int, Int)` 型のタプルとして `(x, y)` 座標を受け取り、グラフに分布しています。

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
// "(1, 1) is inside the box"
```

![switch例のグラフ](./../.gitbook/assets/coordinategraphsimple_2x.png)

`switch` 文は、座標が原点 `(0、0)` にあるか、赤い x 軸上にあるか、オレンジ色の y 軸上にあるか、原点を中心とする青い 4x4 列のボックスの内側にあるか、ボックスの外側にあるかを判別します。

C 言語と異なり、Swift では複数の case で同じ値を検証することができます。実際、この例では `(0、0)` は全ての 4 つの case に当てはまります。複数に合致する場合は、常に最初に合致した case が使われます。`(0、0)` は最初に `case (0, 0)` に合致するので、他の case は無視します。

#### Value Bindings(値バインディング）

---

`switch` case は、合致した値を、case の本文で使用するために、一時的な定数または変数に、一致する 1 つまたは複数の値に名前を付けることができます。この挙動は、値を変数や定数にバインドするので、値バインディング（*value binding*)と呼ばれています。

下記の例では、`(Int, Int)` 型のタプルとして `(x, y)` 座標を受け取り、グラフに分布しています。

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
// "on the x-axis with an x value of 2"
```

![switchタプルの例のグラフ](./../.gitbook/assets/coordinategraphmedium_2x.png)

`switch` 文は、座標が赤い x 軸上にあるか、オレンジ色の y 軸上にあるか、または他の場所にあるか（どちらの軸上にもないか）を判別します。

この 3 つの `switch` case は、`anotherPoint` から 1 つまたは両方のタプルの値を、プレースホルダ定数 `x` と `y` として宣言しています。最初の `case (let x, 0)` は、`y` は 0 で、座標 `x` の値を定数 `x` に代入した任意の値に合致します。同様に、2 番目の `case (0, let y)` は、`x` は 0 で、座標 `y` の値を定数 `y` に代入した任意の値に合致します。

定数は、その case のブロック内で使用することができます。ここでは、座標の分類を出力するために使われています。

`switch` 文は、`default` の case が必ず必要な訳ではありません。最後の `case let (x, y)` は、どんな値にも合致する 2 つのプレースホルダとして定数を宣言しています。`anotherPoint` は、常に 2 つの値を持つタプルなので、この case で、先の 2 つの case 以外の全ての値に合致するため、`switch` 文で全ての値を網羅するための `default` case は不要です。

#### Where

---

`switch` 文は、追加条件として `where` 句を使用することができます。

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
// "(1, -1) is on the line x == -y"
```

![switch whereの例のグラフ](./../.gitbook/assets/coordinategraphcomplex_2x.png)

`switch` 文は、座標が `x == y` の場合は緑色の対角線上にあるか、`x == -y` の場合は紫色の対角線上にあるか、またはどちらでもないかを判別します。

この 3 つの `switch` case は、`yetAnotherPoint` から 1 つまたは両方のタプルの値を、プレースホルダ定数 `x` と `y` として宣言しています。これらの定数は `where` 句の一部で使われており、動的なフィルターを作成しています。`switch` case には、`point` の現在値が `where` の条件で `true` になった場合のみ合致します。

最後の case は、先の 2 つの case 以外の全ての値に合致するため、`switch` 文で全ての値を網羅するための `default` case は不要です。

#### Compound Cases(複合ケース）

---

各 case をカンマ（`,`)で区切って case の後に書くことで、同じ本文を共有する複数の case を書くことができます。その中の 1 つの case に合致した場合に、合致したと見なされます。case のリストが長くなるような場合は、複数行にまたがって書くことができます。例えば:

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
// "e is a vowel"
```

最初の case は英語の小文字の母音全てに合致します。同様に、2 番目の case は英語の子音全てに合致します。`default` は他の全ての文字に合致します。

複合 case は、値バインディング（*value binding*)を含めることもできます。複合 case 内の全てのパターンには、同じ値バインディングのセットが含まれている必要があり、各バインディングは、全てのパターンから同じ型の値を取得する必要があります。こうすることで、複合 case 内のどのパターンでも、本文内でバインディングされた値にアクセスすることができ、型も常に同じことが保証できます。

```swift
let stillAnotherPoint = (9, 0)
switch stillAnotherPoint {
case (let distance, 0), (0, let distance):
    print("On an axis, \(distance) from the origin")
default:
    print("Not on an axis")
}
// "On an axis, 9 from the origin"
```

上記の case では 2 つのパターンを含んでいます: `(let distance, 0)` は x 軸上にある点に合致し、 `(0, let distance)` は y 軸上にある点に合致します。どちらのパターンでも、`distance` へのバインディングが含まれており、どちらも数値です。つまり、case 内では常に `distance` にアクセスできます。

## Control Transfer Statements(制御転送文）

制御転送文（*Control Transfer Statements*)は、コードの一部の制御を他の部分へ転送させることで、コードの実行順序を変更します。Swift は 5 つの制御転送文があります:

* `continue`
* `break`
* `fallthrough`
* `return`
* `throw`

`continue`、`break` と `fallthrough` は下記に記載しています。`return` は[Functions](./functions.md)、`throw` は[Propagating Errors Using Throwing Functions](./error-handling.md#propagating-errors-using-throwing-functions)に記載しています。

### Continue

`continue` 文は、各ループ内の実行を止めて、次のループの最初から処理を開始します。つまり、ループを抜けることなく「今のループを完了します」と伝えます。

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
// "grtmndsthnklk"
```

上記のコードは、母音かスペースの場合に即座に現在のループを終了し、次のループの開始に移動するように `continue` キーワードを使用しています。

### Break

`break` 文は、即座に全体の制御フローの実行を終了させます。`break` は、`switch` 文かループ文の内部で、他の case よりも早期に処理の実行を終了させたい時に使います。

#### Break in a Loop Statement(ループ内でBreak)

---

ループ文の内部で使用する時、`break` はループの実行を即座に終了し、ループの末尾の閉じ括弧（`}`)の後へ制御フローを移します。ループ内の続きの処理は実行されず、次のループも開始されません。

#### Break in a Switch Statement(Switch内でBreak)

---

`switch` 文の内部で使用する時、`break` は `switch` の実行を即座に終了し、`switch` の末尾の閉じ括弧（`}`)の後へ制御フローを移します。`switch` 文は網羅的で空の case が許されないため、明示的に何もしないことを伝えるために、わざと合致させて無視する時に必要になります。無視したい case の本文に `break` のみを書きます。その case に合致した場合、case 内の `break` が `switch` 文全体の実行を即時に終了させます。

> NOTE  
> コメントだけを含めた `switch` caseは、コンパイルエラーになります。コメントは文ではなく、`switch` caseを無視することができません。`switch` caseを無視するには `break` を常に使います。

次の例では、`Character` 型の値が、4 つの言語内の 1 つの番号記号に合致するかどうかを判定しています。簡潔にするために、複数の値を 1 つの `switch` case で網羅しています。

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
    print("The integer value of \(numberSymbol) is \(integerValue).")
} else {
    print("An integer value couldn't be found for \(numberSymbol).")
}
// "The integer value of 三 is 3."
```

この例では、`numberSymbol` が `1`〜`4` のラテン語、アラビア語、中国語、タイ語に合致するかどうかをチェックしています。合致した場合、各 case は、`Int?` の `possibleIntegerValue` 変数に適切な数字を設定しています。

`switch` 文の実行完了後、この例では値が見つかったかどうかを判定するのに、オプショナルバインディング（*optional binding*)を使用しています。`possibleIntegerValue` 変数は、optional 型の性質上、`nil` が暗黙的に初期値になります。`switch` の最初の 4 つの case で値 `possibleIntegerValue` に値が設定された場合のみ、オプショナルバインディングは成功します。

上記の例で `Character` 型の値を全部羅列することは現実的ではないので、`default` case で合致しない全ての値を処理します。`default` case では何もする必要がないので、本文には `break` のみを記載しています。`default` case が合致すると、`break` 文は `switch` 文を終了させ、`if let` からコードを継続します。

### Fallthrough

Swift の `switch` 文は、各 case の底から次の case で抜ける（*fall through*する）ことはしません。つまり、最初に合致した case が完了したらすぐに `switch` 文全体の実行が終了します。逆に、C 言語の `switch` 文では、各 case へ通り抜けることを防ぐために、明示的に `break` を各 case の末尾に書かなければなりません。Swift の `switch` 文は、デフォルトで通り抜けないことで、C 言語とは反対に、より簡潔で挙動が予測しやすくなっています。間違って複数の case を実行することを防ぎます。

もし C 言語のような通り抜け（*fallthrough*)が必要な場合は、`fallthrough` キーワードを使用して場合ごとに設定することができます。下記の例では、`fallthrough` を使用して数値のテキストによる説明を作成します。

```swift
let integerToDescribe = 5
var description = "The number \(integerToDescribe) is"
switch integerToDescribe {
case 2, 3, 5, 7, 11, 13, 17, 19:
    description += " a prime number, and also"
    fallthrough
default:
    description += " an integer."
}
print(description)
// "The number 5 is a prime number, and also an integer."
```

この例では、`description` という新しい `String` 変数を宣言して、初期値を設定しています。この関数は `switch` 文で `integerToDescribe` を検証しています。`integerToDescribe` がリストの素数の 1 つの場合、この関数は `description` の末尾に、数字が素数なことを説明したテキストを追加します。`default` case にも「落ちる」ように、`fallthrough` キーワードを使用しています。`default` case では、説明の末尾に追加のテキストを追加して `switch` 文は完了します。数字の説明は、`print(_:separator:terminator:)` 関数を使用して出力されています。この例では、素数として `5` が適切に特定されています。

> NOTE  
> `fallthrough` キーワードは、前のcaseから落ちて実行されたcaseの条件をチェックしません。`fallthrough` キーワードは次の case (または `default` case)のブロック内の文に直接移動し、コードを実行します。これは C 言語と同じ挙動です。

### Labeled Statements(ラベル付き文）

Swift は、より複雑な制御フローを作成するために、ループや条件文の中にさらにループや条件文をネストできます。しかし、`break` を使用して早期に実行を終了させることもできます。そのため、どの文を `break` するのかを明示すると役に立つことがあります。同様に、複数のネストしたループで、どのループに `continue` が影響するかを明示することも役に立ちます。

これを実現するために、ループや条件文にラベルを付けることができます。条件文では、`break` を使用してラベル付き文（*Labeled Statements*)の実行を終了できます。ループでは、`break` か `continue` を使用してラベル付き文の実行を終了または継続することができます。

ラベル付き文は、文の導入キーワードの前にコロン（`:`)を付けて同じ行にラベルを配置することで示せます。下記は、`while` ループのシンタックの例ですが、他の全てのループや `switch` にも同じ原則が当てはまります。

![ラベル付き文](./../.gitbook/assets/05_lebeledstatements.png)

次の例は、この章の前で見てきた「蛇とはしご」にラベル付き `while` ループと `break` と `continue` 文を適用したバージョンです。今回は、追加のルールがあります。

* 勝つためには、ちょうど枠 25 に到達しなければなりません

サイコロの結果で枠 25 を超えた場合、ちょうど枠 25 に到達するまでサイコロを振らなければなりません。

ゲーム盤は、前と同じです。

![蛇とはしご](./../.gitbook/assets/snakesandladders_2x-2.png)

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
print("Game over!")
```

各ループの最初でサイコロを振ります。即座にプレイヤーが移動する前に、ループの移動結果を検証して、その移動ができるかどうかを判定しています:

* サイコロの結果でプレイヤーが最後の枠に移動したらゲームは終了します。`break gameLoop` は `while` ループの外側の最初の行へフローを移させ、ゲームは終わります
* `diceRoll` が最後の枠を超えた場合は、その移動は不正で、プレイヤーは再度サイコロを振る必要があります。`continue gameLoop` で現在の `while` のループを終了し、次のループを開始します
* 他の全ての case では、サイコロの結果は妥当です。プレイヤーは `diceRoll` 分移動し、蛇かはしごかのチェックをします。ループが終了後、フローは `while` の条件文に戻り、次のループが必要かどうかを判定します

> NOTE  
> もし `break` が `gameLoop` ラベルを付けなかった場合、`while` ではなく `switch` から抜けます。`gameLoop` ラベルを使用することで、どの制御文から抜けるのかを明確にできます  
>  
> 次のループに移動するのに `continue gameLoop` と `gameLoop` ラベルを使用する必要は厳密にはありません。ゲーム内にループは 1 つしかなく、どのループに `continue` が影響を与えるかが明確です。しかし、`gameLoop` ラベルを使用しても害はなく、そうすることで `break` のラベルと一貫性ができ、ロジックを読みやすく理解しやすくすることができます。

## Early Exit(早期リターン）

`guard` 文は `if` 文と同様に、式のブール値に応じて文を実行します。`guard` 文は、`guard` の後のコードを実行するために、`guard` の条件が `true` でなければならない場合に使います。`if` 文と異なり、`guard` 文は常に `else` 句が必要で、条件が `false` の場合に `else` 内が実行されます。

```swift
func greet(person: [String: String]) {
    guard let name = person["name"] else {
        return
    }

    print("Hello \(name)!")

    guard let location = person["location"] else {
        print("I hope the weather is nice near you.")
        return
    }

    print("I hope the weather is nice in \(location).")
}

greet(person: ["name": "John"])
// "Hello John!"
// "I hope the weather is nice near you."
greet(person: ["name": "Jane", "location": "Cupertino"])
// "Hello Jane!"
// "I hope the weather is nice in Cupertino."
```

`guard` 文の条件を満たした場合、`guard` の閉じ括弧（`}`)以降のコードを続けて実行します。条件の一部としてオプショナルバインディングに使われている変数や定数は、`guard` の後のコードで利用できるようになります。

条件を満たさない場合、`else` 句の内部を実行します。この分岐内では、`guard` 文内のコードブロックを抜けために、制御を他へ転送させなければなりません。これは、`return`、`break`、`continue` や `throw` といった制御転送文を使用することができます。あるいは、`fatalError(_:file:line:)` のように値を返さない関数やメソッドを呼びます。

`guard` 文を必須条件に使用することで、`if` 文で同じことを行うよりも可読性を改善させることができます。概ね `else` ブロックで包むことなくコードを書くことができたり、必須条件のすぐ下で条件を満たさない case を処理することができます。

## Checking API Availability(API使用可能性チェック）

Swift は API の使用可能性（*availability*)のチェックをビルトインでサポートしています。つまり、あるデプロイターゲットでは利用できない API を間違って使わないことが保証できます。

コンパイラはコード内で使われている全ての API がプロジェクトで特定したデプロイターゲットで使用可能かどうかを検証するために、SDK 内の使用可能かどうかの情報を使います。利用できない API を使おうとしていた場合、コンパイル時にエラーを報告します。

使用する API が実行時に使用可能かどうかに応じて、`if` 文や `guard` 文で使用可能条件（*availability condition*)を使用して、コードのブロックを条件付きで実行します。コンパイラはこのコードブロックが使用可能かどうかを検証する時に、この使用可能条件からの情報を使います。

```swift
if #available(iOS 10, macOS 10.12, *) {
    // iOS 10以上またはmacOS 10.12以上で実行
} else {
    // それ以前のiOSとmacOSで実行
}
```

上記の使用可能条件は、iOS では、`if` 文が `iOS10` 以降でのみ実行されることを指定しています。 `macOS` では、`macOS10.12` 以降のみになります。最後の引数 `*` は必須で、他のプラットフォームでは、`if` の本文がターゲットで指定された最小のデプロイメントターゲットで実行されることを示します。

基本的な形式として、使用可能条件はプラットフォームの名前とバージョンのリストを受け取ります。`iOS`、`macOS`、`watchOS`、 `tvOS` などをプラットフォームの名前として使います。全リストは[Declaration Attributes](./../language-reference/attributes.md)を参照ください。`iOS 8` や `macOS 10.10` のようなメジャーバージョンに加え、`iOS 11.2.6` や `macOS 10.13.3` のようにマイナーバージョンも指定できます。

![使用可能条件](./../.gitbook/assets/05_availabilitycondition.png)
