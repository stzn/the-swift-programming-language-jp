# Document Revision History

最終更新日: 2021/6/26

## 2021-06-07

* Swift 5.5 に更新しました
* 非同期関数、Task、およびアクターについての情報を[Concurrency](./../language-guide/concurrency.md)の章、および[Actor Declaration](./../language-reference/declarations.md#actor-declarationアクター宣言)、[Asynchronous Functions and Methods](./../language-reference/declarations.md#asynchronous-functions-and-methods非同期関数とメソッド)、および[Await Operator](./../language-reference/expressions.md#await-operatorAwait演算子)セクションに追加しました

## 2021-04-26

* Swift 5.4 に更新しました
* [ResultBuilder](./../language-guide/advanced-operators.md#result-buildersリザルトビルダ)と[resultBuilder](./../language-reference/attributes.md#resultBuilder)セクションに、リザルトビルダについての情報を追加しました
* [Implicit Conversion to a Pointer Type](./../language-guide/extensions.md#implicit-conversion-to-a-pointer-typeポインタ型への暗黙変換)セクションに、関数呼び出しの中で in-out パラメータを unsafe pointer に暗黙的に変換する方法についての情報を追加しました
* [Variadic Parameters](./../language-guide/functions.md#variadic-parameters可変長パラメータ)と[Function Declaration](./../language-reference/declarations.md#function-declaration関数宣言)セクションを更新しました。関数は複数の可変長パラメータを受け取ることができるようになりました
* [Implicit Member Expression](./../language-reference/expressions.md#implicit-member-expression暗黙メンバ式)セクションを更新しました。暗黙メンバ式を繋げて使えるようになりました

## 2020-09-16

* Swift 5.3 に更新しました
* [Trailing Closures](./../language-guide/closures.md#trailing-closures末尾クロージャ)セクションに、複数の末尾クロージャについての情報を追加し、[Function Call Expression](./../language-reference/expressions.md#function-call-expression関数呼び出し式)セクションに、複数の末尾クロージャをパラメータに合わせる方法についての情報を追加しました
* [Adopting a Protocol Using a Synthesized Implementation](./../language-guide/protocols.md#adopting-a-protocol-using-a-synthesized-implementationデフォルト実装を使用したプロトコル準拠)セクションに、列挙型が `Comparable` プロトコルに準拠するための合成実装についての情報を追加しました
* [Contextual Where Clauses](./../language-guide/generics.md#contextual-where-clausesコンテキスト上のWhere句)セクションに、ジェネリックな `where` 句をより多くの場所に記載できる旨を追加しました
* [Unowned Optional References](./../language-guide/automatic-reference-counting.md#unowned-optional-referencesオプショナル非所有参照)セクションに、オプショナル値を `unowned` 参照に使用する方法についての情報を追加しました
* [main](./../.gitbook/assets/attributes.png#main)セクションに `@main` 属性についての情報を追加しました
* [Literal Expression](./../language-reference/expressions.md#literal-expressionリテラル式)セクションに、`#filePath` を追加し、`#file` の記載を更新しました
* [Escaping Closures](./../language-guide/closures.md#escaping-closuresエスケープクロージャ)セクションを更新しました。多くのシナリオでクロージャが暗黙的に `self` を参照できるようになりました
* [Handling Errors Using Do-Catch](./../language-guide/error-handling.md#handling-errors-using-do-catchdo-catchを使ったエラー処理)と[Do Statement](./../language-reference/statements.md#do-statementDo文)のセクションを更新しました。`catch` 句が複数のエラーにマッチングできるようになりました
* `Any` の詳細を追加し、[Any Type](./../language-reference/types.md#any-typeAny型)セクションに移動しました
* [Property Observers](./../language-guide/properties.md#property-observersプロパティオブザーバ)セクションを更新しました、`lazy` プロパティでもオブザーバが使用できるようになりました
* [Protocol Declaration](./../language-reference/declarations.md#protocol-declarationプロトコル宣言)セクションを更新しました。列挙型のメンバがプロトコル要件を満たすことができるようになりました
* [Stored Variable Observers and Property Observers](./../language-reference/declarations.md#stored-variable-observers-and-property-observers格納変数オブザーバとプロパティオブザーバ)セクションを更新して、いつオブザーバの前に get が呼び出されるかついて記述しました
* [Memory Safety](./../language-guide/memory-safety.md)の章を更新して、アトミック操作について記述しました

## 2020-03-24

* Swift 5.2 に更新しました
* [Key-Path Expression](./../language-reference/expressions.md#key-Path-expressionKey-Path式)セクションに、クロージャの代わりに KeyPath を渡すことについての情報を追加しました
* [Methods with Special Names](./../language-reference/declarations.md#methods-with-special-names特別な名前のメソッド)セクションに、関数呼び出し構文にクラス、構造体、および列挙型のインスタンスを使用できるようにする糖衣構文(シンタックスシュガー)についての情報を追加しました
* [Subscript Options](./../language-guide/subscripts.md#subscript-options様々なsubscript)セクションを更新しました。subscript でデフォルトパラメータを使用できるようになりました
* [Self Type](./../language-reference/types.md#self-typeSelf型)セクションを更新しました。より多くのコンテキストで `Self` が使えるようになりました
* [Implicitly Unwrapped Optionals](./../language-guide/the-basics.md#implicitly-unwrapped-optionals暗黙アンラップオプショナル)セクションを更新して、暗黙アンラップオプショナル値をオプショナルまたは非オプショナルのどちらでも使用できることをより明確にしました

## 2019-09-10

* Swift 5.1 に更新しました
* [Opaque Types](./../language-guide/opaque-types.md)の章に、戻り値に特定の名前の戻り値の型を提供するのではなく、戻り値が準拠するプロトコルを指定する関数についての情報を追加しました
* [Property Wrappers](./../language-guide/properties.md#property-wrappersプロパティラッパ)セクションに、プロパティラッパについての情報を追加しました
* [frozen](./../language-reference/attributes.md#frozen)セクションに、library evolution のための frozen な列挙型と構造体についての情報を追加しました
* [Functions With an Implicit Return](./../language-guide/functions.md#functions-with-an-implicit-return暗黙的な戻り値がある関数)と[Shorthand Getter Declaration](./../language-guide/properties.md#shorthand-getter-declaration省略getプロパティ宣言)セクションに、`return` を省略した関数についての情報を追加しました
* [Type Subscripts](./../language-guide/subscripts.md#type-subscripts型subscript)セクションに、型への subscript の使用について情報を追加しました
* [Enumeration Case Pattern](./../language-reference/patterns.md#enumeration-case-pattern列挙型のケースパターン)セクションを更新しました。列挙型ケースとオプショナル値をマッチングできるようになりました
* [Memberwise Initializers for Structure Types](./../language-guide/structures-and-classes.md#memberwise-initializers-for-structure-types構造体のメンバワイズイニシャライザ)セクションを更新しました。メンバワイズイニシャライザがデフォルト値を持つプロパティのパラメータを省略することができるようになりました
* [dynamicMemberLookup](./../language-reference/attributes.md#dynamicMemberLookup)セクションに、実行時に KeyPath で検索された動的メンバについての情報を追加しました
* [Conditional Compilation Block](./../language-reference/statements.md#conditional-compilation-block条件付きコンパイルブロック)のターゲット環境のリストに `macCatalyst` を追加しました
* [Self Type](./../language-reference/types.md#self-typeSelf型)セクションを更新しました。`Self` を使用して、現在のクラス、構造体、または列挙型宣言で導入された型を参照することができるようになりました

## 2019-03-25

* Swift 5.0 に更新しました
* [Extended String Delimiters](./../language-guide/strings-and-characters.md#extended-string-delimiters拡張区切り文字)セクションを追加し、拡張区切り文字についての情報を[String Literals](./../language-reference/lexical-structure.md#string-literals文字列リテラル)セクションに追加しました
* `dynamicCallable` 属性を使用してインスタンスを関数として動的に呼び出すことについての情報を記載した[dynamicCallable](./../language-reference/attributes.md#dynamicCallable)セクションを追加しました
* `unknown` スイッチケース属性を使用した `switch` 文の将来の列挙ケースを扱う方法についての情報を記載した[unknown](./../language-reference/attributes.md#unknown)と[Switching Over Future Enumeration Cases](./../language-reference/statements.md#switching-over-future-enumeration-cases列挙型の将来のケースのスイッチング)セクションを追加しました
* [Key-Path Expression](./../language-reference/expressions.md#key-Path-expressionKey-Path式)セクションに、識別子 KeyPath(`\.self`)についての情報を追加しました
* [Conditional Compilation Block](./../language-reference/statements.md#conditional-compilation-block条件付きコンパイルブロック)のプラットフォーム条件に(`<`)演算子の使用方法についての情報を追加しました

## 2018-09-17

* Swift 4.2 に更新しました
* [Iterating over Enumeration Cases](./../language-guide/enumerations.md#iterating-over-enumeration-cases列挙ケースの繰り返し処理)セクションに、列挙型の全てのケースへのアクセス方法についての情報を追加しました
* [Compile-Time Diagnostic Statement](./../language-reference/statements.md#compile-Time-diagnostic-statementコンパイル時診断文)セクションに、`#error` と `#warning` についての情報を追加しました
* [Declaration Attributes](./../language-reference/attributes.md#declaration-attributes宣言属性)セクションの `inlinable` と `usableFromInline` 属性に、インライン化についての情報を追加しました
* [Declaration Attributes](./../language-reference/attributes.md#declaration-attributes宣言属性)セクションの `dynamicMemberLookup` 属性に、実行時に名前で検索するメンバについての情報を追加しました
* [Declaration Attributes](./../language-reference/attributes.md#declaration-attributes宣言属性)セクションに、`require_stored_property_inits` および `warn_unqualified_access` 属性についての情報を追加しました
* [Conditional Compilation Block](./../language-reference/statements.md#conditional-compilation-block条件付きコンパイルブロック)セクションに、Swift コンパイラのバージョンに応じて、条件付きでコンパイルする方法についての情報を追加しました
* [Literal Expression](./../language-reference/expressions.md#literal-expressionリテラル式)セクションに、`#dsohandle` についての情報を追加しました

## 2018-03-29

* Swift 4.1 に更新しました
* [Equivalence Operators](./../language-guide/advanced-operators.md#equivalence-operators比較演算子)セクションに、等価演算子の合成実装についての情報を追加しました
* [Declarations](./../language-reference/declarations.md)の章の[Extension Declaration](./../language-guide/extensions.md#extension-declaration拡張宣言)セクションと、[Protocols](./../language-guide/protocols.md)の章の[Conditionally Conforming to a Protocol](./../language-guide/protocols.md#conditionally-conforming-to-a-protocol条件付きでのプロトコルへの準拠)セクションに、条件付きプロトコル準拠についての情報を追加しました
* [Using a Protocol in Its Associated Type’s Constraints](./../language-guide/generics.md#using-a-protocol-in-its-associated-type’s-constraints関連型の制約へのプロトコルの使用)セクションに、再帰的プロトコル制約についての情報を追加しました
* [Conditional Compilation Block](./../language-reference/statements.md#conditional-compilation-block条件付きコンパイルブロック)セクションに、`canImport()` と `targetEnvironment()` プラットフォーム条件についての情報を追加しました

## 2017-12-04

* Swift 4.0.3 に更新しました
* [Key-Path Expression](./../language-reference/expressions.md#key-Path-expressionKey-Path式)セクションを更新しました。subscript に KeyPath を使用できるようになりました

## 2017-09-19

* Swift 4.0 に更新しました
* [Memory Safety](./../language-guide/memory-safety.md)の章のメモリへの排他アクセスについての情報を追加しました
* [Associated Types with a Generic Where Clause](./../language-guide/generics.md#associated-types-with-a-generic-where-clauseジェネリックwhere句を使用した関連型)セクションを追加しました。ジェネリックな `where` 句で関連型を制約できるようになりました
* [Strings and Characters](./../language-guide/strings-and-characters.md)の章の[String Literals](./../language-guide/strings-and-characters.md#string-literals文字列リテラル)セクションと、[Lexical Structure](./../language-reference/lexical-structure.md)の章の[String Literals](./../language-reference/lexical-structure.md#string-literals文字列リテラル)セクションに、複数行文字列リテラルについての情報を追加しました
* [Declaration Attributes](./../language-reference/attributes.md#declaration-attributes宣言属性)セクションの `objc` 属性の説明を更新しました。この属性が推論される場所がより少なくなりました
* [Generic Subscripts](./../language-guide/generics.md#generic-subscriptsジェネリックsubscript)セクションを追加しました。subscript はジェネリックになる可能性があります
* [Protocols](./../language-guide/protocols.md)の章の[Protocol Composition](./../language-guide/protocols.md#protocol-compositionプロトコル合成)セクションと、[Types](./../language-reference/types.md)の章の[Protocol Composition Type](./../language-reference/types.md#protocol-composition-typeプロトコル合成型)セクションの説明を更新しました。プロトコル合成型にスーパークラスの要件を含めることができるようになりました
* [Extension Declaration](./../language-guide/extensions.md#extension-declaration拡張宣言)のプロトコル拡張の説明を更新しました。`final` は使用できなくなりました
* [Assertions and Preconditions](./../language-guide/the-basics.md#assertions-and-preconditionsアサーションと事前条件)セクションに、前提条件と致命的エラーについての情報を追加しました

## 2017-03-27

* Swift 3.1 に更新しました
* [Extensions with a Generic Where Clause](./../language-guide/generics.md#extensions-with-a-generic-where-clauseジェネリックwhere句を使った拡張)セクションに、要件を含む extension についての情報を追加しました
* [For-In Loops](./../language-guide/control-flow.md#for-In-loopsFor-Inループ)セクションに範囲全体を繰り返す例を追加しました
* [Failable Initializers](./../language-guide/initialization.md#failable-initializers失敗可能イニシャライザ)セクションに、失敗可能な数値変換の例を追加しました
* [Declaration Attributes](./../language-reference/attributes.md#declaration-attributes宣言属性)セクションに、 Swift 言語バージョンを使用した `available` 属性の使い方についての情報を追加しました
* [Function Type](./../language-reference/types.md#function-type関数型)セクションの説明を更新し、関数型に引数ラベルを書くことができないことを記載しました
* [Conditional Compilation Block](./../language-reference/statements.md#conditional-compilation-block条件付きコンパイルブロック)セクションの Swift 言語バージョン番号の説明を更新しました。オプショナルのパッチ番号が使用できるようになりました
* [Function Type](./../language-reference/types.md#function-type関数型)セクションの説明を更新し、Swift は、タプル型の単一のパラメータを受け取る関数と、複数のパラメータを受け取る関数を区別するようになりました
* [Expressions](./../language-reference/expressions.md)の章から動的型式のセクションを削除しました。`type(of:)` は Swift 標準ライブラリ関数になりました

## 2016-10-27

* Swift 3.0.1 に更新しました
* [Automatic Reference Counting](./../language-guide/automatic-reference-counting.md)の章の弱(`weak`)参照、非所有(`unowned`)参照の説明を更新しました
* [Declaration Modifiers](./../language-reference/declarations.md#declaration-modifiers宣言修飾子)セクションに、`unowned`、`unowned(safe)` と `unowned(unsafe)` 修飾子についての情報を追加しました
* [Type Casting for Any and AnyObject](./../language-guide/type-casting.md#type-casting-for-any-and-anyObjectAnyおよびAnyObjectの型キャスト)セクションに、Any 型の値が期待される場所にオプショナルの値を使用することについての記載を追加しました
* 括弧付き式とタプル式の説明を分けるように、[Expressions](./../language-reference/expressions.md)の章を更新しました

## 2016-09-13

* Swift 3.0 に更新しました
* [Functions](./../language-guide/functions.md)の章と[Function Declaration](./../language-reference/declarations.md#function-declaration関数宣言)セクションの関数の説明を、デフォルトで全てのパラメータが引数ラベルを持つように更新しました
* [Advanced Operators](./../language-guide/advanced-operators.md)の章の演算子の説明を更新しました。これらの演算子をグローバル関数としてはなく型メソッドとして実装するようになりました
* [Access Control](./../language-guide/access-control.md)の章に、`open` アクセスレベル修飾子と `filePrivate` アクセスレベル修飾子についての情報を追加しました
* [Function Declaration](./../language-reference/declarations.md#function-declaration関数宣言)セクションの `inout` の説明を、パラメータ名の前ではなくパラメータの型の前に使用するように更新しました
* [Escaping Closures](./../language-guide/closures.md#escaping-closuresエスケープクロージャ)と[Autoclosures](./../language-guide/closures.md#autoclosures自動クロージャ)セクションと、[Attributes](./../language-reference/attributes.md)の章の説明で、`@noescape` および `@autoclosures` 属性を宣言属性から型属性へ更新しました
* [Advanced Operators](./../language-guide/advanced-operators.md)の章の[Precedence for Custom Infix Operators](./../language-guide/advanced-operators.md#precedence-for-custom-infix-operatorsカスタム中置演算子の優先順位)セクションと、[Declaration](./../language-reference/declarations.md)の章の[Precedence Group Declaration](./../language-reference/declarations.md#precedence-group-declaration優先順位グループ宣言)セクションに、演算子の優先順位グループについての情報を追加しました
* OS X の変わりに macOS、`ErrorProtocol` の代わりに `Error`、`StringLiteralConvertible` の代わりに `ExpressibleByStringLiteral` などのプロトコル名を使用するように説明を更新しました
* [Generic](./../language-guide/generics.md)の章の[Generic Where Clauses](./../language-guide/generics.md#generic-where-clausesジェネリックwhere句)セクションと、[Generic Parameters and Arguments](./../language-reference/generic-parameters-and-arguments.md)の章の説明を更新しました。ジェネリックな `where` 句を宣言の最後に書くことができるようになりました
* [Escaping Closures](./../language-guide/closures.md#escaping-closuresエスケープクロージャ)セクションの説明を更新しました。クロージャはデフォルトが非エスケープになりました
* [The Basics](./../language-guide/the-basics.md)の章の[Optional Binding](./../language-guide/the-basics.md#optional-bindingオプショナルバインディング)セクションと、[Statement](./../language-reference/statements.md)の章の[While Statement](./../language-reference/statements.md#while-statementWhile文)セクションの説明を更新しました。`if`、`while`、`guard` 文は、`where` 句なしで、カンマ区切りリストの条件を使用できるようになりました
* [Control Flow](./../language-guide/control-flow.md)の章の[Switch](./../language-guide/control-flow.md#switch)セクションと、[Statement](./../language-reference/statements.md)の章の[Switch Statement](./../language-reference/statements.md#switch-statementSwitch文)セクションに、複数のパターンを持つ列挙ケースについての情報を追加しました
* [Function Type](./../language-reference/types.md#function-type関数型)セクションの関数型の説明を更新しました。引数ラベルが関数の型の一部ではなくなりました
* [Protocols](./../language-guide/protocols.md)の章の[Protocol Composition](./../language-guide/protocols.md#protocol-compositionプロトコル合成)セクションと、[Types](./../language-reference/types.md)の章の[Protocol Composition Type](./../language-reference/types.md#protocol-composition-typeプロトコル合成型)セクションの説明を更新し、新しい `Protocol1 & Protocol2` の構文を使用するようになりました
* 動的型式セクションで、新しい `type(of:)` を使用するように説明を更新しました
* [Line Control Statement](./../language-reference/statements.md#line-control-statement行制御文)セクションで、`#sourceLocation(file:line:)` 構文を使用するように、行制御文の説明を更新しました
* [Functions that Never Return](./../language-reference/declarations.md#functions-that-never-returnノーリターン関数)の説明を更新し、新しい `Never` 型を使用するようになりました
* [Literal Expression](./../language-reference/expressions.md#literal-expressionリテラル式)セクションに、playground リテラルについての情報を追加しました
* [In-Out Parameters](./../language-reference/declarations.md#in-Out-parametersIn-Outパラメータ)セクションの説明を更新し、非エスケープクロージャのみが in-out パラメータをキャプチャできることを記載しました
* [Default Parameter Values](./../language-guide/functions.md#default-parameter-valuesデフォルトパラメータ値)セクションのデフォルトパラメータの説明を更新しました。関数呼び出し時に並べ替えることができなくなりました
* [Attributes](./../language-reference/attributes.md)の章でコロンを使用するように属性引数を更新しました
* [Rethrowing Functions and Methods](./../language-reference/declarations.md#rethrowing-functions-and-methods再スロー関数と再スローメソッド)セクションに、再スロー関数のキャッチブロック内でエラーをスローすることについての情報を追加しました
* [Selector Expression](./../language-reference/expressions.md#selector-expressionSelector式)セクションに、Objective-C プロパティの get または set のセレクタへのアクセスについての情報を追加しました
* [Type Alias Declaration](./document-revision-history.md#type-alias-declarationタイプエイリアス宣言)セクションに、ジェネリックなタイプエイリアスとプロトコル内でのタイプエイリアスの使用について情報を追加しました
* [Function Type](./../language-reference/types.md#function-type関数型)セクションの説明を更新し、パラメータ型の周囲に括弧が必須だということを記載しました
* [Attributes](./../language-reference/attributes.md)の章を更新して、`@IBAction`、`@IBOutlet`、および `@NSManaged` 属性は `@objc` 属性を暗黙的に含んでいることを記載しました
* [Declaration Attributes](./../language-reference/attributes.md#declaration-attributes宣言属性)セクションに、`@GKInspectable` 属性についての情報を追加しました
* [Optional Protocol Requirements](./../language-guide/protocols.md#optional-protocol-requirementsオプショナルのプロトコル要件)のオプショナルのプロトコル要件の説明を更新し、Objective-C と相互運用されるコードでのみ使用されることを明確にしました
* [Function Declaration](./../language-reference/declarations.md#function-declaration関数宣言)セクションから、関数パラメータを使用して明示的に `let` を使用することの説明を削除しました
* [Statement](./../language-reference/statements.md)の章から `Boolean` プロトコルの説明を削除しました。このプロトコルは SWwift 標準ライブラリから削除されました
* [Declaration Attributes](./../language-reference/attributes.md#declaration-attributes宣言属性)セクションの `@NSApplicationMain` 属性の説明を修正しました

## 2016-03-21

* Swift 2.2 に更新しました
* [Conditional Compilation Block](./../language-reference/statements.md#conditional-compilation-block条件付きコンパイルブロック)セクションに、Swift バージョンに応じて、コードをコンパイルする方法についての情報を追加しました
* [Explicit Member Expression](./../language-reference/expressions.md#explicit-member-expression明示的メンバ式)セクションに、引数名だけが異なるメソッドまたはイニシャライザを区別する方法についての情報を追加しました
* [Selector Expression](./../language-reference/expressions.md#selector-expressionSelector式)セクションに、Objective-C セレクタの `#Selector` 構文についての情報を追加しました
* [Associated Types](./../language-guide/generics.md#associated-types関連型)と[Protocol Associated Type Declaration](./../language-reference/declarations.md#protocol-associated-type-declarationプロトコル関連型宣言)セクションの、`associatedtype` キーワードを使った関連型の説明を更新しました
* [Failable Initializers](./../language-guide/initialization.md#failable-initializers失敗可能イニシャライザ)セクションの、インスタンスが完全に初期化される前に `nil` を返すイニシャライザについての情報を更新しました
* [Comparison Operators](./../language-guide/basic-operators.md#comparison-operators比較演算子)セクションに、タプルの比較についての情報を追加しました
* [Keywords and Punctuation](./../language-reference/lexical-structure.md#keywords-and-punctuation単語と句読点)セクションに、外部パラメータ名にキーワードを使用する方法についての情報を追加しました
* [Declaration Attributes](./../language-reference/attributes.md#declaration-attributes宣言属性)セクションの `@objc` 属性の説明を更新し、列挙型と列挙ケースでもこの属性を使用できることを記載しました
* [Operators](./../language-reference/lexical-structure.md#operators演算子)セクションの、ドットを含むカスタム演算子の説明を更新しました
* [Rethrowing Functions and Methods](./../language-reference/declarations.md#rethrowing-functions-and-methods再スロー関数と再スローメソッド)セクションに、再スロー関数が直接エラーをスローできないことを記載した記載を追加しました
* [Property Observers](./../language-guide/properties.md#property-observersプロパティオブザーバ)セクションに、in-out パラメータとしてプロパティを渡した際に呼ばれるプロパティオブサーバについての記載を追加しました
* [A Swift Tour](./../welcome-to-swift-swift/a-swift-tour.md)の章に、エラー処理についてのセクションを追加しました
* [Weak References](./../language-guide/automatic-reference-counting.md#weak-references弱参照)セクションに、メモリ割り当て解除プロセスをより明確に表示するために図を更新しました
* C 言語スタイルの `for` ループ、`++` 前置および後置演算子、`--` 前置および後置演算子の説明を削除しました
* 可変関数引数とカリー関数の特別な構文の説明を削除しました

## 2015-10-20

* Swift 2.1 に更新しました
* [String Interpolation](./../language-guide/strings-and-characters.md#string-interpolation文字列補間)と[String Literals](./../language-guide/strings-and-characters.md#string-literals文字列リテラル)セクションを更新しました。文字列補間に文字列リテラルを含めることができるようになりました
* [Escaping Closures](./../language-guide/closures.md#escaping-closuresエスケープクロージャ)セクションに、`@noescape` 属性についての情報を追加しました
* [Declaration Attributes](./../language-reference/attributes.md#declaration-attributes宣言属性)セクションと[Conditional Compilation Block](./../language-reference/statements.md#conditional-compilation-block条件付きコンパイルブロック)セクションの、tvOS についての情報をを更新しました
* [In-Out Parameters](./../language-reference/declarations.md#in-Out-parametersIn-Outパラメータ)セクションに、in-out パラメータの動作についての情報を追加しました
* [Capture Lists](./../language-guide/extensions.md#capture-listsキャプチャリスト)セクションに、キャプチャリストで指定された値がキャプチャされる方法についての情報を追加しました
* [Accessing Properties Through Optional Chaining](./../language-guide/optional-chaining.md#accessing-properties-through-optional-chainingオプショナルチェーンを通したプロパティへのアクセス)セクションを更新し、オプショナルチェーンを介して代入する方法が明確になるようにしました
* [Autoclosures](./../language-guide/closures.md#autoclosures自動クロージャ)セクションの autoclosure の説明を改善しました
* [A Swift Tour](./../welcome-to-swift-swift/a-swift-tour.md)の章の `??` 演算子を使用する例を追加しました

## 2015-09-16

* Swift 2.0 に更新しました
* [Error Handling](./../language-guide/error-handling.md)の章、[Do Statement](./../language-reference/statements.md#do-statementDo文)セクション、[Throw Statement](./../language-reference/statements.md#throw-statementThrow文)セクション、[Defer Statement](./../language-reference/statements.md#defer-statementDefer文)セクション、および[Try Operator](./../language-reference/expressions.md#try-operatorTry演算子)セクションに、エラー処理についての情報を追加しました
* [Representing and Throwing Errors](./../language-guide/error-handling.md#representing-and-throwing-errorsエラーの表現とスロー)セクションを更新しました。全ての型が `ErrorType` プロトコルに準拠するようになりました
* [Converting Errors to Optional Values](./../language-guide/error-handling.md#converting-errors-to-optional-valuesエラーからオプショナル値への変換)セクションに、新しい `try?` キーワードについての情報を追加しました
* [Enumerations](./../language-guide/enumerations.md)の章の[Recursive Enumerations](./../language-guide/enumerations.md#recursive-enumerations再帰的列挙型)セクションと、[Declarations](./../language-reference/declarations.md)の章の[Enumerations with Cases of Any Type](./../language-reference/declarations.md#enumerations-with-cases-of-any-type任意の型のケースを持つ列挙型)セクションに、再帰的な列挙型の情報を追加しました
* [Control Flow](./../language-guide/control-flow.md)の章の[Checking API Availability](./../language-guide/control-flow.md#checking-API-availabilityAPI可用性チェック)セクションと、[Statements](./../language-reference/statements.md)の章の[Availability Condition](./../language-reference/statements.md#availability-conditionAvailability条件)セクションに、API 可用性チェックについての情報を追加しました
* [Control Flow](./../language-guide/control-flow.md)の章の[Early Exit](./../language-guide/control-flow.md#early-exit早期リターン)と、[Statements](./../language-reference/statements.md)の章の[Guard Statement](./../language-reference/statements.md#guard-statementGuard文)セクションに、新しい `guard` 文についての情報を追加しました
* [Protocols](./../language-guide/protocols.md)の章の[Protocol Extensions](./../language-guide/protocols.md#protocol-extensionsプロトコルExtension)についての情報を追加しました
* [Access Control](./../language-guide/access-control.md)の章の[Access Levels for Unit Test Targets](./../language-guide/access-control.md#access-levels-for-unit-test-targets単体テストターゲットのアクセスレベル)セクションに、単体テストでのアクセス制御についての情報を追加しました
* [Patterns](./../language-reference/patterns.md)の章の[](./../language-reference/patterns.md#optional-patternオプショナルパターン)セクションに、新しいオプショナルパターンについての情報を追加しました
* [Repeat-While](./../language-guide/control-flow.md#repeat-While)セクションを更新し、`repeat-while` ループについての情報を追加しました
* [Strings and Characters](./../language-guide/strings-and-characters.md)の章を更新しました。`String` は Swift 標準ライブラリの `CollectionType` プロトコルに準拠しなくなりました
* [Printing Constants and Variables](./../language-guide/the-basics.md#printing-constants-and-variables定数と変数の出力)セクションに、新しい Swift 標準ライブラリの `print(_:separator:terminator)` 関数についての情報を追加しました
* [Enumerations](./../language-guide/enumerations.md)の章の[Implicitly Assigned Raw Values](./../language-guide/enumerations.md#implicitly-assigned-raw-values暗黙的に割り当てられたrawvalue)セクションと、[Declarations](./../language-reference/declarations.md)の章の[Enumerations with Cases of a Raw-Value Type](./../language-reference/declarations.md#enumerations-with-cases-of-a-raw-Value-typeRawValue型のケースを持つ列挙型)セクションに、`String` 型の Raw Value を持つ列挙ケースの動作についての情報を追加しました
* [Autoclosures](./../language-guide/closures.md#autoclosures自動クロージャ)セクションに、`@autoclosure` 属性または `@autoclosure(escaping)` についての情報を追加しました
* [Declaration Attributes](./../language-reference/attributes.md#declaration-attributes宣言属性)セクションを更新し、`@available` および `@warn_unused_result` 属性についての情報を追加しました
* [Type Attributes](./../language-reference/attributes.md#type-attributes)セクションを更新し、`@convention` 属性についての情報を追加しました
* [Optional Binding](./../language-guide/the-basics.md#optional-bindingオプショナルバインディング)セクションに、`where` 句を使用して複数のオプショナルバインディングを使用する例を追加しました
* [String Literals](./../language-guide/strings-and-characters.md#string-literals文字列リテラル)セクションに、`+` 演算子を使用して文字列リテラルを連結する際のコンパイル時の動作についての情報を追加しました
* [Metatype Type](./../language-reference/types.md#metatype-typeMetatype型)セクションに、イニシャライザ式を使用してインスタンスを構築する際の Metatype の比較についての情報を追加しました
* [Debugging with Assertions](./../language-guide/the-basics.md#debugging-with-assertionsアサーションを使ったデバッグ)セクションに、ユーザ定義のアサーションが無効になる場合についての記載を追加しました
* [Declaration Attributes](./../language-reference/attributes.md#declaration-attributes宣言属性)セクションの `@NSManaged` 属性の説明を更新しました。属性を特定のインスタンスメソッドに適用できるようになりました
* [Variadic Parameters](./../language-guide/functions.md#variadic-parameters可変長パラメータ)セクションを更新しました。可変長パラメータは、関数のパラメータリスト内の任意の位置で宣言できるようになりました
* [Overriding a Failable Initializer](./../language-guide/initialization.md#overriding-a-failable-initializer失敗可能イニシャライザのオーバーライド)セクションに、スーパークラスのイニシャライザの結果を強制アンラップすることで、失敗しないイニシャライザから失敗可能イニシャライザに委譲できることについての情報を追加しました
* [Enumerations with Cases of Any Type](./../language-reference/declarations.md#enumerations-with-cases-of-any-type任意の型のケースを持つ列挙型)セクションに、関数を列挙ケースをとして使用できることについての情報を追加しました
* [Initializer Expression](./../language-reference/expressions.md#initializer-expressionイニシャライザ式)セクションに、イニシャライザを明示的に参照する方法についての情報を追加しました
* [Compiler Control Statements](./../language-reference/statements.md#compiler-control-statementsコンパイラ制御文)セクションに、ビルド構成および行制御文についての情報を追加しました
* [Metatype Type](./../language-reference/types.md#metatype-typeMetatype型)セクションに、Metatype の値からクラスインスタンスを構築する方法についての記載を追加しました
* [Weak References](./../language-guide/automatic-reference-counting.md#weak-references弱参照)セクションに、弱参照がキャッシュに適していないことについての記載を追加しました
* [Type Properties](./../language-guide/properties.md#type-properties型プロパティ)セクションの記載を更新し、格納型プロパティが遅延初期化されることを記載しました
* [Capturing Values](./../language-guide/closures.md#capturing-values値のキャプチャ)セクションを更新し、変数と定数がクロージャ内でキャプチャされる方法を明確にしました
* [Declaration Attributes](./../language-reference/attributes.md#declaration-attributes宣言属性)セクションを更新し、クラスに `@objc` 属性を適用できる場合についての説明を追加しました
* [Handling Errors](./../language-guide/error-handling.md#handling-errorsエラー処理)セクションに、`throw` 文を実行する際のパフォーマンスについての記載を追加しました。[Do Statement](./../language-reference/statements.md#do-statementDo文)のセクションの `do` 文にも同様の情報を追加しました
* [Type Properties](./../language-guide/properties.md#type-properties型プロパティ)セクションの、クラス、構造体、および列挙型の格納型プロパティおよび計算型プロパティについての情報を更新しました
* [Break Statement](./../language-reference/statements.md#break-statementBreak文)セクションの、ラベル付き Break 文についての情報を更新しました
* `willSet` と `didSet` オブザーバの動作を明確にするために、[Property Observers](./../language-guide/properties.md#property-observersプロパティオブザーバ)セクションの記載を更新しました
* [Access Levels](./../language-guide/access-control.md#access-levelsアクセスレベル)セクションに、`private` アクセスの範囲についての記載を追加しました
* [Weak References](./../language-guide/automatic-reference-counting.md#weak-references弱参照)セクションに、ガベージコレクションと ARC の弱参照の違いについての記載を追加しました
* [Special Characters in String Literals](./../language-guide/strings-and-characters.md#special-characters-in-string-literals文字列内の特殊文字)セクションの Unicode スカラの定義をより正確なものに更新しました

## 2015-04-08

* Swift 1.2 に更新しました
* Swift ネイティブの `Set` コレクション型ができました。詳細については、[Sets](./../language-guide/collection-types.md#Setsセット)を参照ください
* `@autoclosure` は、型ではなく、パラメータ宣言の属性になりました。新しい `@noescape` パラメータ属性もあります。詳細については、[Declaration Attributes](./../language-reference/attributes.md#declaration-attributes宣言属性)を参照ください
* 型メソッドと型プロパティは、宣言修飾子として `static` キーワードを使用できるようになりました。詳細については、[Type Variable Properties](./../language-reference/declarations.md#type-variable-properties型変数プロパティ)を参照ください
* Swift に `as?` と `as?` の失敗可能ダウンキャスト演算子が含まれました。詳細については、[Checking for Protocol Conformance](./../language-guide/protocols.md#checking-for-protocol-conformanceプロトコル準拠チェック)を参照ください
* 文字列インデックス([String Indices](./../language-guide/strings-and-characters.md#string-indices文字列のインデックス))について新しいガイドセクションを追加しました
* [Overflow Operators](./../language-guide/advanced-operators.md#overflow-operatorsオーバーフロー演算子)からオーバーフロー分割演算子(`&/`)とオーバーフロ剰余演算子(`&%`)を削除しました
* 定数および定数プロパティ宣言と初期化のルールを更新しました。詳細については、[Constant Declaration](./../language-reference/declarations.md#constant-declaration定数宣言)を参照ください
* 文字列リテラル内の Unicode スカラの定義を更新しました。[Special Characters in String Literals](./../language-guide/strings-and-characters.md#special-characters-in-string-literals文字列内の特殊文字)を参照ください
* [Range Operators](./../language-guide/basic-operators.md#range-operators範囲演算子)を更新し、開始と終了インデックスが同じ半開範囲は空になることを記載しました
* [Closures Are Reference Types](./../language-guide/closures.md#closures-are-reference-typesクロージャは参照型)を更新し、変数のキャプチャルールを明確にしました
* [Value Overflow](./../language-guide/advanced-operators.md#value-overflow値のオーバーフロー)を更新し、符号付き整数と符号なし整数のオーバーフローの動作を明確にしました
* [Protocol Declaration](./../language-reference/declarations.md#protocol-declarationプロトコル宣言)を更新し、プロトコル宣言の範囲とメンバを明確にしました
* [Defining a Capture List](./../language-guide/automatic-reference-counting.md#defining-a-capture-listキャプチャリストの定義)を更新し、クロージャのキャプチャリストでの weak や unowned 参照の構文を明確にしました
* [Operators](./../language-reference/lexical-structure.md#operators演算子)セクションを更新し、数学演算子、その他のシンボル、および Dingbats Unicode ブロックなど、カスタム演算子がサポートしている文字の例を明示的に説明しました
* 定数は、ローカル関数スコープで初期化せずに宣言できるようになりました。最初に使用する前に値の設定が必要です。詳細については、[Constant Declaration](./../language-reference/declarations.md#constant-declaration定数宣言)を参照ください
* イニシャライザ内で、定数プロパティは値を一度だけ割り当てることができるようになりました。詳細については、[Assigning Constant Properties During Initialization](./../language-guide/initialization.md#assigning-constant-properties-during-initialization初期化中の定数プロパティの割り当て)を参照ください
* 複数のオプショナルバインディングは、代入式のカンマ区切りのリストとして、単一の `if` 文で使用できるようになりました。詳しくは、[Optional Binding](./../language-guide/the-basics.md#optional-bindingオプショナルバインディング)を参照ください
* [Optional-Chaining Expression](./../language-reference/expressions.md#optional-Chaining-expressionオプショナルチェーン式)は、後置式に使用しなければなりません
* プロトコルのキャストは、`@objc` プロトコルに制限されなくなりました
* 実行時に型キャストに失敗する可能性がある型は、`as?` または `as!` 演算子を使用し、失敗しないことが保証されている型キャストは、`as` 演算子を使用します。詳細については、[Type-Casting Operators](./../language-reference/expressions.md#type-Casting-operators型キャスト演算子)を参照ください

## 2014-10-16

* Swift 1.1 に更新しました
* 失敗可能イニシャライザ([Failable Initializers](./../language-guide/initialization.md#failable-initializers失敗可能イニシャライザ))の全ガイドを追加しました
* プロトコルの失敗可能イニシャライザ要件([Failable Initializer Requirements](./../language-guide/protocols.md#failable-initializer-requirements失敗可能イニシャライザ要件))の説明を追加しました
* `Any` 型の定数と変数に関数インスタンスを含めることができるようになりました。`switch` 文内での関数型のチェックとキャスト方法を示すために、[Type Casting for Any and AnyObject](./../language-guide/type-casting.md#type-casting-for-any-and-anyObjectAnyおよびAnyObjectの型キャスト)の例を更新しました
* Raw Value を持つ列挙型は、`toRaw()` メソッドではなく `rawValue` プロパティ、 `fromRaw()` メソッドではなく `rawValue` パラメータを持つ失敗可能イニシャライザを持つようになりました。 詳しくは、[Raw Values](./../language-guide/enumerations.md#raw-values)と[Enumerations with Cases of a Raw-Value Type](./../language-reference/declarations.md#enumerations-with-cases-of-a-raw-Value-typeRawValue型のケースを持つ列挙型)を参照ください
* 初期化の失敗を引き起こす失敗可能イニシャライザ([Failable Initializers](./../language-guide/initialization.md#failable-initializers失敗可能イニシャライザ))についての新しいセクションを追加しました
* カスタム演算子に `?` を含めることができるようになりました。[Operators](./../language-reference/lexical-structure.md#operators演算子)を更新し、改訂されたルールを記載しました。カスタム演算子([Custom Operators](./../language-guide/advanced-operators.md#custom-operatorsカスタム演算子))から有効な演算子文字セットの説明の重複を削除しました

## 2014-08-18

* iOS と OS X アプリを構築するための Apple の新しいプログラミング言語 Swift 1.0 についての新しい文書
* プロトコル内の [Initializer Requirements](./../language-guide/protocols.md#initializer-requirementsイニシャライザ要件)についての新しいセクションを追加しました
* [Class-Only Protocols](./../language-guide/protocols.md#class-Only-protocolsクラス専用プロトコル)についての新しいセクションを追加しました
* [Assertions and Preconditions](./../language-guide/the-basics.md#assertions-and-preconditionsアサーションと事前条件)は、文字列補間を使用できるようになりました。反対の記載を削除しました
* [Concatenating Strings and Characters](./../language-guide/strings-and-characters.md#concatenating-strings-and-characters文字と文字列の連結)セクションを更新し、`String` と `Character` を加算演算子(`+`)または加算代入演算子(`+=`)と組み合わせることができないことを反映しました。これらの演算子は `String` でのみ使用されます。`String` の末尾に単一の `Character` を追加するには、`String` 型の `append(_:)` メソッドを使用します
* [Declaration Attributes](./../language-reference/attributes.md#declaration-attributes宣言属性)セクションに、`availability` 属性についての情報を追加しました
* [Optionals](./../language-guide/the-basics.md#optionalsオプショナル)は、オプショナルの `Bool` 値を使用しているときに、混乱を避けるために、値がある場合に `true`、ない場合に `false` と評価されません。代わりに、`==` または `!=` 演算子を指定して、オプショナルに値が含まれているかどうか、明示的な `nil` チェックを行いましょう
* Swift には、[Nil-Coalescing Operator](./../language-guide/basic-operators.md#nil-Coalescing-operatornil合体演算子)(`a ?? b`)があります。これは、オプショナルの値が存在する場合はオプショナルの値をアンラップし、`nil` の場合はデフォルト値を返します
* [Comparing Strings](./../language-guide/strings-and-characters.md#comparing-strings文字列の比較)セクションを更新、拡張し、文字列と文字の比較や、前方比較、後方比較は、拡張書記素クラスタの Unicode の正規等価性に基づいていることを反映し、例を示しました
* [Optional Chaining](./../language-guide/optional-chaining.md)を使って、mutating メソッドや演算子を呼び出し、subscript への値の割り当て、プロパティ値の設定を試すことができます。[Accessing Properties Through Optional Chaining](./../language-guide/optional-chaining.md#accessing-properties-through-optional-chainingオプショナルチェーンを通したプロパティへのアクセス)の情報は、これに応じて更新され、[Calling Methods Through Optional Chaining](./../language-guide/optional-chaining.md#calling-methods-through-optional-chainingオプショナルチェーンを通したメソッドの呼び出し)のメソッド呼び出しが成功したかどうかの例は、プロパティの設定が成功したかどうかをチェックする方法も例として示すように拡張しました
* オプショナルチェーンを通じて、オプショナルの subscript にアクセスするための新しいセクション([Accessing Subscripts of Optional Type](./../language-guide/optional-chaining.md#accessing-subscripts-of-optional-typeオプショナル型のsubscriptへのアクセス))を追加しました
* [Accessing and Modifying an Array](./../language-guide/collection-types.md#accessing-and-modifying-an-array配列へのアクセスと変更)を更新し、`+=` 演算子を使って配列にその単一の要素を追加できなくなりました。代わりに、`append(_:)` メソッドを使用するか、単一の要素を持つ配列に `+=` 演算子を使用して追加します
* 範囲演算子([Range Operators](./../language-guide/basic-operators.md#range-operators範囲演算子)) `a...b` と `a..<b` の開始値 `a` を、終了値 `b` より大きくしてはならないことを記載に記載しました
* [Inheritance](./../language-guide/inheritance.md)の章を書き直して、イニシャライザのオーバーライドの紹介部分を削除しました。この章では、サブクラス内に新しい機能を追加し、オーバーライドを使用して既存の機能を変更することに焦点を当てています。[Overriding Property Getters and Setters](./../language-guide/inheritance.md#overriding-property-getters-and-settersプロパティのgetプロパティ、setプロパティドのオーバーライド)の章の例は、`description` プロパティをオーバーライドする方法を示すために書き換えられました。(サブクラスのイニシャライザで継承したプロパティのデフォルト値を変更する例は、[Initialization](./../language-guide/initialization.md)の章に移動しました)
* [Initializer Inheritance and Overriding](./../language-guide/initialization.md#initializer-inheritance-and-overridingイニシャライザの継承とオーバーライド)セクションを更新し、指定イニシャライザのオーバーライドを `override` 修飾子でマークする必要があることを記載しました
* [Required Initializers](./../language-guide/initialization.md#required-initializers必須イニシャライザ)セクションを更新し、`required` 修飾子が全てのサブクラスの必須イニシャライザの実装の前に書かれ、自動的に継承したイニシャライザが必須イニシャライザの要件を満たすことができることを記載しました
* 中置演算子メソッド([Operator Methods](./../language-guide/advanced-operators.md#operator-methods演算子メソッド))に `@infix` 属性が必要なくなりました
* [Prefix and Postfix Operators](./../language-guide/advanced-operators.md#prefix-and-postfix-operators前置、後置演算子)の `@prefix` および `@postfix` 演算子属性は、`prefix` および  `postfix` 宣言修飾子に置き換えられました
* [Prefix and Postfix Operators](./../language-guide/advanced-operators.md#prefix-and-postfix-operators前置、後置演算子)が同じオペランドに適用されたときに、`prefix` と `postfix` 演算子が適用される順序についての記載を追加しました
* 複合代入演算子([Compound Assignment Operators](./../language-guide/advanced-operators.md#compound-assignment-operators複合代入演算子))の演算子関数は、関数を定義するときに `@assignment` 属性を使用しなくなりました
* カスタム演算子([Custom Operators](./../language-guide/advanced-operators.md#custom-operatorsカスタム演算子))を定義するときに修飾子を指定する順序が変更されました。例えば、`operator prefix` ではなく `prefix operator` と書くようになりました
* [Declaration Modifiers](./../language-reference/declarations.md#declaration-modifiers宣言修飾子)に、`dynamic` 修飾子についての情報を追加しました
* 型推論が[Literals](./../language-reference/lexical-structure.md#literalsリテラル)で動作する方法についての情報を追加しました
* カリー関数のより詳しい情報を追加しました
* [Access Control](./../language-guide/access-control.md)についての新しい章を追加しました
* [Strings and Characters](./../language-guide/strings-and-characters.md)の章を更新し、Swift の `Character` 型が単一の Unicode 拡張書記素クラスタを表すことを反映しました。[Extended Grapheme Clusters](./../language-guide/strings-and-characters.md#extended-grapheme-clusters拡張書記素クラスタ)上の新しいセクションと、[Unicode Scalar Values](./../language-guide/strings-and-characters.md#unicode-scalar-valuesUnicodeスカラ値)と[Comparing Strings](./../language-guide/strings-and-characters.md#comparing-strings文字列の比較)についての詳しい情報も含まれています
* [String Literals](./../language-guide/strings-and-characters.md#string-literals文字列リテラル)セクションを更新し、文字列内の Unicode スカラを `\u{n}`(n には 0 から 10FFFF の間の 16 進数が入る)で書けることを記載しました
* `NSString` の `length` プロパティは、`utf16count` ではなく、Swift のネイティブの `String` 型の `utf16count` にマッピングされるようになりました
* Swift のネイティブ `String` 型は、`uppercaseString` または `lowercasestring` プロパティを使用しなくなりました。[Strings and Characters](./../language-guide/strings-and-characters.md)の対応するセクションは削除され、様々なコード例を更新しました
* [Initializer Parameters Without Argument Labels](./../language-guide/initialization.md#initializer-parameters-without-argument-labels引数ラベルのないイニシャライザパラメータ)についての新しいセクションを追加しました
* [Required Initializers](./../language-guide/initialization.md#required-initializers必須イニシャライザ)についての新しいセクションを追加しました
* [Optional Tuple Return Types](./../language-guide/functions.md#optional-tuple-return-typesオプショナルのタプルの戻り値の型)についての新しいセクションを追加しました
* [Type Annotations](./../language-guide/the-basics.md#type-annotations型注釈)セクションを更新し、1 つの型注釈を持つ単一の行に複数の関連変数を定義できることを記載しました
* `@optional`、`@lazy`、`@final` と `@required` 属性は、`optional`、`lazy`、`final` と `required` 宣言修飾子([Declaration Modifiers](./../language-reference/declarations.md#declaration-modifiers宣言修飾子))になりました
* 本全体を参照して、`..<` を(「半閉鎖範囲演算子」ではなく)半開範囲演算子([Half-Open Range Operator](./../language-guide/basic-operators.md#half-Open-range-operator半開範囲演算子))として参照するように更新しました
* [Accessing and Modifying a Dictionary](./../language-guide/collection-types.md#accessing-and-modifying-a-dictionary辞書へのアクセスと変更)を更新し、`Dictionary` がブール型の `isEmpty` プロパティを持つことを記載しました
* カスタム演算子([Custom Operators](./../language-guide/advanced-operators.md#custom-operatorsカスタム演算子))を定義するときに使用できる文字の全リストを明確にしました
* `nil` とブール値 `true` と `false` は[Literals](./../language-reference/lexical-structure.md#literalsリテラル)になりました
* Swift の `Array` 型には、完全に値型のセマンティクスになりました。[Mutability of Collections](./../language-guide/collection-types.md#mutability-of-collectionsコレクションの可変性)と[Arrays](./../language-guide/collection-types.md#arrays配列)を更新し、新しいアプローチを反映しました
* 配列型の省略構文([Array Type Shorthand Syntax](./../language-guide/collection-types.md#array-type-shorthand-syntax配列型の省略構文))は `SomeType[]` ではなく `[SomeType]` として書くようになりました
* 辞書型の省略構文([Dictionary Type Shorthand Syntax](./../language-guide/collection-types.md#dictionary-type-shorthand-syntax辞書型の省略構文))は、`[KeyType: ValueType]` と書くように新しいセクションを追加しました
* セット型のハッシュ値([Hash Values for Set Types](./../language-guide/collection-types.md#hash-values-for-set-typesセット型のハッシュ値))についての新しいセクションを追加しました
* [Closure Expressions](./../language-guide/closures.md#closure-expressionsクロージャ式)の例に、グローバルの `sort(_:_:)` 関数ではなくグローバルの `sorted(_:_:)` 関数を使用して、配列の新しい値型のセマンティクスを反映しています
* [Memberwise Initializers for Structure Types](./../language-guide/initialization.md#memberwise-initializers-for-structure-types構造体のメンバワイズイニシャライザ)についての情報を更新し、構造体の格納プロパティにデフォルト値がない場合でもメンバワイズイニシャライザが使用可能だということを明確にしました
* 半開範囲演算子([Half-Open Range Operator](./../language-guide/basic-operators.md#half-Open-range-operator半開範囲演算子))に `..` ではなく `..<` を使用するようになりました
* [Extending a Generic Type](./../language-guide/generics.md#extending-a-generic-typeジェネリック型の拡張)の例を追加しました
