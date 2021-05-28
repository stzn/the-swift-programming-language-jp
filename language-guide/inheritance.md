# Inheritance(継承)

最終更新日:

クラスは、メソッド、プロパティ、およびその他の特性を別のクラスから継承できます。あるクラスが別のクラスから継承する場合、継承するクラスはサブクラス(*subclass*)と呼ばれ、継承するクラスはそのスーパークラス(*superclass*)と呼ばれます。継承は、Swift の他の型とクラスの違う基本的な挙動です。

Swift のクラスは、スーパークラスに属するメソッド、プロパティ、および subscript を呼び出してアクセスし、それらのメソッド、プロパティ、および `subscript` の独自のオーバーライドバージョンを提供して、挙動を変更できます。Swift は、オーバーライドしている定義がスーパークラスの定義に一致していることを確認することで、オーバーライドが正しいことを確認します。

クラスは、プロパティの値が変更されたときに通知を受けるために、継承されたプロパティにプロパティオブザーバ(*property observer*)を追加することもできます。プロパティオブザーバは、元々格納プロパティとして定義されているか計算プロパティとして定義されているかに関係なく、任意のプロパティに追加できます。

## Defining a Base Class(基本クラスの定義)

別のクラスから継承しないクラスは、基本クラス(*base class*)と呼ばれます。

> NOTE  
> Swiftクラスは、全てのクラスに共通する基本クラスを継承しません。スーパークラスを指定せずに定義したクラスは、自動的に基礎となる基本クラスになります。

以下の例では、`Vehicle` という基本クラスを定義しています。この基本クラスは、`currentSpeed` と呼ばれる格納プロパティを定義します。デフォルト値は `0.0` です(プロパティの型は `Double` と推論されます)。`currentSpeed` プロパティの値は、`description` と呼ばれる読み取り専用の `String` 型の計算プロパティによって使用され、乗り物の説明をします。

`Vehicle` 基本クラスは、`makeNoise` と呼ばれるメソッドも定義します。このメソッドは、基本の `Vehicle` インスタンスに対して何もしませんが、後で `Vehicle` のサブクラスによってカスタマイズされます。

```swift
class Vehicle {
    var currentSpeed = 0.0
    var description: String {
        return "traveling at \(currentSpeed) miles per hour"
    }
    func makeNoise() {
        // 何もしない - 乗り物は必ずしても騒音を出しません
    }
}
```

初期化構文を使用して `Vehicle` の新しいインスタンスを作成します。これは、型名とそれに続く空の括弧(`()`)として記述されます。

```swift
let someVehicle = Vehicle()
```

新しい `Vehicle` インスタンスを作成したら、その `description` プロパティにアクセスして、乗り物の現在の速度の人間が読める形式の説明を出力します。

```swift
print("Vehicle: \(someVehicle.description)")
// Vehicle: traveling at 0.0 miles per hour
```

`Vehicle` クラスは、任意の乗り物に共通の特性を定義しますが、そのまま使用されることはあまりありません。より便利にするには、より具体的な種類の乗り物を記述するようにする必要があります。

## Subclassing(サブクラス)

## Overriding(オーバーライド)

### Accessing Superclass Methods, Properties, and Subscripts(親クラスのメソッド、プロパティ、subscriptへのアクセス)

### Overriding Methods(メソッドのオーバーライド)

### Overriding Properties(プロパティのオーバーライド)

#### Overriding Property Getters and Setters(プロパティのゲッターセッタードのオーバーライド)

---

#### Overriding Property Observers(プロパティオブザーバーのオーバーライド)

---

## Preventing Overrides(オーバーライドを防ぐ)
