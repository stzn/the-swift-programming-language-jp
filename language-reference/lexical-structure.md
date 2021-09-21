# 構文の構造\(Lexical Structure\)

最終更新日: 2021/9/21

Swift の_字句構造_は、文字のシーケンスの中で Swift 言語で有効なトークンを形成する方法を示します。これらの有効なトークンは、言語の最小レベルの構成要素を形成し、後の章で言語の残りの部分を説明することに使用されます。トークンは、識別子、キーワード、句読点、リテラル、または演算子で構成されます。

ほとんどの場合、トークンは、下記に指定されている文法の制約内で、入力テキストから可能な限り最長の部分文字列を検討して、Swift ソースファイルの文字から形成されます。この動作は、_最長一致_または_最大ムンク_と呼ばれます。

## 空白とコメント\(Whitespace and Comments\)

空白には 2 つの用途があります: 1 つは、ソースファイル内のトークンを分離する、もう 1 つは、_前置_、_後置_、およびバイナリ演算子を区別する\([Operators\(演算子\)](../language-reference/lexical-structure.md#operators)を参照\)、それ以外の場合は無視されます。次の文字は空白と見なされます: スペース\(U+0020\)、ラインフィード\(U+000A\)、キャリッジリターン \(U+000D\)、水平タブ\(U+0009\)、垂直タブ\(U+000B\)、フォームフィード\(U +000C\) および null\(U+0000\)。

コメントは、コンパイラによって空白として扱われます。単一行のコメントは `//` で始まり、ラインフィード\(U+000A\) またはキャリッジリターン\(U+000D\) まで続きます。複数行のコメントは `/*` で始まり、`*/` で終わります。複数行のコメントをネストすることはできますが、コメントマーカで始まりと終わりを囲む必要があります。

コメントには、[Markup Formatting Reference](https://developer.apple.com/library/content/documentation/Xcode/Reference/xcode_markup_formatting_ref/index.html)で説明されているように、追加のフォーマットとマークアップを含めることができます。

> GRAMMAR OF WHITESPACE  
> whitespace → [whitespace-item](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_whitespace-item) [whitespace](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_whitespace)<sub>opt</sub>  
> whitespace-item → [line-break](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_line-break)  
> whitespace-item → [inline-space](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_inline-space)  
> whitespace-item → [comment](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_comment)  
> whitespace-item → [multiline-comment](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_multiline-comment)  
> whitespace-item → U+0000, U+000B, または U+000C  
> line-break → U+000A  
> line-break → U+000D  
> line-break → U+000D に続く U+000A  
> inline-spaces → [inline-space](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_inline-space) [inline-spaces](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_inline-spaces)<sub>opt</sub>  
> inline-space → U+0009 または U+0020  
> comment → `//` [comment-text](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_comment-text) [line-break](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_line-break)  
> multiline-comment → `/*` [multiline-comment-text](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_multiline-comment-text) `*/`  
> comment-text → [comment-text-item](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_comment-text-item) [comment-text](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_comment-text)<sub>opt</sub>  
> comment-text-item → U+000A または U+000D を除く任意のUnicodeスカラ値  
> multiline-comment-text → [multiline-comment-text-item](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_multiline-comment-text-item) [multiline-comment-text](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_multiline-comment-text)<sub>opt</sub>  
> multiline-comment-text-item → [multiline-comment](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_multiline-comment)  
> multiline-comment-text-item → [comment-text-item](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_comment-text-item)  
> multiline-comment-text-item → `/*` または `*/` を除く任意のUnicodeスカラ値

## <a id="identifiers">識別子\(Identifiers\)</a>

_識別子_は、大文字または小文字の `A` から `Z`、アンダースコア\(`_`\)、基本多言語平面内の非結合英数 `Unicode` 文字、または私用面にない基本多言語平面外の文字で始まります。最初の文字の後に、数字と `Unicode` 文字を組み合わせることもできます。

宣言に `public` アクセスレベル修飾子がある場合でも、アンダースコアで始まる識別子を internal として扱います。 この規則により、フレームワークの作成者は、宣言を public にしなければならない制限がある場合でも、クライアントが対話したり依存したりしてはならない API の一部をマークできます。 さらに、2 つのアンダースコアで始まる識別子は、Swift コンパイラと標準ライブラリ用に予約されています。

予約語を識別子として使用するには、その前後にバッククォート\(\`\)を置きます。例えば、`class` は有効な識別子ではありませんが、\`class\` は有効です。バッククォートは識別子の一部とは見なされません。\`x\`と `x` は同じ意味です。

明示的な引数名のないクロージャ内では、引数には暗黙的に `$0`、`$1`、`$2` などの名前が付けられます。これらの名前は、クロージャのスコープ内で有効な識別子です。

コンパイラは、プロパティラッパプロジェクション値を持つプロパティにドル記号\(`$`\)で始まる識別子を合成します。コード上でこれらの識別子とやり取りできますが、ドル記号\(`$`\)から始まる識別子を宣言することはできません。詳細については、[Attributes\(属性\)](attributes.md)の章の[propertyWrapper](../language-reference/attributes.md#propertywrapper)セクションを参照ください。

> GRAMMAR OF AN IDENTIFIER  
> identifier → [identifier-head](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_identifier-head) [identifier-characters](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_identifier-characters)<sub>opt</sub>  
> identifier → **\`** [identifier-head](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_identifier-head) [identifier-characters](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_identifier-characters)<sub>opt</sub> **\`**  
> identifier → [implicit-parameter-name](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_implicit-parameter-name)  
> identifier → [property-wrapper-projection](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_property-wrapper-projection)  
> identifier-list → [identifier](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_identifier) \| [identifier](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_identifier) `,` [identifier-list](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_identifier-list)  
> identifier-head → 大文字または小文字の A ~ Z  
> identifier-head → `_`  
> identifier-head → U+00A8, U+00AA, U+00AD, U+00AF, U+00B2–U+00B5, または U+00B7–U+00BA  
> identifier-head → U+00BC–U+00BE, U+00C0–U+00D6, U+00D8–U+00F6, または U+00F8–U+00FF  
> identifier-head → U+0100–U+02FF, U+0370–U+167F, U+1681–U+180D, または U+180F–U+1DBF  
> identifier-head → U+1E00–U+1FFF  
> identifier-head → U+200B–U+200D, U+202A–U+202E, U+203F–U+2040, U+2054, または U+2060–U+206F  
> identifier-head → U+2070–U+20CF, U+2100–U+218F, U+2460–U+24FF, または U+2776–U+2793  
> identifier-head → U+2C00–U+2DFF または U+2E80–U+2FFF  
> identifier-head → U+3004–U+3007, U+3021–U+302F, U+3031–U+303F, または U+3040–U+D7FF  
> identifier-head → U+F900–U+FD3D, U+FD40–U+FDCF, U+FDF0–U+FE1F, または U+FE30–U+FE44  
> identifier-head → U+FE47–U+FFFD  
> identifier-head → U+10000–U+1FFFD, U+20000–U+2FFFD, U+30000–U+3FFFD, または U+40000–U+4FFFD  
> identifier-head → U+50000–U+5FFFD, U+60000–U+6FFFD, U+70000–U+7FFFD, または U+80000–U+8FFFD  
> identifier-head → U+90000–U+9FFFD, U+A0000–U+AFFFD, U+B0000–U+BFFFD, または U+C0000–U+CFFFD  
> identifier-head → U+D0000–U+DFFFD または U+E0000–U+EFFFD  
> identifier-character → 数値 0 ~ 9  
> identifier-character → U+0300–U+036F, U+1DC0–U+1DFF, U+20D0–U+20FF, または U+FE20–U+FE2F  
> identifier-character → [identifier-head](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_identifier-head)  
> identifier-characters → [identifier-character](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_identifier-character) [identifier-characters](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_identifier-characters)<sub>opt</sub>  
> implicit-parameter-name → `$` [decimal-digits](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_decimal-digits)  
> property-wrapper-projection → `$` [identifier-characters](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_identifier-characters)

## <a id="keywords-and-punctuation">単語と句読点\(Keywords and Punctuation\)</a>

次のキーワードは予約されており、識別子として前述のようにバッククォート\(\`\)でエスケープしない限り使用できません。`inout`、`var`、`let` 以外のキーワードは、バッククォートでエスケープすることなく、関数宣言または関数の引数名として使用できます。メンバの名前がキーワードと同じ場合、メンバの参照とキーワードの使用の間にあいまいさがある場合を除いて、そのメンバへの参照をバッククォートでエスケープする必要はありません。例えば、`self`、`Type`、および `Protocol` には明示的なメンバ式の中で特別な意味があるため、そのコンテキストではバッククォートでエスケープする必要があります。

* 宣言で使用されるキーワード: `associatedtype`、`class`、`deinit`、`enum`、`extension`、`fileprivate`、`func`、`import`、`init`、`inout`、`internal`、`let`、`open`、`operator`、`private`、`protocol`、`public`、`rethrows`、`static`、`struct`、`subscript`、`typealias`、および `var`
* ステートメントで使用されるキーワード: `break`、`case`、`continue`、`default`、`defer`、`do`、`else`、`fallthrough`、`for`、`guard`、`if`、`in`、`repeat`、`return`、`switch`、`where`、`while`
* 式と型で使用されるキーワード: `as`、`Any`、`catch`、`false`、`is`、`nil`、`super`、`self`、`Self`、`throw`、`throws`、`true`、および `try`
* パターンで使用されるキーワード: `_`
* シャープ記号\(`#`\)で始まるキーワード: `#available`、`#colorLiteral`、`#column`、`#else`、`#elseif`、`#endif`、`#error`、`#file`、`#fileID`、`#fileLiteral`、`#filePath`、`#function`、`#if`、`#imageLiteral`、`#line`、`#selector`、`#sourceLocation`、および `#warning`
* 特定のコンテキストで予約されているキーワード: `associativity`、`convenient`、`dynamic`、`didSet`、`final`、`get`、`infix`、`indirect`、`lazy`、`left`、`mutating`、`none`、`nonmutating`、`optional`、`override`、`postfix`、`precedence`、`prefix`、プロトコル、`Protocol`、 `required`、`right`、`set`、`Type`、 `unowned`、 `weak`、 および `willSet`。該当するコンテキスト外では、識別子として使用することができます

次のトークンは句読点として予約されており、カスタム演算子としては使用できません: `(`、 `)`、 `{`、 `}`、 `[`、 `]`、 `.`、 `,`、 `:`、 `;`、 `=`、 `@`、 `#`、 `&`\(前置演算子として\)、`->`、\`、`?`、および `!`\(後置演算子として\)

## <a id="literals">リテラル\(Literals\)</a>

_リテラル_は、数値や文字列といった型の値をソースコード上で表現するものです。

下記はリテラルの例です。

```swift
42  // 整数リテラル
3.14159  // 浮動小数点リテラル
"Hello, world!"  // 文字列リテラル
true  // ブールリテラル
```

リテラルにはそれ自体に型はありません。代わりに、リテラルは無限の精度を持つものとして解析され、Swift の型推論はリテラルの型を推論しようとします。例えば、宣言 `let x: Int8 = 42` では、Swift は明示的な型注釈\(`: Int8`\)を使用すると、整数リテラル `42` の型が `Int8` だと推論します。適切な型情報が利用できない場合、Swift はリテラルの型が Swift 標準ライブラリで定義されているデフォルトのリテラル型の 1 つだと推論します。デフォルトの型は、整数リテラルの場合は `Int`、浮動小数点リテラルの場合は `Double`、文字列リテラルの場合は `String`、ブールリテラルの場合は `Bool` です。例えば、`let str = "Hello, world"` という宣言では、文字列リテラル `"Hello, world"` のデフォルトの推論型は `String` です。

リテラル値の型注釈を指定する場合、型注釈は、そのリテラル値からインスタンス化できる型にする必要があります。つまり、型は次の Swift 標準ライブラリのプロトコルのいずれかに準拠する必要があります: 整数リテラルは `ExpressibleByIntegerLiteral`、浮動小数点リテラルは `ExpressibleByFloatLiteral`、文字列リテラルは `ExpressibleByStringLiteral`、ブールリテラルは `ExpressibleByBooleanLiteral`、単一の Unicode スカラのみを含む文字列リテラルは `ExpressibleByUnicodeScalarLiteral`、および単一の拡張書記素クラスタのみを含む文字列リテラルは `ExpressibleByExtendedGraphemeClusterLiteral`。例えば、`Int8` は `ExpressibleByIntegerLiteral` プロトコルに準拠しているため、宣言 `let x: Int8 = 42` の整数リテラル `42` の型注釈で使用できます。

> GRAMMAR OF A LITERAL  
> literal → [numeric-literal](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_numeric-literal) \| [string-literal](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_string-literal) \| [boolean-literal](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_boolean-literal) \| [nil-literal](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_nil-literal)  
> numeric-literal → `-`<sub>opt</sub> [integer-literal](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_integer-literal) \| `-`<sub>opt</sub> [floating-point-literal](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_floating-point-literal)  
> boolean-literal → `true` \| `false`  
> nil-literal → `nil`

### 整数リテラル\(Integer Literals\)

_整数リテラル_は、不特定の精度の整数値を表します。デフォルトでは、整数リテラルは 10 進数で表されます。プレフィックスを使用して他の基数を指定できます。2 進数リテラルは `0b` で始まり、8 進数リテラルは `0o` で始まり、16 進数リテラルは `0x` で始まります。

10 進数リテラルには `0` ～ `9` の数字が含まれます。2 進数リテラルには `0` と `1` が含まれ、8 進数リテラルには `0` ～ `7` が含まれ、16 進数リテラルには `0` ～ `9` および大文字または小文字の `A` ～ `F` が含まれます。

負の整数リテラルは、`-42` のように、整数リテラルの前にマイナス記号\(`-`\)を付けることで表現されます。

アンダースコア\(`_`\)を読みやすくするために数字の間に使用できますが、それ自体は無視されるため、リテラルの値には影響しません。整数リテラルは先行ゼロ\(`0`\)で始めることができますが、同様に無視され、リテラルの基数または値には影響しません。

特に指定がない限り、整数リテラルのデフォルトの推論型は Swift 標準ライブラリの `Int` 型です。Swift 標準ライブラリは、[Integers\(整数\)](../language-guide/the-basics.md#integers)で説明されているように、様々なサイズの符号付き整数および符号なし整数の型も定義します。

> GRAMMAR OF AN INTEGER LITERAL  
> integer-literal → [binary-literal](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_binary-literal)  
> integer-literal → [octal-literal](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_octal-literal)  
> integer-literal → [decimal-literal](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_decimal-literal)  
> integer-literal → [hexadecimal-literal](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_hexadecimal-literal)  
> binary-literal → `0b` [binary-digit](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_binary-digit) [binary-literal-characters](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_binary-literal-characters)<sub>opt</sub>  
> binary-digit → 数値 0 または 1  
> binary-literal-character → [binary-digit](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_binary-digit) \| `_`  
> binary-literal-characters → [binary-literal-character](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_binary-literal-character) [binary-literal-characters](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_binary-literal-characters)<sub>opt</sub>  
> octal-literal → `0o` [octal-digit](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_octal-digit) [octal-literal-characters](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_octal-literal-characters)<sub>opt</sub>  
> octal-digit → 数値 0 ~ 7 octal-literal-character → [octal-digit](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_octal-digit) \| `_`  
> octal-literal-characters → [octal-literal-character](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_octal-literal-character) [octal-literal-characters](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_octal-literal-characters)<sub>opt</sub>  
> decimal-literal → [decimal-digit](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_decimal-digit) [decimal-literal-characters](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_decimal-literal-characters)<sub>opt</sub>  
> decimal-digit → 数値 0 ~ 9  
> decimal-digits → [decimal-digit](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_decimal-digit) [decimal-digits](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_decimal-digits)<sub>opt</sub>  
> decimal-literal-character → [decimal-digit](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_decimal-digit) \| `_`  
> decimal-literal-characters → [decimal-literal-character](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_decimal-literal-character) [decimal-literal-characters](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_decimal-literal-characters)<sub>opt</sub>  
> hexadecimal-literal → `0x` [hexadecimal-digit](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_hexadecimal-digit) [hexadecimal-literal-characters](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_hexadecimal-literal-characters)<sub>opt</sub>  
> hexadecimal-digit → 数値 0 ~ 9、 a ~ f、または A ~ F  
> hexadecimal-literal-character → [hexadecimal-digit](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_hexadecimal-digit) \| `_`  
> hexadecimal-literal-characters → [hexadecimal-literal-character](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_hexadecimal-literal-character) [hexadecimal-literal-characters](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_hexadecimal-literal-characters)<sub>opt</sub>

### 浮動小数点リテラル\(Floating-Point Literals\)

_浮動小数点リテラル_は、不特定の精度の浮動小数点値を表します。

デフォルトでは、浮動小数点リテラルは 10 進数\(前置なし\)で表されますが、16 進数\(`0x` 前置付き\)で表すこともできます。

10 進数の浮動小数点リテラルは、10 進数の数字のシーケンスと、その後に続く 10 進数の小数、10 進数の指数、またはその両方で構成されます。小数部は、小数点\(`.`\)とそれに続く一連の 10 進数で構成されます。指数は、大文字または小文字の前置の `e` と、その後に続く 10 進数の数字のシーケンスで構成され、`e` の前の値に 10 の累乗が掛けられます。例えば、`1.25e2` は 1.25 x $$10^2$$ を表し、`125.0` に評価されます。同様に、`1.25e-2` は 1.25 x $$10^{-2}$$ を表し、`0.0125` に評価されます。

16 進数の浮動小数点リテラルは、`0x` 前置、オプションの 16 進数の小数、16 進数の指数で構成されます。16 進数は、小数点とそれに続く 16 進数の数字で構成されます。指数は、大文字または小文字の前置の `p` と、その後に続く 10 進数の数字のシーケンスで構成されます。これは、`p` の前の値に乗算される 2 のべき乗を示します。例えば、`0xFp2` は 15 x $$2^2$$ を表し、`60` に評価されます。同様に、`0xFp-2` は 15 x $$2^{-2}$$ を表し、`3.75` に評価されます。

負の浮動小数点リテラルは、`-42.5` のように、浮動小数点リテラルの前にマイナス記号\(`-`\)を付けることで表現されます。

アンダースコア\(`_`\)を読みやすくするために数字の間に使用できますが、無視されるため、リテラルの値には影響しません。浮動小数点リテラルは先行ゼロ\(`0`\)で始めることができますが、同様に無視され、リテラルの基数または値には影響しません。

特に指定されていない限り、浮動小数点リテラルのデフォルトの推論型は、64 ビット浮動小数点数を表す Swift の標準ライブラリ `Double` 型です。Swift 標準ライブラリでは、32 ビットの浮動小数点数を表す `Float` 型も定義されています。

> GRAMMAR OF A FLOATING-POINT LITERAL  
> floating-point-literal → [decimal-literal](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_decimal-literal) [decimal-fraction](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_decimal-fraction)<sub>opt</sub> [decimal-exponent](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_decimal-exponent)<sub>opt</sub>  
> floating-point-literal → [hexadecimal-literal](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_hexadecimal-literal) [hexadecimal-fraction](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_hexadecimal-fraction)<sub>opt</sub> [hexadecimal-exponent](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_hexadecimal-exponent)  
> decimal-fraction → `.` [decimal-literal](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_decimal-literal)  
> decimal-exponent → [floating-point-e](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_floating-point-e) [sign](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_sign)<sub>opt</sub> [decimal-literal](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_decimal-literal)  
> hexadecimal-fraction → `.` [hexadecimal-digit](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_hexadecimal-digit) [hexadecimal-literal-characters](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_hexadecimal-literal-characters)<sub>opt</sub>  
> hexadecimal-exponent → [floating-point-p](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_floating-point-p) [sign](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_sign)<sub>opt</sub> [decimal-literal](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_decimal-literal)  
> floating-point-e → `e` \| `E`  
> floating-point-p → `p` \| `P`  
> sign → `+` \| `-`

### <a id="lexical-structure-string-literals">文字列リテラル\(String Literals\)</a>

_文字列リテラル_は、引用符で囲まれた一連の文字です。単一行の文字列リテラルは二重引用符で囲まれ、次の形式になります:

![&#x5358;&#x4E00;&#x884C;&#x306E;&#x6587;&#x5B57;&#x5217;&#x30EA;&#x30C6;&#x30E9;&#x30EB;](../assets/singleline_strings.png)

文字列リテラルには、エスケープされていない二重引用符\(`"`\)、エスケープされていないバックスラッシュ \(`\`\)、キャリッジリターン、またはラインフィードを含めることはできません。

複数行の文字列リテラルは、3 つの二重引用符で囲まれ、次の形式になります。

![&#x8907;&#x6570;&#x884C;&#x306E;&#x6587;&#x5B57;&#x5217;&#x30EA;&#x30C6;&#x30E9;&#x30EB;](../assets/multipleline_strings.png)

単一行の文字列リテラルとは異なり、複数行の文字列リテラルには、エスケープされていない二重引用符 \(`"`\)、キャリッジリターン、およびラインフィードを含めることができます。3 つのエスケープされていない二重引用符を隣り合わせに含めることはできません。

複数行の文字列リテラルを開始する `"""` の後の改行は文字列の一部ではありません。リテラルを終了する `"""` の前の改行も文字列の一部ではありません。改行で開始または終了する複数行の文字列リテラルを作成するには、最初または最後の行として空白行を記述します。

複数行の文字列リテラルは、スペースとタブの任意の組み合わせを使用してインデントできます。このインデントは文字列に含まれていません。リテラルを終了する `"""` がインデントを決定します。リテラル内のすべての非空白行は、終わりの `"""` の前に表示されるものと同じインデントで開始する必要があります。タブとスペース間の変換はありません。インデントの後に追加のスペースとタブを含めることができます。これらのスペースとタブは文字列に表示されます。

複数行の文字列リテラルの改行は、改行文字を使用するように正規化されます。ソースファイルにキャリッジリターンとラインフィードが混在している場合でも、文字列内のすべての改行は同じになります。

複数行の文字列リテラルでは、行末にバックスラッシュ\(`\`\)を記述すると、文字列からその改行が省略されます。バックスラッシュと改行の間の空白もすべて省略されます。この構文を使用すると、結果の文字列の値を変更せずに、ソースコードで複数行の文字列リテラルをハードラップできます。

次のエスケープシーケンスを使用して、単一行形式と複数行形式の両方の文字列リテラルに特殊文字を含めることができます:

* ヌル文字\(`\0`\)
* バックスラッシュ\(`\`\)
* 水平タブ\(`\t`\)
* 改行\(`\n`\)
* キャリッジリターン\(`\r`\)
* 二重引用符\(`\"`\)
* 単一引用符\(`\'`\)
* Unicode スカラー\(`\u{n}`\)、`n` は 1 ～ 8 桁の 16 進数

式の値を文字列リテラルに挿入するには、式をバックスラッシュ\(`\`\)の後に括弧で囲みます。補間された式には文字列リテラルを含めることができますが、エスケープされていないバックスラッシュ、キャリッジリターン、またはラインフィードを含めることはできません。

例えば、次の文字列リテラルはすべて同じ値です:

```swift
"1 2 3"
"1 2 \("3")"
"1 2 \(3)"
"1 2 \(1 + 2)"
let x = 3; "1 2 \(x)"
```

拡張区切り文字で区切られた文字列は、引用符で囲まれた一連の文字と、1 つ以上の文字列の両側に同じ数のシャープ記号\(`#`\) のセットです。拡張区切り文字で区切られた文字列には、次の形式があります:

![&#x62E1;&#x5F35;&#x533A;&#x5207;&#x308A;&#x6587;&#x5B57;&#x3067;&#x533A;&#x5207;&#x3089;&#x308C;&#x305F;&#x6587;&#x5B57;&#x5217;](../assets/string_delimited_by_extended_delimiters.png)

拡張区切り文字で区切られた文字列内の特殊文字は、通常の文字として表示されます。拡張区切り文字を使用して、通常は文字列補間の生成、エスケープシーケンスの開始、文字列の終了などの特殊な効果を持つ文字を使って通常の文字列を作成できます。

次の例は、同等の文字列値を作成する拡張区切り文字で区切られた文字列リテラルと文字列を示しています:

```swift
let string = #"\(x) \ " \u{2603}"#
let escaped = "\\(x) \\ \" \\u{2603}"
print(string)
// \(x) \ " \u{2603}
print(string == escaped)
// true
```

複数の番号記号を使用して、拡張区切り文字で区切られた文字列を形成する場合は、番号記号の間に空白を入れないでください。

```swift
print(###"Line 1\###nLine 2"###) // OK
print(# # #"Line 1\# # #nLine 2"# # #) // Error
```

拡張区切り文字を使用して作成する複数行の文字列リテラルには、通常の複数行の文字列リテラルと同じインデントの要件が適用されます。

文字列リテラルのデフォルトで推論される型は `String` です。`String` 型の詳細については、[Strings and Characters\(文字列と文字\)](../language-guide/strings-and-characters.md)、[String](https://developer.apple.com/documentation/swift/string)を参照ください。

`+` 演算子によって連結された文字列リテラルは、コンパイル時に連結されます。例えば、下記の例の `textA` と `textB` の値は同じで、実行時に連結は実行されません。

```swift
let textA = "Hello " + "world"
let textB = "Hello world"
```

> GRAMMAR OF A STRING LITERAL  
> string-literal → [static-string-literal](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_static-string-literal) \| [interpolated-string-literal](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_interpolated-string-literal)  
> string-literal-opening-delimiter → [extended-string-literal-delimiter](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_extended-string-literal-delimiter)<sub>opt</sub> `"`  
> string-literal-closing-delimiter → `"` [extended-string-literal-delimiter](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_extended-string-literal-delimiter)<sub>opt</sub>  
> static-string-literal → [string-literal-opening-delimiter](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_string-literal-opening-delimiter) [quoted-text](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_quoted-text)<sub>opt</sub> [string-literal-closing-delimiter](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_string-literal-closing-delimiter)  
> static-string-literal → [multiline-string-literal-opening-delimiter](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_multiline-string-literal-opening-delimiter) [multiline-quoted-text](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_multiline-quoted-text)<sub>opt</sub> [multiline-string-literal-closing-delimiter](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_multiline-string-literal-closing-delimiter)  
> multiline-string-literal-opening-delimiter → [extended-string-literal-delimiter](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_extended-string-literal-delimiter) `"""`  
> multiline-string-literal-closing-delimiter → `"""` [extended-string-literal-delimiter](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_extended-string-literal-delimiter)  
> extended-string-literal-delimiter → `#` [extended-string-literal-delimiter](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_extended-string-literal-delimiter)<sub>opt</sub>  
> quoted-text → [quoted-text-item](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_quoted-text-item) [quoted-text](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_quoted-text)<sub>opt</sub>  
> quoted-text-item → [escaped-character](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_escaped-character)  
> quoted-text-item → **"**、**\**、 U+000A、または U+000D を除く任意のUnicodeスカラ値  
> multiline-quoted-text → [multiline-quoted-text-item](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_multiline-quoted-text-item) [multiline-quoted-text](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_multiline-quoted-text)<sub>opt</sub>  
> multiline-quoted-text-item → [escaped-character](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_escaped-character)  
> multiline-quoted-text-item → **\** を除く任意のUnicodeスカラ値  
> multiline-quoted-text-item → [escaped-newline](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_escaped-newline)  
> interpolated-string-literal → [string-literal-opening-delimiter](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_string-literal-opening-delimiter) [interpolated-text](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_interpolated-text)<sub>opt</sub> [string-literal-closing-delimiter](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_string-literal-closing-delimiter)  
> interpolated-string-literal → [multiline-string-literal-opening-delimiter](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_multiline-string-literal-opening-delimiter) [multiline-interpolated-text](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_multiline-interpolated-text)<sub>opt</sub> [multiline-string-literal-closing-delimiter](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_multiline-string-literal-closing-delimiter)  
> interpolated-text → [interpolated-text-item](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_interpolated-text-item) [interpolated-text](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_interpolated-text)<sub>opt</sub>  
> interpolated-text-item → `\(` [expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_expression) `)` \| [quoted-text-item](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_quoted-text-item)  
> multiline-interpolated-text → [multiline-interpolated-text-item](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_multiline-interpolated-text-item) [multiline-interpolated-text](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_multiline-interpolated-text)<sub>opt</sub>  
> multiline-interpolated-text-item → `\(` [expression](https://docs.swift.org/swift-book/ReferenceManual/Expressions.html#grammar_expression) `)` \| [multiline-quoted-text-item](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_multiline-quoted-text-item)  
> escape-sequence → `\` [extended-string-literal-delimiter](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_extended-string-literal-delimiter)  
> escaped-character → [escape-sequence](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_escape-sequence) `0` \| [escape-sequence](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_escape-sequence) `\` \| [escape-sequence](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_escape-sequence) `t` \| [escape-sequence](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_escape-sequence) `n` \| [escape-sequence](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_escape-sequence) `r` \| [escape-sequence](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_escape-sequence) `"` \| [escape-sequence](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_escape-sequence) `'`  
> escaped-character → [escape-sequence](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_escape-sequence) `u` `{` [unicode-scalar-digits](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_unicode-scalar-digits) `}`  
> unicode-scalar-digits → 0 から 8 までの16進数  
> escaped-newline → [escape-sequence](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_escape-sequence) [inline-spaces](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_inline-spaces)<sub>opt</sub> [line-break](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_line-break)

## <a id="operators">演算子\(Operators\)</a>

Swift 標準ライブラリは、数多くの_演算子_を定義します。その多くは、[Basic Operators\(基本演算子\)](../language-guide/basic-operators.md)と[Advanced Operators\(高度な演算子\)](../language-guide/advanced-operators.md)で説明しています。このセクションでは、カスタム演算子を定義するために使用できる文字を説明します。

カスタム演算子は、ASCII 文字 `/`、 `=`、 `-` 、`+`、`！`、`*`、`％`、`<`、`>`、`＆`、`|`、`^`、`？`、または `〜` または下記の文法で定義されている Unicode 文字の 1 つ\(これは、数学演算子、その他のシンボル、および装飾記号 Unicode ブロックからの文字を含む\)で始めることができます。最初の文字の後に、Unicode 文字を組み合わせることもできます。

ドット\(`.`\)で始まるカスタム演算子を定義することもできます。これらの演算子は追加のドットを含めることができます。例えば、`.+.` は単一の演算子として扱われます。演算子がドットで始まらない場合は、他の場所にドットを含めることはできません。例えば、`+.+` は `+` 演算子の後に `.+` 演算子が続くと扱われます。

疑問符\(`？`\)を含むカスタム演算子を定義することはできますが、単一の疑問符文字のみで構成することはできません。さらに、感嘆符\(`！`\)を含めることができますが、後置演算子は疑問符または感嘆符で始めることはできません。

> NOTE  
> トークン `=`、 `-` `>`、`//`、`/ *`、`* /`、`.`、前置演算子 `<`、`＆`、および `？`、中置演算子 `？`、および 後置演算子 `>`、`！` は既に予約されています。これらのトークンはオーバーロードすることも、カスタム演算子として使用することもできません。

演算子の周囲のホワイトスペースは、演算子が前置演算子、後置演算子、またはバイナリ演算子として使用されるかどうかを判断するために使用されます。この動作は次の規則にまとめられています:

* 演算子が両側またはどちらの側面の周囲に空白を持つ場合は、バイナリ演算子として扱われます。例として、`a+++b` と `a +++ b` の `+++` 演算子はバイナリ演算子として扱われます
* 演算子が左側のみの空白を持つ場合は、前置単項演算子として扱われます。例として、`a +++b` の `+++` 演算子は、前置単項演算子として扱われます
* 演算子に右側の空白がある場合は、後置単項演算子として扱われます。例として、`a+++ b` の `+++` 演算子は後置単項演算子として扱われます
* 演算子に左側に空白がないが、ドット\(`.`\)が続いている場合は、後置単項演算子として扱われます。例として、`a+++.b` の `+++` 演算子は後置単項演算子\(`a +++ .b` ではなく `a+++ .b`\)と扱われます

これらの規則によって、演算子の前の `(`、`[`、`{`、`)`、演算子の後の `)`、`]`、`}`、および `,`、`;`、`:` も、空白と見なされます。

上記の規則で注意が必要な部分があります。`！` または `？` のような定義済み演算子には左側に空白があったとしても。右側に空白があるかどうかにかかわらず、後置演算子として扱われます。`？` オプショナルチェーン演算子として使用する場合には、左側に空白を入れないでください。三項演算子\(`?:`\)として使用する場合には、両側に空白を入れなければなりません。

特定の構成要素では、先頭に `<` または `>` を持つ演算子は 2 つ以上のトークンに分割されることもあります。残りの部分は同じ方法で扱われ、再び分割される可能性があります。その結果、`Dictionary<String、Array<Int>>` のようなコンストラクト内の閉じ `>` の間の文字は曖昧さを解消するために空白を追加する必要はありません。この例では、閉じ `>` を誤って単一のトークンのビットシフト\(`>>`\)としては解釈されません。

新しいカスタム演算子を定義する方法については、[Custom Operators\(カスタム演算子\)](../language-guide/advanced-operators.md#custom-operators)と[Operator Declaration](declarations.md#operator-declaration演算子宣言)を参照ください。既存の演算子をオーバーロードについては、[Operator Methods\(演算子メソッド\)](../language-reference/declarations.md#operator-declaration)を参照ください

> GRAMMAR OF OPERATORS  
> operator → [operator-head](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_operator-head) [operator-characters](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_operator-characters)<sub>opt</sub>  
> operator → [dot-operator-head](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_dot-operator-head) [dot-operator-characters](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_dot-operator-characters)  
> operator-head → `/` \| `=` \| `-` \| `+` \| `!` \| `*` \| `%` \| `<` \| `>` \| `&` \| `|` \| `^` \| `~` \| `?`  
> operator-head → U+00A1–U+00A7  
> operator-head → U+00A9 または U+00AB  
> operator-head → U+00AC または U+00AE  
> operator-head → U+00B0–U+00B1  
> operator-head → U+00B6, U+00BB, U+00BF, U+00D7, または U+00F7  
> operator-head → U+2016–U+2017  
> operator-head → U+2020–U+2027  
> operator-head → U+2030–U+203E  
> operator-head → U+2041–U+2053  
> operator-head → U+2055–U+205E  
> operator-head → U+2190–U+23FF  
> operator-head → U+2500–U+2775  
> operator-head → U+2794–U+2BFF  
> operator-head → U+2E00–U+2E7F  
> operator-head → U+3001–U+3003  
> operator-head → U+3008–U+3020  
> operator-head → U+3030  
> operator-character → [operator-head](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_operator-head)  
> operator-character → U+0300–U+036F  
> operator-character → U+1DC0–U+1DFF  
> operator-character → U+20D0–U+20FF  
> operator-character → U+FE00–U+FE0F  
> operator-character → U+FE20–U+FE2F  
> operator-character → U+E0100–U+E01EF  
> operator-characters → [operator-character](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_operator-character) [operator-characters](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_operator-characters)<sub>opt</sub>  
> dot-operator-head → `.`  
> dot-operator-character → `.` \| [operator-character](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_operator-character)  
> dot-operator-characters → [dot-operator-character](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_dot-operator-character) [dot-operator-characters](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_dot-operator-characters)<sub>opt</sub>  
> binary-operator → [operator](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_operator)  
> prefix-operator → [operator](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_operator)  
> postfix-operator → [operator](https://docs.swift.org/swift-book/ReferenceManual/LexicalStructure.html#grammar_operator)
