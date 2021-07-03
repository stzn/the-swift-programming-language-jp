# Opaque Types (Opaque 型)

最終更新日: 2021/6/30

戻り値に不透明な型(以下*Opaque 型*)を持つ関数またはメソッドは、その戻り値の型情報を隠します。関数の戻り値の型として具体的な型を提供する代わりに、準拠するプロトコルの観点から戻り値を記述します。型情報を隠すことで、戻り値の基になる型を private のままにすることができるため、モジュールとモジュールを呼び出すコードの間に境界を設けることに役立ちます。プロトコル型の値を返すのとは異なり、Opaque 型は具体的な型情報を保持し続けます。コンパイラは型情報にアクセスできますが、モジュールのクライアントはアクセスできません。

## The Problem That Opaque Types Solve(Opaque 型が解決する問題)

例えば、ASCII アートを描画するモジュールを作成しているとします。ASCII アートの基本的な特徴は、その文字列表現を返す `draw()` 関数です。これは、`Shape` プロトコルの要件として使用されています:

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

下記のコードに示すように、ジェネリクスを使用して、形状を垂直方向に反転するなどの操作を実装できます。ただし、このアプローチには重要な制限があります。反転した結果は、その作成に使用された具体的な型を外部に公開しています。

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

下記のコードに示すように、2 つの図形を垂直に結合する `JoinedShape<T: Shape, U:Shape>` 構造体を定義するアプローチでは、反転した三角形を別の三角形と結合すると `JoinedShape<FlippedShape<Triangle>, Triangle>` のような型の結果を返します。

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

形状の作成に関する詳細情報を公開すると、戻り値に完全な型を指定する必要があるため、ASCII アートモジュールの public インターフェイスで公開する意図のない型をリークしてしまうことがあります。モジュール内のコードは、様々な方法で同じ形状を構築でき、その形状を使用するモジュール外の他のコードは、変換に関する実装の詳細を考慮する必要があるべきではありません。`JoinedShape` や `FlippedShape` などのラッパ型は、モジュールのユーザにとって重要ではなく、公開されるべきではありません。モジュールの public インターフェイスは、形状の結合や反転などの操作で構成され、これらの操作は別の `Shape` 値を返します。

## Returning an Opaque Types(Opaque 型を返却する)

Opaque 型は、ジェネリック型の逆と考えることができます。ジェネリック型を使用すると、関数を呼び出すコードは、関数の実装から抽象化された方法で、その関数のパラメータの型を選択して値を返すことができます。例えば、次のコードの関数は、呼び出し元に依存する型を返します。

```swift
func max<T>(_ x: T, _ y: T) -> T where T: Comparable { ... }
```

`max(_:_:)` を呼び出すコードは `x` と `y` の値を選択し、それらの値の型によって `T` の具体的な型が決まります。呼び出し側のコードは、`Comparable` プロトコルに準拠する任意の型を使用できます。関数内のコードはジェネリックな方法で記述されているため、呼び出し元が提供する型を処理できます。`max(_:_:)` の実装は、全ての `Comparable` 型が共有する機能のみを使用できます。

Opaque 型を戻り値に持つ関数では、これらの役割が逆になります。Opaque 型を使用すると、関数を呼び出すコードから抽象化された方法で、関数の実装が返す値の型を選択できます。例えば、次の例の関数は、その形状の基になる型を公開せずに台形を返します。

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

この例の `makeTrapezoid()` 関数は、その戻り値の型を `some Shape` として宣言します。結果として、関数は、特定の具体的な型を指定せずに、`Shape` プロトコルに準拠する特定の型の値を返します。このような `makeTrapezoid()` を作成すると、public インターフェイスから特定の型の `Shape` を公開することなく、public インターフェイスの基本的な側面\(戻り値は `Shape`\)を表現できます。この実装では、2 つの三角形と 1 つの正方形を使用していますが、関数は、戻り値の型を変更せずに、他の様々な方法で台形を描画するように書き直すことができます。

この例は、Opaque な戻り値の型がジェネリック型の逆のようなものだということを強調しています。`makeTrapezoid()` 内のコードは、呼び出し側のコードがジェネリック関数に対して行うように、その型が `Shape` プロトコルに準拠している限り、必要な型を返すことができます。関数を呼び出すコードは、ジェネリック関数の実装のようにジェネリックな方法で記述する必要があります。これにより、`makeTrapezoid()` によって返される任意の `Shape` 値を処理できるようになります。

Opaque な戻り値の型をジェネリクスと組み合わせることもできます。次のコードの関数は両方とも、`Shape` プロトコルに準拠した何らかの型の値を返します。

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

この例の `opaqueJoinedTriangles` の値は、この章の前半の[The Problem That Opaque Types Solve(Opaque 型が解決する問題)](#the-problem-that-opaque-types-solveOpaque型が解決する問題)セクションのジェネリクスの例の `joinedTriangles` と同じです。ただし、その例の値とは異なり、`flip(_:)` と `join(_:_:)` は、ジェネリックな形状の操作が返す基本的な型を Opaque でラップし、それらの型が外部からは見えないようにしています。どちらの関数も、依存する型がジェネリックなのでジェネリック関数で、型パラメータは `FlippedShape` と `JoinedShape` に必要な型情報を渡します。

Opaque な戻り値の型を持つ関数が複数の場所から呼び出される場合、返す可能性のある戻り値は全て同じ型にする必要があります。ジェネリック関数の場合、その戻り値の型は関数のジェネリック型パラメータを使用できますが、それでも単一の型にする必要があります。例えば、正方形の特殊なケースを含む、形状反転関数の無効なバージョンを次に示します:

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

常に単一の型を返すという要件は、Opaque な戻り値の型でジェネリクスを使用することを妨げるわけではありません。型パラメータを戻り値の基になる型へ組み込む関数の例を次に示します:

```swift
func `repeat`<T: Shape>(shape: T, count: Int) -> some Collection {
    return Array<T>(repeating: shape, count: count)
}
```

この場合、戻り値の基になる型は `T` によって異なります。どの形状が渡されても、`repeat(shape:count:)` はその形状の配列を作成して返します。それにもかかわらず、戻り値は常に同じ `[T]` の基になる型のため、Opaque な戻り値の型を持つ関数は単一の型の値のみを返す必要があるという要件に従います。

## Differences Between Opaque Types and Protocol Types(Opaque 型とプロトコルの違い)

Opaque 型を返すことは、プロトコル型を関数の戻り値の型として使用する場合と非常によく似ていますが、これら 2 種類の戻り値の型は、型情報を保持するかどうかが異なります。Opaque 型は 1 つの特定の型を参照しますが、関数の呼び出し側はどの型を参照するかはわかりません。プロトコル型は、プロトコルに準拠する任意の型を参照できます。一般に、プロトコル型では、格納する値の基になる型についてより柔軟に対応でき、Opaque 型を使用すると、基になる型についてより強力な保証を行うことができます。

例えば、Opaque 型の代わりにプロトコル型を戻り値の型として使用する、`flip(_:)` のバージョンを次に示します:

```swift
func protoFlip<T: Shape>(_ shape: T) -> Shape {
    return FlippedShape(shape: shape)
}
```

このバージョンの `protoFlip(_:)` は、`flip(_:)` と同じ本文を持ち、常に同じ型の値を返します。`flip(_:)` とは異なり、`protoFlip(_:)` が返す値は、常に同じ型にする必要はありません。ただ、`Shape` プロトコルに準拠する必要があるだけです。別の言い方をすれば、`protoFlip(_:)` は、`flip(_:)` よりも呼び出し元との API 契約をはるかに緩くします。複数の型の値を返す柔軟性を確保しています:

```swift
func protoFlip<T: Shape>(_ shape: T) -> Shape {
    if shape is Square {
        return shape
    }

    return FlippedShape(shape: shape)
}
```

このコードの改訂版は、渡された形状に応じて、`Square` のインスタンスまたは `FlippedShape` のインスタンスを返します。この関数によって返される 2 つの反転した形状は、まったく異なる型を持つ可能性があります。この関数の他のバージョンが、同じ形状の複数のインスタンスを反転するときに、異なる型の値を返す可能性もあります。`protoFlip(_:)` からの戻り値の型情報がそれほど具体的でないということは、型情報に依存する多くの操作が戻り値で使用できないことを意味します。例えば、この関数によって返される結果を比較する `==` 演算子を記述することはできません。

```swift
let protoFlippedTriangle = protoFlip(smallTriangle)
let sameThing = protoFlip(smallTriangle)
protoFlippedTriangle == sameThing  // エラー
```

この例の最終行のエラーは、いくつかの理由で発生します。当面の問題は、`Shape` のプロトコル要件に `==` 演算子が含まれていないことです。追加しようとすると、次に遭遇する問題は、`==` 演算子がその左辺と右辺の引数の型を知る必要があることです。この種の演算子は通常、`Self` 型の引数を取り、プロトコルに準拠する具体的な型に一致しますが、プロトコルに `Self` 要件を追加すると、プロトコルを型として使用することができなくなります。

プロトコル型を関数の戻り値の型として使用すると、プロトコルに準拠する任意の型を柔軟に返すことができます。ただし、その柔軟性によって、返された値の一部の操作が犠牲になります。この例では、プロトコル型では保持されない特定の型情報に依存しているため、`==` 演算子が使用できないことを示しています。

このアプローチのもう 1 つの問題は、形状変換をネストできないことです。三角形を反転した結果は、`Shape` 型の値で、`protoFlip(_:)` 関数は、`Shape` プロトコルに準拠した何らかの型の引数を取ります。ただし、プロトコル型はそのプロトコル自体に準拠しません。`protoFlip(_:)` によって返される値は `Shape` に準拠していません。これは、反転した形状が `protoFlip(_:)` の有効な引数ではないため、複数の変換を適用する `protoFlip(protoFlip(smallTriange))` のようなコードが無効だということを意味します。

対照的に、Opaque 型は、基になる型情報を保持します。Swift は関連型を推論できるため、プロトコル型を戻り値として使用できない場所で Opaque 型な戻り値の型を使用できます。例えば、これは [Generics(ジェネリクス)](./generics.md)の `Container` プロトコルの別のバージョンです:

```swift
protocol Container {
    associatedtype Item
    var count: Int { get }
    subscript(i: Int) -> Item { get }
}
extension Array: Container { }
```

このプロトコルには関連型があるため、`Container` を関数の戻り値の型として使用することはできません。また、関数本文の外部にそのジェネリック型が何かを推論するための必要な情報が十分にないため、ジェネリックな戻り値の型の制約として使用することもできません。

```swift
// エラー: 関連型があるプロトコルは、戻り値の型として使用できません
func makeProtocolContainer<T>(item: T) -> Container {
    return [item]
}

// エラー: C を推論するための十分な情報がありません
func makeProtocolContainer<T, C: Container>(item: T) -> C {
    return [item]
}
```

Opaque 型を使用して、`some Container` を戻り値の型として使用すると、望ましい API 契約が表現できます。関数はコンテナを返しますが、コンテナの型を特定しません。

```swift
func makeOpaqueContainer<T>(item: T) -> some Container {
    return [item]
}
let opaqueContainer = makeOpaqueContainer(item: 12)
let twelve = opaqueContainer[0]
print(type(of: twelve))
// "Int"
```

`twelve` の型は `Int` だと推論されます。これは、型推論が Opaque 型でも機能することを示しています。`makeOpaqueContainer(item:)` の実装では、Opaque 型の基になる型は `[T]` です。この場合、`T` は `Int` のため、戻り値は整数の配列で、関連型 `Item` は `Int` だと推論されます。`Container` の subscript は `Item` を返します。これは、`twelve` の型も `Int` だと推論されることを意味します。
