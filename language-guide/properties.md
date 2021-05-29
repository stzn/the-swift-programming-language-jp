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

構造体のインスタンスを作成し、そのインスタンスを定数に割り当てる場合、インスタンスのプロパティは、変数で宣言されていても変更できません。

```swift
let rangeOfFourItems = FixedLengthRange(firstValue: 0, length: 4)
// 0, 1, 2 の整数の範囲を表しています
rangeOfFourItems.firstValue = 6
// firstValue は変数ですがエラーになります
```

`rangeOfFourItems` は定数として(`let` キーワードを使用して)宣言されているため、`firstValue` が変数でも、その `firstValue` プロパティを変更することはできません。

この挙動は、構造型が値型のためです。値型のインスタンスが定数としてマークされている場合、その全てのプロパティも同様です。

参照型のクラスには同じことが当てはまりません。参照型のインスタンスを定数に割り当てた場合でも、そのインスタンスの変数を変更できます。

### Lazy Stored Properties(遅延格納プロパティ)

遅延格納プロパティ(*lazy stored property*)は、最初に使用されるまで初期値が計算されないプロパティです。宣言の前に `lazy` 修飾子を記述して、遅延格納プロパティを示します。

> NOTE  
> インスタンスの初期化が完了するまで初期値が取得できない可能性があるため、遅延プロパティは常に変数として(`var` キーワードを使用して)宣言する必要があります。定数プロパティは、初期化が完了する前に常に値を持っている必要があるため、`lazy` で宣言することはできません。

遅延格納プロパティは、プロパティの初期値が、インスタンスの初期化が完了後でないと値がわからない外部要因に依存している場合に役立ちます。プロパティの初期値が必要になるまで実行すべきではない、複雑または計算コストの高いセットアップが必要な場合にも役立ちます。

下記の例では、遅延格納プロパティを使用して、複雑なクラスの不必要な初期化を回避しています。この例では、`DataImporter` と `DataManager` という 2 つのクラスを定義していますが、どちらも全体像は見せていません:

```swift
class DataImporter {
    /*
    DataImporter は、外部ファイルからデータをインポートするためのクラスです
    このクラスは、初期化にかなりの時間がかかると想定します
    */
    var filename = "data.txt"
    // DataImporter クラスは、ここでデータのインポート機能を提供します
}

class DataManager {
    lazy var importer = DataImporter()
    var data = [String]()
    // DataManager クラスはここでデータの管理機能を提供します
}

let manager = DataManager()
manager.data.append("Some data")
manager.data.append("Some more data")
// importer プロパティの DataImporter インスタンスはまだ作成されていません
```

`DataManager` クラスには、`data` と呼ばれる格納プロパティがあり、新しい空の `String` 型の配列で初期化されています。残りの機能は示されていませんが、この `DataManager` クラスの目的は、この `String` の配列を管理し、アクセスする機能を提供することです。

`DataManager` クラスの機能の一部は、ファイルからデータをインポートする機能です。この機能は、`DataImporter` クラスによって提供されています。このクラスは、初期化にかなりの時間がかかると想定されています。これは、`DataImporter` インスタンスが初期化されるときに、`DataImporter` インスタンスがファイルを開いてその内容をメモリに読み込む必要があるからかもしれません。

`DataManager` インスタンスはファイルからデータをインポートしなくてもデータを管理できるため、`DataManager` 自体が作成されるときに、`DataManager` は新しい `DataImporter` インスタンスを作成しません。代わりに、`DataImporter` インスタンスを最初に使用するときに作成する方が理にかなっています。

`lazy` 修飾子でマークされているため、`importer` プロパティの `DataImporter` インスタンスは、`importer` プロパティが最初にアクセスされたとき(`filename` プロパティが照会されたときなど)にのみ作成されます。

```swift
print(manager.importer.filename)
// importer プロパティの DataImporter インスタンスが作成されました
// "data.txt"
```

> NOTE  
> `lazy` 修飾子でマークされたプロパティが複数のスレッドから同時にアクセスされ、そのプロパティがまだ初期化されていない場合、そのプロパティが 1 回だけ初期化されるという保証はありません。

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