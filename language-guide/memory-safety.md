# Memory Safety

最終更新日:

デフォルトでは、Swift はコード内での安全でない動作の発生を防ぎます。例えば、Swift では、変数が使用される前に初期化され、メモリの割り当てが解除された後にメモリにアクセスされず、範囲外エラーがないか配列インデックスがチェックされます。

また、メモリ内のアドレスを変更するコードへの排他的アクセスを要求することで、メモリの同じ領域へ複数が競合してアクセスしないようにします。Swift はメモリを自動的に管理するため、ほとんどの場合、メモリへのアクセスについて考える必要はまったくありません。ただし、潜在的な競合が発生する可能性がある場所を理解することが重要です。そうすることで、メモリへのアクセスが競合するコードを記述しないようにすることができます。コードに競合が含まれている場合、コンパイル時または実行時エラーが発生します。

## Understanding Conflicting Access to Memory(メモリへのアクセスの競合を理解する)

### Characteristics of Memory Access(メモリアクセスの性質)

## Conflicting Access to In-Out Parameters(In-Out引数へのアクセスの競合)

## Conflicting Access to self in Methods(メソッド内のselfへのアクセスの競合)

## Conflicting Access to Properties(プロパティへのアクセスの競合)
