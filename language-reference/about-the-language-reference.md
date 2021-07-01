# About the Language Reference

最終更新日: 2021/7/1

このパートでは、Swift プログラミング言語の正式な文法について説明します。ここで説明する文法は、パーサやコンパイラを直接実装できるようにするのではなく、言語をより詳細に理解できるようにすることを目的としています。

Swift のコードのほぼどこにでも現れる、多くの一般的な型、関数、および演算子は、実際には Swift 標準ライブラリで定義されているため、Swift 言語のサイズは比較的小さいです。これらの型、関数、および演算子は Swift 言語そのものではありませんが、このパートのディスカッションやコード例で広く使用されています。

## How to Read the Grammar\(文法の読み方\)

Swift プログラミング言語の正式な文法を記述するために使用される表記法は、いくつかの規則に従います。

* 矢印\(`→`\)は、文法の生成を示すために使用され、「〇〇で構成される」と読むことができます
* 構文カテゴリはイタリックのテキストで示され、文法生成規則の両側に現れます
* 単語リテラルと句読点は、太字の `constant width` で示され、文法生成規則の右側にのみ現れます
* 代わりとして使える文法生成は、縦棒 \(`|`\) で区切られます。代替案が長すぎて簡単に読めない場合、それらは新しい行で複数の文法作成規則に分割されます
* 場合によっては、通常のフォントテキストを使用して、文法生成規則の右側を記述します
* 必須ではない構文カテゴリとリテラルは、末尾に添字 `opt` がマークされます

例として、getter-setter ブロックの文法は次のように定義されます:

> GRAMMAR OF A GETTER-SETTER BLOCK  
> getter-setter-block → `{` [getter-clause](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_getter-clause)  [setter-clause](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_setter-clause)<sub>*opt*</sub> `}` \|  `{` [setter-clause](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_setter-clause)  [getter-clause](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_getter-clause)  `}`

この定義は、getter-setter ブロックが、中括弧\(`{}`\)で囲まれたオプションの setter 句が続く getter 句、または中括弧で囲まれた getter 句が続く setter 句で構成できることを示しています。上記の文法生成は、次の 2 つの生成と同等で、代替案が明示的に記述されています

> GRAMMAR OF A GETTER-SETTER BLOCK  
> getter-setter-block → `{` [getter-clause](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_getter-clause)  [setter-clause](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_setter-clause)<sub>*opt*</sub> `}`   
> getter-setter-block → `{` [setter-clause](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_setter-clause)  [getter-clause](https://docs.swift.org/swift-book/ReferenceManual/Declarations.html#grammar_getter-clause)  `}`
