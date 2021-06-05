# Opaque Types

最終更新日:

戻り値に不透明な型(以下*Opaque Type*)を持つ関数またはメソッドは、その戻り値の型情報を隠します。関数の戻り値の型として具体的な型を提供する代わりに、準拠するプロトコルを戻り値として記述します。型情報を隠すことは、戻り値の基になる型をプライベートのままにすることができるため、モジュールとモジュールを呼び出すコードとの境界で役立ちます。プロトコルの型の値を返すのとは異なり、Opaque Type は具体的な型情報を保持し続けます。コンパイラは型情報にアクセスできますが、モジュールのクライアントはアクセスできません。

## The Problem That Opaque Types Solve(Opaque Typeが解決する問題)

## Returning an Opaque Type(Opaque Typeを返却する)

## Differences Between Opaque Types and Protocol Types(Opaque Typeとプロトコルの違い)
