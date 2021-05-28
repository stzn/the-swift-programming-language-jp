# Inheritance(継承)

最終更新日:

クラスは、メソッド、プロパティ、およびその他の特性を別のクラスから継承できます。あるクラスが別のクラスから継承する場合、継承するクラスはサブクラス(*subclass*)と呼ばれ、継承するクラスはそのスーパークラス(*superclass*)と呼ばれます。継承は、Swift の他の型とクラスの違う基本的な挙動です。

Swift のクラスは、スーパークラスに属するメソッド、プロパティ、および subscript を呼び出してアクセスし、それらのメソッド、プロパティ、および `subscript` の独自のオーバーライドバージョンを提供して、挙動を変更できます。Swift は、オーバーライドしている定義がスーパークラスの定義に一致していることを確認することで、オーバーライドが正しいことを確認します。

クラスは、プロパティの値が変更されたときに通知を受けるために、継承されたプロパティにプロパティオブザーバ(*property observer*)を追加することもできます。プロパティオブザーバは、元々格納プロパティとして定義されているか計算プロパティとして定義されているかに関係なく、任意のプロパティに追加できます。

## Defining a Base Class(基本クラスの定義)

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
