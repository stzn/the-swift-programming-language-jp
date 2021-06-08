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
> *numeric-literal* → -<sub>*opt*</sub> *integer-literal* | -<sub>*opt*</sub> floating-point-literal  
> *boolean-literal* → **true** | **false**  
> *nil-literal* → **nil**

### Integer Literals(整数リテラル)

整数リテラル(*integer literal*)は、不特定の精度の整数値を表します。デフォルトでは、整数リテラルは 10 進数で表されます。前置を使用して他の基数を指定できます。2 進数リテラルは `0b` で始まり、8 進数リテラルは `0o` で始まり、16 進数リテラルは `0x` で始まります。

10 進数リテラルには `0` ～ `9` の数字が含まれます。2 進数リテラルには `0` と `1` が含まれ、8 進数リテラルには `0` ～ `7` が含まれ、16 進数リテラルには `0` ～ `9` および大文字または小文字の `A` ～ `F` が含まれます。

負の整数リテラルは、`-42` のように、整数リテラルの前にマイナス記号(`-`)を付けることで表現されます。

アンダースコア(`_`)は読みやすくするために数字の間に使用できますが、無視されるため、リテラルの値には影響しません。整数リテラルは先行ゼロ(`0`)で始めることができますが、同様に無視され、リテラルの基数または値には影響しません。

特に指定がない限り、整数リテラルのデフォルトの推論型は Swift 標準ライブラリの型 `Int` です。Swift 標準ライブラリは、[Integers](./../language-guide/the-basics.md#integers整数)で説明されているように、様々なサイズの符号付き整数および符号なし整数の型も定義します。

> GRAMMAR OF AN INTEGER LITERAL
> *integer-literal* → binary-literal  
> *integer-literal* → octal-literal  
> *integer-literal* → decimal-literal  
> *integer-literal* → hexadecimal-literal  
> 
> *binary-literal* → **0b** binary-digit binary-literal-characters<sub>*opt*</sub>  
> *binary-digit* → 桁 0 または 1  
> *binary-literal-character* → binary-digit | _  
> *binary-literal-characters* → binary-literal-character binary-literal-characters<sub>*opt*</sub>  
> 
> *octal-literal* → **0o** octal-digit octal-literal-characters<sub>*opt*</sub>  
> *octal-digit* → 桁 0 ~ 7  
> *octal-literal-character* → octal-digit |_  
> *octal-literal-characters* → octal-literal-character octal-literal-characters<sub>*opt*</sub>  
> 
> *decimal-literal* → decimal-digit decimal-literal-characters<sub>*opt*</sub>  
> *decimal-digit* → 桁 0 ~ 9  
> *decimal-digits* → decimal-digit decimal-digits<sub>*opt*</sub>  
> *decimal-literal-character* → decimal-digit | _  
> *decimal-literal-characters* → decimal-literal-character decimal-literal-characters<sub>*opt*</sub>  
> 
> *hexadecimal-literal* → **0x** hexadecimal-digit hexadecimal-literal-characters<sub>*opt*</sub>  
> *hexadecimal-digit* → 桁 0 ~ 9、 a ~ f、 or A ~ F  
> *hexadecimal-literal-character* → hexadecimal-digit |_  
> *hexadecimal-literal-characters* → hexadecimal-literal-character hexadecimal-literal-characters<sub>*opt*</sub>  

### Floating-Point Literals(浮動小数点リテラル)

浮動小数点リテラルは、不特定の精度の浮動小数点値を表します。

デフォルトでは、浮動小数点リテラルは 10 進数(前置なし)で表されますが、16 進数(`0x` 前置付き)で表すこともできます。

10 進数の浮動小数点リテラルは、10 進数の数字のシーケンスと、その後に続く 10 進数の小数、10 進数の指数、またはその両方で構成されます。小数部は、小数点(`.`)とそれに続く一連の 10 進数で構成されます。指数は、大文字または小文字の `e` 前置と、その後に続く 10 進数の数字のシーケンスで構成され、`e` の前の値に 10 の累乗が掛けられます。例えば、`1.25e2` は 1.25 x $10^{2}$ を表し、`125.0` に評価されます。同様に、`1.25e-2` は 1.25 x $10^{-2}$ を表し、`0.0125` に評価されます。

16 進数の浮動小数点リテラルは、`0x` 前置、オプションの 16 進数分数、16 進指数で構成されます。16 進数は、小数点とそれに続く 16 進数の数字で構成されます。指数は、大文字または小文字の `p` 前置と、その後に続く 10 進数の数字のシーケンスで構成されます。これは、`p` の前の値に乗算される 2 のべき乗を示します。例えば、`0xFp2` は 15 x $2^{2}$ を表し、`60` に評価されます。同様に、`0xFp-2` は 15 x $2^{-2}$ を表し、`3.75` に評価されます。

負の浮動小数点リテラルは、`-42.5` のように、浮動小数点リテラルの前にマイナス記号(`-`)を付けることで表現されます。

アンダースコア(`_`)は読みやすくするために数字の間に使用できますが、無視されるため、リテラルの値には影響しません。浮動小数点リテラルは先行ゼロ(`0`)で始めることができますが、同様に無視され、リテラルの基数または値には影響しません。

特に指定されていない限り、浮動小数点リテラルのデフォルトの推論型は、64 ビット浮動小数点数を表す Swift の標準ライブラリ `Double` 型です。Swift 標準ライブラリでは、32 ビットの浮動小数点数を表す `Float` 型も定義されています。

> GRAMMAR OF A FLOATING-POINT LITERAL  
> *floating-point-literal* → decimal-literal decimal-fraction<sub>*opt*</sub> decimal-exponent<sub>*opt*</sub>  
> *floating-point-literal* → hexadecimal-literal hexadecimal-fraction<sub>*opt*</sub> hexadecimal-exponent  
> 
> *decimal-fraction* → **.** decimal-literal  
> *decimal-exponent* → floating-point-e sign<sub>*opt*</sub> decimal-literal  
> 
> *hexadecimal-fraction* → **.** hexadecimal-digit hexadecimal-literal-characters<sub>*opt*</sub>  
> *hexadecimal-exponent* → floating-point-p sign<sub>*opt*</sub> decimal-literal  
> 
> *floating-point-e* → **e** | **E**  
> *floating-point-p* → **p** | **P**  
> *sign* → **+** | **-**  

### String Literals(文字列リテラル)

文字列リテラル(*string literal*)は、引用符で囲まれた一連の文字です。単一行の文字列リテラルは二重引用符で囲まれ、次の形式になります:

![単一行の文字列リテラル](./../.gitbook/assets/singleline_strings.png)

文字列リテラルには、エスケープされていない二重引用符(`"`)、エスケープされていないバックスラッシュ (`\`)、キャリッジリターン、またはラインフィードを含めることはできません。

複数行の文字列リテラルは、3 つの二重引用符で囲まれ、次の形式になります。

![複数行の文字列リテラル](./../.gitbook/assets/multipleline_strings.png)

単一行の文字列リテラルとは異なり、複数行の文字列リテラルには、エスケープされていない二重引用符 (`"`)、キャリッジリターン、およびラインフィードを含めることができます。3 つのエスケープされていない二重引用符を隣り合わせに含めることはできません。

複数行の文字列リテラルを開始する `"""` の後の改行は文字列の一部ではありません。リテラルを終了する `"""` の前の改行も文字列の一部ではありません。改行で開始または終了する複数行の文字列リテラルを作成するには、最初または最後の行として空白行を記述します。

複数行の文字列リテラルは、スペースとタブの任意の組み合わせを使用してインデントできます。このインデントは文字列に含まれていません。リテラルを終了する `"""` は、インデントを決定します。リテラル内のすべての非空白行は、閉じている `"""` の前に表示されるものと同じインデントで開始する必要があります。タブとスペース間の変換はありません。インデントの後に追加のスペースとタブを含めることができます。これらのスペースとタブは文字列に表示されます。

複数行の文字列リテラルの改行は、改行文字を使用するように正規化されます。ソースファイルにキャリッジリターンとラインフィードが混在している場合でも、文字列内のすべての改行は同じになります。

複数行の文字列リテラルでは、行末にバックスラッシュ(`\`)を記述すると、文字列からその改行が省略されます。バックスラッシュと改行の間の空白もすべて省略されます。この構文を使用すると、結果の文字列の値を変更せずに、ソースコードで複数行の文字列リテラルをハードラップできます。

次のエスケープシーケンスを使用して、単一行形式と複数行形式の両方の文字列リテラルに特殊文字を含めることができます:

* ヌル文字(`\0`)
* バックスラッシュ(`\`)
* 水平タブ(`\t`)
* 改行(`\n`)
* キャリッジリターン(`\r`)
* 二重引用符(`\"`)
* 単一引用符(`\'`)
* Unicode スカラー(`\u{n}`)、`n` は 1 ～ 8 桁の 16 進数

式の値を文字列リテラルに挿入するには、式をバックスラッシュ(`\`)の後にかっこで囲みます。補間された式には文字列リテラルを含めることができますが、エスケープされていないバックスラッシュ、キャリッジリターン、またはラインフィードを含めることはできません。

例えば、次の文字列リテラルはすべて同じ値です:

```swift
"1 2 3"
"1 2 \("3")"
"1 2 \(3)"
"1 2 \(1 + 2)"
let x = 3; "1 2 \(x)"
```

拡張区切り文字で区切られた文字列は、引用符で囲まれた一連の文字と、1 つ以上の文字列の両側に同じ数のシャープ記号(`#`) のセットです。拡張区切り文字で区切られた文字列には、次の形式があります:

![拡張区切り文字で区切られた文字列](./../.gitbook/assets/string_delimited_by_extended_delimiters.png)

拡張区切り文字で区切られた文字列内の特殊文字は、結果の文字列では特殊文字ではなく通常の文字として表示されます。拡張区切り文字を使用して、通常は文字列補間の生成、エスケープシーケンスの開始、文字列の終了などの特殊な効果を持つ文字を使って通常の文字列を作成できます。

次の例は、同等の文字列値を作成する拡張区切り文字で区切られた文字列リテラルと文字列を示しています:

```swift
let string = #"\(x) \ " \u{2603}"#
let escaped = "\\(x) \\ \" \\u{2603}"
print(string)
// "\(x) \ " \u{2603}"
print(string == escaped)
// "true"
```

複数の番号記号を使用して、拡張区切り文字で区切られた文字列を形成する場合は、番号記号の間に空白を入れないでください。

```swift
print(###"Line 1\###nLine 2"###) // OK
print(# # #"Line 1\# # #nLine 2"# # #) // Error
```

拡張区切り文字を使用して作成する複数行文字列リテラルには、通常の複数行文字列リテラルと同じインデント要件があります。

文字列リテラルのデフォルトの推論型は `String` です。`String` 型の詳細については、[Strings and Characters](./../language-guide/strings-and-characters.md)、[String](https://developer.apple.com/documentation/swift/string)を参照ください。

`+` 演算子によって連結された文字列リテラルは、コンパイル時に連結されます。例えば、下記の例の `textA` と `textB` の値は同じで、実行時に連結は実行されません。

```swift
let textA = "Hello " + "world"
let textB = "Hello world"
```

> GRAMMAR OF A STRING LITERAL  
> *string-literal* → static-string-literal | interpolated-string-literal  
> *string-literal-opening-delimiter* → extended-string-literal-delimiter<sub>*opt*</sub> **"**  
> *string-literal-closing-delimiter* → **"** extended-string-literal-delimiter<sub>*opt*</sub>  
> *static-string-literal* → string-literal-opening-delimiter quoted-text<sub>*opt*</sub> string-literal-closing-delimiter  
> *static-string-literal* → multiline-string-literal-opening-delimiter multiline-quoted-text<sub>*opt*</sub> multiline-string-literal-closing-delimiter  
> *multiline-string-literal-opening-delimiter* → extended-string-literal-delimiter **"""**  
> *multiline-string-literal-closing-delimiter* → **"""** extended-string-literal-delimiter  
> *extended-string-literal-delimiter* → **#** extended-string-literal-delimiter<sub>*opt*</sub>  
> *quoted-text* → quoted-text-item quoted-text<sub>*opt*</sub>  
> *quoted-text-item* → escaped-character  
> *quoted-text-item* → Any Unicode scalar value except **"**, **\**, U+000A, or U+000D  
> *multiline-quoted-text* → *multiline-quoted-text-item* multiline-quoted-text<sub>*opt*</sub>  
> *multiline-quoted-text-item* → escaped-character  
> *multiline-quoted-text-item* → Any Unicode scalar value except **\**  
> *multiline-quoted-text-item* → escaped-newline  
> *interpolated-string-literal* → string-literal-opening-delimiter interpolated-text<sub>*opt*</sub> string-literal-closing-delimiter  
> *interpolated-string-literal* → multiline-string-literal-opening-delimiter multiline-interpolated-text<sub>*opt*</sub> multiline-string-literal-closing-delimiter  
> *interpolated-text* → interpolated-text-item interpolated-text<sub>*opt*</sub>  
> *interpolated-text-item* → **\\** **(** expression **)** | quoted-text-item  
> *multiline-interpolated-text* → multiline-interpolated-text-item multiline-interpolated-text<sub>*opt*</sub>  
> *multiline-interpolated-text-item* → **\\** **(** expression **)** | multiline-quoted-text-item  
> *escape-sequence* → **\** extended-string-literal-delimiter  
> *escaped-character* → escape-sequence **0** | escape-sequence **\** | escape-sequence **t** | escape-sequence **n** | escape-sequence **r** | escape-sequence **"** | escape-sequence **'**  
> *escaped-character* → escape-sequence **u** **{** unicode-scalar-digits **}**  
> *unicode-scalar-digits* → Between one and eight hexadecimal digits  
> *escaped-newline* → escape-sequence inline-spaces<sub>*opt*</sub> line-break  

## Operators(演算子)

Swift 標準ライブラリは、使用のための数多くの演算子(*operator*)を定義します。その多くは、[Basic Operators](./../language-guide/basic-operators.md)と[Advanced Operators](./../language-guide/advanced-operators.md)で説明しています。このセクションでは、カスタム演算子を定義するために使用できる文字を説明します。

カスタム演算子は、ASCII 文字 `/`、 `=`、 `-` 、`+`、`！`、`*`、`％`、`<`、`>`、`＆`、`|`、`^`、`？`、または `〜` または下の文法で定義されている Unicode 文字の 1 つ(これは、数学演算子、その他のシンボル、および Dingbats Unicode ブロックからの文字を含む)で始めることができます。最初の文字の後、Unicode 文字を組み合わせることもできます。

ドット(`.`)で始まるカスタム演算子を定義することもできます。これらの演算子は追加の点を含めることができます。例えば、`.+.` は単一の演算子として扱われます。演算子がドットで始まらない場合は、他の場所にドットを含めることはできません。例えば、`+.+` は `.+` 演算子の後に `+` 演算子が続くと扱われます。

疑問符(`？`)を含むカスタム演算子を定義することはできますが、単一の疑問符文字のみで構成することはできません。さらに、感嘆符(`！`)を含めることができますが、後置演算子は疑問符または感嘆符で始めることはできません。

> NOTE  
> トークン `=`、 `-` `>`、`//`、`/ *`、`* /`、`.`、前置演算子 `<`、`＆`、および `？`、中置演算子 `？`、および 後置演算子 `>`、`！` は既に予約されています。これらのトークンはオーバーロードすることも、カスタム演算子として使用することもできません。

演算子の周囲のホワイトスペースは、演算子が前置演算子、後置演算子、またはバイナリ演算子として使用されるかどうかを判断するために使用されます。この動作は次の規則にまとめられています:

* 演算子が両側またはどちらの側面の周囲に空白を持つ場合は、バイナリ演算子として扱われます。例として、`a+++b` と `a +++ b` の `+++` 演算子はバイナリ演算子として扱われます
* 演算子が左側のみの空白を持つ場合は、前置単項演算子として扱われます。例として、`a +++b` の `+++` 演算子は、前置単項演算子として扱われます
* 演算子に右側の空白がある場合は、後置単項演算子として扱われます。例として、`a+++ b` の `+++` 演算子は後置単項演算子として扱われます
* 演算子に左側に空白がないが、ドット(`.`)が続いている場合は、後置単項演算子として扱われます。例として、`a+++.b` の `+++` 演算子は後置単項演算子(`a +++ .b` ではなく `a+++ .b`)と扱われます

これらの規則によって、演算子の前の `(`、`[`、`{`、)、演算子の後の `)`、`]`、`}`、および `,`、`;`、`:` も、空白と見なされます。

上記の規則で注意が必要な部分があります。`！` または `？` のような定義済み演算子には左側の空白があったとしても。右側の空白があるかどうかにかかわらず、後置演算子として扱われます。`？` オプション連鎖演算子として使用するには、左側に空白を入れないでください。三項演算子(`?:`)として使用するには、両側に空白を持たなければなりません。

特定の構成要素では、先頭に `<` または `>` を持つ演算子は 2 つ以上のトークンに分割されることもあります。残りの部分は同じ方法で扱われ、再び分割される可能性があります。その結果、`Dictionary<String、Array<Int>>` のようなコンストラクト内の閉じ `>` の間の文字は曖昧を解消するために空白を追加する必要はありません。この例では、閉じ `>` を誤って単一のトークンのビットシフト(`>>`)として解釈されません。

新しいカスタム演算子を定義する方法については、[Custom Operators](./../language-guide/advanced-operators.md#custom-operatorsカスタム演算子)と[Operator Declaration](./declarations.md#operator-declaration演算子宣言)を参照ください。既存の演算子をオーバーロードについては、[Operator Methods](./attributes.md#operator-methods演算子メソッド)を参照ください

> GRAMMAR OF OPERATORS  
> *operator* → operator-head operator-characters<sub>*opt*</sub>  
> *operator* → dot-operator-head dot-operator-characters  
> *operator-head* → **/** | **=** | **-** | **+** | **!** | **\*** | **%** | **<** | **>** | **&** | **|** | **^** | **~** | **?**  
> *operator-head* → U+00A1–U+00A7  
> *operator-head* → U+00A9 or U+00AB  
> *operator-head* → U+00AC or U+00AE  
> *operator-head* → U+00B0–U+00B1  
> *operator-head* → U+00B6, U+00BB, U+00BF, U+00D7, or U+00F7  
> *operator-head* → U+2016–U+2017  
> *operator-head* → U+2020–U+2027  
> *operator-head* → U+2030–U+203E  
> *operator-head* → U+2041–U+2053  
> *operator-head* → U+2055–U+205E  
> *operator-head* → U+2190–U+23FF  
> *operator-head* → U+2500–U+2775  
> *operator-head* → U+2794–U+2BFF  
> *operator-head* → U+2E00–U+2E7F  
> *operator-head* → U+3001–U+3003  
> *operator-head* → U+3008–U+3020  
> *operator-head* → U+3030  
> 
> *operator-character* → operator-head  
> *operator-character* → U+0300–U+036F  
> *operator-character* → U+1DC0–U+1DFF  
> *operator-character* → U+20D0–U+20FF  
> *operator-character* → U+FE00–U+FE0F  
> *operator-character* → U+FE20–U+FE2F  
> *operator-character* → U+E0100–U+E01EF  
> *operator-character*s → operator-character operator-characters<sub>*opt*</sub>  
> 
> *dot-operator-head* → **.**  
> *dot-operator-character* → **.** | operator-character  
> *dot-operator-characters* → dot-operator-character dot-operator-characters<sub>*opt*</sub>  
> 
> *binary-operator* → operator  
> *prefix-operator* → operator  
> *postfix-operator* → operator  
