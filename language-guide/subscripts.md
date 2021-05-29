# Subscripts(サブスクリプト)

最終更新日:

クラス、構造体、および列挙型は、コレクション、リスト、またはシーケンスのメンバー要素にアクセスするためのショートカット subscript を定義できます。subscript を使用すると、インデックスによって値を設定および取得でき、設定と取得に個別のメソッドを必要としません。たとえば、`Array` インスタンスの要素には `someArray[index]` としてアクセスし、`Dictionary` インスタンスの要素には `someDictionary[Key]` としてアクセスします。

単一の型に対して複数の subscript を定義でき、使用する適切な subscript のオーバーロードは、subscript に渡すインデックス値の型に基づいて選択されます。subscript は 1 つの次元に限定されず、カスタムの型のニーズに合わせて複数の入力パラメータで subscript を定義できます。

## Subscript Syntax(subscript構文)

subscript を使用すると、インスタンス名の後に 1 つ以上の値を角かっこ(`[]`)で囲むことで、型のインスタンスをクエリできます。それらの構文は、インスタンスメソッドの構文と計算プロパティの構文の両方に似ています。インスタンスメソッドと同じ方法で、`subscript` キーワードを使用して subscript 定義を記述し、1 つ以上の入力パラメータと戻り値の型を指定します。インスタンスメソッドとは異なり、subscript は読み取り/書き込みまたは読み取り専用にすることができます。この挙動は、計算プロパティの場合と同じ方法でゲッタとセッタとやり取りをします。

```swift
subscript(index: Int) -> Int {
    get {
        // ここで適切な値を返します
    }
    set(newValue) {
        // ここで適切な値を設定するアクションをします
    }
}
```

`newValue` の型は、subscript の戻り値と同じです。計算プロパティと同様に、セッタの `(newValue)` パラメータを指定しないこともできます。自分で設定しない場合、`newValue` というデフォルトのパラメータがセッタに提供されます。

読み取り専用の計算プロパティと同様に、`get` キーワードとその中括弧(`{}`)を削除することで、読み取り専用の subscript の宣言を簡単にできます:

```swift
subscript(index: Int) -> Int {
    // ここで適切な値を返します
}
```

これは、整数の n 倍のテーブルを表す `TimesTable` 構造体を定義する読み取り専用の subscript の実装の例です。

```swift
struct TimesTable {
    let multiplier: Int
    subscript(index: Int) -> Int {
        return multiplier * index
    }
}
let threeTimesTable = TimesTable(multiplier: 3)
print("six times three is \(threeTimesTable[6])")
// "six times three is 18"
```

この例では、`TimesTable` の新しいインスタンスが作成され、3 の倍数テーブルを表します。これは、インスタンスの `multiplier` パラメータに使用する値として、構造体のイニシャライザに値 `3` を渡すことで示しています。

`threeTimesTable[6]` への呼び出しでも示されているように、subscript を呼び出すことで `threeTimesTable` インスタンスにクエリを実行できます。これは、3 の倍数テーブルの `6` 番目のエントリを要求し、値 `18`、つまり 3 x 6 を返します。

> NOTE  
> n 倍数テーブル は、決まった数学ルールに基づいています。`threeTimesTable[someIndex]` を新しい値に設定することは適切ではないため、`TimesTable` のsubscriptは読み取り専用のsubscript として定義されています。

## Subscript Usage(subscriptの利用)

「subscript」の正確な意味は、それが使用されるコンテキストによって異なります。subscript は通常、コレクション、リスト、またはシーケンス内のメンバー要素にアクセスするためのショートカットとして使用されます。特定のクラスまたは構造体の機能に最も適した方法で、自由に subscript を実装できます。

例えば、Swift の `Dictionary` 型は、`Dictionary` インスタンスに格納されている値を設定および取得するための subscript を実装します。辞書に値を設定するには、辞書のキー の型を subscript の角括弧(`[]`)で囲み、辞書のバリュー の型の値を subscript に割り当てます:

```swift
var numberOfLegs = ["spider": 8, "ant": 6, "cat": 4]
numberOfLegs["bird"] = 2
```

上記の例では、`numberOfLegs` という変数を定義し、3 つのキーとバリューのペアを含む辞書テラルで初期化します。`numberOfLegs` 辞書の型は `[String: Int]` だと推論されます。この例では、辞書を作成した後、subscript の割り当てを使用して、`"bird"` の `String` のキーと 2 の `Int` のバリューを辞書に追加します。

`Dictionary` の subscript の詳細については、[Accessing and Modifying a Dictionary](./collection-types.md#accessing-and-modifying-a-dictionary辞書へのアクセスと変更)を参照ください。

> NOTE  
> Swift の `Dictionary` 型は、optional の型を受け取って返すsubscriptとしてキーの subscript を実装します。上記の `numberOfLegs` 辞書の場合、キーとバリューの subscript は `Int?` または optional Int 型の値を受け取り、返します。`Dictionary` 型は、 optional の subscript の型を使用して、全てのキーにバリューがあるわけではないということをモデル化し、そのキーに `nil` 値を割り当てることによってキーのバリューを削除する方法を提供します。

## Subscript Options(subscriptのオプション)

## Type Subscripts(型subscript)
