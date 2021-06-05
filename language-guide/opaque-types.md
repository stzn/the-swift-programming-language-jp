# Opaque Types

最終更新日:

戻り値に不透明な型(以下*Opaque Type*)を持つ関数またはメソッドは、その戻り値の型情報を隠します。関数の戻り値の型として具体的な型を提供する代わりに、準拠するプロトコルを戻り値として記述します。型情報を隠すことは、戻り値の基になる型を private のままにすることができるため、モジュールとモジュールを呼び出すコードとの境界で役立ちます。プロトコルの型の値を返すのとは異なり、Opaque Type は具体的な型情報を保持し続けます。コンパイラは型情報にアクセスできますが、モジュールのクライアントはアクセスできません。

## The Problem That Opaque Types Solve(Opaque Typeが解決する問題)

例えば、ASCII アートを描画するモジュールを作成しているとします。ASCII アートの基本的な特徴は、その文字列表現を返す `draw()` 関数です。これは、`Shape` プロトコルの要件として使用できます:

```swift
protocol Shape {
    func draw() -> String
}

struct Triangle: Shape {
    var size: Int
    func draw() -> String {
        var result = [String]()
        for length in 1...size {
            result.append(String(repeating: "*", count: length))
        }
        return result.joined(separator: "\n")
    }
}
let smallTriangle = Triangle(size: 3)
print(smallTriangle.draw())
// *
// **
// ***
```

下記のコードに示すように、ジェネリックを使用して、形状を垂直方向に反転するなどの操作を実装できます。ただし、このアプローチには重要な制限があります。反転した結果は、その作成に使用された正確なジェネリック型を公開しています。

```swift
struct FlippedShape<T: Shape>: Shape {
    var shape: T
    func draw() -> String {
        let lines = shape.draw().split(separator: "\n")
        return lines.reversed().joined(separator: "\n")
    }
}
let flippedTriangle = FlippedShape(shape: smallTriangle)
print(flippedTriangle.draw())
// ***
// **
// *
```

下記のコードに示すように、2 つの図形を垂直に結合する `JoinedShape<T: Shape, U:Shape>` 構造体を定義するアプローチは、結果として `JoinedShape<FlippedShape<Triangle>, Triangle>` のような型が反転した三角形を別の三角形と結合することになります。

```swift
struct JoinedShape<T: Shape, U: Shape>: Shape {
    var top: T
    var bottom: U
    func draw() -> String {
        return top.draw() + "\n" + bottom.draw()
    }
}
let joinedTriangles = JoinedShape(top: smallTriangle, bottom: flippedTriangle)
print(joinedTriangles.draw())
// *
// **
// ***
// ***
// **
// *
```

形状の作成に関する詳細情報を公開すると、戻り値に完全な型を指定する必要があるため、ASCII アートモジュールの public インターフェイスの一部として意図されていない型がリークすることがあります。モジュール内のコードは、様々な方法で同じ形状を構築でき、その形状を使用するモジュール外の他のコードは、変換に関する実装の詳細を考慮する必要があるべきではありません。`JoinedShape` や `FlippedShape` などのラッパ型は、モジュールのユーザにとって重要ではなく、表示されるべきではありません。モジュールの public インターフェイスは、形状の結合や反転などの操作で構成され、これらの操作は別の `Shape` 値を返します。

## Returning an Opaque Type(Opaque Typeを返却する)

Opaque Type は、ジェネリック型の逆と考えることができます。ジェネリック型を使用すると、関数を呼び出すコードは、関数の実装から抽象化された方法で、その関数の引数の型を選択して値を返すことができます。例えば、次のコードの関数は、呼び出し元に依存する型を返します。

```swift
func max<T>(_ x: T, _ y: T) -> T where T: Comparable { ... }
```

`max(_:_:)` を呼び出すコードは `x` と `y` の値を選択し、それらの値の型によって `T` の具体的な型が決まります。呼び出し側のコードは、`Comparable` プロトコルに準拠する任意の型を使用できます。関数内のコードはジェネリックな方法で記述されているため、呼び出し元が提供する型を処理できます。`max(_:_:)` の実装は、全ての `Comparable` 型が共有する機能のみを使用します。

Opaque Type を戻り値に持つ関数では、これらの役割が逆になります。Opaque Type 型を使用すると、関数を呼び出すコードから抽象化された方法で、関数の実装が返す値の型を選択できます。例えば、次の例の関数は、その形状の基になる型を公開せずに台形を返します。

```swift
struct Square: Shape {
    var size: Int
    func draw() -> String {
        let line = String(repeating: "*", count: size)
        let result = Array<String>(repeating: line, count: size)
        return result.joined(separator: "\n")
    }
}

func makeTrapezoid() -> some Shape {
    let top = Triangle(size: 2)
    let middle = Square(size: 2)
    let bottom = FlippedShape(shape: top)
    let trapezoid = JoinedShape(
        top: top,
        bottom: JoinedShape(top: middle, bottom: bottom)
    )
    return trapezoid
}
let trapezoid = makeTrapezoid()
print(trapezoid.draw())
// *
// **
// **
// **
// **
// *
```

この例の `makeTrapezoid()` 関数は、その戻り型を `some Shape` として宣言します。結果として、関数は、特定の具体的な型を指定せずに、`Shape` プロトコルに準拠する特定の型の値を返します。このような `makeTrapezoid()` を作成すると、public インターフェイスから特定の型の `Shape` を公開することなく、public インターフェイスの基本的な側面(戻り値は `Shape`)を表現できます。この実装では、2 つの三角形と 1 つの正方形を使用していますが、関数は、戻り値の型を変更せずに、他のさまざまな方法で台形を描画するように書き直すことができます。

この例は、Opaque Type な戻り値の型がジェネリック型の逆のようなものなことを強調しています。`makeTrapezoid()` 内のコードは、呼び出しコードがジェネリック関数に対して行うように、その型が `Shape` プロトコルに準拠している限り、必要な型を返すことができます。関数を呼び出すコードは、ジェネリック関数の実装のようにジェネリックな方法で記述する必要があります。これにより、`makeTrapezoid()` によって返される任意の `Shape` 値を処理できるようになります。

Opaque Type なな戻り値の型をジェネリクスと組み合わせることができます。次のコードの関数は両方とも、`Shape` プロトコルに準拠した何らかの型の値を返します。

```swift
func flip<T: Shape>(_ shape: T) -> some Shape {
    return FlippedShape(shape: shape)
}
func join<T: Shape, U: Shape>(_ top: T, _ bottom: U) -> some Shape {
    JoinedShape(top: top, bottom: bottom)
}

let opaqueJoinedTriangles = join(smallTriangle, flip(smallTriangle))
print(opaqueJoinedTriangles.draw())
// *
// **
// ***
// ***
// **
// *
```

この例の `opaqueJoinedTriangles` の値は、この章の前半の[The Problem That Opaque Types Solve](#the-problem-that-opaque-types-solveopaque-typeが解決する問題)セクションのジェネリクスの例の `joinedTriangles` と同じです。ただし、その例の値とは異なり、`flip(_:)` と `join(_:_:)` は、ジェネリックな形状の操作が返す基本的な型を Opaque Type でラップし、それらの型が外部からは見えないようにします。どちらの関数も、依存する型がジェネリックのためジェネリックで、型引数は `FlippedShape` と `JoinedShape` に必要な型情報を渡します。

戻り値の型が Opaque Type の関数が複数の場所から戻る場合、可能な戻り値は全て同じ型にする必要があります。ジェネリック関数の場合、その戻り型は関数のジェネリック型引数を使用できますが、それでも単一の型にする必要があります。例えば、正方形の特殊なケースを含む、形状反転関数の無効なバージョンを次に示します:

```swift
func invalidFlip<T: Shape>(_ shape: T) -> some Shape {
    if shape is Square {
        return shape // エラー: 戻り値の型が一致しません
    }
    return FlippedShape(shape: shape) //  エラー: 戻り値の型が一致しません
}
```

この関数を `Square` で呼び出すと、`Square` が返されます。それ以外の場合は `FlippedShape` を返します。これは、1 つの型のみの値を返すという要件に違反し、`invalidFlip(_:)` を無効なコードにしてしまいます。`invalidFlip(_:)` を修正する 1 つの方法は、正方形の特殊なケースを `FlippedShape` の実装に移動することです。これにより、この関数は常に `FlippedShape` 値を返すことができます:

```swift
struct FlippedShape<T: Shape>: Shape {
    var shape: T
    func draw() -> String {
        if shape is Square {
            return shape.draw()
        }
        let lines = shape.draw().split(separator: "\n")
        return lines.reversed().joined(separator: "\n")
    }
}
```

常に単一の型を返すという要件は、Opaque Type の戻り値の型でジェネリクスを使用することを妨げるわけではありません。型引数を戻り値の基になる型へ組み込む関数の例を次に示します:

```swift
func `repeat`<T: Shape>(shape: T, count: Int) -> some Collection {
    return Array<T>(repeating: shape, count: count)
}
```

この場合、戻り値の基になる型は `T` によって異なります。どの形状が渡されても、`repeat(shape:count:)` はその形状の配列を作成して返します。それにもかかわらず、戻り値は常に同じ `[T]` の基になる型のため、Opaque Type な戻り値の型を持つ関数は単一の型の値のみを返す必要があるという要件に従います。

## Differences Between Opaque Types and Protocol Types(Opaque Typeとプロトコルの違い)
