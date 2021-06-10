# Types \(型\)

最終更新日:

Swift では、名前付き型(*named　type*)と複合型(*compound type**)の 2 種類があります。名前付き型は、定義されているときに特定の名前を指定できる型です。名前付き型には、クラス、構造体、列挙型、およびプロトコルが含まれます。例えば、`MyClass` という名前のユーザ定義クラスのインスタンスは `MyClass` 型です。ユーザ定義の型に加えて、Swift 標準ライブラリは、配列、辞書、およびオプショナルの値を含む多くの一般的に使用されている名前の型を定義します。

通常、数字、文字、文字列を表す型など、他の言語で基本的またはプリミティブと見なされるデータ型は、Swift 標準ライブラリでは構造体を使用して、定義され、実装されている名前付き型です。名前付き型のため、[Extensions](./expressions.md)、 [Extension Declaration](./declarations.md#extension-declaration拡張宣言)で説明されている拡張宣言を使用して、プログラムのニーズに合わせて振る舞いを拡張することができます。

複合型は、Swift 言語自体で定義されている名前のない型です。関数型とタプル型の 2 つの複合型があります。複合型の種類は、名前付き型および他の複合型を含んでいることがあります。例えば、タプル型の `(Int, (Int, Int))` には、名前付き型の `Int` と複合型の(Int, Int)2 つの要素が含まれています。

名前付き型または複合型の周囲に括弧(`()`)を付けることができます。ただし、型の周囲に括弧を必ずしも追加する必要はありません。例えば、`(Int)` は `Int` に相当します。

この章では、Swift 言語自体で定義されている型について説明し、Swift の型推論の動作について説明します。

> GRAMMAR OF A TYPE
> *type* → 関数型(function type)  
> *type* → 配列型(array type)  
> *type* → 辞書型(dictionary type)  
> *type* → 型識別子(type identifier)  
> *type* → タプル型(tuple type)  
> *type* → オプショナル型(optional type)  
> *type* → 暗黙アンラップオプショナル型(implicitly unwrapped optional type)  
> *type* → プロトコル合成型(protocol composition type)  
> *type* → Opaque Type(opaque type)  
> *type* → メタタイプ型(metatype type)  
> *type* → Any型(any type)  
> *type* → Self型(self type)  
> *type* → **(** 型 **)**  

## Type Annotation(型アノテーション)

## Type Identifier

## Tuple Type

## Function Type

### Restrictions for Nonescaping Closures

## Array Type

## Dictionary Type

## Optional Type

## Implicitly Unwrapped Optional Type

## Protocol Composition Type

## Opaque Type

## Meta*type* Type

## Any Type

## Self Type

## Type Inheritance Clause

## Type Inference
