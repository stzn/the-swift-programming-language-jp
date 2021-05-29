# Methods(メソッド)

最終更新日:

メソッドは、特定の型に関連付けられた関数です。クラス、構造体、および列挙型は全て、インスタンスメソッドを定義できます。インスタンスメソッドは、特定の型のインスタンスを操作するための特定のタスクと機能をカプセル化します。クラス、構造体、および列挙型は、型自体に関連付けられている型メソッドを定義することもできます。 型メソッドは、Objective-C のクラスメソッドに似ています。

Swift で構造体と列挙型がメソッドを定義できるという事実は、C 言語および Objective-C との大きな違いです。Objective-C では、クラスはメソッドを定義できる唯一の型です。Swift では、クラス、構造体、または列挙型を定義するかどうかを選択でき、作成した型のメソッドを柔軟に定義できます。

## Instance Methods(インスタンスメソッド)

インスタンスメソッドは、特定のクラス、構造体、または列挙型のインスタンスに属する関数です。それらは、インスタンスプロパティにアクセスしたり、変更する方法を提供するか、インスタンスの目的に関連する機能を提供することによって、そのインスタンスの機能をサポートします。[Functions](./functions.md)で説明されているように、インスタンスメソッドの構文は関数とまったく同じです。

インスタンスメソッドは、それが属する型の開き括弧(`{`)と閉じ括弧(`}`)内に記述します。インスタンスメソッドは、その型の他のすべてのインスタンスメソッドおよびプロパティに暗黙的にアクセスできます。インスタンスメソッドは、それが属する型の特定のインスタンスでのみ呼び出すことができます。既存のインスタンスがなければ、単独で呼び出すことはできません。

アクションが発生した回数をカウントするために使用できる、シンプルな `Counter` クラスを定義する例を次に示します:

```swift
class Counter {
    var count = 0
    func increment() {
        count += 1
    }
    func increment(by amount: Int) {
        count += amount
    }
    func reset() {
        count = 0
    }
}
```

`Counter` クラスは 3 つのインスタンスメソッドを定義します。

* `increment()` は、カウンタを 1 ずつインクリメントします
* `increment(by: Int)` は、指定された整数分だけカウンタをインクリメントします
* `reset()` は、カウンタを 0 にリセットします

`Counter` クラスは、現在のカウンタ値を追跡するために変数プロパティ `count` も宣言しています

プロパティと同じドット構文でインスタンスメソッドを呼び出します。

```swift
let counter = Counter()
// counter の初期値は 0
counter.increment()
// counter の値は 1
counter.increment(by: 5)
// counter の値は 6
counter.reset()
// counter の値は 0
```

[Function Argument Labels and Parameter Names](./functions.md#function-argument-labels-and-parameter-names引数ラベルと引数名)で説明されているように、関数パラメータは、(関数の本文内で使用する)名前と(関数を呼び出すときに使用する)引数ラベルの両方を持つことができます。メソッドは型に関連付けられた関数にすぎないため、メソッドパラメータについても同じことが言えます。

### The self Property(selfプロパティ)

### Modifying Value Types from Within Instance Methods(インスタンスメソッド内からの値型の変更)

### Assigning to self Within a Mutating Method(mutatingメソッド内からselfへの代入)

### Type Methods(型メソッド)
