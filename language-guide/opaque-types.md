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

## Differences Between Opaque Types and Protocol Types(Opaque Typeとプロトコルの違い)
