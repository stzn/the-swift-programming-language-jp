# Initialization(初期化)

最終更新日:

初期化(*Initialization*)は、使用するクラス、構造体、または列挙型のインスタンスを準備するプロセスです。このプロセスには、そのインスタンスに保存されている各プロパティの初期値の設定と、新しいインスタンスの使用準備が整う前に必要なその他の要素のセットアップや初期化の実行が含まれます。

この初期化プロセスを実装するには、イニシャライザ(*initializers*)を定義します。イニシャライザは、特定の型の新しいインスタンスを作成するために呼び出すことができる特別なメソッドのようなものです。Objective-C のイニシャライザとは異なり、Swift のイニシャライザは値を返しません。主な役割は、型の新しいインスタンスが初めて使用される前に正しく初期化されることを保証することです。

クラス型のインスタンスは、そのクラスのインスタンスが解放される直前にカスタムのクリーンアップを実行するデイニシャライザ(*deinitializer*) を実装することもできます。デイニシャライザの詳細については、[Deinitialization](./deinitialization.md)を参照ください。

## Setting Initial Values for Stored Properties(格納プロパティの初期値の設定)

クラスと構造は、そのクラスまたは構造体のインスタンスが作成されるまでに、格納されている全てのプロパティの適切な初期値を設定する必要があります。格納プロパティは、不確定な状態のままにすることはできません。

イニシャライザ内で、またはプロパティの定義の一部としてデフォルトのプロパティ値を割り当てることにより、格納プロパティの初期値を設定できます。これらのアクションについては、次のセクションで説明します。

> NOTE  
> 格納プロパティにデフォルト値を割り当てるか、イニシャライザ内でその初期値を設定すると、プロパティオブザーバを呼び出さずに、そのプロパティの値が直接設定されます。

### Initializers(イニシャライザ)

イニシャライザ(*initializers*)は、特定の型の新しいインスタンスを作成するために呼び出されます。最もシンプルな形式では、イニシャライザはパラメータのないインスタンスメソッドのようなもので、`init` キーワードを使用して記述されます:

```swift
init() {
    // ここで初期化を実行します
}
```

下記の例では、華氏で表された温度を保存するために `Fahrenheit` という新しい構造体を定義しています。`Fahrenheit` 構造体には、`Double` 型の `temperature` という 1 つの格納プロパティがあります。

```swift
struct Fahrenheit {
    var temperature: Double
    init() {
        temperature = 32.0
    }
}
var f = Fahrenheit()
print("The default temperature is \(f.temperature)° Fahrenheit")
// "The default temperature is 32.0° Fahrenheit"
```

この構造体は、パラメータのない単一のイニシャライザ `init` を定義します。これは、格納された温度を `32.0`(華氏での水の凝固点)の値で初期化します。

### Default Property Values(デフォルトのプロパティ値)

## Customizing Initialization(初期化のカスタマイズ)

### Initialization Parameters(イニシャライザの引数)

### Parameter Names and Argument Labels(パラメータ名と引数ラベル)

### Initializer Parameters Without Argument Labels(引数ラベルのないイニシャライザパラメータ)

### Optional Property Types(optional のプロパティ型)

### Assigning Constant Properties During Initialization(初期化中の定数プロパティの割り当て)

## Default Initializers(デフォルトイニシャライザ)

### Memberwise Initializers for Structure Types(構造体の全メンバーを引数に取るイニシャライザ)

## Initializer Delegation for Value Types(値型のイニシャライザの委譲)

## Class Inheritance and Initialization(クラスの継承と初期化)

### Designated Initializers and Convenience Initializers(指定イニシャライザとcovenience イニシャライザ)

### Syntax for Designated and Convenience Initializers(指定イニシャライザとcovenience イニシャライザの構文)

### Initializer Delegation for Class Types(クラス型のイニシャライザの委譲)

### Two-Phase Initialization(2段階イニシャライザ)

### Initializer Inheritance and Overriding(イニシャライザの継承とオーバーライド)

### Automatic Initializer Inheritance(自動イニシャライザの継承)

### Designated and Convenience Initializers in Action(指定とcovenience イニシャライザの挙動)

## Failable Initializers(失敗可能イニシャライザ)

### Failable Initializers for Enumerations(列挙型の失敗可能イニシャライザ)

### Failable Initializers for Enumerations with Raw Values(Raw Valueを持つ列挙型の失敗可能イニシャライザ)

### Propagation of Initialization Failure(初期化の失敗の伝播)

### Overriding a Failable Initializer(失敗可能イニシャライザのオーバーライド)

### The init! Failable Initializer(init!を使った失敗可能イニシャライザ)

## Required Initializers(必須イニシャライザ)

## Setting a Default Property Value with a Closure or Function(クロージャまたは関数を使用したデフォルトのプロパティ値の設定)
