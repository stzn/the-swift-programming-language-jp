# Automatic Reference Counting

最終更新日:

Swift は自動参照カウント(以下*ARC*)を使用して、アプリのメモリ使用量を追跡および管理します。ほとんどの場合、これはメモリ管理が Swift が「ただ機能している」ことを意味し、メモリ管理について自分で考える必要はありません。ARC は、クラスインスタンスが不要になったときに、クラスインスタンスによって使用されていたメモリを自動的に解放します。

ただし、場合によっては、ARC がメモリを管理するために、コードの各部分間の関係についてより多くの情報を必要とすることがあります。この章では、これらの状況について説明し、ARC でアプリの全てのメモリを管理できるようにする方法を示します。Swift での ARC の使用は、[Transitioning to ARC Release Notes](https://developer.apple.com/library/archive/releasenotes/ObjectiveC/RN-TransitioningToARC/Introduction/Introduction.html)で説明されているアプローチと非常によく似ています。

参照カウントは、クラスインスタンスにのみ適用されます。構造体と列挙型は、参照型ではなく値型で、参照が格納および渡されることはありません。

## How ARC Works(ARCはどう機能するのか)

## ARC in Action(ARCの挙動)

## Strong Reference Cycles Between Class Instances(クラスインスタンス間の強参照循環)

## Resolving Strong Reference Cycles Between Class Instances(クラスインスタンス間の強参照循環の解消)

### Weak References(弱参照)

### Unowned References(非所有参照)

### Unowned Optional References(非所有オプショナル参照)

### Unowned References and Implicitly Unwrapped Optional Properties(非所有参照と暗黙的にアンラップしたオプショナルプロパティ)

## Strong Reference Cycles for Closures(クロージャの強参照循環)

## Resolving Strong Reference Cycles for Closures(クロージャの強参照循環の解消)

## Defining a Capture List(キャプチャリストの定義)

## Weak and Unowned References(弱参照と非所有参照)
