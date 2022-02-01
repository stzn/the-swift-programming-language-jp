# ドキュメント改訂履歴\(Document Revision History\)

最終更新日: 2022/1/31

## 2022-01-27

* Swift 5.6 に更新しました
* [明示的メンバ式\(Explicit Member Expression\)](../language-reference/expressions.md#explicit-member-expression)セクションにメソッドチェーンや他の後置式の周りで `#if` が使えることについての情報を追加しました
* 全体的な数字の視覚的スタイリングを更新しました

## 2021-09-20

* Swift 5.5 に更新しました
* 非同期関数、Task、およびアクターについての情報を[同時並行処理\(Concurrency\)](../language-guide/concurrency.md)の章、および[アクター宣言\(Actor Declaration\)](../language-reference/declarations.md#actor-declaration)、[非同期関数と非同期メソッド\(Asynchronous Functions and Methods\)](../language-reference/declarations.md#asynchronous-functions-and-asynchronous-methods)、および[Await 演算子\(Await Operator\)](../language-reference/expressions.md#await-operator)セクションに追加しました
* [識別子\(Identifiers\)](../language-reference/lexical-structure.md#identifiers)セクションに、アンダースコアで始まる識別子についての情報を追加しました

## 2021-04-26

* Swift 5.4 に更新しました
* [リザルトビルダ\(Result Builders\)](../language-guide/advanced-operators.md#result-builders)と[resultBuilder](../language-reference/attributes.md#resultbuilder)セクションに、リザルトビルダについての情報を追加しました
* [ポインタ型への暗黙変換\(Implicit Conversion to a Pointer Type\)](../language-reference/expressions.md#implicit-conversion-to-a-pointer-type)セクションに、関数呼び出しの中で in-out パラメータを unsafe pointer に暗黙的に変換する方法についての情報を追加しました
* [可変長パラメータ\(Variadic Parameters\)](../language-guide/functions.md#variadic-parameters)と[関数宣言\(Function Declaration\)](../language-reference/declarations.md#function-declaration)セクションを更新しました。関数は複数の可変長パラメータを受け取ることができるようになりました
* [暗黙メンバ式\(Implicit Member Expression\)](../language-reference/expressions.md#implicit-member-expression)セクションを更新しました。暗黙メンバ式を繋げて使えるようになりました

## 2020-09-16

* Swift 5.3 に更新しました
* [末尾クロージャ\(Trailing Closures\)](../language-guide/closures.md#trailing-closures)セクションに、複数の末尾クロージャについての情報を追加し、[関数呼び出し式\(Function Call Expression\)](../language-reference/expressions.md#function-call-expression)セクションに、複数の末尾クロージャをパラメータに合わせる方法についての情報を追加しました
* [デフォルト実装を使用したプロトコル準拠\(Adopting a Protocol Using a Synthesized Implementation\)](../language-guide/protocols.md#adopting-a-protocol-using-a-synthesized-implementation)セクションに、列挙型が `Comparable` プロトコルに準拠するための合成実装についての情報を追加しました
* [コンテキスト上の where 句\(Contextual Where Clauses\)](../language-guide/generics.md#contextual-where-clauses)セクションに、ジェネリック `where` 句をより多くの場所に記載できる旨を追加しました
* [オプショナル値への非所有参照\(Unowned Optional References\)](../language-guide/automatic-reference-counting.md#unowned-optional-references)セクションに、オプショナル値を `unowned` 参照に使用する方法についての情報を追加しました
* [main](../language-reference/attributes.md#main)セクションに `@main` 属性についての情報を追加しました
* [リテラル式\(Literal Expression\)](../language-reference/expressions.md#literal-expression)セクションに、`#filePath` を追加し、`#file` の記載を更新しました
* [エスケープクロージャ\(Escaping Closures\)](../language-guide/closures.md#escaping-closures)セクションを更新しました。多くのシナリオでクロージャが暗黙的に `self` を参照できるようになりました
* [do catch を使ったエラー処理\(Handling Errors Using Do-Catch\)](../language-guide/error-handling.md#handling-errors-using-do-catch)と[Do 文\(Do Statement\)](../language-reference/statements.md#do-statement)のセクションを更新しました。`catch` 句が複数のエラーにマッチングできるようになりました
* `Any` の詳細を追加し、[Any 型\(Any Type\)](../language-reference/types.md#any-type)セクションに移動しました
* [プロパティオブザーバ\(Property Observers\)](../language-guide/properties.md#property-observers)セクションを更新しました、`lazy` プロパティでもオブザーバが使用できるようになりました
* [プロトコル宣言\(Protocol Declaration\)](../language-reference/declarations.md#protocol-declaration)セクションを更新しました。列挙型のメンバがプロトコル要件を満たすことができるようになりました
* [格納変数オブザーバとプロパティオブザーバ\(Stored Variable Observers and Property Observers\)](../language-reference/declarations.md#stored-variable-observers-and-property-observers)セクションを更新して、get がいつオブザーバの前に呼び出されるかついて記述しました
* [メモリ安全性\(Memory Safety\)](../language-guide/memory-safety.md)の章を更新して、アトミック操作について記述しました

## 2020-03-24

* Swift 5.2 に更新しました
* [Key-Path 式\(Key-Path Expression\)](../language-reference/expressions.md#keypath-expression)セクションに、クロージャの代わりにキーパスを渡すことについての情報を追加しました
* [特別な名前のメソッド\(Methods with Special Names\)](../language-reference/declarations.md#methods-with-special-names)セクションに、関数呼び出し構文にクラス、構造体、および列挙型のインスタンスを使用できるようにする糖衣構文\(シンタックスシュガー\)についての情報を追加しました
* [様々な subscript\(Subscript Options\)](../language-guide/subscripts.md#subscript-options)セクションを更新しました。subscript でデフォルトパラメータを使用できるようになりました
* [Self 型\(Self Type\)](../language-reference/types.md#self-type)セクションを更新しました。より多くのコンテキストで `Self` が使えるようになりました
* [暗黙アンラップオプショナル\(Implicitly Unwrapped Optionals\)](../language-guide/the-basics.md#implicitly-unwrapped-optionals)セクションを更新して、暗黙アンラップオプショナル値をオプショナルまたは非オプショナルのどちらでも使用できることをより明確にしました

## 2019-09-10

* Swift 5.1 に更新しました
* [Opaque 型\(Opaque Types\)](../language-guide/opaque-types.md)の章に、戻り値に特定の名前の戻り値の型を提供するのではなく、戻り値が準拠するプロトコルを指定する関数についての情報を追加しました
* [プロパティラッパ\(Property Wrappers\)](../language-guide/properties.md#property-wrappers)セクションに、プロパティラッパについての情報を追加しました
* [frozen](../language-reference/attributes.md#frozen)セクションに、ライブラリエボリューションのための frozen な列挙型と構造体についての情報を追加しました
* [暗黙的な戻り値がある関数\(Functions With an Implicit Return\)](../language-guide/functions.md#functions-with-an-implicit-return)と[省略 get プロパティ宣言\(Shorthand Getter Declaration\)](../language-guide/properties.md#shorthand-getter-declaration)セクションに、`return` を省略した関数についての情報を追加しました
* [型 subscript\(Type Subscripts\)](../language-guide/subscripts.md#type-subscripts)セクションに、型への subscript の使用について情報を追加しました
* [列挙型ケースパターン\(Enumeration Case Pattern\)](../language-reference/patterns.md#enumeration-case-pattern)セクションを更新しました。列挙型ケースとオプショナル値をマッチングできるようになりました
* [構造体のメンバワイズイニシャライザ\(Memberwise Initializers for Structure Types\)](../language-guide/structures-and-classes.md#structures-and-classes-memberwise-initializers-for-structure-types)セクションを更新しました。メンバワイズイニシャライザがデフォルト値を持つプロパティのパラメータを省略することができるようになりました
* [dynamicMemberLookup](../language-reference/attributes.md#dynamicmemberlookup)セクションに、実行時にキーパスで検索された動的メンバについての情報を追加しました
* [条件付きコンパイルブロック\(Conditional Compilation Block\)](../language-reference/statements.md#conditional-compilation-block)のターゲット環境リストに `macCatalyst` を追加しました
* [Self 型\(Self Type\)](../language-reference/types.md#self-type)セクションを更新しました。`Self` を使用して、現在のクラス、構造体、または列挙型宣言で導入された型を参照することができるようになりました

## 2019-03-25

* Swift 5.0 に更新しました
* [拡張区切り文字\(Extended String Delimiters\)](../language-guide/strings-and-characters.md#extended-string-delimiters)セクションを追加し、拡張区切り文字についての情報を[文字列リテラル\(String Literals\)](../language-reference/lexical-structure.md#lexical-structure-string-literals)セクションに追加しました
* `dynamicCallable` 属性を使用してインスタンスを関数として動的に呼び出すことについての情報を記載した[dynamicCallable](../language-reference/attributes.md#dynamiccallable)セクションを追加しました
* `unknown` スイッチケース属性を使用した `switch` 文の将来の列挙ケースを扱う方法についての情報を記載した[unknown](../language-reference/attributes.md#unknown)と[列挙型の将来のケースのスイッチング\(Switching Over Future Enumeration Cases\)](../language-reference/statements.md#switching-over-future-enumeration-cases)セクションを追加しました
* [Key-Path 式\(Key-Path Expression\)](../language-reference/expressions.md#keypath-expression)セクションに、識別キーパス\(`\.self`\)についての情報を追加しました
* [条件付きコンパイルブロック\(Conditional Compilation Block\)](../language-reference/statements.md#conditional-compilation-block)のプラットフォーム条件に\(`<`\)演算子の使用方法についての情報を追加しました

## 2018-09-17

* Swift 4.2 に更新しました
* [列挙ケースの繰り返し処理\(Iterating over Enumeration Cases\)](../language-guide/enumerations.md#iterating-over-enumeration-cases)セクションに、列挙型の全てのケースへのアクセス方法についての情報を追加しました
* [コンパイル時診断文\(Compile-Time Diagnostic Statement\)](../language-reference/statements.md#compile-time-diagnostic-statement)セクションに、`#error` と `#warning` についての情報を追加しました
* [宣言属性\(Declaration Attributes\)](../language-reference/attributes.md#declaration-attributes)セクションの `inlinable` と `usableFromInline` 属性に、インライン化についての情報を追加しました
* [宣言属性\(Declaration Attributes\)](../language-reference/attributes.md#declaration-attributes)セクションの `dynamicMemberLookup` 属性に、実行時に名前で検索するメンバについての情報を追加しました
* [宣言属性\(Declaration Attributes\)](../language-reference/attributes.md#declaration-attributes)セクションに、`require_stored_property_inits` および `warn_unqualified_access` 属性についての情報を追加しました
* [条件付きコンパイルブロック\(Conditional Compilation Block\)](../language-reference/statements.md#conditional-compilation-block)セクションに、Swift コンパイラのバージョンに応じて、条件付きでコンパイルする方法についての情報を追加しました
* [リテラル式\(Literal Expression\)](../language-reference/expressions.md#literal-expression)セクションに、`#dsohandle` についての情報を追加しました

## 2018-03-29

* Swift 4.1 に更新しました
* [等価演算子\(Equivalence Operators\)](../language-guide/advanced-operators.md#equivalence-operators)セクションに、等価演算子の合成実装についての情報を追加しました
* [宣言\(Declarations\)](../language-reference/declarations.md)の章の[拡張宣言\(Extension Declaration\)](../language-reference/declarations.md#extension-declaration)セクションと、[プロトコル\(Protocols\)](../language-guide/protocols.md)の章の[条件付きでのプロトコルへの準拠\(Conditionally Conforming to a Protocol\)](../language-guide/protocols.md#conditionally-conforming-to-a-protocol)セクションに、条件付きプロトコル準拠についての情報を追加しました
* [関連型の制約へのプロトコルの使用\(Using a Protocol in Its Associated Type’s Constraints\)](../language-guide/generics.md#using-a-protocol-in-its-associated-types-constraintsnohenopurotokoruno)セクションに、再帰的プロトコル制約についての情報を追加しました
* [条件付きコンパイルブロック\(Conditional Compilation Block\)](../language-reference/statements.md#conditional-compilation-block)セクションに、`canImport()` と `targetEnvironment()` プラットフォーム条件についての情報を追加しました

## 2017-12-04

* Swift 4.0.3 に更新しました
* [Key-Path 式\(Key-Path Expression\)](../language-reference/expressions.md#keypath-expression)セクションを更新しました。subscript にキーパスを使用できるようになりました

## 2017-09-19

* Swift 4.0 に更新しました
* [メモリ安全性\(Memory Safety\)](../language-guide/memory-safety.md)の章に、メモリへの排他アクセスについての情報を追加しました
* [ジェネリック where 句を使用した関連型\(Associated Types with a Generic Where Clause\)](../language-guide/generics.md#associated-types-with-a-generic-where-clause)セクションを追加しました。ジェネリック `where` 句で関連型を制約できるようになりました
* [文字列と文字\(Strings and Characters\)](../language-guide/strings-and-characters.md)の章の[文字列リテラル\(String Literals\)](../language-guide/strings-and-characters.md#strings-and-characters-string-literals)セクションと、[字句構造\(Lexical Structure\)](../language-reference/lexical-structure.md)の章の[文字列リテラル\(String Literals\)](../language-reference/lexical-structure.md#lexical-structure-string-literals)セクションに、複数行文字列リテラルについての情報を追加しました
* [宣言属性\(Declaration Attributes\)](../language-reference/attributes.md#declaration-attributes)セクションの `objc` 属性の説明を更新しました。この属性が推論される場所がより少なくなりました
* [ジェネリック subscript\(Generic Subscripts\)](../language-guide/generics.md#generic-subscripts)セクションを追加しました。subscript はジェネリックにできます
* [プロトコル\(Protocols\)](../language-guide/protocols.md)の章の[プロトコル合成\(Protocol Composition\)](../language-guide/protocols.md#protocol-composition)セクションと、[型\(Types\)](../language-reference/types.md)の章の[プロトコル合成型\(Protocol Composition Type\)](../language-reference/types.md#protocol-composition-type)セクションの説明を更新しました。プロトコル合成型にスーパークラスの要件を含めることができるようになりました
* [拡張宣言\(Extension Declaration\)](../language-reference/declarations.md#extension-declaration)のプロトコル拡張の説明を更新しました。`final` は使用できなくなりました
* [アサーションと事前条件\(Assertions and Preconditions\)](../language-guide/the-basics.md#assertions-and-preconditions)セクションに、前提条件と致命的エラーについての情報を追加しました

## 2017-03-27

* Swift 3.1 に更新しました
* [ジェネリック where 句を使った拡張\(Extensions with a Generic Where Clause\)](../language-guide/generics.md#extensions-with-a-generic-where-clause)セクションに、要件を含む extension についての情報を追加しました
* [For-In ループ\(For-In Loops\)](../language-guide/control-flow.md#for-in-loops)セクションに範囲全体を繰り返す例を追加しました
* [失敗可能イニシャライザ\(Failable Initializers\)](../language-guide/initialization.md#initialization-failable-initializers)セクションに、失敗可能な数値変換の例を追加しました
* [宣言属性\(Declaration Attributes\)](../language-reference/attributes.md#declaration-attributes)セクションに、Swift 言語バージョンを使用した `available` 属性の使い方についての情報を追加しました
* [関数型\(Function Type\)](../language-reference/types.md#types-function-type)セクションの説明を更新し、関数型に引数ラベルを書くことができないことを記載しました
* [条件付きコンパイルブロック\(Conditional Compilation Block\)](../language-reference/statements.md#conditional-compilation-block)セクションの Swift 言語バージョン番号の説明を更新しました。オプショナルのパッチ番号が使用できるようになりました
* [関数型\(Function Type\)](../language-reference/types.md#types-function-type)セクションの説明を更新し、Swift は、タプル型の単一のパラメータを受け取る関数と、複数のパラメータを受け取る関数が区別できるようになりました
* [式\(Expressions\)](../language-reference/expressions.md)の章から動的型式のセクションを削除しました。`type(of:)` は Swift 標準ライブラリ関数になりました

## 2016-10-27

* Swift 3.0.1 に更新しました
* [自動参照カウント\(Automatic Reference Counting\)](../language-guide/automatic-reference-counting.md)の章の弱参照、非所有参照の説明を更新しました
* [宣言修飾子\(Declaration Modifiers\)](../language-reference/declarations.md#declaration-modifiers)セクションに、`unowned`、`unowned(safe)` と `unowned(unsafe)` 修飾子についての情報を追加しました
* [Any および AnyObject の型キャスト\(Type Casting for Any and AnyObject\)](../language-guide/type-casting.md#type-casting-for-any-and-anyobject)セクションに、Any 型の値が期待される場所にオプショナルの値を使用することについての記載を追加しました
* 括弧付き式とタプル式の説明を分けるように、[Expressions\(式\)](../language-reference/expressions.md)の章を更新しました

## 2016-09-13

* Swift 3.0 に更新しました
* [関数\(Functions\)](../language-guide/functions.md)の章と[関数宣言\(Function Declaration\)](../language-reference/declarations.md#function-declaration)セクションの関数の説明を、デフォルトで全てのパラメータが引数ラベルを持つように更新しました
* [高度な演算子\(Advanced Operators\)](../language-guide/advanced-operators.md)の章の演算子の説明を更新しました。これらの演算子をグローバル関数としてはなく型メソッドとして実装できるようになりました
* [アクセスコントロール\(Access Control\)](../language-guide/access-control.md)の章に、`open` と `filePrivate` アクセスレベル修飾子についての情報を追加しました
* [関数宣言\(Function Declaration\)](../language-reference/declarations.md#function-declaration)セクションの `inout` の説明を、パラメータ名の前ではなくパラメータの型の前に使用するように更新しました
* [エスケープクロージャ\(Escaping Closures\)](../language-guide/closures.md#escaping-closures)と[自動クロージャ\(AutoClosures\)](../language-guide/closures.md#autoclosures)セクションと、[属性\(Attributes\)](../language-reference/attributes.md)の章の説明で、`@noescape` および `@autoclosures` 属性を宣言属性から型属性へ更新しました
* [高度な演算子\(Advanced Operators\)](../language-guide/advanced-operators.md)の章の[Precedence for Custom Infix Operators\(カスタム中置演算子の優先順位\)](../language-guide/advanced-operators.md#precedence-for-custom-infix-operators)セクションと、[宣言\(Declaration\)](../language-reference/declarations.md)の章の[優先順位グループ宣言\(Precedence Group Declaration\)](../language-guide/advanced-operators.md#precedence-for-custom-infix-operators)セクションに、演算子の優先順位グループについての情報を追加しました
* OS X の変わりに macOS、`ErrorProtocol` の代わりに `Error`、`StringLiteralConvertible` の代わりに `ExpressibleByStringLiteral` などのプロトコル名を使用するように説明を更新しました
* [ジェネリクス\(Generics\)](../language-guide/generics.md)の章の[ジェネリック where 句\(Generic Where Clauses\)](../language-guide/generics.md#generic-where-clauses)セクションと、[ジェネリックパラメータと引数\(Generic Parameters and Arguments\)](../language-reference/generic-parameters-and-arguments.md)の章の説明を更新しました。ジェネリック `where` 句を宣言の最後に書くことができるようになりました
* [エスケープクロージャ\(Escaping Closures\)](../language-guide/closures.md#escaping-closures)セクションの説明を更新しました。クロージャはデフォルトが非エスケープになりました
* [基本\(The Basics\)](../language-guide/the-basics.md)の章の[オプショナルバインディング\(Optional Binding\)](../language-guide/the-basics.md#optional-binding)セクションと、[文\(Statement\)](../language-reference/statements.md)の章の[While 文\(While Statement\)](../language-reference/statements.md#while-statement)セクションの説明を更新しました。`if`、`while`、`guard` 文は、`where` 句なしで、カンマ区切りリストの条件を使用できるようになりました
* [制御フロー\(Control Flow\)](../language-guide/control-flow.md)の章の[Switch](../language-guide/control-flow.md#switch)セクションと、[文\(Statement\)](../language-reference/statements.md)の章の[Switch 文\(Switch Statement\)](../language-reference/statements.md#switch-statement)セクションに、複数のパターンを持つ列挙ケースについての情報を追加しました
* [関数型\(Function Type\)](../language-reference/types.md#types-function-type)セクションの関数型の説明を更新しました。引数ラベルが関数の型の一部ではなくなりました
* [プロトコル\(Protocols\)](../language-guide/protocols.md)の章の[プロトコル合成\(Protocol Composition\)](../language-guide/protocols.md#protocol-composition)セクションと、[型\(Types\)](../language-reference/types.md)の章の[プロトコル合成型\(Protocol Composition Type\)](../language-reference/types.md#protocol-composition-type)セクションの説明を更新し、新しい `Protocol1 & Protocol2` 構文を使用するようになりました
* 動的型式セクションで、新しい `type(of:)` を使用するように説明を更新しました
* [行制御文\(Line Control Statement\)](../language-reference/statements.md#line-control-statement)セクションで、`#sourceLocation(file:line:)` 構文を使用するように、行制御文の説明を更新しました
* [ノーリターン関数\(Functions that Never Return\)](../language-reference/declarations.md#functions-that-never-return)の説明を更新し、新しい `Never` 型を使用するようにしました
* [リテラル式\(Literal Expression\)](../language-reference/expressions.md#literal-expression)セクションに、playground リテラルについての情報を追加しました
* [In-Out パラメータ\(In-Out Parameters\)](../language-reference/declarations.md#declarations-in-out-parameters)セクションの説明を更新し、非エスケープクロージャのみが in-out パラメータをキャプチャできることを記載しました
* [デフォルトパラメータ値\(Default Parameter Values\)](../language-guide/functions.md#default-parameter-values)セクションのデフォルトパラメータの説明を更新しました。関数呼び出し時にデフォルトパラメータを並べ替えることができなくなりました
* [属性\(Attributes\)](../language-reference/attributes.md)の章でコロンを使用するように属性引数を更新しました
* [再スロー関数と再スローメソッド\(Rethrowing Functions and Methods\)](../language-reference/declarations.md#rethrowing-functions-and-methods)セクションに、再スロー関数のキャッチブロック内でエラーをスローすることについての情報を追加しました
* [Selector 式\(Selector Expression\)](../language-reference/expressions.md#selector-expression)セクションに、Objective-C プロパティの get または set のセレクタへのアクセスについての情報を追加しました
* [タイプエイリアス宣言\(Type Alias Declaration\)](../language-reference/declarations.md#type-alias-declaration)セクションに、ジェネリックなタイプエイリアスとプロトコル内でのタイプエイリアスの使用について情報を追加しました
* [関数型\(Function Type\)](../language-reference/types.md#types-function-type)セクションの説明を更新し、パラメータ型の周囲に括弧が必須だということを記載しました
* [属性\(Attributes\)](../language-reference/attributes.md)の章を更新して、`@IBAction`、`@IBOutlet`、および `@NSManaged` 属性は `@objc` 属性を暗黙的に含んでいることを記載しました
* [宣言属性\(Declaration Attributes\)](../language-reference/attributes.md#declaration-attributes)セクションに、`@GKInspectable` 属性についての情報を追加しました
* [オプショナルのプロトコル要件\(Optional Protocol Requirements\)](../language-guide/protocols.md#optional-protocol-requirements)のオプショナルのプロトコル要件の説明を更新し、Objective-C と相互運用されるコードでのみ使用できることを明確にしました
* [関数宣言\(Function Declaration\)](../language-reference/declarations.md#function-declaration)セクションから、関数パラメータを使用して明示的に `let` を使用することの説明を削除しました
* [文\(Statement\)](../language-reference/statements.md)の章から `Boolean` プロトコルの説明を削除しました。このプロトコルは Swift 標準ライブラリから削除されました
* [宣言属性\(Declaration Attributes\)](../language-reference/attributes.md#declaration-attributes)セクションの `@NSApplicationMain` 属性の説明を修正しました

## 2016-03-21

* Swift 2.2 に更新しました
* [条件付きコンパイルブロック\(Conditional Compilation Block\)](../language-reference/statements.md#conditional-compilation-block)セクションに、Swift バージョンに応じて、コードをコンパイルする方法についての情報を追加しました
* [明示的メンバ式\(Explicit Member Expression\)](../language-reference/expressions.md#explicit-member-expression)セクションに、引数名だけが異なるメソッドまたはイニシャライザを区別する方法についての情報を追加しました
* [Selector 式\(Selector Expression\)](../language-reference/expressions.md#selector-expression)セクションに、Objective-C セレクタの `#Selector` 構文についての情報を追加しました
* [関連型\(Associated Types\)](../language-guide/generics.md#associated-types)と[プロトコル関連型宣言\(Protocol Associated Type Declaration\)](../language-reference/declarations.md#protocol-associated-type-declaration)セクションの、`associatedtype` キーワードを使った関連型の説明を更新しました
* [失敗可能イニシャライザ\(Failable Initializers\)](../language-guide/initialization.md#initialization-failable-initializers)セクションの、インスタンスが完全に初期化される前に `nil` を返すイニシャライザについての情報を更新しました
* [Comparison Operators\(比較演算子\)](../language-guide/basic-operators.md#comparison-operators)セクションに、タプルの比較についての情報を追加しました
* [Keywords and Punctuation\(単語と句読点\)](../language-reference/lexical-structure.md#keywords-and-punctuation)セクションに、外部パラメータ名にキーワードを使用する方法についての情報を追加しました
* [宣言属性\(Declaration Attributes\)](../language-reference/attributes.md#declaration-attributes)セクションの `@objc` 属性の説明を更新し、列挙型と列挙ケースでもこの属性を使用できることを記載しました
* [演算子\(Operators\)](../language-reference/lexical-structure.md#operators)セクションの、ドットを含むカスタム演算子の説明を更新しました
* [再スロー関数と再スローメソッド\(Rethrowing Functions and Methods\)](../language-reference/declarations.md#rethrowing-functions-and-methods)セクションに、再スロー関数が直接エラーをスローできないことを記載した記載を追加しました
* [プロパティオブザーバ\(Property Observers\)](../language-guide/properties.md#property-observers)セクションに、in-out パラメータとしてプロパティを渡した際に呼ばれるプロパティオブサーバについての記載を追加しました
* [Swift ツアー\(A Swift Tour\)](https://github.com/stzn/the-swift-programming-language-jp/tree/4a13fdef628bb6cc8c2acf9fc744c981d1891aa0/welcome-to-swift-swift/a-swift-tour.md)の章に、エラー処理についてのセクションを追加しました
* [弱参照\(Weak References\)](../language-guide/automatic-reference-counting.md#weak-references)セクションに、メモリ割り当て解除プロセスをより明確に表示するために図を更新しました
* C 言語スタイルの `for` ループ、`++` 前置および後置演算子、`--` 前置および後置演算子の説明を削除しました
* 可変関数引数とカリー関数の特別な構文の説明を削除しました

## 2015-10-20

* Swift 2.1 に更新しました
* [文字列補間\(String Interpolation\)](../language-guide/strings-and-characters.md#string-interpolation)と[文字列リテラル\(String Literals\)](../language-guide/strings-and-characters.md#strings-and-characters-string-literals)セクションを更新しました。文字列補間に文字列リテラルを含めることができるようになりました
* [エスケープクロージャ\(Escaping Closures\)](../language-guide/closures.md#escaping-closures)セクションに、`@noescape` 属性についての情報を追加しました
* [宣言属性\(Declaration Attributes\)](../language-reference/attributes.md#declaration-attributes)セクションと[条件付きコンパイルブロック\(Conditional Compilation Block\)](../language-reference/statements.md#conditional-compilation-block)セクションの、tvOS についての情報を更新しました
* [In-Out パラメータ\(In-Out Parameters\)](../language-reference/declarations.md#declarations-in-out-parameters)セクションに、in-out パラメータの動作についての情報を追加しました
* [キャプチャリスト\(Capture Lists\)](../language-reference/expressions.md#capture-lists)セクションに、キャプチャリストで指定された値がキャプチャされる方法についての情報を追加しました
* [オプショナルチェーンを通したプロパティへのアクセス\(Accessing Properties Through Optional Chaining\)](../language-guide/optional-chaining.md#accessing-properties-through-optional-chaining)セクションを更新し、オプショナルチェーンを介して代入する方法が明確になるようにしました
* [自動クロージャ\(AutoClosures\)](../language-guide/closures.md#autoclosures)セクションの autoclosure の説明を改善しました
* [Swift ツアー\(A Swift Tour\)](https://github.com/stzn/the-swift-programming-language-jp/tree/4a13fdef628bb6cc8c2acf9fc744c981d1891aa0/welcome-to-swift-swift/a-swift-tour.md)の章の `??` 演算子を使用する例を追加しました

## 2015-09-16

* Swift 2.0 に更新しました
* [Error Handling\(エラーハンドリング\)](../language-guide/error-handling.md)の章、[Do 文\(Do Statement\)](../language-reference/statements.md#do-statement)セクション、[Throw 文\(Throw Statement\)](../language-reference/statements.md#throw-statement)セクション、[Defer 文\(Defer Statement\)](../language-reference/statements.md#defer-statement)セクション、および[Try 演算子\(Try Operator\)](../language-reference/expressions.md#try-operator)セクションに、エラー処理についての情報を追加しました
* [エラーの表現とスロー\(Representing and Throwing Errors\)](../language-guide/error-handling.md#representing-and-throwing-errors)セクションを更新しました。全ての型が `ErrorType` プロトコルに準拠するようになりました
* [エラーからオプショナル値への変換\(Converting Errors to Optional Values\)](../language-guide/error-handling.md#converting-errors-to-optional-values)セクションに、新しい `try?` キーワードについての情報を追加しました
* [列挙型\(Enumerations\)](../language-guide/enumerations.md)の章の[再帰的列挙型\(Recursive Enumerations\)](../language-guide/enumerations.md#recursive-enumerations)セクションと、[宣言\(Declarations\)](../language-reference/declarations.md)の章の[任意の型のケースを持つ列挙型\(Enumerations with Cases of Any Type\)](../language-reference/declarations.md#enumerations-with-cases-of-any-type)セクションに、再帰的な列挙型の情報を追加しました
* [制御フロー\(Control Flow\)](../language-guide/control-flow.md)の章の[API アベイラビリティチェック\(Checking API Availability\)](../language-guide/control-flow.md#checking-api-availability)セクションと、[文\(Statements\)](../language-reference/statements.md)の章の[Availability Condition\(アベイラビリティ条件\)](../language-reference/statements.md#availability-condition)セクションに、API アベイラビリティチェックについての情報を追加しました
* [制御フロー\(Control Flow\)](../language-guide/control-flow.md)の章の[Early Exit\(早期リターン\)](../language-guide/control-flow.md#early-exit)と、[文\(Statements\)](../language-reference/statements.md)の章の[Guard 文\(Guard Statement\)](../language-reference/statements.md#guard-statement)セクションに、新しい `guard` 文についての情報を追加しました
* [プロトコル\(Protocols\)](../language-guide/protocols.md)の章の[プロトコル Extension\(Protocol Extensions\)](../language-guide/protocols.md#protocol-extensions)についての情報を追加しました
* [アクセスコントロール\(Access Control\)](../language-guide/access-control.md)の章の[単体テストターゲットのアクセスレベル\(Access Levels for Unit Test Targets\)](../language-guide/access-control.md#access-levels-for-unit-test-targets)セクションに、単体テストでのアクセス制御についての情報を追加しました
* [パターン\(Patterns\)](../language-reference/patterns.md)の章の[オプショナルパターン\(Optional Pattern\)](../language-reference/patterns.md#optional-pattern)セクションに、新しいオプショナルパターンについての情報を追加しました
* [Repeat-While](../language-guide/control-flow.md#repeat-while)セクションを更新し、`repeat-while` ループについての情報を追加しました
* [文字列と文字\(Strings and Characters\)](../language-guide/strings-and-characters.md)の章を更新しました。`String` は Swift 標準ライブラリの `CollectionType` プロトコルに準拠しなくなりました
* [定数と変数の出力\(Printing Constants and Variables\)](../language-guide/the-basics.md#printing-constants-and-variables)セクションに、新しい Swift 標準ライブラリの `print(_:separator:terminator)` 関数についての情報を追加しました
* [列挙型\(Enumerations\)](../language-guide/enumerations.md)の章の[暗黙的に割り当てられた Raw Value\(Implicitly Assigned Raw Values\)](../language-guide/enumerations.md#implicitly-assigned-raw-values)セクションと、[宣言\(Declarations\)](../language-reference/declarations.md)の章の[Raw Value 型のケースを持つ列挙型\(Enumerations with Cases of a Raw-Value Type\)](../language-reference/declarations.md#enumerations-with-cases-of-a-raw-value-type)セクションに、`String` 型の Raw Value を持つ列挙ケースの動作についての情報を追加しました
* [自動クロージャ\(AutoClosures\)](../language-guide/closures.md#autoclosures)セクションに、`@autoclosure` 属性または `@autoclosure(escaping)` についての情報を追加しました
* [宣言属性\(Declaration Attributes\)](../language-reference/attributes.md#declaration-attributes)セクションを更新し、`@available` および `@warn_unused_result` 属性についての情報を追加しました
* [Type Attributes\(型属性\)](../language-reference/attributes.md#type-attributes)セクションを更新し、`@convention` 属性についての情報を追加しました
* [オプショナルバインディング\(Optional Binding\)](../language-guide/the-basics.md#optional-binding)セクションに、`where` 句を使用して複数のオプショナルバインディングを使用する例を追加しました
* [文字列リテラル\(String Literals\)](../language-guide/strings-and-characters.md#strings-and-characters-string-literals)セクションに、`+` 演算子を使用して文字列リテラルを連結する際のコンパイル時の動作についての情報を追加しました
* [Metatype 型\(Metatype Type\)](../language-reference/types.md#metatype-type)セクションに、イニシャライザ式を使用してインスタンスを構築する際の Metatype の比較についての情報を追加しました
* Debugging with Assertions\(アサーションを使ったデバッグ\)セクションに、ユーザ定義のアサーションが無効になる場合についての記載を追加しました
* [宣言属性\(Declaration Attributes\)](../language-reference/attributes.md#declaration-attributes)セクションの `@NSManaged` 属性の説明を更新しました。属性を特定のインスタンスメソッドに適用できるようになりました
* [Variadic Parameters\(可変長パラメータ\)](../language-guide/functions.md#variadic-parameters)セクションを更新しました。可変長パラメータは、関数のパラメータリスト内の任意の位置で宣言できるようになりました
* [Overriding a Failable Initializer\(失敗可能イニシャライザのオーバーライド\)](../language-guide/initialization.md#overriding-a-failable-initializer)セクションに、スーパークラスのイニシャライザの結果を強制アンラップすることで、失敗しないイニシャライザから失敗可能イニシャライザに委譲できることについての情報を追加しました
* [任意の型のケースを持つ列挙型\(Enumerations with Cases of Any Type\)](../language-reference/declarations.md#enumerations-with-cases-of-any-type)セクションに、関数を列挙ケースとして使用できることについての情報を追加しました
* [Initializer Expression\(イニシャライザ式\)](../language-reference/expressions.md#initializer-expression)セクションに、イニシャライザを明示的に参照する方法についての情報を追加しました
* [コンパイラ制御文\(Compiler Control Statements\)](../language-reference/statements.md#compiler-control-statements)セクションに、ビルド構成および行制御文についての情報を追加しました
* [Metatype 型\(Metatype Type\)](../language-reference/types.md#metatype-type)セクションに、Metatype の値からクラスインスタンスを構築する方法についての記載を追加しました
* [弱参照\(Weak References\)](../language-guide/automatic-reference-counting.md#weak-references)セクションに、弱参照がキャッシュに適していないことについての記載を追加しました
* [型プロパティ\(Type Properties\)](../language-guide/properties.md#type-properties)セクションの記載を更新し、格納型プロパティが遅延初期化されることを記載しました
* [値のキャプチャ\(Capturing Values\)](../language-guide/closures.md#capturing-values値のキャプチャ)セクションを更新し、変数と定数がクロージャ内でキャプチャされる方法を明確にしました
* [宣言属性\(Declaration Attributes\)](../language-reference/attributes.md#declaration-attributes)セクションを更新し、クラスに `@objc` 属性を適用できる場合についての説明を追加しました
* [エラー処理\(Handling Errors\)](../language-guide/error-handling.md#handling-errorsエラー処理)セクションに、`throw` 文を実行する際のパフォーマンスについての記載を追加しました。[Do 文\(Do Statement\)](../language-reference/statements.md#do-statement)のセクションの `do` 文にも同様の情報を追加しました
* [型プロパティ\(Type Properties\)](../language-guide/properties.md#type-properties)セクションの、クラス、構造体、および列挙型の格納型プロパティおよび計算型プロパティについての情報を更新しました
* [Break Statement\(Break 文\)](../language-reference/statements.md#break-statement)セクションの、ラベル付き break 文についての情報を更新しました
* `willSet` と `didSet` オブザーバの動作を明確にするために、[プロパティオブザーバ\(Property Observers\)](../language-guide/properties.md#property-observers)セクションの記載を更新しました
* [アクセスレベル\(Access Levels\)](../language-guide/access-control.md#access-levels)セクションに、`private` アクセスの範囲についての記載を追加しました
* [弱参照\(Weak References\)](../language-guide/automatic-reference-counting.md#weak-references)セクションに、ガベージコレクションと ARC の弱参照の違いについての記載を追加しました
* [文字列内の特殊文字\(Special Characters in String Literals\)](../language-guide/strings-and-characters.md#special-characters-in-string-literals)セクションの Unicode スカラの定義をより正確なものに更新しました

## 2015-04-08

* Swift 1.2 に更新しました
* Swift ネイティブの `Set` コレクション型ができました。詳細については、[Sets\(セット\)](../language-guide/collection-types.md#sets)を参照ください
* `@autoclosure` は、型ではなく、パラメータ宣言の属性になりました。新しい `@noescape` パラメータ属性もあります。詳細については、[宣言属性\(Declaration Attributes\)](../language-reference/attributes.md#declaration-attributes)を参照ください
* 型メソッドと型プロパティは、宣言修飾子の `static` キーワードを使用できるようになりました。詳細については、[型変数プロパティ\(Type Variable Properties\)](../language-reference/declarations.md#type-variable-properties)を参照ください
* Swift に `as?` と `as!` の失敗可能ダウンキャスト演算子が追加されました。詳細については、[プロトコル準拠チェック\(Checking for Protocol Conformance\)](../language-guide/protocols.md#checking-for-protocol-conformance)を参照ください
* 文字列インデックス\([文字列のインデックス\(String Indices\)](../language-guide/strings-and-characters.md#string-indices)\)について新しいガイドセクションを追加しました
* [Overflow Operators\(オーバーフロー演算子\)](../language-guide/advanced-operators.md#overflow-operators)からオーバーフロー分割演算子\(`&/`\)とオーバーフロー剰余演算子\(`&%`\)を削除しました
* 定数および定数プロパティ宣言と初期化のルールを更新しました。詳細については、[定数宣言\(Constant Declaration\)](../language-reference/declarations.md#constant-declaration)を参照ください
* 文字列リテラル内の Unicode スカラの定義を更新しました。[文字列内の特殊文字\(Special Characters in String Literals\)](../language-guide/strings-and-characters.md#special-characters-in-string-literals)を参照ください
* [範囲演算子\(Range Operators\)](../language-guide/basic-operators.md#range-operators)を更新し、開始と終了インデックスが同じ半開範囲は空になることを記載しました
* [クロージャは参照型\(Closures Are Reference Types\)](../language-guide/closures.md#closures-are-reference-types)を更新し、変数のキャプチャルールを明確にしました
* [値のオーバーフロー\(Value Overflow\)](../language-guide/advanced-operators.md#value-overflow)を更新し、符号付き整数と符号なし整数のオーバーフローの動作を明確にしました
* [プロトコル宣言\(Protocol Declaration\)](../language-reference/declarations.md#protocol-declaration)を更新し、プロトコル宣言の範囲とメンバを明確にしました
* [キャプチャリストの定義\(Defining a Capture List\)](../language-guide/automatic-reference-counting.md#defining-a-capture-list)を更新し、クロージャのキャプチャリストでの weak や unowned 参照の構文を明確にしました
* [演算子\(Operators\)](../language-reference/lexical-structure.md#operators)セクションを更新し、数学演算子、その他のシンボル、および Dingbats Unicode ブロックなど、カスタム演算子がサポートしている文字の例を明示的に説明しました
* 定数は、ローカル関数スコープで初期化せずに宣言できるようになりました。最初に使用する前に値の設定が必要です。詳細については、[定数宣言\(Constant Declaration\)](../language-reference/declarations.md#constant-declaration)を参照ください
* イニシャライザ内で、定数プロパティは値を一度だけ割り当てることができるようになりました。詳細については、[初期化中の定数プロパティの割り当て\(Assigning Constant Properties During Initialization\)](../language-guide/initialization.md#assigning-constant-properties-during-initialization)を参照ください
* 複数のオプショナルバインディングは、代入式のカンマ区切りのリストとして、単一の `if` 文で使用できるようになりました。詳しくは、[オプショナルバインディング\(Optional Binding\)](../language-guide/the-basics.md#optional-binding)を参照ください
* [オプショナルチェーン式\(Optional-Chaining Expression\)](../language-reference/expressions.md#optional-chaining-expression)は、後置式として使用しなければなりません
* プロトコルのキャストは、`@objc` プロトコルに制限されなくなりました
* 実行時に型キャストに失敗する可能性がある型は、`as?` または `as!` 演算子を使用し、失敗しないことが保証されている型キャストは、`as` 演算子を使用します。詳細については、[型キャスト演算子\(Type-Casting Operators\)](../language-reference/expressions.md#type-casting-operators)を参照ください

## 2014-10-16

* Swift 1.1 に更新しました
* 失敗可能イニシャライザ\([失敗可能イニシャライザ\(Failable Initializers\)](../language-guide/initialization.md#initialization-failable-initializers)\)の全ガイドを追加しました
* プロトコルの失敗可能イニシャライザ要件\([失敗可能イニシャライザ要件\(Failable Initializer Requirements\)](../language-guide/protocols.md#failable-initializer-requirements)\)の説明を追加しました
* `Any` 型の定数と変数に関数インスタンスを含めることができるようになりました。`switch` 文内での関数型のチェックとキャスト方法を示すために、[Any および AnyObject の型キャスト\(Type Casting for Any and AnyObject\)](../language-guide/type-casting.md#type-casting-for-any-and-anyobject)の例を更新しました
* Raw Value を持つ列挙型は、`toRaw()` メソッドではなく `rawValue` プロパティ、 `fromRaw()` メソッドではなく `rawValue` パラメータを持つ失敗可能イニシャライザを持つようになりました。 詳しくは、[Raw Values](../language-guide/enumerations.md#raw-values)と[Raw Value 型のケースを持つ列挙型\(Enumerations with Cases of a Raw-Value Type\)](../language-reference/declarations.md#enumerations-with-cases-of-a-raw-value-type)を参照ください
* 初期化の失敗を引き起こす失敗可能イニシャライザ\([失敗可能イニシャライザ\(Failable Initializers\)](../language-guide/initialization.md#initialization-failable-initializers)\)についての新しいセクションを追加しました
* カスタム演算子に `?` を含めることができるようになりました。[演算子\(Operators\)](../language-reference/lexical-structure.md#operators)を更新し、改訂されたルールを記載しました。[カスタム演算子\(Custom Operators\)](../language-guide/advanced-operators.md#custom-operators)から有効な演算子文字セットの説明の重複を削除しました

## 2014-08-18

* iOS と OS X アプリを構築するための Apple の新しいプログラミング言語 Swift 1.0 についての新しい文書
* プロトコル内の[イニシャライザ要件\(Initializer Requirements\)](../language-guide/protocols.md#initializer-requirements)についての新しいセクションを追加しました
* [クラス専用プロトコル\(Class-Only Protocols\)](../language-guide/protocols.md#class-only-protocols)についての新しいセクションを追加しました
* [アサーションと事前条件\(Assertions and Preconditions\)](../language-guide/the-basics.md#assertions-and-preconditions)は、文字列補間を使用できるようになりました。使用できない記載を削除しました
* [文字と文字列の連結\(Concatenating Strings and Characters\)](../language-guide/strings-and-characters.md#concatenating-strings-and-characterstono)セクションを更新し、`String` と `Character` を加算演算子\(`+`\)または加算代入演算子\(`+=`\)で組み合わせることができないことを反映しました。これらの演算子は `String` でのみ使用されます。`String` の末尾に単一の `Character` を追加するには、`String` 型の `append(_:)` メソッドを使用します
* [宣言属性\(Declaration Attributes\)](../language-reference/attributes.md#declaration-attributes)セクションに、`availability` 属性についての情報を追加しました
* [オプショナル\(Optionals\)](../language-guide/the-basics.md#optionals)は、オプショナルの `Bool` 値を使用しているときに、混乱を避けるために、値がある場合に `true`、ない場合に `false` と評価されません。代わりに、`==` または `!=` 演算子を指定して、オプショナルに値が含まれているかどうか、明示的な `nil` チェックを行いましょう
* [nil 合体演算子\(Nil-Coalescing Operator\)](../language-guide/basic-operators.md#nilcoalescing-operator)\(`a ?? b`\)ができました。これは、オプショナルの値が存在する場合はオプショナルの値をアンラップし、`nil` の場合はデフォルト値を返します
* [文字列の比較\(Comparing Strings\)](../language-guide/strings-and-characters.md#comparing-strings)セクションを更新、拡張し、文字列と文字の比較や、前方比較、後方比較は、拡張書記素クラスタの Unicode の正規等価性に基づいていることを反映し、例を示しました
* [オプショナルチェーン\(Optional Chaining\)](../language-guide/optional-chaining.md)を使って、mutating メソッドや演算子の呼び出し、subscript への値の割り当て、プロパティ値の設定を試すことができます。[オプショナルチェーンを通したプロパティへのアクセス\(Accessing Properties Through Optional Chaining\)](../language-guide/optional-chaining.md#accessing-properties-through-optional-chaining)の情報は、これに応じて更新され、[オプショナルチェーンを通したメソッドの呼び出し\(Calling Methods Through Optional Chaining\)](../language-guide/optional-chaining.md#calling-methods-through-optional-chaining)のメソッド呼び出しが成功したかどうかの例では、プロパティの設定が成功したかどうかをチェックする方法も例として示すように拡張しました
* オプショナルチェーンを通じて、オプショナルの subscript にアクセスするための新しいセクション\([オプショナル型の subscript へのアクセス\(Accessing Subscripts of Optional Type\)](../language-guide/optional-chaining.md#accessing-subscripts-of-optional-type)\)を追加しました
* [Array 配列へのアクセスと変更\(Accessing and Modifying an Array\)](../language-guide/collection-types.md#accessing-and-modifying-an-array)を更新し、`+=` 演算子を使って配列にその単一の要素を追加できなくなりました。代わりに、`append(_:)` メソッドを使用するか、単一の要素を持つ配列に `+=` 演算子を使用して追加します
* 範囲演算子\([範囲演算子\(Range Operators\)](../language-guide/basic-operators.md#range-operators)\) `a...b` と `a..<b` の開始値 `a` を、終了値 `b` より大きくしてはならないことを記載しました
* [継承\(Inheritance\)](../language-guide/inheritance.md)の章を書き直して、イニシャライザのオーバーライドの紹介部分を削除しました。この章では、サブクラス内に新しい機能を追加し、オーバーライドを使用して既存の機能を変更することに焦点を当てています。[プロパティの get プロパティ、set プロパティドのオーバーライド\(Overriding Property Getters and Setters\)](../language-guide/inheritance.md#overriding-property-getters-and-setters)の章の例は、`description` プロパティをオーバーライドする方法を示すために書き換えられました。\(サブクラスのイニシャライザで継承したプロパティのデフォルト値を変更する例は、[イニシャライザ\(Initialization\)](../language-guide/initialization.md)の章に移動しました\)
* [イニシャライザの継承とオーバーライド\(Initializer Inheritance and Overriding\)](../language-guide/initialization.md#initializer-inheritance-and-overriding)セクションを更新し、指定イニシャライザのオーバーライドを `override` 修飾子でマークする必要があることを記載しました
* [必須イニシャライザ\(Required Initializers\)](../language-guide/initialization.md#required-initializers)セクションを更新し、`required` 修飾子が全てのサブクラスの必須イニシャライザの実装の前に書かれ、自動的に継承したイニシャライザが必須イニシャライザの要件を満たすことができることを記載しました
* 中置演算子メソッド\([演算子メソッド\(Operator Methods\)](../language-guide/advanced-operators.md#advanced-operators-operator-methods)\)に `@infix` 属性が必要なくなりました
* [前置、後置演算子\(Prefix and Postfix Operators\)](../language-guide/advanced-operators.md#prefix-and-postfix-operators)の `@prefix` および `@postfix` 演算子属性は、`prefix` および  `postfix` 宣言修飾子に置き換えられました
* [前置、後置演算子\(Prefix and Postfix Operators\)](../language-guide/advanced-operators.md#prefix-and-postfix-operators)が同じオペランドに適用されたときに `prefix` と `postfix` 演算子が適用される順序についての記載を追加しました
* [複合代入演算子\(Compound Assignment Operators\)](../language-guide/advanced-operators.md#advanced-operators-compound-assignment-operators)の演算子関数は、関数を定義するときに `@assignment` 属性を使用しなくなりました
* [カスタム演算子\(Custom Operators\)](../language-guide/advanced-operators.md#custom-operators)を定義するときに修飾子を指定する順序が変更されました。例えば、`operator prefix` ではなく `prefix operator` と書くようになりました
* [宣言修飾子\(Declaration Modifiers\)](../language-reference/declarations.md#declaration-modifiers)に、`dynamic` 修飾子についての情報を追加しました
* 型推論が[リテラル\(Literals\)](../language-reference/lexical-structure.md#literals)で動作する方法についての情報を追加しました
* カリー関数のより詳しい情報を追加しました
* [アクセスコントロール\(Access Control\)](../language-guide/access-control.md)についての新しい章を追加しました
* [文字列と文字\(Strings and Characters\)](../language-guide/strings-and-characters.md)の章を更新し、Swift の `Character` 型が単一の Unicode 拡張書記素クラスタを表すことを反映しました。[拡張書記素クラスタ\(Extended Grapheme Clusters\)](../language-guide/strings-and-characters.md#extended-grapheme-clusters)上の新しいセクションと、[Unicode Scalar Values\(Unicode スカラ値\)](../language-guide/strings-and-characters.md#unicode-scalar-values)と[文字列の比較\(Comparing Strings\)](../language-guide/strings-and-characters.md#comparing-strings)についての詳しい情報も含まれています
* [文字列リテラル\(String Literals\)](../language-guide/strings-and-characters.md#strings-and-characters-string-literals)セクションを更新し、文字列内の Unicode スカラを `\u{n}`\(n には 0 から 10FFFF の間の 16 進数が入る\)で書けることを記載しました
* `NSString` の `length` プロパティは、`utf16count` ではなく、Swift のネイティブの `String` 型の `utf16count` にマッピングされるようになりました
* Swift のネイティブ `String` 型は、`uppercaseString` または `lowercaseString` プロパティを使用しなくなりました。[文字列と文字\(Strings and Characters\)](../language-guide/strings-and-characters.md)の対応するセクションは削除され、様々なコード例を更新しました
* [引数ラベルのないイニシャライザパラメータ\(Initializer Parameters Without Argument Labels\)](../language-guide/initialization.md#initializer-parameters-without-argument-labels)についての新しいセクションを追加しました
* [必須イニシャライザ\(Required Initializers\)](../language-guide/initialization.md#required-initializers)についての新しいセクションを追加しました
* [オプショナルのタプルの戻り値の型\(Optional Tuple Return Types\)](../language-guide/functions.md#optional-tuple-return-types)についての新しいセクションを追加しました
* [型注釈\(Type Annotations\)](../language-guide/the-basics.md#type-annotations)セクションを更新し、1 つの型注釈を持つ単一の行に複数の関連変数を定義できることを記載しました
* `@optional`、`@lazy`、`@final` と `@required` 属性は、`optional`、`lazy`、`final` と `required` 宣言修飾子\([宣言修飾子\(Declaration Modifiers\)](../language-reference/declarations.md#declaration-modifiers)\)になりました
* 本全体を参照して、`..<` を\(「半閉鎖範囲演算子」ではなく\)[半開範囲演算子\(Half-Open Range Operator\)](../language-guide/basic-operators.md#halfopen-range-operator)として参照するように更新しました
* [辞書へのアクセスと変更\(Accessing and Modifying a Dictionary\)](../language-guide/collection-types.md#accessing-and-modifying-a-dictionary)を更新し、`Dictionary` がブール値の `isEmpty` プロパティを持つことを記載しました
* [カスタム演算子\(Custom Operators\)](../language-guide/advanced-operators.md#custom-operators)を定義するときに使用できる文字の全リストを明確にしました
* `nil` とブール値 `true` と `false` は[リテラル\(Literals\)](../language-reference/lexical-structure.md#literals)になりました
* Swift の `Array` 型は、完全に値型のセマンティクスになりました。[コレクションの可変性\(Mutability of Collections\)](../language-guide/collection-types.md#mutability-of-collections)と[配列\(Arrays\)](../language-guide/collection-types.md#arrays)を更新し、新しいアプローチを反映しました
* [配列型の省略構文\(Array Type Shorthand Syntax\)](../language-guide/collection-types.md#array-type-shorthand-syntax)は `SomeType[]` ではなく `[SomeType]` として書くようになりました
* [辞書型の省略構文\(Dictionary Type Shorthand Syntax\)](../language-guide/collection-types.md#dictionary-type-shorthand-syntax)は、`[KeyType: ValueType]` と書くように新しいセクションを追加しました
* [セット型のハッシュ値\(Hash Values for Set Types\)](../language-guide/collection-types.md#hash-values-for-set-types)についての新しいセクションを追加しました
* [クロージャ式\(Closure Expressions\)](../language-guide/closures.md#closure-expressions)の例に、グローバルの `sort(_:_:)` 関数ではなくグローバルの `sorted(_:_:)` 関数を使用し、配列の新しい値型のセマンティクスを反映しています
* [構造体のメンバワイズイニシャライザ\(Memberwise Initializers for Structure Types\)](../language-guide/initialization.md#initialization-memberwise-initializers-for-structure-types)についての情報を更新し、構造体の格納プロパティにデフォルト値がない場合でもメンバワイズイニシャライザが使用可能だということを明確にしました
* [半開範囲演算子\(Half-Open Range Operator\)](../language-guide/basic-operators.md#halfopen-range-operator)に `..` ではなく `..<` を使用するようになりました
* [ジェネリック型の拡張\(Extending a Generic Type\)](../language-guide/generics.md#extending-a-generic-type)の例を追加しました

