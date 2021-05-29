# Properties(プロパティ)

最終更新日:

プロパティは、値を特定のクラス、構造体、または列挙型に関連付けます。格納プロパティ(*stored property*)は定数と変数の値をインスタンスの一部として保存しますが、計算プロパティ(*computed property*)は値を(保存するのではなく)計算します。計算プロパティは、クラス、構造体、および列挙型から使用できます。格納プロパティは、クラスと構造体のみで使用できます。

格納および計算プロパティは、通常、特定の型のインスタンスに関連付けられています。ただし、プロパティを型自体に関連付けることもできます。このようなプロパティは、型プロパティ(*type property*)と呼ばれます。

さらに、プロパティオブザーバ(*property observer*)を定義して、プロパティ値の変更を監視して、それに応じたアクションを起こすことができます。プロパティオブザーバは、自分で定義した格納プロパティ、およびサブクラスがそのスーパークラスから継承するプロパティに追加できます。

プロパティラッパー(*property wrapper*)を使用して、複数のプロパティのゲッタとセッタでコードを再利用することもできます。

## Stored Properties(格納プロパティ)

最もシンプルな形式だと、格納プロパティ(*Stored Property*)は、特定のクラスまたは構造体のインスタンスの一部として保存される定数または変数です。格納プロパティは、変数格納プロパティ(`var`)または定数格納プロパティ(`let`) のいずれかです。

[Default Property Values](./initialization.md#default-property-valuesデフォルトプロパティ値)で説明されているように、格納プロパティのデフォルト値をその定義の一部として設定できます。また、初期化中に格納プロパティの初期値を設定および変更することもできます。これは、[Assigning Constant Properties During Initialization](./initialization.md#assigning-constant-properties-during-initialization初期化中の定数プロパティへの値の設定)で説明されているように、定数の格納にも当てはまります。

以下の例では、`FixedLengthRange` という構造体を定義しています。これは、作成後に範囲の長さを変更できない整数の範囲を記述します。

```swift
struct FixedLengthRange {
    var firstValue: Int
    let length: Int
}
var rangeOfThreeItems = FixedLengthRange(firstValue: 0, length: 3)
// 0, 1, 2 の整数の範囲を表しています
rangeOfThreeItems.firstValue = 6
// 6, 7, 8 の整数の範囲を表しています
```

`FixedLengthRange` のインスタンスには、`firstValue` と呼ばれる変数格納プロパティと `length` という定数格納プロパティがあります。上記の例では、定数のため、長さは新しい範囲が作成されたときに初期化され、それ以降は変更できません。

### Stored Properties of Constant Structure Instances(定数に保存されたインスタンスのプロパティ)

### Lazy Stored Properties(遅延格納プロパティ)

### Stored Properties and Instance Variables(格納プロパティはインスタンス変数)

## Computed Properties(計算プロパティ)

### Shorthand Setter Declaration(短縮セッタ宣言)

### Shorthand Getter Declaration(短縮ゲッタ宣言)

### Read-Only Computed Properties(読み取り専用計算プロパティ)

## Property Observers

## Property Wrappers(プロパティラッパー)

### Setting Initial Values for Wrapped Properties(ラップされたプロパティの初期値の設定)

### Projecting a Value From a Property Wrapper(Property Wrapperからの値の投影)

## Global and Local Variables(グローバル変数とローカル変数)

## Type Properties

### Type Property Syntax

### Querying and Setting Type Properties