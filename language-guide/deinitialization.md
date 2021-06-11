# Deinitialization \(デイニシャライゼーション\)

最終更新日: 2021/5/30

クラスのインスタンスが解放される直前に、デイニシャライザ\(_deinitializer_\)が呼び出されます。`init` キーワードを使用してイニシャライザを作成する方法と同様に、`deinit` キーワードを使用してデイニシャライザを作成します。デイニシャライザは、クラス型でのみ使用できます。

## How Deinitialization Works\(デイニシャライゼーションの仕組み\)

Swift は、不要になったインスタンスのメモリへの割り当てを自動的に解除して、リソースを解放します。[Automatic Reference Counting](automatic-reference-counting.md)で説明されているように、自動参照カウント\(_automatic reference counting\(ARC\)_\) を通じてインスタンスのメモリを管理します。通常、インスタンスが解放されるときに手動でクリーンアップを実行する必要はありません。ただし、独自のリソースを取り扱っている場合は、自分で追加のクリーンアップを実行する必要な場合があります。例えば、ファイルを開いてデータを書き込むカスタムクラスを作成する場合、クラスのインスタンスを解放する前にファイルを閉じる必要があります。

クラス定義は、クラスごとに最大 1 つのデイニシャライザを持つことができます。デイニシャライザは引数を取らず、括弧なしで記述されます:

```swift
deinit {
    // デイニシャライザの実行
}
```

インスタンスの解放が行われる直前に、デイニシャライザは自動的に呼び出されます。デイニシャライザを直接呼び出すことは許可されていません。スーパークラスのデイニシャライザは、そのサブクラスに継承され、サブクラスのデイニシャライザの実装の最後に自動的に呼び出されます。サブクラスが独自のデイニシャライザを提供しなくても、スーパークラスのデイニシャライザは常に呼び出されます。

インスタンスは、デイニシャライザが呼び出されるまで解放されないため、デイニシャライザは、呼び出されたインスタンスの全てのプロパティにアクセスでき、それらのプロパティに基づいて挙動を変更できます\(閉じる必要があるファイルの名前を検索するなど\)。

## Deinitializers in Action\(動作中のデイニシャライザ\)

デイニシャライザの例を次に示します。この例では、シンプルなゲーム用に 2 つの新しいタイプ `Bank` と `Player` を定義しています。`Bank` クラスは、10,000 を超えるコインを流通させることのできない偽造通貨を管理します。ゲーム内に存在できる `Bank` は 1 つのみなので、`Bank` は現在の状態を保存および管理するための型プロパティと型メソッドを持つクラスとして実装されています:

```swift
class Bank {
    static var coinsInBank = 10_000
    static func distribute(coins numberOfCoinsRequested: Int) -> Int {
        let numberOfCoinsToVend = min(numberOfCoinsRequested, coinsInBank)
        coinsInBank -= numberOfCoinsToVend
        return numberOfCoinsToVend
    }
    static func receive(coins: Int) {
        coinsInBank += coins
    }
}
```

`Bank` は、`coinsInBank` プロパティを使用して、保有しているコインの現在の数を追跡します。また、コインの配布と受領を処理するために、`distribute(coins:)` と `receive(coins:)` という 2 つのメソッドも提供します。

`distribute(coins:)` メソッドは、配布する前に、銀行に十分なコインがあることを確認します。十分コインがない場合、`Bank` は要求された数よりも少ない数を返します\(銀行にコインが残っていない場合はゼロを返します\)。`Bank` は配布された実際のコイン数を返します。

`receive(coins:)` メソッドは、受け取った数のコインを `Bank` のコインストアに追加するだけです。

`Player` クラスは、ゲーム内のプレイヤーを表します。各プレイヤーは、常に一定数のコインを財布に保管しています。これは、プレイヤーの `coinsInPurse` プロパティによって表されます:

```swift
class Player {
    var coinsInPurse: Int
    init(coins: Int) {
        coinsInPurse = Bank.distribute(coins: coins)
    }
    func win(coins: Int) {
        coinsInPurse += Bank.distribute(coins: coins)
    }
    deinit {
        Bank.receive(coins: coinsInPurse)
    }
}
```

各 `Player` インスタンスは、銀行から指定数のコインを受け取って初期化されますが、銀行に指定された数のコインがない場合、`Player` インスタンスはその数よりも少ないコインを受け取る場合があります。

`Player` クラスは `win(coins:)` メソッドを定義し、銀行から一定数のコインを取得してプレイヤーの財布に追加します。`Player` クラスは、`Player` インスタンスの解放直前に呼び出されるデイニシャライザも実装します。ここで、デイニシャライザは単純にプレイヤーのすべてのコインを銀行に返します。

```swift
var playerOne: Player? = Player(coins: 100)
print("A new player has joined the game with \(playerOne!.coinsInPurse) coins")
// "A new player has joined the game with 100 coins"
print("There are now \(Bank.coinsInBank) coins left in the bank")
// "There are now 9900 coins left in the bank"
```

新しい `Player` インスタンスは、可能ならば 100 コインを受け取って作成されています。この `Player` インスタンスは、`playerOne` というオプショナルの `Player` 変数に保存されます。プレイヤーはいつでもゲームを終了できるため、ここではオプショナルの変数が使用されます。オプショナルを使用すると、現在ゲームにプレイヤーがいるかどうかを追跡できます。

`playerOne` はオプショナルのため、デフォルトのコイン数を出力するためにその `coinsInPurse` プロパティにアクセスするとき、および `win(coins:)` メソッドが呼び出されるときはいつでも、感嘆符 \(!\) を付けています:

```swift
playerOne!.win(coins: 2_000)
print("PlayerOne won 2000 coins & now has \(playerOne!.coinsInPurse) coins")
// "PlayerOne won 2000 coins & now has 2100 coins"
print("The bank now only has \(Bank.coinsInBank) coins left")
// "The bank now only has 7900 coins left"
```

ここで、プレイヤーは 2,000 コインを獲得しました。プレイヤーの財布には 2,100 枚のコインがあり、銀行には 7,900 枚のコインしか残っていません。

```swift
playerOne = nil
print("PlayerOne has left the game")
// "PlayerOne has left the game"
print("The bank now has \(Bank.coinsInBank) coins")
// "The bank now has 10000 coins"
```

プレイヤーはゲームから離れました。これは、オプショナルの `playerOne` 変数を `nil` に設定することで示しています。つまり「`Player` インスタンスがない」ことを意味します。この時点で、`playerOne` 変数の `Player` インスタンスへの参照が解放されています。他のプロパティや変数が `Player` インスタンスを参照していないため、メモリから解放されます。解放される直前に、デイニシャライザが自動的に呼び出され、コインが銀行に戻されています。

