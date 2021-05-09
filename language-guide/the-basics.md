# The Basics

最終更新日: 2020/5/9

Swift は iOS, macOS, watchOS, tyOS のアプリ開発のための新しいプログラミング言語です。一方で、 Swift の多くの部分は C 言語と Objective-C の開発経験から馴染みのある感じを受けるでしょう。

Swift は C 言語と Objective-C の全ての基本的な型に対応した Swift バージョンの型を提供します。 integer に対する `Int`、floating-point に対する `Double` と `Float`、Boolean に対する `Bool`、text データに対する `String`があります。また、3つのより強力な collection 型(`Array`, `Set`, `Dictionary` )も提供します。詳細は [Collection Types](./language-guide/../collection-types.md)に記載。

C 言語のように、Swift は名前を特定することで、値を保持したり、その値を参照するために変数を使います。また、Swift は変数の値を変更できなくすることで、より幅広い方法で変数を利用しています。これらは定数として知られており、C 言語の定数よりもかなり強力です。定数は、値を変更する必要がない場合に、定数を利用することで、コードを意図的に、より安全に、よりわかりやすくするために Swift 全体で利用されます。

馴染みのある型に加え、Swift は Objective-C にはなかった、tuple のようなより応用的な型を導入します。tuple は値を1つのグループとして扱うことができます。tuple を使うと、関数から複数の値を1つの値の組み合わせとして返すことができます。

Swift は 値が存在しないかもしれない値を扱う optional 型を導入します。optional は、「値が存在していて、これは x と等しい」もしくは「値は一切存在しない」ということを伝えます。optional は Objective-C のポインタと `nil` を扱うのに似ていますが、class だけではなく、あらゆる型に利用することができます。Objective-C の `nil` ポインタよりも、安全で表現的なだけでなく、多くの Swift の最も強力な特徴のコアな部分になります。

Swift は型安全な言語です。つまり、言語がコードで扱う値の型を明確にしてくれます。`String` が必要な場合、この型安全な特徴が、間違って `Int` を渡してしまうことを防いでくれます。同様に、型安全なことで、optional ではない `String` に optional の `String` を気がつかずに渡してしまうことも防いでくれます。型安全なことで、開発プロセスの中でできる限り早くエラーに気がついて、修正する手助けをしてくれます。

## Constants and Variables

定数と変数は特定の型の値(`10`、`"Hello"`など)と名前(`maximumNumberOfLoginAttempts`、`welcomeMessage`など)を関連付けます。定数は一度値を設定すると変更することはできません。一方で変数は先々に異なった値を設定できます。

### Declaring Constants and Variables

### Type Annotations

### Naming Constants and Variables

### Printing Constants and Variables

## Comments

## Semicolons

## Integers

### Integer Bounds

### Int

### UInt

## Floating-Point Numbers

## Type Safety and Type Inference

## Numeric Literals

## Numeric Type Conversion

### Integer Conversion

### Integer and Floating-Point Conversion

## Type Aliases

## Booleans

## Tuples

## Optionals

### nil

### If Statements and Forced Unwrapping

### Optional Binding

### Implicitly Unwrapped Optionals

## Error Handling

## Assertions and Preconditions

### Debugging with Assertions

### Enforcing Preconditions

