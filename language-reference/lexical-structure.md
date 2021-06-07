# Lexical Structure

最終更新日:

Swift の字句構造(*lexical structure*)は、文字のシーケンスが言語の有効なトークンを形成する方法を記述します。これらの有効なトークンは、言語の最小レベルの構成要素を形成し、後の章で言語の残りの部分を説明するために使用されます。トークンは、識別子、キーワード、句読点、リテラル、または演算子で構成されます。

ほとんどの場合、トークンは、下記に指定されている文法の制約内で、入力テキストから可能な限り最長の部分文字列を検討して、Swift ソースファイルの文字から生成されます。この動作は、最長一致(*longest match*)または最大ムンク(*maximal munch*)と呼ばれます。

## Whitespace and Comments(空白とコメント)

空白には 2 つの用途があります: ソースファイル内のトークンを分離する、または前置(*prefix*)、後置(*postfix*)、およびバイナリ演算子を区別し([Operators](#operators演算子)演算子を参照)、それ以外の場合は無視されます。次の文字は空白と見なされます: スペース(U+0020)、ラインフィード(U+000A)、キャリッジリターン (U+000D)、水平タブ(U+0009)、垂直タブ(U+000B)、フォームフィード(U +000C) および null(U+0000)。

コメントは、コンパイラによって空白として扱われます。単一行のコメントは `//` で始まり、ラインフィード(U+000A) またはキャリッジリターン(U+000D) まで続きます。複数行のコメントは `/*` で始まり、`*/` で終わります。複数行のコメントをネストすることはできますが、コメントマーカのバランスを取る必要があります。

コメントには、[Markup Formatting Reference](https://developer.apple.com/library/content/documentation/Xcode/Reference/xcode_markup_formatting_ref/index.html)で説明されているように、追加のフォーマットとマークアップを含めることができます。

> GRAMMAR OF WHITESPACE  
> *whitespace* → whitespace-item whitespace<sub>*opt*</sub>  
> *whitespace-item* → line-break  
> *whitespace-item* → inline-space  
> *whitespace-item* → comment  
> *whitespace-item* → multiline-comment  
> *whitespace-item* → U+0000, U+000B, or U+000C  
> 
> *line-break* → U+000A  
> *line-break* → U+000D  
> *line-break* → U+000D followed by U+000A  
> 
> *inline-spaces* → inline-space inline-spaces<sub>*opt*</sub>  
> *inline-space* → U+0009 or U+0020  
> 
> *comment* → **//** comment-text *line-break*  
> *multiline-comment* → **/*** multiline-comment-text ***/**  
> *comment-text* → comment-text-item comment-text<sub>*opt*</sub>  
> *comment-text-item* → Any Unicode scalar value except U+000A or U+000D  
> 
> *multiline-comment-text* → multiline-comment-text-item  
> *multiline-comment-text*<sub>*opt*</sub>  
> *multiline-comment-text-item* → multiline-comment  
> *multiline-comment-text-item* → comment-text-item  
> *multiline-comment-text-item* → Any Unicode scalar value except ****/* or ***/**

## Identifiers(識別子)

識別子(*identifier*)は、大文字または小文字の `A` から `Z`、アンダースコア(`_`)、基本多言語平面内の非結合英数字 `Unicode` 文字、または私用面にない基本多言語平面外の文字で始まります。最初の文字の後に、数字と `Unicode` 文字の組み合わせも許可されます。

予約語を識別子として使用するには、その前後にバッククォート(\`)を置きます。例えば、`class` は有効な識別子ではありませんが、\`class\` は有効です。バッククォートは識別子の一部とは見なされません。\`x\`と `x` は同じ意味です。

明示的な引数名のないクロージャー内では、引数には暗黙的に `$0`、`$1`、`$2` などの名前が付けられます。これらの名前は、クロージャのスコープ内で有効な識別子です。

コンパイラは、プロパティラッパプロジェクションを持つプロパティはドル記号(`$`)で始まる識別子を合成します。コードはこれらの識別子とやり取りできますが、その前置で識別子を宣言することはできません。詳細については、[Attributes](./attributes.md)の章の[propertyWrapper](./attributes.md#propertyWrapperプロパティラッパ)セクションを参照ください。

## Keywords and Punctuation(単語と句読点)

次のキーワードは予約されており、識別子として前述のようにバッククォート(\`)でエスケープしない限り使用できません。`inout`、`var`、`let` 以外のキーワードは、バッククォートでエスケープすることなく、関数宣言または関数呼び出しで引数名として使用できます。メンバの名前がキーワードと同じ場合、そのメンバへの参照をバッククォートでエスケープする必要はありません。ただし、メンバの参照とキーワードの使用の間にあいまいさがある場合を除きます。例えば、`self`、`Type`、および `Protocol` には明示的なメンバ式の中で特別な意味があるため、そのコンテキストではバッククォートでエスケープする必要があります。

* 宣言で使用されるキーワード: `associatedtype`、`class`、`deinit`、`enum`、`extension`、`fileprivate`、`func`、`import`、`init`、`inout`、`internal`、`let`、`open`、`operator`、`private`、`protocol`、`public`、`rethrows`、`static`、`struct`、`subscript`、`typealias`、および `var`
* ステートメントで使用されるキーワード: `break`、`case`、`continue`、`default`、`defer`、`do`、`else`、`fallthrough`、`for`、`guard`、`if`、`in`、`repeat`、`return`、`switch`、`where`、`while`
* 式と型で使用されるキーワード: `as`、`Any`、`catch`、`false`、`is`、`nil`、`super`、`self`、`Self`、`throw`、`throws`、`true`、および `try`
* パターンで使用されるキーワード: `_`
* シャープ記号(`#`)で始まるキーワード: `#available`、`#colorLiteral`、`#column`、`#else`、`#elseif`、`#endif`、`#error`、`#file`、`#fileID`、`#fileLiteral`、`#filePath`、`#function`、`#if`、`#imageLiteral`、`#line`、`#selector`、`#sourceLocation`、および `#warning`
* 特定のコンテキストで予約されているキーワード: `associativity`、`convenient`、`dynamic`、`didSet`、`final`、`get`、`infix`、`indirect`、`lazy`、`left`、`mutating`、`none`、`nonmutating`、`optional`、`override`、`postfix`、`precedence`、`prefix`、プロトコル、`Protocol`、 `required`、`right`、`set`、`Type`、 `unowned`、 `weak`、 および `willSet`。該当するコンテキスト外では、識別子として使用することができます

次のトークンは句読点として予約されており、カスタム演算子としては使用できません: `(`, `)`, `{`, `}`, `[`, `]`, `.`, `,`, `:`, `;`, `=`, `@`, `#`, `&`(前置演算子として)、`->`、\`、`?`、および `!` (後置演算子として)

## Literals(リテラル)

リテラル(*literal*)は、数値や文字列などの型の値のソースコード表現です。

以下はリテラルの例です。

```swift
42               // 整数リテラル
3.14159          // 浮動小数点リテラル
"Hello, world!"  // 文字列リテラル
true             // ブールリテラル
```

リテラルにはそれ自体に型はありません。代わりに、リテラルは無限の精度を持つものとして解析され、Swift の型推論はリテラルの型を推論しようとします。例えば、宣言 `let x: Int8 = 42` では、Swift は明示的な型アノテーション(`: Int8`)を使用して、整数リテラル `42` の型が `Int8` だと推論します。適切な型情報が利用できない場合、Swift はリテラルの型が Swift 標準ライブラリで定義されているデフォルトのリテラル型の 1 つだと推論します。デフォルトの型は、整数リテラルの場合は `Int`、浮動小数点リテラルの場合は `Double`、文字列リテラルの場合は `String`、ブールリテラルの場合は `Bool` です。例えば、`let str = "Hello, world"` という宣言では、文字列リテラル `"Hello, world"` のデフォルトの推論型は `String` です。

リテラル値の型アノテーションを指定する場合、アノテーションの型は、そのリテラル値からインスタンス化できる型にする必要があります。つまり、型は次の Swift 標準ライブラリプロトコルのいずれかに準拠する必要があります: 整数リテラルは `ExpressibleByIntegerLiteral`、浮動小数点リテラルは `ExpressibleByFloatLiteral`、文字列リテラルは `ExpressibleByStringLiteral`、ブールリテラルは `ExpressibleByBooleanLiteral`、単一の Unicode スカラーのみを含む文字列リテラルは `ExpressibleByUnicodeScalarLiteral`、および単一の拡張書記素クラスタのみを含む文字列リテラルは `ExpressibleByExtendedGraphemeClusterLiteral`。例えば、`Int8` は `ExpressibleByIntegerLiteral` プロトコルに準拠しているため、宣言 `let x: Int8 = 42` の整数リテラル `42` の型アノテーションで使用できます。

> GRAMMAR OF A LITERAL  
> *literal* → numeric-literal | string-literal | boolean-literal | nil-literal  
> *numeric-literal* → -<sub>*opt*</sub> integer-literal | -<sub>*opt*</sub> floating-point-literal  
> *boolean-literal* → **true** | **false**  
> *nil-literal* → **nil**

### Integer Literals(整数リテラル)

### Floating-Point Literals(浮動小数点リテラル)

### String Literals((文字列リテラル))

## Operators(演算子)
