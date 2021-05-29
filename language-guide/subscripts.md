# Subscripts(サブスクリプト)

最終更新日:

クラス、構造体、および列挙型は、コレクション、リスト、またはシーケンスのメンバー要素にアクセスするためのショートカット subscript を定義できます。subscript を使用すると、インデックスによって値を設定および取得でき、設定と取得に個別のメソッドを必要としません。たとえば、`Array` インスタンスの要素には `someArray[index]` としてアクセスし、`Dictionary` インスタンスの要素には `someDictionary[key]` としてアクセスします。

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

## Subscript Options(subscriptのオプション)

## Type Subscripts(型subscript)
