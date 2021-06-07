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

## Literals(リテラル)

### Integer Literals(整数リテラル)

### Floating-Point Literals(浮動小数点リテラル)

### String Literals((文字列リテラル))

## Operators(演算子)
