# Closures

最終更新日: 2021/5/23

クロージャ(*Closures*)は、コード内で受け渡して使用できるある機能の独立したブロックです。 Swift のクロージャは、C 言語や Objective-C のブロック、他のプログラミング言語のラムダに似ています。

クロージャは、定数と変数への参照を、それらを定義したコンテキストからキャプチャ(*capture*)して保存できます。 これは、これらの定数と変数をスコープに閉じ込める(*closing*する)と呼ばれます。 Swift は、キャプチャに関連した全てのメモリ管理を行います。

> NOTE  
> キャプチャの概念に慣れていなくても心配しないでください。 これについては、以下の[Capturing Values](#capturing-values)で詳しく説明します。

[Functions](./functions.md)で紹介したグローバル関数やネスト関数は、実際にはクロージャの特殊なケースです。 クロージャは、次の 3 つの形式のいづれかを取ります:

* グローバル関数は、名前があり、値をキャプチャしないクロージャです
* ネスト関数は、名前があり、囲んでいる関数から値を取得できるクロージャです
* クロージャ式は、周囲のコンテキストから値をキャプチャできる軽量の構文で書かれた名前のないクロージャです

Swift のクロージャは、一般的に、簡潔で、混乱のないシンタックスで書くことを促進すうように最適化された、すっきりとした明確なスタイルを備えています。最適化には次のものが含まれます。

* コンテキストから引数と戻り値の型を推測できる
* 単一の式のクロージャは `return` キーワードなしで暗黙のリターンできる
* 引数名の短縮ができる
* 末尾のクロージャ構文を使うことができる

## Closure Expressions

[Nested Functions](./functions.md#nested-functions)で紹介したネスト関数は、より大きな関数の一部として独立したコードブロックに名前を付けて定義するための便利な手段です。ただし、完全な宣言や名前を書かずに、関数のような構造のより短いバージョンを作成できれば便利な場合もあります。これは、1 つ以上の引数を受け取る関数またはメソッドで特に当てはまります。

クロージャ式(*Closure Expressions*)は、簡潔で明瞭にインラインのクロージャを記述する方法です。クロージャ式は、明確さや意図を失うことなく、短縮された形式でクロージャを記述するためのいくつかの最適化を行うことができます。下記のクロージャ式は、複数回繰り返し使用される `sorted(by:)` メソッドを、同じ機能をより簡潔なものに改良した、最適化の例です。

### The Sorted Method

Swift の標準ライブラリは、`sorted(by:)` と呼ばれるメソッドを提供します。このメソッドは、指定した並べ替えクロージャの出力に基づいて、その型の値の配列を並べ替えます。並べ替えプロセスが完了すると、`sorted(by:)` メソッドは、古い配列と同じ型とサイズの新しい配列を返し、その要素は正しい並べ替え順序になります。元の配列は、`sorted(by:)` メソッドによって変更されません。

以下のクロージャ式の例では、`sorted(by:)` メソッドを使用して、`String` の配列をアルファベットの逆順で並べ替えています。並べ替える初期配列は次のとおりです:

```swift
let names = ["Chris", "Alex", "Ewa", "Barry", "Daniella"]
```

`sorted(by:)` メソッドは、配列の内容と同じ型の 2 つの引数を取るクロージャを受け入れ、値が並べ替えられた後、最初の値が 2 番目の値の前に表示されるか後に表示されるかを示す `Bool` 値を返します。ソートクロージャは、最初の値が 2 番目の値の前に表示される場合は `true` を返し、それ以外の場合は `false` を返す必要があります。

この例では、`String` の配列を並べ替えているため、並べ替えクロージャは型 `(String, String) -> Bool` の関数な必要があります。

ソートクロージャを提供する 1 つの方法は、正しい型の通常の関数を記述し、それを引数として `sorted(by:)` メソッドに渡すことです。

```swift
func backward(_ s1: String, _ s2: String) -> Bool {
    return s1 > s2
}
var reversedNames = names.sorted(by: backward)
// `reversedNames` は ["Ewa", "Daniella", "Chris", "Barry", "Alex"] と等しい
```

最初の文字列(`s1`)が 2 番目の文字列(`s2`)より大きい場合、`backward(_:_:)` 関数は `true` を返し、ソートされた配列で `s1` が `s2` の前に表示される必要があることを示します。文字列内の文字の場合、「より大きい」は「アルファベットの後半に現れる」を意味します。これは、文字 `"B"` が文字 `"A"` より「大きい」ことを意味し、文字列 `"Tom"` が文字列 `"Tim"` よりも大きいことを意味します。 これにより、アルファベットの逆ソートが行われ、`"Barry"` が `"Alex"` の前に配置されます。

ただし、これは本質的に単一式の関数(`a > b`)を記述するためのかなり長い方法です。この例では、クロージャ式の構文を使用して、ソートクロージャをインラインで記述することが望ましいでしょう。

### Closure Expression Syntax

クロージャ式の構文には、次の一般的な形式があります:

![クロージャ式の構文](./../.gitbook/assets/07_closureexpressionsyntax.png)

クロージャ式構文の引数は、in-out 引数にすることができますが、デフォルト値を設定することはできません。可変個引数に名前を付けると、可変個引数を使用できます。タプルは、引数の型および戻り値の型としても使用できます。

以下の例は、上記の `backward(_:_:)` 関数のクロージャ式バージョンです。

```swift
reversedNames = names.sorted(by: { (s1: String, s2: String) -> Bool in
    return s1 > s2
})
```

このインラインクロージャの引数と戻り値の宣言は、`backward(_:_:)` 関数からの宣言と同じなことに注目してください。どちらの場合も、`(s1: String, s2: String) -> Bool` と書きます。ただし、インラインクロージャ式の場合、引数と戻り値の型は中括弧(`{}`)の外側ではなく、中括弧の内側に記述されます。

クロージャの本文は、`in` キーワードの後から始まります。このキーワードは、クロージャの引数と戻り値の型の定義が完了し、クロージャの本文がまもなく開始されることを示します。

クロージャの本文は非常に短いため、1 行で書くこともできます。

```swift
reversedNames = names.sorted(by: { (s1: String, s2: String) -> Bool in return s1 > s2 } )
```

これは、`sorted(by:)` メソッドへの呼び出しが同じままなことを示しています。括弧のペア(`()`)は、メソッドの引数全体をラップしますが、引数はインラインクロージャです。

### Inferring Type From Context

ソートクロージャは、引数としてメソッドに渡されるため、Swift はその引数の型と返す値の型を推測できます。`sorted(by:)` メソッドは、文字列の配列で呼び出されるため、その引数は `(String, String) -> Bool` 型の関数な必要があります。つまり、`(String, String)` 型と `Bool` 型は、クロージャ式に記述する必要はありません。全ての型を推測できるため、戻り矢印(`->`)と引数名を囲む括弧も省略できます。

```swift
reversedNames = names.sorted(by: { s1, s2 in return s1 > s2 } )
```

インラインクロージャ式としてクロージャを関数またはメソッドに渡す場合、引数型と戻り値の型を推測することは常に可能です。その結果、クロージャが関数またはメソッドの引数として使用される場合、完全な形式でインラインクロージャを記述する必要はありません。

必要に応じて型を明示的にすることもできます。コードのあいまいさを回避できる場合は、そうすることをお勧めします。 `sorted(by:)` メソッドの場合、ソートが行われているという事実からクロージャの目的は明らかで、文字列の配列を扱っていることから、クロージャは `String` で機能していると読み手は容易に想定できます。

### Implicit Returns from Single-Expression Closures

上記の例のように、単一式のクロージャは、宣言から `return` キーワードを省略して結果を暗黙的に返すことができます。

```swift
reversedNames = names.sorted(by: { s1, s2 in s1 > s2 } )
```

ここで、`sorted(by:)` メソッドの引数の関数型は、`Bool` 値がクロージャによって返される必要があることを明確にしています。 クロージャの本文は、ブール値を返す単一の式(`s1 > s2`)のため、あいまいさはなく、`return` キーワードは省略できます。

### Shorthand Argument Names

Swift は、インラインクロージャに短縮引数名(*Shorthand Argument Names*)を自動的に提供しています。これを使用して、クロージャの引数の値を `$0`、`$1`、`$2` などの名前で参照できます。

クロージャ式内でこれらの省略引数名を使用する場合は、クロージャの引数リストをその定義から省略できます。短縮引数名の型は、期待されている関数型から推測され、最も大きい番号の短縮引数で、クロージャが受け取る引数の数が決まります。クロージャ式は本文だけで完全に構築(推論)できているため、`in` キーワードは省略できます。

```swift
reversedNames = names.sorted(by: { $0 > $1 } )
```

ここで、`$0` と `$1` は、クロージャの最初と 2 番目の文字列引数を指します。 `$1` は最大数の省略引数のため、クロージャは 2 つの引数を取ることがわかります。ここでの `sorted(by:)` 関数は、引数が両方とも文字列のクロージャを想定しているため、省略形の引数 `$0` と `$1` は両方とも `String` 型です。

### Operator Methods

実際には、上記のクロージャ式を記述するさらに短い方法があります。Swift の `String` 型は、大なり演算子（`>`）の文字列固有の実装を、`String` 型の 2 つの引数を持ち、`Bool` 型の値を返すメソッドとして定義しています。これは、`sorted(by:)` メソッドに必要なメソッド型と完全に一致します。したがって、`>` 演算子を渡すだけで、Swift は文字列固有の実装を使用していると推測します。

```swift
reversedNames = names.sorted(by: >)
```

演算子メソッドの詳細については、[Operator Methods](./advanced-operators.md#operator-methods)を参照ください。

## Trailing Closures

関数の最後の引数としてクロージャ式を関数に渡す必要があってその式が長い場合、代わりに末尾クロージャ(*Trailing Closures*)として記述すると便利な場合があります。末尾クロージャは関数の引数ですが、関数呼び出しの括弧の後に記述します。末尾クロージャ構文を使用する場合、関数を呼び出す時に、最初の末尾クロージャの引数ラベルは記述しません。複数の末尾クロージャを含めることもできます。ただし、下記の最初のいくつかの例では、1 つの末尾クロージャを使用しています。

```swift
func someFunctionThatTakesAClosure(closure: () -> Void) {
    // 関数本文
}

// 末尾クロージャを使用せずにこの関数を呼び出す方法は次のとおりです:

someFunctionThatTakesAClosure(closure: {
    // クロージャ本文
})

// 代わりに末尾クロージャを使用してこの関数を呼び出す方法は次のとおりです:

someFunctionThatTakesAClosure() {
    // クロージャ本文
}
```

上記の[Closure Expression Syntax](#closure-expression-syntax)の文字列ソートクロージャは、`sorted(by:)` メソッドの括弧の外側に末尾クロージャとして記述できます。

```swift
reversedNames = names.sorted() { $0 > $1 }
```

クロージャ式が関数またはメソッドの唯一の引数で、その式を末尾クロージャにする場合、関数を呼び出すときに関数またはメソッドの名前の後に括弧のペア(`()`)を記述する必要はありません。

```swift
reversedNames = names.sorted { $0 > $1 }
```

末尾クロージャは、クロージャが長く、1 行にインラインで書き込むことができない場合に特に立ちます。例として、Swift の配列型には `map(_:)` メソッドがあり、単一の引数としてクロージャ式を取ります。クロージャは、配列内のアイテムごとに 1 回呼び出され、そのアイテムからマッピングされた値（おそらく他の型の値）を返します。`map(_:)` に渡すクロージャ内に、map で何をしていて、どんな戻り値の型を返すのかを指定します。

クロージャを各配列要素に適用した後、`map(_:)` メソッドは、元の配列の対応する値と同じ順序で、全ての新しくマッピングされた値を含む新しい配列を返します。

末尾のクロージャで `map(_:)` メソッドを使用して、`Int` の配列を `String` の配列に変換する方法は次のとおりです。 配列 `[16, 58, 510]` は、新しい配列 `["OneSix", "FiveEight", "FiveOneZero"]` を作成するために使用されています。

```swift
let digitNames = [
    0: "Zero", 1: "One", 2: "Two",   3: "Three", 4: "Four",
    5: "Five", 6: "Six", 7: "Seven", 8: "Eight", 9: "Nine"
]
let numbers = [16, 58, 510]
```

上記のコードは、整数値と英語バージョンの名前をマッピングした辞書を作成します。 また、文字列に変換できる整数の配列も定義しています。

クロージャ式を配列の  `map(_:)` メソッドに末尾クロージャとして渡すことにより、`numbers` 配列を使用して文字列値の配列を作成できます。

```swift
let strings = numbers.map { (number) -> String in
    var number = number
    var output = ""
    repeat {
        output = digitNames[number % 10]! + output
        number /= 10
    } while number > 0
    return output
}
// `strings` は [String]型に推論されます
// 値は ["OneSix", "FiveEight", "FiveOneZero"] です
```

`map(_:)` メソッドは、配列内のアイテムごとにクロージャ式を 1 回呼び出します。型がマッピングされる配列の値から推測できるため、クロージャの入力引数 `number` の型を指定する必要はありません。

この例では、変数 `number` はクロージャの `number` 引数の値で初期化されるため、クロージャ本文内で値を変更できます。(関数とクロージャの引数は常に定数です）。クロージャ式は、マッピングされた出力配列に格納される型を示すために、戻り値の型 `String` も指定します。

クロージャ式は、呼び出されるたびに `output` という文字列を作成します。 剰余演算子(`number % 10`)を使用してて `number` の最後の桁を計算し、この桁を使用して `digitNames` 辞書で適切な文字列を検索します。クロージャを使用して、ゼロより大きい任意の整数の文字列表現を作成できます。

> NOTE  
> 辞書の subscript は、キーが存在しない場合に辞書の検索が失敗する可能性があることを示す optional の値を返すため、`digitNames` 辞書の subscript の呼び出しの後に感嘆符（`！`）が続きます。 上記の例では、`number % 10` が常に `digitNames` 辞書の妥当な subscript のキーであることが保証されているため、感嘆符を使用して、subscript の optional の値に格納されている `String` 値を強制的にアンラップします。

`digitNames` 辞書から取得した文字列が `output` の前に追加され、数値の文字列バージョンが逆順で構築されます。(式 `number % 10` は、`16` の場合は `6`、`58` の場合は `8`、`510` の場合は `0` の値を示します)

次に、`number` 変数は `10` で除算されます。整数のため、小数部分は切り捨てられます。したがって、`16` は `1` になり、`58` は `5` になり、`510` は `51` になります。

このプロセスは、`number` が `0` に等しくなるまで繰り返されます。その時点で、`output` 文字列がクロージャーによって返され、 `map(_:)` メソッドによって出力配列に追加されます。

上記の例では、末尾クロージャ構文を使用すると、クロージャを引数に受け取る関数の直後にクロージャの機能をカプセル化できます。クロージャ全体を `map(_:)` メソッドの外側の括弧で囲む必要はありません。

関数が複数のクロージャを受け取る場合は、最初の末尾クロージャの引数ラベルを省略し、残りの末尾クロージャにラベルを付けます。例えば、次の関数はフォトギャラリーの画像を読み込みます:

```swift
func loadPicture(from server: Server, completion: (Picture) -> Void, onFailure: () -> Void) {
    if let picture = download("photo.jpg", from: server) {
        completion(picture)
    } else {
        onFailure()
    }
}
```

この関数を呼び出して画像をロードするときは、2 つのクロージャを渡します。最初のクロージャは、ダウンロードが成功した後に画像を表示する完了ハンドラです。2 番目のクロージャは、ユーザにエラーを表示するエラーハンドラです。

```swift
loadPicture(from: someServer) { picture in
    someView.currentPicture = picture
} onFailure: {
    print("Couldn't download the next picture.")
}
```

この例では、`loadPicture(from:completion:onFailure:)` 関数がネットワークタスクをバックグラウンドにディスパッチし、ネットワークタスクが終了すると 2 つのハンドラのいずれかを呼び出します。このように関数を作成すると、両方の状況を処理する 1 つのクロージャを使用する代わりに、ネットワーク障害の処理を担当するコードを、ダウンロードが成功した後にユーザーインターフェイスを更新するコードから明確に分離できます。

## Capturing Values

クロージャは、定義されている周囲のコンテキストから定数と変数をキャプチャ(*Capture*)できます。クロージャは、定数と変数を定義した元のスコープが存在しなくなった場合でも、本文内からそれらの定数と変数の値を参照および変更できます。

Swift では、値をキャプチャできるクロージャの最もシンプルな形式は、別の関数の本文内に記述されたネスト関数です。 ネスト関数は、その外部関数の引数や、外部関数内で定義された定数や変数をキャプチャできます。

これは、`makeIncrementer` と呼ばれる関数の例です。これには、`incrementer` と呼ばれるネスト関数が含まれています。`incrementer()` 関数は、周囲のコンテキストから、`runningTotal` と `amount` の 2 つの値をキャプチャします。これらの値をキャプチャした後、インクリメンタは、呼び出されるたびに `runningTotal` を `amount` 分だけ増加するクロージャとして `makeIncrementer` によって返されます。

```swift
func makeIncrementer(forIncrement amount: Int) -> () -> Int {
    var runningTotal = 0
    func incrementer() -> Int {
        runningTotal += amount
        return runningTotal
    }
    return incrementer
}
```

`makeIncrementer` の戻り値の型は `() -> Int` です。これは、シンプルな値ではなく、関数を返すことを意味します。返される関数には引数がなく、呼び出されるたびに `Int` 値を返します。関数が他の関数を返す方法については、[Function Types as Return Types](./functions.md#function-types-as-return-types)を参照ください。

`makeIncrementer(forIncrement:)` 関数は、`runningTotal` と呼ばれる整数の変数を定義して、返される `incrementer` の現在の合計を格納します。この変数は値 `0` で初期化されます。

`makeIncrementer(forIncrement:)` 関数には、`forIncrement` 引数ラベルと `amount` 引数名を持つ 1 つの `Int` 引数があります。この引数に渡される値は、返されたインクリメント関数が呼び出されるたびに、`runningTotal` を増加する量を指定します。 `makeIncrementer` 関数は、実際に値を増加させる `incrementer` と呼ばれるネスト関数を定義します。この関数は単に `runningTotal` に `amount` を加算して、結果を返します。

独立して検討すると、ネストされた `incrementer()` 関数は少し変に見えるかもしれません。

```swift
func incrementer() -> Int {
    runningTotal += amount
    return runningTotal
}
```

`incrementer()` 関数には引数がありませんが、関数本文内から `runningTotal` と `amount` を参照します。これは、`runningTotal` と `amount` への参照を周囲の関数からキャプチャし、それらを独自の関数本文内で使用します。参照によるキャプチャにより、`makeIncrementer` の呼び出しが終了したときに `runningTotal` と `amount` が解放されないようにし、次に `incrementer` 関数が呼び出されたときにも `runningTotal` を使用可能にします。

> NOTE  
> 最適化として、Swift は、値がクロージャによって変更されていない場合やクロージャの作成後に値が変更されていない場合、代わりに値のコピーをキャプチャして保存する場合があります。  
> Swiftは、変数が不要になったときの変数の破棄など全てのメモリを管理します。

`makeIncrementer` の動作例を次に示します。

```swift
let incrementByTen = makeIncrementer(forIncrement: 10)
```

この例では、`incrementByTen` という定数を設定して、呼び出されるたびに `runningTotal` 変数に `10` を追加するインクリメンタ関数を参照します。関数を複数回呼び出すと、次のようになります:

```swift
incrementByTen()
// 10 を返します
incrementByTen()
// 20 を返します
incrementByTen()
// 30 を返します
```

2 番目のインクリメントを作成すると、新しい別の `runningTotal` 変数への参照を格納します。

```swift
let incrementBySeven = makeIncrementer(forIncrement: 7)
incrementBySeven()
// 7 を返します
```

元のインクリメンター（`incrementByTen`）を再度呼び出すと、それ自体の `runningTotal` 変数が引き続きインクリメントされ、`incrementBySeven` によってキャプチャされた変数には影響しません。

```swift
incrementByTen()
// 40 を返します
```

> NOTE  
> class インスタンスのプロパティにクロージャを代入して、クロージャがインスタンスまたはそのメンバーを参照して、そのインスタンスをキャプチャする場合、クロージャとインスタンスの間に循環参照(strong reference cycle)が作成されます。 Swift は、キャプチャリストを使用して、これらの循環参照を中断します。 詳細については、 [StrongReference Cycles for Closures](./automatic-reference-counting.md#strong-reference-cycles-for-closures) を参照ください。

## Closures Are Reference Types

上記の例では、`incrementBySeven` と `incrementByTen` は定数ですが、これらの定数が参照するクロージャは、関数とクロージャが参照型なので、キャプチャした `runningTotal` 変数をインクリメントできます。

関数またはクロージャを定数または変数に代入するときはいつでも、実際にはその定数または変数を関数またはクロージャへの参照を設定しています。上記の例では、`incrementByTen` が参照するのはクロージャの選択で、クロージャ自体の内容ではありません。

つまり、2 つの異なる定数または変数にクロージャを代入する場合、それらは同じクロージャを参照することを意味します。

```swift
let alsoIncrementByTen = incrementByTen
alsoIncrementByTen()
// 50 を返します

incrementByTen()
// 60 を返します
```

上記の例は、`alsoIncrementByTen` を呼び出すことは `incrementByTen` を呼び出すことと同じなを示しています。どちらも同じクロージャを参照しているため、両方ともインクリメントして同じ `runningTotal` を返します。

## Escaping Closures

関数の引数として渡されたクロージャが、関数本文が終了した後に呼び出される場合、関数をエスケープ(*escaping*)すると言われます。引数の 1 つとしてクロージャを受け取る関数を宣言する場合、引数の型の前に `@escaping` を記述して、クロージャがエスケープできることを示すことができます。

クロージャをエスケープする 1 つの方法は、関数の外部で定義された変数に格納することです。 例として、非同期操作を開始する多くの関数は、完了ハンドラとしてクロージャ引数を取ります。関数は操作の開始後に戻り値を返しますが、操作が完了するまでクロージャは呼び出されません。クロージャはエスケープする必要があり、後で呼び出す必要があります。 例えば:

```swift
var completionHandlers = [() -> Void]()
func someFunctionWithEscapingClosure(completionHandler: @escaping () -> Void) {
    completionHandlers.append(completionHandler)
}
```

`someFunctionWithEscapingClosure(_:)` 関数は、引数としてクロージャを取り、関数の外部で宣言されている配列に追加します。この関数の引数を `@escaping` でマークしなかった場合、コンパイルエラーが発生します。

`self` が class のインスタンスを参照する場合、`self` を参照するエスケープクロージャには特別な考慮が必要です。エスケープクロージャで `self` をキャプチャすると、誤って循環参照を作りやすくなります。循環参照については、[Automatic Reference Counting](./automatic-reference-counting.md)を参照ください。

通常、クロージャは、本文で変数を使用して暗黙的に変数をキャプチャしますが、この場合は明示的に宣言する必要があります。 `self` をキャプチャする場合は、使用するときに `self` を記述するか、クロージャのキャプチャリスト(*capture list*)に `self` を含めます。`self` を明示的に書くことで、意図を表現でき、循環参照がないことを確認するように促されます。例えば、下記のコードでは、`someFunctionWithEscapingClosure(_:)` に渡されるクロージャは `self` を明示的に参照しています。対照的に、`someFunctionWithNonescapingClosure(_:)` に渡されるクロージャは、エスケープなしのクロージャです。つまり、暗黙的に `self` を参照できます。

```swift
func someFunctionWithNonescapingClosure(closure: () -> Void) {
    closure()
}

class SomeClass {
    var x = 10
    func doSomething() {
        someFunctionWithEscapingClosure { self.x = 100 }
        someFunctionWithNonescapingClosure { x = 200 }
    }
}

let instance = SomeClass()
instance.doSomething()
print(instance.x)
// Prints "200"

completionHandlers.first?()
print(instance.x)
// Prints "100"
```

下記は、クロージャーのキャプチャリストに含めることで `self` をキャプチャし、暗黙的に `self` を参照する `doSomething()` のバージョンです。

```swift
class SomeOtherClass {
    var x = 10
    func doSomething() {
        someFunctionWithEscapingClosure { [self] in x = 100 }
        someFunctionWithNonescapingClosure { x = 200 }
    }
}
```

`self` が `struct` または `enum` のインスタンスの場合は、いつでも暗黙的に `self` を参照できます。 ただし、`self` が `struct` または `enum` のインスタンスの場合、エスケープクロージャは `self` への変更可能な参照をキャプチャできません。[Structures and Enumerations Are Value Types](./structures-and-classes.md#structures-and-enumerations-are-value-types)でも説明されているように、`struct` または `enum` は変更可能な値の共有はできません。

```swift
struct SomeStruct {
    var x = 10
    mutating func doSomething() {
        someFunctionWithNonescapingClosure { x = 200 }  // Ok
        someFunctionWithEscapingClosure { x = 100 }     // Error
    }
}
```

上記の例の `someFunctionWithEscapingClosure` 関数の呼び出しは、可変メソッド内で `self` は変更可能なので、エラーとなります。`self` は変更可能です。これは、エスケープクロージャが `struct` の変更可能な `self` への参照をキャプチャできないというルールに違反します。

## Autoclosures

自動クロージャ(*autoclosures*)は、関数の引数として渡された式をラップして自動的に作成されるクロージャです。引数はとらず、このクロージャが呼び出されると、ラップされている式の値を返します。この構文は便利で、明示的なクロージャの代わりに通常の式を記述することで、関数のパラメータの中括弧(`{}`)を省略できます。

自動クロージャを引数に取る関数を呼び出すことは一般的ですが、そのような関数を実装することは一般的ではありません。例えば、`assert(condition:message:file:line:)` 関数は、`condition` と `message` 引数を自動クロージャで受け取ります。`condition` 引数はデバッグビルドでのみ評価され、`message` 引数は `condition` が `false` の場合にのみ評価されます。

自動クロージャを使用すると、クロージャを呼び出すまで内部のコードが実行されないため、評価を遅らせることができます。遅延評価は、コードがいつ評価されるかを制御できるため、副作用があるコードや計算コストが高いコードを書く時に役に立ちます。下記のコードは、クロージャが評価をどのように遅らせるかを示しています。

```swift
var customersInLine = ["Chris", "Alex", "Ewa", "Barry", "Daniella"]
print(customersInLine.count)
// Prints "5"

let customerProvider = { customersInLine.remove(at: 0) }
print(customersInLine.count)
// Prints "5"

print("Now serving \(customerProvider())!")
// Prints "Now serving Chris!"
print(customersInLine.count)
// Prints "4"
```

`customersInLine` 配列の最初の要素はクロージャ内のコードによって削除されますが、配列要素はクロージャが実際に呼び出されるまで削除されません。 クロージャが呼び出されない場合、クロージャ内の式が評価されることはありません。つまり、配列要素が削除されることはありません。`customerProvider` の型は `String` ではなく、`() -> String` で、文字列を返す引数のない関数だということに注意してください。

関数の引数としてクロージャを渡すと、遅延評価と同じ動作が得られます。

```swift
// customersInLine は ["Alex", "Ewa", "Barry", "Daniella"]
func serve(customer customerProvider: () -> String) {
    print("Now serving \(customerProvider())!")
}
serve(customer: { customersInLine.remove(at: 0) } )
// Prints "Now serving Alex!"
```

上記のリストの `serve(customer:)` 関数は、顧客の名前を返す明示的なクロージャを受け取ります。下記のバージョンの `serve(customer:)` は同じ操作を実行しますが、明示的なクロージャを取得する代わりに、引数の型を `@autoclosure` 属性でマークすることによって自動クロージャを取得します。 これで、クロージャの代わりに `String` 引数を受け取ったかのように関数を呼び出すことができます。`customerProvider` 引数の型は `@autoclosure` 属性でマークされているため、引数は自動的にクロージャに変換されます。

```swift
// customersInLine は ["Ewa", "Barry", "Daniella"]
func serve(customer customerProvider: @autoclosure () -> String) {
    print("Now serving \(customerProvider())!")
}
serve(customer: customersInLine.remove(at: 0))
// Prints "Now serving Ewa!"
```

> NOTE  
> 自動クロージャを使いすぎると、コードが理解しにくくなる可能性があります。コンテキストと関数名は、遅延評価されていることを明確にする必要があります。

エスケープを許可する自動クロージャが必要な場合は、`@autoclosure` 属性と `@escaping` 属性の両方を使用します。 `@escaping` 属性については、上記の[Escaping Closures](#escaping-closures)で説明しています。

```swift
// customersInLine は ["Barry", "Daniella"]
var customerProviders: [() -> String] = []
func collectCustomerProviders(_ customerProvider: @autoclosure @escaping () -> String) {
    customerProviders.append(customerProvider)
}
collectCustomerProviders(customersInLine.remove(at: 0))
collectCustomerProviders(customersInLine.remove(at: 0))

print("Collected \(customerProviders.count) closures.")
// Prints "Collected 2 closures."
for customerProvider in customerProviders {
    print("Now serving \(customerProvider())!")
}
// Prints "Now serving Barry!"
// Prints "Now serving Daniella!"
```

上記のコードでは、`customerProvider` 引数として渡されたクロージャを呼び出す代わりに、`collectCustomerProviders(_:)` 関数がクロージャを `customerProviders 配列に追加します。配列は関数のスコープ外で宣言されています。つまり、配列のクロージャは関数が戻った後に実行される場合があります。その結果、`customerProvider` 引数の値は、関数のスコープをエスケープできるようにする必要があります。
