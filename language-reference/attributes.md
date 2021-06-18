# Attributes \(属性\)

最終更新日:

Swift には、宣言に適用される属性と型に適用される属性の 2 種類があります。属性は、宣言または型に関する追加情報を提供します。たとえば、関数宣言の `discardableResult` 属性は、関数は値を返しますが、戻り値が使用されていない場合、コンパイラは警告を生成しないことを示します。

`@` 記号に続けて属性の名前と、属性が受け入れる引数を書き込むことにより、属性を指定します。

![属性](./../.gitbook/assets/attributes.png)

一部の宣言属性は、属性とそれが特定の宣言にどのように適用されるかについての詳細情報を指定する引数を受け入れます。これらの attribute arguments は括弧で囲まれ、その形式は属する属性によって定義されています。

## Declaration Attributes

### available

### discardableResult

### dynamicCallable

### dynamicMemberLookup

### frozen

### GKInspectable

### inlinable

### main

### nonobjc

### NSApplicationMain

### NSCopying

### NSManaged

### objc

### objcMembers

### propertyWrapper

### resultBuilder

#### Result-Building Methods

#### Result Transformations

#### Custom Result-Builder Attributes

### requires_stored_property_inits

### testable

### UIApplicationMain

### usableFromInline

### warn_unqualified_access

### Declaration Attributes Used by Interface Builder

## Type Attributes

### autoclosure

### convention

### escaping

## Switch Case Attributes

### unknown
