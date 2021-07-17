# ドキュメント改訂履歴\(Document Revision History\)

最終更新日: 2021/7/3

## 2021-06-07

* Swift 5.5 に更新しました
* 非同期関数、Task、およびアクターについての情報を[Concurrency\(同時並行\)](../language-guide/concurrency.md)の章、および[Actor Declaration\(アクター宣言\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/declarations#actor-declaration)、[Asynchronous Functions and Methods\(非同期関数と非同期メソッド\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/declarations#asynchronous-functions-and-asynchronous-methods)、および[Await Operator\(Await 演算子\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/expressions#await-operator)セクションに追加しました

## 2021-04-26

* Swift 5.4 に更新しました
* [Result Builders\(リザルトビルダ\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/advanced-operators#result-builders)と[resultBuilder](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/attributes#resultbuilder)セクションに、リザルトビルダについての情報を追加しました
* [Implicit Conversion to a Pointer Type\(ポインタ型への暗黙変換\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/expressions#implicit-conversion-to-a-pointer-type)セクションに、関数呼び出しの中で in-out パラメータを unsafe pointer に暗黙的に変換する方法についての情報を追加しました
* [Variadic Parameters\(可変長パラメータ\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/functions#variadic-parameters)と[Function Declaration\(関数宣言\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/declarations#function-declaration)セクションを更新しました。関数は複数の可変長パラメータを受け取ることができるようになりました
* [Implicit Member Expression\(暗黙メンバ式\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/expressions#implicit-member-expression)セクションを更新しました。暗黙メンバ式を繋げて使えるようになりました

## 2020-09-16

* Swift 5.3 に更新しました
* [Trailing Closures\(末尾クロージャ\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/closures#trailing-closures)セクションに、複数の末尾クロージャについての情報を追加し、[Function Call Expression\(関数呼び出し式\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/expressions#function-call-expression)セクションに、複数の末尾クロージャをパラメータに合わせる方法についての情報を追加しました
* [Adopting a Protocol Using a Synthesized Implementation\(デフォルト実装を使用したプロトコル準拠\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/protocols#adopting-a-protocol-using-a-synthesized-implementation)セクションに、列挙型が `Comparable` プロトコルに準拠するための合成実装についての情報を追加しました
* [Contextual Where Clauses\(コンテキスト上の where 句\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/generics#contextual-where-clauses)セクションに、ジェネリック `where` 句をより多くの場所に記載できる旨を追加しました
* [Unowned Optional References\(オプショナル値への非所有参照\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/automatic-reference-counting#unowned-optional-references)セクションに、オプショナル値を `unowned` 参照に使用する方法についての情報を追加しました
* [main](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/attributes#main)セクションに `@main` 属性についての情報を追加しました
* [Literal Expression\(リテラル式\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/expressions#literal-expression)セクションに、`#filePath` を追加し、`#file` の記載を更新しました
* [Escaping Closures\(エスケープクロージャ\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/closures#escaping-closures)セクションを更新しました。多くのシナリオでクロージャが暗黙的に `self` を参照できるようになりました
* [Handling Errors Using Do-Catch\(do catch を使ったエラー処理\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/error-handling#handling-errors-using-do-catch)と[Do Statement\(Do 文\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/statements#do-statement)のセクションを更新しました。`catch` 句が複数のエラーにマッチングできるようになりました
* `Any` の詳細を追加し、[Any Type\(Any 型\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/types#any-type)セクションに移動しました
* [Property Observers\(プロパティオブザーバ\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/properties#property-observers)セクションを更新しました、`lazy` プロパティでもオブザーバが使用できるようになりました
* [Protocol Declaration\(プロトコル宣言\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/declarations#protocol-declaration)セクションを更新しました。列挙型のメンバがプロトコル要件を満たすことができるようになりました
* [Stored Variable Observers and Property Observers\(格納変数オブザーバとプロパティオブザーバ\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/declarations#stored-variable-observers-and-property-observers)セクションを更新して、get がいつオブザーバの前に呼び出されるかついて記述しました
* [Memory Safety\(メモリ安全性\)](../language-guide/memory-safety.md)の章を更新して、アトミック操作について記述しました

## 2020-03-24

* Swift 5.2 に更新しました
* [Key-Path Expression\(Key-Path 式\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/expressions#keypath-expression)セクションに、クロージャの代わりにキーパスを渡すことについての情報を追加しました
* [Methods with Special Names\(特別な名前のメソッド\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/declarations#methods-with-special-names)セクションに、関数呼び出し構文にクラス、構造体、および列挙型のインスタンスを使用できるようにする糖衣構文\(シンタックスシュガー\)についての情報を追加しました
* [Subscript Options\(様々な subscript\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/subscripts#subscript-options)セクションを更新しました。subscript でデフォルトパラメータを使用できるようになりました
* [Self Type\(Self 型\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/types#self-type)セクションを更新しました。より多くのコンテキストで `Self` が使えるようになりました
* [Implicitly Unwrapped Optionals\(暗黙アンラップオプショナル\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/the-basics#implicitly-unwrapped-optionals)セクションを更新して、暗黙アンラップオプショナル値をオプショナルまたは非オプショナルのどちらでも使用できることをより明確にしました

## 2019-09-10

* Swift 5.1 に更新しました
* [Opaque Types \(Opaque 型\)](../language-guide/opaque-types.md)の章に、戻り値に特定の名前の戻り値の型を提供するのではなく、戻り値が準拠するプロトコルを指定する関数についての情報を追加しました
* [Property Wrappers\(プロパティラッパ\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/properties#property-wrappers)セクションに、プロパティラッパについての情報を追加しました
* [frozen](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/attributes#frozen)セクションに、ライブラリエボリューションのための frozen な列挙型と構造体についての情報を追加しました
* [Functions With an Implicit Return\(暗黙的な戻り値がある関数\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/functions#functions-with-an-implicit-return)と[Shorthand Getter Declaration\(省略 get プロパティ宣言\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/properties#shorthand-getter-declaration)セクションに、`return` を省略した関数についての情報を追加しました
* [Type Subscripts\(型 subscript\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/subscripts#type-subscripts)セクションに、型への subscript の使用について情報を追加しました
* [Enumeration Case Pattern\(列挙型ケースパターン\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/patterns#enumeration-case-pattern)セクションを更新しました。列挙型ケースとオプショナル値をマッチングできるようになりました
* [Memberwise Initializers for Structure Types\(構造体のメンバワイズイニシャライザ\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/structures-and-classes#structures-and-classes-memberwise-initializers-for-structure-types)セクションを更新しました。メンバワイズイニシャライザがデフォルト値を持つプロパティのパラメータを省略することができるようになりました
* [dynamicMemberLookup](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/attributes#dynamicmemberlookup)セクションに、実行時にキーパスで検索された動的メンバについての情報を追加しました
* [Conditional Compilation Block\(条件付きコンパイルブロック\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/statements#conditional-compilation-block)のターゲット環境リストに `macCatalyst` を追加しました
* [Self Type\(Self 型\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/types#self-type)セクションを更新しました。`Self` を使用して、現在のクラス、構造体、または列挙型宣言で導入された型を参照することができるようになりました

## 2019-03-25

* Swift 5.0 に更新しました
* [Extended String Delimiters\(拡張区切り文字\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/strings-and-characters#extended-string-delimiters)セクションを追加し、拡張区切り文字についての情報を[String Literals\(文字列リテラル\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/lexical-structure#lexical-structure-string-literals)セクションに追加しました
* `dynamicCallable` 属性を使用してインスタンスを関数として動的に呼び出すことについての情報を記載した[dynamicCallable](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/attributes#dynamiccallable)セクションを追加しました
* `unknown` スイッチケース属性を使用した `switch` 文の将来の列挙ケースを扱う方法についての情報を記載した[unknown](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/attributes#unknown)と[Switching Over Future Enumeration Cases\(列挙型の将来のケースのスイッチング\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/statements#switching-over-future-enumeration-cases)セクションを追加しました
* [Key-Path Expression\(Key-Path 式\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/expressions#keypath-expression)セクションに、識別キーパス\(`\.self`\)についての情報を追加しました
* [Conditional Compilation Block\(条件付きコンパイルブロック\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/statements#conditional-compilation-block)のプラットフォーム条件に\(`<`\)演算子の使用方法についての情報を追加しました

## 2018-09-17

* Swift 4.2 に更新しました
* [Iterating over Enumeration Cases\(列挙ケースの繰り返し処理\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/enumerations#iterating-over-enumeration-cases)セクションに、列挙型の全てのケースへのアクセス方法についての情報を追加しました
* [Compile-Time Diagnostic Statement\(コンパイル時診断文\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/statements#compile-time-diagnostic-statement)セクションに、`#error` と `#warning` についての情報を追加しました
* [Declaration Attributes\(宣言属性\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/attributes#declaration-attributes)セクションの `inlinable` と `usableFromInline` 属性に、インライン化についての情報を追加しました
* [Declaration Attributes\(宣言属性\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/attributes#declaration-attributes)セクションの `dynamicMemberLookup` 属性に、実行時に名前で検索するメンバについての情報を追加しました
* [Declaration Attributes\(宣言属性\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/attributes#declaration-attributes)セクションに、`require_stored_property_inits` および `warn_unqualified_access` 属性についての情報を追加しました
* [Conditional Compilation Block\(条件付きコンパイルブロック\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/statements#conditional-compilation-block)セクションに、Swift コンパイラのバージョンに応じて、条件付きでコンパイルする方法についての情報を追加しました
* [Literal Expression\(リテラル式\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/expressions#literal-expression)セクションに、`#dsohandle` についての情報を追加しました

## 2018-03-29

* Swift 4.1 に更新しました
* [Equivalence Operators\(等価演算子\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/advanced-operators#equivalence-operators)セクションに、等価演算子の合成実装についての情報を追加しました
* [Declarations\(宣言\)](../language-reference/declarations.md)の章の[Extension Declaration\(拡張宣言\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/declarations#extension-declaration)セクションと、[Protocols\(プロトコル\)](../language-guide/protocols.md)の章の[Conditionally Conforming to a Protocol\(条件付きでのプロトコルへの準拠\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/protocols#conditionally-conforming-to-a-protocol)セクションに、条件付きプロトコル準拠についての情報を追加しました
* [Using a Protocol in Its Associated Type’s Constraints\(関連型の制約へのプロトコルの使用\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/generics#using-a-protocol-in-its-associated-types-constraintsnohenopurotokoruno)セクションに、再帰的プロトコル制約についての情報を追加しました
* [Conditional Compilation Block\(条件付きコンパイルブロック\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/statements#conditional-compilation-block)セクションに、`canImport()` と `targetEnvironment()` プラットフォーム条件についての情報を追加しました

## 2017-12-04

* Swift 4.0.3 に更新しました
* [Key-Path Expression\(Key-Path 式\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/expressions#keypath-expression)セクションを更新しました。subscript にキーパスを使用できるようになりました

## 2017-09-19

* Swift 4.0 に更新しました
* [Memory Safety\(メモリ安全性\)](../language-guide/memory-safety.md)の章に、メモリへの排他アクセスについての情報を追加しました
* [Associated Types with a Generic Where Clause\(ジェネリック where 句を使用した関連型\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/generics#associated-types-with-a-generic-where-clause)セクションを追加しました。ジェネリック `where` 句で関連型を制約できるようになりました
* [Strings and Characters\(文字列と文字\)](../language-guide/strings-and-characters.md)の章の[String Literals\(文字列リテラル\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/strings-and-characters#strings-and-characters-string-literals)セクションと、[Lexical Structure\(字句構造\)](../language-reference/lexical-structure.md)の章の[String Literals\(文字列リテラル\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/lexical-structure#lexical-structure-string-literals)セクションに、複数行文字列リテラルについての情報を追加しました
* [Declaration Attributes\(宣言属性\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/attributes#declaration-attributes)セクションの `objc` 属性の説明を更新しました。この属性が推論される場所がより少なくなりました
* [Generic Subscripts\(ジェネリック subscript\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/generics#generic-subscripts)セクションを追加しました。subscript はジェネリックにできます
* [Protocols\(プロトコル\)](../language-guide/protocols.md)の章の[Protocol Composition\(プロトコル合成\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/protocols#protocol-composition)セクションと、[Types\(型\)](../language-reference/types.md)の章の[Protocol Composition Type\(プロトコル合成型\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/types#protocol-composition-type)セクションの説明を更新しました。プロトコル合成型にスーパークラスの要件を含めることができるようになりました
* [Extension Declaration\(拡張宣言\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/declarations#extension-declaration)のプロトコル拡張の説明を更新しました。`final` は使用できなくなりました
* [Assertions and Preconditions\(アサーションと事前条件\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/the-basics#assertions-and-preconditions)セクションに、前提条件と致命的エラーについての情報を追加しました

## 2017-03-27

* Swift 3.1 に更新しました
* [Extensions with a Generic Where Clause\(ジェネリック where 句を使った拡張\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/generics#extensions-with-a-generic-where-clause)セクションに、要件を含む extension についての情報を追加しました
* [For-In Loops\(For-In ループ\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/control-flow#for-in-loops)セクションに範囲全体を繰り返す例を追加しました
* [Failable Initializers\(失敗可能イニシャライザ\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/initialization#initialization-failable-initializers)セクションに、失敗可能な数値変換の例を追加しました
* [Declaration Attributes\(宣言属性\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/attributes#declaration-attributes)セクションに、Swift 言語バージョンを使用した `available` 属性の使い方についての情報を追加しました
* [Function Type\(関数型\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/types#types-function-type)セクションの説明を更新し、関数型に引数ラベルを書くことができないことを記載しました
* [Conditional Compilation Block\(条件付きコンパイルブロック\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/statements#conditional-compilation-block)セクションの Swift 言語バージョン番号の説明を更新しました。オプショナルのパッチ番号が使用できるようになりました
* [Function Type\(関数型\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/types#types-function-type)セクションの説明を更新し、Swift は、タプル型の単一のパラメータを受け取る関数と、複数のパラメータを受け取る関数が区別できるようになりました
* [Expressions\(式\)](../language-reference/expressions.md)の章から動的型式のセクションを削除しました。`type(of:)` は Swift 標準ライブラリ関数になりました

## 2016-10-27

* Swift 3.0.1 に更新しました
* [Automatic Reference Counting\(自動参照カウント\)](../language-guide/automatic-reference-counting.md)の章の弱参照、非所有参照の説明を更新しました
* [Declaration Modifiers\(宣言修飾子\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/declarations#declaration-modifiers)セクションに、`unowned`、`unowned(safe)` と `unowned(unsafe)` 修飾子についての情報を追加しました
* [Type Casting for Any and AnyObject\(Any および AnyObject の型キャスト\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/type-casting#type-casting-for-any-and-anyobject)セクションに、Any 型の値が期待される場所にオプショナルの値を使用することについての記載を追加しました
* 括弧付き式とタプル式の説明を分けるように、[Expressions\(式\)](../language-reference/expressions.md)の章を更新しました

## 2016-09-13

* Swift 3.0 に更新しました
* [Functions\(関数\)](../language-guide/functions.md)の章と[Function Declaration\(関数宣言\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/declarations#function-declaration)セクションの関数の説明を、デフォルトで全てのパラメータが引数ラベルを持つように更新しました
* [Advanced Operators\(高度な演算子\)](../language-guide/advanced-operators.md)の章の演算子の説明を更新しました。これらの演算子をグローバル関数としてはなく型メソッドとして実装できるようになりました
* [Access Control\(アクセスコントロール\)](../language-guide/access-control.md)の章に、`open` と `filePrivate` アクセスレベル修飾子についての情報を追加しました
* [Function Declaration\(関数宣言\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/declarations#function-declaration)セクションの `inout` の説明を、パラメータ名の前ではなくパラメータの型の前に使用するように更新しました
* [Escaping Closures\(エスケープクロージャ\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/closures#escaping-closures)と[AutoClosures\(自動クロージャ\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/closures#autoclosures)セクションと、[Attributes\(属性\)](../language-reference/attributes.md)の章の説明で、`@noescape` および `@autoclosures` 属性を宣言属性から型属性へ更新しました
* [Advanced Operators\(高度な演算子\)](../language-guide/advanced-operators.md)の章の[Precedence for Custom Infix Operators\(カスタム中置演算子の優先順位\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/advanced-operators#precedence-for-custom-infix-operators)セクションと、[Declaration\(宣言\)](../language-reference/declarations.md)の章の[Precedence Group Declaration\(優先順位グループ宣言\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/advanced-operators#precedence-for-custom-infix-operators)セクションに、演算子の優先順位グループについての情報を追加しました
* OS X の変わりに macOS、`ErrorProtocol` の代わりに `Error`、`StringLiteralConvertible` の代わりに `ExpressibleByStringLiteral` などのプロトコル名を使用するように説明を更新しました
* [Generic\(ジェネリクス\)](../language-guide/generics.md)の章の[Generic Where Clauses\(ジェネリック where 句\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/generics#generic-where-clauses)セクションと、[Generic Parameters and Arguments\(ジェネリックパラメータと引数\)](../language-reference/generic-parameters-and-arguments.md)の章の説明を更新しました。ジェネリック `where` 句を宣言の最後に書くことができるようになりました
* [Escaping Closures\(エスケープクロージャ\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/closures#escaping-closures)セクションの説明を更新しました。クロージャはデフォルトが非エスケープになりました
* [The Basics\(基本\)](../language-guide/the-basics.md)の章の[Optional Binding\(オプショナルバインディング\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/the-basics#optional-binding)セクションと、[Statement\(文\)](../language-reference/statements.md)の章の[While Statement\(While 文\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/statements#while-statement)セクションの説明を更新しました。`if`、`while`、`guard` 文は、`where` 句なしで、カンマ区切りリストの条件を使用できるようになりました
* [Control Flow\(制御フロー\)](../language-guide/control-flow.md)の章の[Switch](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/control-flow#switch)セクションと、[Statement\(文\)](../language-reference/statements.md)の章の[Switch Statement\(Switch 文\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/statements#switch-statement)セクションに、複数のパターンを持つ列挙ケースについての情報を追加しました
* [Function Type\(関数型\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/types#types-function-type)セクションの関数型の説明を更新しました。引数ラベルが関数の型の一部ではなくなりました
* [Protocols\(プロトコル\)](../language-guide/protocols.md)の章の[Protocol Composition\(プロトコル合成\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/protocols#protocol-composition)セクションと、[Types\(型\)](../language-reference/types.md)の章の[Protocol Composition Type\(プロトコル合成型\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/types#protocol-composition-type)セクションの説明を更新し、新しい `Protocol1 & Protocol2` 構文を使用するようになりました
* 動的型式セクションで、新しい `type(of:)` を使用するように説明を更新しました
* [Line Control Statement\(行制御文\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/statements#line-control-statement)セクションで、`#sourceLocation(file:line:)` 構文を使用するように、行制御文の説明を更新しました
* [Functions that Never Return\(ノーリターン関数\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/declarations#functions-that-never-return)の説明を更新し、新しい `Never` 型を使用するようにしました
* [Literal Expression\(リテラル式\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/expressions#literal-expression)セクションに、playground リテラルについての情報を追加しました
* [In-Out Parameters\(In-Out パラメータ\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/declarations#declarations-in-out-parameters)セクションの説明を更新し、非エスケープクロージャのみが in-out パラメータをキャプチャできることを記載しました
* [Default Parameter Values\(デフォルトパラメータ値\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/functions#default-parameter-values)セクションのデフォルトパラメータの説明を更新しました。関数呼び出し時にデフォルトパラメータを並べ替えることができなくなりました
* [Attributes\(属性\)](../language-reference/attributes.md)の章でコロンを使用するように属性引数を更新しました
* [Rethrowing Functions and Methods\(再スロー関数と再スローメソッド\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/declarations#rethrowing-functions-and-methods)セクションに、再スロー関数のキャッチブロック内でエラーをスローすることについての情報を追加しました
* [Selector Expression\(Selector 式\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/expressions#selector-expression)セクションに、Objective-C プロパティの get または set のセレクタへのアクセスについての情報を追加しました
* [Type Alias Declaration\(タイプエイリアス宣言\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/declarations#type-alias-declaration)セクションに、ジェネリックなタイプエイリアスとプロトコル内でのタイプエイリアスの使用について情報を追加しました
* [Function Type\(関数型\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/types#types-function-type)セクションの説明を更新し、パラメータ型の周囲に括弧が必須だということを記載しました
* [Attributes\(属性\)](../language-reference/attributes.md)の章を更新して、`@IBAction`、`@IBOutlet`、および `@NSManaged` 属性は `@objc` 属性を暗黙的に含んでいることを記載しました
* [Declaration Attributes\(宣言属性\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/attributes#declaration-attributes)セクションに、`@GKInspectable` 属性についての情報を追加しました
* [Optional Protocol Requirements\(オプショナルのプロトコル要件\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/protocols#optional-protocol-requirements)のオプショナルのプロトコル要件の説明を更新し、Objective-C と相互運用されるコードでのみ使用できることを明確にしました
* [Function Declaration\(関数宣言\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/declarations#function-declaration)セクションから、関数パラメータを使用して明示的に `let` を使用することの説明を削除しました
* [Statement\(文\)](../language-reference/statements.md)の章から `Boolean` プロトコルの説明を削除しました。このプロトコルは Swift 標準ライブラリから削除されました
* [Declaration Attributes\(宣言属性\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/attributes#declaration-attributes)セクションの `@NSApplicationMain` 属性の説明を修正しました

## 2016-03-21

* Swift 2.2 に更新しました
* [Conditional Compilation Block\(条件付きコンパイルブロック\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/statements#conditional-compilation-block)セクションに、Swift バージョンに応じて、コードをコンパイルする方法についての情報を追加しました
* [Explicit Member Expression\(明示的メンバ式\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/expressions#explicit-member-expression)セクションに、引数名だけが異なるメソッドまたはイニシャライザを区別する方法についての情報を追加しました
* [Selector Expression\(Selector 式\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/expressions#selector-expression)セクションに、Objective-C セレクタの `#Selector` 構文についての情報を追加しました
* [Associated Types\(関連型\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/generics#associated-types)と[Protocol Associated Type Declaration\(プロトコル関連型宣言\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/declarations#protocol-associated-type-declaration)セクションの、`associatedtype` キーワードを使った関連型の説明を更新しました
* [Failable Initializers\(失敗可能イニシャライザ\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/initialization#initialization-failable-initializers)セクションの、インスタンスが完全に初期化される前に `nil` を返すイニシャライザについての情報を更新しました
* [Comparison Operators\(比較演算子\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/basic-operators#comparison-operators)セクションに、タプルの比較についての情報を追加しました
* [Keywords and Punctuation\(単語と句読点\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/lexical-structure#keywords-and-punctuation)セクションに、外部パラメータ名にキーワードを使用する方法についての情報を追加しました
* [Declaration Attributes\(宣言属性\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/attributes#declaration-attributes)セクションの `@objc` 属性の説明を更新し、列挙型と列挙ケースでもこの属性を使用できることを記載しました
* [Operators\(演算子\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/lexical-structure#operators)セクションの、ドットを含むカスタム演算子の説明を更新しました
* [Rethrowing Functions and Methods\(再スロー関数と再スローメソッド\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/declarations#rethrowing-functions-and-methods)セクションに、再スロー関数が直接エラーをスローできないことを記載した記載を追加しました
* [Property Observers\(プロパティオブザーバ\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/properties#property-observers)セクションに、in-out パラメータとしてプロパティを渡した際に呼ばれるプロパティオブサーバについての記載を追加しました
* [A Swift Tour\(Swift ツアー\)](https://github.com/stzn/the-swift-programming-language-jp/tree/4a13fdef628bb6cc8c2acf9fc744c981d1891aa0/welcome-to-swift-swift/a-swift-tour.md)の章に、エラー処理についてのセクションを追加しました
* [Weak References\(弱参照\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/automatic-reference-counting#weak-references)セクションに、メモリ割り当て解除プロセスをより明確に表示するために図を更新しました
* C 言語スタイルの `for` ループ、`++` 前置および後置演算子、`--` 前置および後置演算子の説明を削除しました
* 可変関数引数とカリー関数の特別な構文の説明を削除しました

## 2015-10-20

* Swift 2.1 に更新しました
* [String Interpolation\(文字列補間\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/strings-and-characters#string-interpolation)と[String Literals\(文字列リテラル\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/strings-and-characters#strings-and-characters-string-literals)セクションを更新しました。文字列補間に文字列リテラルを含めることができるようになりました
* [Escaping Closures\(エスケープクロージャ\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/closures#escaping-closures)セクションに、`@noescape` 属性についての情報を追加しました
* [Declaration Attributes\(宣言属性\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/attributes#declaration-attributes)セクションと[Conditional Compilation Block\(条件付きコンパイルブロック\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/statements#conditional-compilation-block)セクションの、tvOS についての情報を更新しました
* [In-Out Parameters\(In-Out パラメータ\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/declarations#declarations-in-out-parameters)セクションに、in-out パラメータの動作についての情報を追加しました
* [Capture Lists\(キャプチャリスト\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/expressions#capture-lists)セクションに、キャプチャリストで指定された値がキャプチャされる方法についての情報を追加しました
* [Accessing Properties Through Optional Chaining\(オプショナルチェーンを通したプロパティへのアクセス\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/optional-chaining#accessing-properties-through-optional-chaining)セクションを更新し、オプショナルチェーンを介して代入する方法が明確になるようにしました
* [AutoClosures\(自動クロージャ\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/closures#autoclosures)セクションの autoclosure の説明を改善しました
* [A Swift Tour\(Swift ツアー\)](https://github.com/stzn/the-swift-programming-language-jp/tree/4a13fdef628bb6cc8c2acf9fc744c981d1891aa0/welcome-to-swift-swift/a-swift-tour.md)の章の `??` 演算子を使用する例を追加しました

## 2015-09-16

* Swift 2.0 に更新しました
* [Error Handling\(エラーハンドリング\)](../language-guide/error-handling.md)の章、[Do Statement\(Do 文\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/statements#do-statement)セクション、[Throw Statement\(Throw 文\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/statements#throw-statement)セクション、[Defer Statement\(Defer 文\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/statements#defer-statement)セクション、および[Try Operator\(Try 演算子\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/expressions#try-operator)セクションに、エラー処理についての情報を追加しました
* [Representing and Throwing Errors\(エラーの表現とスロー\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/error-handling#representing-and-throwing-errors)セクションを更新しました。全ての型が `ErrorType` プロトコルに準拠するようになりました
* [Converting Errors to Optional Values\(エラーからオプショナル値への変換\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/error-handling#converting-errors-to-optional-values)セクションに、新しい `try?` キーワードについての情報を追加しました
* [Enumerations\(列挙型\)](../language-guide/enumerations.md)の章の[Recursive Enumerations\(再帰的列挙型\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/enumerations#recursive-enumerations)セクションと、[Declarations\(宣言\)](../language-reference/declarations.md)の章の[Enumerations with Cases of Any Type\(任意の型のケースを持つ列挙型\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/declarations#enumerations-with-cases-of-any-type)セクションに、再帰的な列挙型の情報を追加しました
* [Control Flow\(制御フロー\)](../language-guide/control-flow.md)の章の[Checking API Availability\(API アベイラビリティチェック\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/control-flow#checking-api-availability)セクションと、[Statements\(文\)](../language-reference/statements.md)の章の[Availability Condition\(アベイラビリティ条件\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/statements#availability-condition)セクションに、API アベイラビリティチェックについての情報を追加しました
* [Control Flow\(制御フロー\)](../language-guide/control-flow.md)の章の[Early Exit\(早期リターン\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/control-flow#early-exit)と、[Statements\(文\)](../language-reference/statements.md)の章の[Guard Statement\(Guard 文\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/statements#guard-statement)セクションに、新しい `guard` 文についての情報を追加しました
* [Protocols\(プロトコル\)](../language-guide/protocols.md)の章の[Protocol Extensions\(プロトコル Extension\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/protocols#protocol-extensions)についての情報を追加しました
* [Access Control\(アクセスコントロール\)](../language-guide/access-control.md)の章の[Access Levels for Unit Test Targets\(単体テストターゲットのアクセスレベル\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/access-control#access-levels-for-unit-test-targets)セクションに、単体テストでのアクセス制御についての情報を追加しました
* [Patterns\(パターン\)](../language-reference/patterns.md)の章の[Optional Pattern\(オプショナルパターン\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/patterns#optional-pattern)セクションに、新しいオプショナルパターンについての情報を追加しました
* [Repeat-While](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/control-flow#repeat-while)セクションを更新し、`repeat-while` ループについての情報を追加しました
* [Strings and Characters\(文字列と文字\)](../language-guide/strings-and-characters.md)の章を更新しました。`String` は Swift 標準ライブラリの `CollectionType` プロトコルに準拠しなくなりました
* [Printing Constants and Variables\(定数と変数の出力\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/the-basics#printing-constants-and-variables)セクションに、新しい Swift 標準ライブラリの `print(_:separator:terminator)` 関数についての情報を追加しました
* [Enumerations\(列挙型\)](../language-guide/enumerations.md)の章の[Implicitly Assigned Raw Values\(暗黙的に割り当てられた Raw Value\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/enumerations#implicitly-assigned-raw-values)セクションと、[Declarations\(宣言\)](../language-reference/declarations.md)の章の[Enumerations with Cases of a Raw-Value Type\(Raw Value 型のケースを持つ列挙型\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/declarations#enumerations-with-cases-of-a-raw-value-type)セクションに、`String` 型の Raw Value を持つ列挙ケースの動作についての情報を追加しました
* [AutoClosures\(自動クロージャ\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/closures#autoclosures)セクションに、`@autoclosure` 属性または `@autoclosure(escaping)` についての情報を追加しました
* [Declaration Attributes\(宣言属性\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/attributes#declaration-attributes)セクションを更新し、`@available` および `@warn_unused_result` 属性についての情報を追加しました
* [Type Attributes\(型属性\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/attributes#type-attributes)セクションを更新し、`@convention` 属性についての情報を追加しました
* [Optional Binding\(オプショナルバインディング\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/the-basics#optional-binding)セクションに、`where` 句を使用して複数のオプショナルバインディングを使用する例を追加しました
* [String Literals\(文字列リテラル\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/strings-and-characters#strings-and-characters-string-literals)セクションに、`+` 演算子を使用して文字列リテラルを連結する際のコンパイル時の動作についての情報を追加しました
* [Metatype Type\(Metatype 型\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/types#metatype-type)セクションに、イニシャライザ式を使用してインスタンスを構築する際の Metatype の比較についての情報を追加しました
* Debugging with Assertions\(アサーションを使ったデバッグ\)セクションに、ユーザ定義のアサーションが無効になる場合についての記載を追加しました
* [Declaration Attributes\(宣言属性\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/attributes#declaration-attributes)セクションの `@NSManaged` 属性の説明を更新しました。属性を特定のインスタンスメソッドに適用できるようになりました
* [Variadic Parameters\(可変長パラメータ\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/functions#variadic-parameters)セクションを更新しました。可変長パラメータは、関数のパラメータリスト内の任意の位置で宣言できるようになりました
* [Overriding a Failable Initializer\(失敗可能イニシャライザのオーバーライド\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/initialization#overriding-a-failable-initializer)セクションに、スーパークラスのイニシャライザの結果を強制アンラップすることで、失敗しないイニシャライザから失敗可能イニシャライザに委譲できることについての情報を追加しました
* [Enumerations with Cases of Any Type\(任意の型のケースを持つ列挙型\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/declarations#enumerations-with-cases-of-any-type)セクションに、関数を列挙ケースとして使用できることについての情報を追加しました
* [Initializer Expression\(イニシャライザ式\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/expressions#initializer-expression)セクションに、イニシャライザを明示的に参照する方法についての情報を追加しました
* [Compiler Control Statements\(コンパイラ制御文\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/statements#compiler-control-statements)セクションに、ビルド構成および行制御文についての情報を追加しました
* [Metatype Type\(Metatype 型\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/types#metatype-type)セクションに、Metatype の値からクラスインスタンスを構築する方法についての記載を追加しました
* [Weak References\(弱参照\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/automatic-reference-counting#weak-references)セクションに、弱参照がキャッシュに適していないことについての記載を追加しました
* [Type Properties\(型プロパティ\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/properties#type-properties)セクションの記載を更新し、格納型プロパティが遅延初期化されることを記載しました
* [Capturing Values\(値のキャプチャ\)](../language-guide/closures.md#capturing-values値のキャプチャ)セクションを更新し、変数と定数がクロージャ内でキャプチャされる方法を明確にしました
* [Declaration Attributes\(宣言属性\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/attributes#declaration-attributes)セクションを更新し、クラスに `@objc` 属性を適用できる場合についての説明を追加しました
* [Handling Errors\(エラー処理\)](../language-guide/error-handling.md#handling-errorsエラー処理)セクションに、`throw` 文を実行する際のパフォーマンスについての記載を追加しました。[Do Statement\(Do 文\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/statements#do-statement)のセクションの `do` 文にも同様の情報を追加しました
* [Type Properties\(型プロパティ\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/properties#type-properties)セクションの、クラス、構造体、および列挙型の格納型プロパティおよび計算型プロパティについての情報を更新しました
* [Break Statement\(Break 文\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/statements#break-statement)セクションの、ラベル付き break 文についての情報を更新しました
* `willSet` と `didSet` オブザーバの動作を明確にするために、[Property Observers\(プロパティオブザーバ\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/properties#property-observers)セクションの記載を更新しました
* [Access Levels\(アクセスレベル\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/access-control#access-levels)セクションに、`private` アクセスの範囲についての記載を追加しました
* [Weak References\(弱参照\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/automatic-reference-counting#weak-references)セクションに、ガベージコレクションと ARC の弱参照の違いについての記載を追加しました
* [Special Characters in String Literals\(文字列内の特殊文字\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/strings-and-characters#special-characters-in-string-literals)セクションの Unicode スカラの定義をより正確なものに更新しました

## 2015-04-08

* Swift 1.2 に更新しました
* Swift ネイティブの `Set` コレクション型ができました。詳細については、[Sets\(セット\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/collection-types#sets)を参照ください
* `@autoclosure` は、型ではなく、パラメータ宣言の属性になりました。新しい `@noescape` パラメータ属性もあります。詳細については、[Declaration Attributes\(宣言属性\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/attributes#declaration-attributes)を参照ください
* 型メソッドと型プロパティは、宣言修飾子の `static` キーワードを使用できるようになりました。詳細については、[Type Variable Properties\(型変数プロパティ\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/declarations#type-variable-properties)を参照ください
* Swift に `as?` と `as!` の失敗可能ダウンキャスト演算子が追加されました。詳細については、[Checking for Protocol Conformance\(プロトコル準拠チェック\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/protocols#checking-for-protocol-conformance)を参照ください
* 文字列インデックス\([String Indices\(文字列のインデックス\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/strings-and-characters#string-indices)\)について新しいガイドセクションを追加しました
* [Overflow Operators\(オーバーフロー演算子\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/advanced-operators#overflow-operators)からオーバーフロー分割演算子\(`&/`\)とオーバーフロー剰余演算子\(`&%`\)を削除しました
* 定数および定数プロパティ宣言と初期化のルールを更新しました。詳細については、[Constant Declaration\(定数宣言\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/declarations#constant-declaration)を参照ください
* 文字列リテラル内の Unicode スカラの定義を更新しました。[Special Characters in String Literals\(文字列内の特殊文字\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/strings-and-characters#special-characters-in-string-literals)を参照ください
* [Range Operators\(範囲演算子\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/basic-operators#range-operators)を更新し、開始と終了インデックスが同じ半開範囲は空になることを記載しました
* [Closures Are Reference Types\(クロージャは参照型\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/closures#closures-are-reference-types)を更新し、変数のキャプチャルールを明確にしました
* [Value Overflow\(値のオーバーフロー\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/advanced-operators#value-overflow)を更新し、符号付き整数と符号なし整数のオーバーフローの動作を明確にしました
* [Protocol Declaration\(プロトコル宣言\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/declarations#protocol-declaration)を更新し、プロトコル宣言の範囲とメンバを明確にしました
* [Defining a Capture List\(キャプチャリストの定義\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/automatic-reference-counting#defining-a-capture-list)を更新し、クロージャのキャプチャリストでの weak や unowned 参照の構文を明確にしました
* [Operators\(演算子\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/lexical-structure#operators)セクションを更新し、数学演算子、その他のシンボル、および Dingbats Unicode ブロックなど、カスタム演算子がサポートしている文字の例を明示的に説明しました
* 定数は、ローカル関数スコープで初期化せずに宣言できるようになりました。最初に使用する前に値の設定が必要です。詳細については、[Constant Declaration\(定数宣言\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/declarations#constant-declaration)を参照ください
* イニシャライザ内で、定数プロパティは値を一度だけ割り当てることができるようになりました。詳細については、[Assigning Constant Properties During Initialization\(初期化中の定数プロパティの割り当て\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/initialization#assigning-constant-properties-during-initialization)を参照ください
* 複数のオプショナルバインディングは、代入式のカンマ区切りのリストとして、単一の `if` 文で使用できるようになりました。詳しくは、[Optional Binding\(オプショナルバインディング\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/the-basics#optional-binding)を参照ください
* [Optional-Chaining Expression\(オプショナルチェーン式\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/expressions#optional-chaining-expression)は、後置式として使用しなければなりません
* プロトコルのキャストは、`@objc` プロトコルに制限されなくなりました
* 実行時に型キャストに失敗する可能性がある型は、`as?` または `as!` 演算子を使用し、失敗しないことが保証されている型キャストは、`as` 演算子を使用します。詳細については、[Type-Casting Operators\(型キャスト演算子\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/expressions#type-casting-operators)を参照ください

## 2014-10-16

* Swift 1.1 に更新しました
* 失敗可能イニシャライザ\([Failable Initializers\(失敗可能イニシャライザ\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/initialization#initialization-failable-initializers)\)の全ガイドを追加しました
* プロトコルの失敗可能イニシャライザ要件\([Failable Initializer Requirements\(失敗可能イニシャライザ要件\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/protocols#failable-initializer-requirements)\)の説明を追加しました
* `Any` 型の定数と変数に関数インスタンスを含めることができるようになりました。`switch` 文内での関数型のチェックとキャスト方法を示すために、[Type Casting for Any and AnyObject\(Any および AnyObject の型キャスト\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/type-casting#type-casting-for-any-and-anyobject)の例を更新しました
* Raw Value を持つ列挙型は、`toRaw()` メソッドではなく `rawValue` プロパティ、 `fromRaw()` メソッドではなく `rawValue` パラメータを持つ失敗可能イニシャライザを持つようになりました。 詳しくは、[Raw Values](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/enumerations#raw-values)と[Enumerations with Cases of a Raw-Value Type\(Raw Value 型のケースを持つ列挙型\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/declarations#enumerations-with-cases-of-a-raw-value-type)を参照ください
* 初期化の失敗を引き起こす失敗可能イニシャライザ\([Failable Initializers\(失敗可能イニシャライザ\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/initialization#initialization-failable-initializers)\)についての新しいセクションを追加しました
* カスタム演算子に `?` を含めることができるようになりました。[Operators\(演算子\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/lexical-structure#operators)を更新し、改訂されたルールを記載しました。[Custom Operators\(カスタム演算子\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/advanced-operators#custom-operators)から有効な演算子文字セットの説明の重複を削除しました

## 2014-08-18

* iOS と OS X アプリを構築するための Apple の新しいプログラミング言語 Swift 1.0 についての新しい文書
* プロトコル内の[Initializer Requirements\(イニシャライザ要件\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/protocols#initializer-requirements)についての新しいセクションを追加しました
* [Class-Only Protocols\(クラス専用プロトコル\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/protocols#class-only-protocols)についての新しいセクションを追加しました
* [Assertions and Preconditions\(アサーションと事前条件\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/the-basics#assertions-and-preconditions)は、文字列補間を使用できるようになりました。使用できない記載を削除しました
* [Concatenating Strings and Characters\(文字と文字列の連結\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/strings-and-characters#concatenating-strings-and-characterstono)セクションを更新し、`String` と `Character` を加算演算子\(`+`\)または加算代入演算子\(`+=`\)で組み合わせることができないことを反映しました。これらの演算子は `String` でのみ使用されます。`String` の末尾に単一の `Character` を追加するには、`String` 型の `append(_:)` メソッドを使用します
* [Declaration Attributes\(宣言属性\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/attributes#declaration-attributes)セクションに、`availability` 属性についての情報を追加しました
* [Optionals\(オプショナル\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/the-basics#optionals)は、オプショナルの `Bool` 値を使用しているときに、混乱を避けるために、値がある場合に `true`、ない場合に `false` と評価されません。代わりに、`==` または `!=` 演算子を指定して、オプショナルに値が含まれているかどうか、明示的な `nil` チェックを行いましょう
* [Nil-Coalescing Operator\(nil 合体演算子\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/basic-operators#nilcoalescing-operator)\(`a ?? b`\)ができました。これは、オプショナルの値が存在する場合はオプショナルの値をアンラップし、`nil` の場合はデフォルト値を返します
* [Comparing Strings\(文字列の比較\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/strings-and-characters#comparing-strings)セクションを更新、拡張し、文字列と文字の比較や、前方比較、後方比較は、拡張書記素クラスタの Unicode の正規等価性に基づいていることを反映し、例を示しました
* [Optional Chaining\(オプショナルチェーン\)](../language-guide/optional-chaining.md)を使って、mutating メソッドや演算子の呼び出し、subscript への値の割り当て、プロパティ値の設定を試すことができます。[Accessing Properties Through Optional Chaining\(オプショナルチェーンを通したプロパティへのアクセス\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/optional-chaining#accessing-properties-through-optional-chaining)の情報は、これに応じて更新され、[Calling Methods Through Optional Chaining\(オプショナルチェーンを通したメソッドの呼び出し\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/optional-chaining#calling-methods-through-optional-chaining)のメソッド呼び出しが成功したかどうかの例では、プロパティの設定が成功したかどうかをチェックする方法も例として示すように拡張しました
* オプショナルチェーンを通じて、オプショナルの subscript にアクセスするための新しいセクション\([Accessing Subscripts of Optional Type\(オプショナル型の subscript へのアクセス\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/optional-chaining#accessing-subscripts-of-optional-type)\)を追加しました
* [Accessing and Modifying an Array\(Array 配列へのアクセスと変更\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/collection-types#accessing-and-modifying-an-array)を更新し、`+=` 演算子を使って配列にその単一の要素を追加できなくなりました。代わりに、`append(_:)` メソッドを使用するか、単一の要素を持つ配列に `+=` 演算子を使用して追加します
* 範囲演算子\([Range Operators\(範囲演算子\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/basic-operators#range-operators)\) `a...b` と `a..<b` の開始値 `a` を、終了値 `b` より大きくしてはならないことを記載しました
* [Inheritance\(継承\)](../language-guide/inheritance.md)の章を書き直して、イニシャライザのオーバーライドの紹介部分を削除しました。この章では、サブクラス内に新しい機能を追加し、オーバーライドを使用して既存の機能を変更することに焦点を当てています。[Overriding Property Getters and Setters\(プロパティの get プロパティ、set プロパティドのオーバーライド\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/inheritance#overriding-property-getters-and-setters)の章の例は、`description` プロパティをオーバーライドする方法を示すために書き換えられました。\(サブクラスのイニシャライザで継承したプロパティのデフォルト値を変更する例は、[Initialization\(イニシャライザ\)](../language-guide/initialization.md)の章に移動しました\)
* [Initializer Inheritance and Overriding\(イニシャライザの継承とオーバーライド\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/initialization#initializer-inheritance-and-overriding)セクションを更新し、指定イニシャライザのオーバーライドを `override` 修飾子でマークする必要があることを記載しました
* [Required Initializers\(必須イニシャライザ\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/initialization#required-initializers)セクションを更新し、`required` 修飾子が全てのサブクラスの必須イニシャライザの実装の前に書かれ、自動的に継承したイニシャライザが必須イニシャライザの要件を満たすことができることを記載しました
* 中置演算子メソッド\([Operator Methods\(演算子メソッド\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/advanced-operators#advanced-operators-operator-methods)\)に `@infix` 属性が必要なくなりました
* [Prefix and Postfix Operators\(前置、後置演算子\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/advanced-operators#prefix-and-postfix-operators)の `@prefix` および `@postfix` 演算子属性は、`prefix` および  `postfix` 宣言修飾子に置き換えられました
* [Prefix and Postfix Operators\(前置、後置演算子\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/advanced-operators#prefix-and-postfix-operators)が同じオペランドに適用されたときに `prefix` と `postfix` 演算子が適用される順序についての記載を追加しました
* [Compound Assignment Operators\(複合代入演算子\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/advanced-operators#advanced-operators-compound-assignment-operators)の演算子関数は、関数を定義するときに `@assignment` 属性を使用しなくなりました
* [Custom Operators\(カスタム演算子\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/advanced-operators#custom-operators)を定義するときに修飾子を指定する順序が変更されました。例えば、`operator prefix` ではなく `prefix operator` と書くようになりました
* [Declaration Modifiers\(宣言修飾子\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/declarations#declaration-modifiers)に、`dynamic` 修飾子についての情報を追加しました
* 型推論が[Literals\(リテラル\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/lexical-structure#literals)で動作する方法についての情報を追加しました
* カリー関数のより詳しい情報を追加しました
* [Access Control\(アクセスコントロール\)](../language-guide/access-control.md)についての新しい章を追加しました
* [Strings and Characters\(文字列と文字\)](../language-guide/strings-and-characters.md)の章を更新し、Swift の `Character` 型が単一の Unicode 拡張書記素クラスタを表すことを反映しました。[Extended Grapheme Clusters\(拡張書記素クラスタ\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/strings-and-characters#extended-grapheme-clusters)上の新しいセクションと、[Unicode Scalar Values\(Unicode スカラ値\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/strings-and-characters#unicode-scalar-values)と[Comparing Strings\(文字列の比較\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/strings-and-characters#comparing-strings)についての詳しい情報も含まれています
* [String Literals\(文字列リテラル\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/strings-and-characters#strings-and-characters-string-literals)セクションを更新し、文字列内の Unicode スカラを `\u{n}`\(n には 0 から 10FFFF の間の 16 進数が入る\)で書けることを記載しました
* `NSString` の `length` プロパティは、`utf16count` ではなく、Swift のネイティブの `String` 型の `utf16count` にマッピングされるようになりました
* Swift のネイティブ `String` 型は、`uppercaseString` または `lowercaseString` プロパティを使用しなくなりました。[Strings and Characters\(文字列と文字\)](../language-guide/strings-and-characters.md)の対応するセクションは削除され、様々なコード例を更新しました
* [Initializer Parameters Without Argument Labels\(引数ラベルのないイニシャライザパラメータ\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/initialization#initializer-parameters-without-argument-labels)についての新しいセクションを追加しました
* [Required Initializers\(必須イニシャライザ\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/initialization#required-initializers)についての新しいセクションを追加しました
* [Optional Tuple Return Types\(オプショナルのタプルの戻り値の型\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/functions#optional-tuple-return-types)についての新しいセクションを追加しました
* [Type Annotations\(型注釈\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/the-basics#type-annotations)セクションを更新し、1 つの型注釈を持つ単一の行に複数の関連変数を定義できることを記載しました
* `@optional`、`@lazy`、`@final` と `@required` 属性は、`optional`、`lazy`、`final` と `required` 宣言修飾子\([Declaration Modifiers\(宣言修飾子\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/declarations#declaration-modifiers)\)になりました
* 本全体を参照して、`..<` を\(「半閉鎖範囲演算子」ではなく\)[Half-Open Range Operator\(半開範囲演算子\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/basic-operators#halfopen-range-operator)として参照するように更新しました
* [Accessing and Modifying a Dictionary\(辞書へのアクセスと変更\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/collection-types#accessing-and-modifying-a-dictionary)を更新し、`Dictionary` がブール値の `isEmpty` プロパティを持つことを記載しました
* [Custom Operators\(カスタム演算子\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/advanced-operators#custom-operators)を定義するときに使用できる文字の全リストを明確にしました
* `nil` とブール値 `true` と `false` は[Literals\(リテラル\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-reference/lexical-structure#literals)になりました
* Swift の `Array` 型は、完全に値型のセマンティクスになりました。[Mutability of Collections\(コレクションの可変性\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/collection-types#mutability-of-collections)と[Arrays\(配列\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/collection-types#arrays)を更新し、新しいアプローチを反映しました
* [Array Type Shorthand Syntax\(配列型の省略構文\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/collection-types#array-type-shorthand-syntax)は `SomeType[]` ではなく `[SomeType]` として書くようになりました
* [Dictionary Type Shorthand Syntax\(辞書型の省略構文\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/collection-types#dictionary-type-shorthand-syntax)は、`[KeyType: ValueType]` と書くように新しいセクションを追加しました
* [Hash Values for Set Types\(セット型のハッシュ値\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/collection-types#hash-values-for-set-types)についての新しいセクションを追加しました
* [Closure Expressions\(クロージャ式\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/closures#closure-expressions)の例に、グローバルの `sort(_:_:)` 関数ではなくグローバルの `sorted(_:_:)` 関数を使用し、配列の新しい値型のセマンティクスを反映しています
* [Memberwise Initializers for Structure Types\(構造体のメンバワイズイニシャライザ\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/initialization#initialization-memberwise-initializers-for-structure-types)についての情報を更新し、構造体の格納プロパティにデフォルト値がない場合でもメンバワイズイニシャライザが使用可能だということを明確にしました
* [Half-Open Range Operator\(半開範囲演算子\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/basic-operators#halfopen-range-operator)に `..` ではなく `..<` を使用するようになりました
* [Extending a Generic Type\(ジェネリック型の拡張\)](https://swift-programming-language-jp.gitbook.io/the-swift-programming-language-jp/language-guide/generics#extending-a-generic-type)の例を追加しました

