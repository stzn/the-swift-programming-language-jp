# マクロ\(Macros\)

最終更新日: 2023/6/3  
原文: XXX

繰り返しコードを生成するために、コンパイル時にコードを変換します。

マクロは、ソースコードのコンパイル時にコードを変換し、あなたが繰り返し同じコードを手で書く手間を省いてくれます。コンパイルの間、Swift は、通常通りのコードの構築をする前にマクロを展開します。

![マクロ展開の概要を示す図。左側は、Swiftのコードを様式化したもの。右は、マクロによって数行が追加された同じコード。](../assets/macro-expansion%402x.png)

マクロを展開することは、常に加算操作です。つまり、マクロは新しいコードを追加しますが、既存のコードを削除したり修正したりすることはありません。

マクロへの入力とマクロ展開の出力の両方が、構文的に有効な Swift コードであることを確認するためにチェックされます。同様に、マクロに渡す値とマクロによって生成されるコードの値は、それらが正しい型であるかを確認するためにチェックされます。さらに、マクロの実装がそのマクロを展開するときにエラーが発生した場合、コンパイラはこれをコンパイルエラーとして扱います。これらの保証により、マクロを使用するコードの推論が容易になり、マクロの使い方が間違っている、マクロの実装にバグがある、などの問題を特定しやすくなります。

Swift には 2 種類のマクロがあります:

- *自立型マクロ*は、宣言に添付されることなく、それ自体独立して表示されます
- *付属型マクロ*は、それが添付されている宣言を変更します

付属型マクロと自立型マクロの呼び出し方は若干異なりますが、どちらもマクロ展開のための同じモデルに従っており、同じ手法で両方を実装します。以下のセクションでは、この 2 種類のマクロについてより詳しく説明します。

## 自立型マクロ

自立型マクロを呼び出すには、その名前の前に数字記号(`#`)を書き、その名前の後の括弧内にマクロの引数を書きます。たとえば、次のようになります:

```swift
let currentFunctionName = #function
#warning("何かが間違っています")
```
最初の行で、`#function` は Swift 標準ライブラリから `function` マクロを呼び出します。このコードをコンパイルするとき、Swift は、`#function` を現在の関数の名前に置き換えるマクロの実装を呼び出します。2 行目では、`#warning` は、カスタムコンパイル時の警告を生成するために標準ライブラリから別のマクロを呼び出します。

自立型マクロは、`#function` のように値を生成したり、または `#warning` のようにコンパイル時にアクションを実行できます。

## 付属型マクロ

付属型マクロを呼び出すには、名前の前にアットマーク(`@`)を書き、名前の後の括弧の中にマクロの引数を書きます。

付属型マクロは、それが添付されている宣言を変更します。例えば、新しいメソッドを定義したり、プロトコルに準拠するコードを追加したりします。

例えば、マクロを使用しない次のようなコードを考えてみましょう:

```swift
struct SundaeToppings: OptionSet {
    let rawValue: Int
    static let nuts = SundaeToppings(rawValue: 1 << 0)
    static let cherry = SundaeToppings(rawValue: 1 << 1)
    static let fudge = SundaeToppings(rawValue: 1 << 2)
}
```

このコードでは、`SundaeToppings` オプションセットの各オプションに、イニシャライザの呼び出しが含まれており、これは繰り返し手で書かなければなりません。そのため、新しいオプションを追加するときに、行末に間違った数字を入力するなどのミスを犯しやすいでしょう。

ここでは、このコードの代わりにマクロを使用したバージョンを紹介します:

```swift
@OptionSet<Int>
struct SundaeToppings {
    private enum Options: Int {
        case nuts
        case cherry
        case fudge
    }
}
```

このバージョンの `SundaeToppings` は Swift 標準ライブラリから `@OptionSet` マクロを呼び出します。このマクロは private な `enum` の `case` のリストを読み、各選択肢に対する定数のリストを生成し、`OptionSet` プロトコルへの準拠を追加します。

比較のために、`@OptionSet` マクロの展開版がどのようなものであるかを示します。あなたはこのコードを書きませんし、マクロの展開を表示するように Swift に特別に依頼した場合にのみ、それを見ることになります。

```swift
struct SundaeToppings {
    private enum Options: Int {
        case nuts
        case cherry
        case fudge
    }

    typealias RawValue = Int
    var rawValue: RawValue
    init() { self.rawValue = 0 }
    init(rawValue: RawValue) { self.rawValue = rawValue }
    static let nuts: Self = Self(rawValue: 1 << Options.nuts.rawValue)
    static let cherry: Self = Self(rawValue: 1 << Options.cherry.rawValue)
    static let fudge: Self = Self(rawValue: 1 << Options.fudge.rawValue)
}
extension SundaeToppings: OptionSet { }
```

private な `enum` 以降のコードはすべて `@OptionSet` マクロからきています。マクロを使ってすべての静的変数を生成する `SundaeToppings` のバージョンは、先ほどの手動でコード化したバージョンよりも読みやすく、保守もしやすくなっています。

## マクロ宣言

ほとんどの Swift のコードでは、関数や型のようなシンボルを実装し、別の宣言はありません。しかし、マクロの場合、宣言と実装は別々です。マクロの実装には、Swift のコードを生成してマクロを展開するコードとその宣言、マクロの名前とマクロが受け取るパラメータ、マクロが使用できる場所、生成されるコードの種類、が含まれます。
マクロの宣言は、`macro` キーワードで導入します。例えば、前の例で使用した `@OptionSet` マクロの宣言の一部を以下に示します:

```swift
public macro OptionSet<RawType>() =。
        #externalMacro(module: "SwiftMacros", type: "OptionSetMacro")
```

最初の行は、マクロの名前である `OptionSet` と、受け取る引数(このケースではなし)を指定します。2 行目は、マクロの実装がどこにあるかを Swift に伝えるために、Swift 標準ライブラリから `#externalMacro(module:type:)` マクロを使用します。この場合、`SwiftMacros` モジュールは、`@OptionSet` マクロを実装する `OptionSetMacro` という名前の型を含んでいます。
`OptionSet` は付属型マクロなので、その名前は構造体やクラスの名前のように、大文字始まりのキャメルケースを使用します。自立型マクロは、変数や関数の名前と同じように、小文字始まりのキャメルケースを使用します。

> NOTE: マクロは常に `public` として宣言します。マクロを宣言するコードは、そのマクロを使用するコードとは別のモジュールにあるため、public ではないマクロを適用できる場所はありません。

マクロ宣言は、マクロの役割(マクロが呼び出されるソースコード内の場所、マクロが生成できるコードの種類)を定義します。すべてのマクロには 1 つ以上の役割があり、マクロ宣言の冒頭で属性の一部として記述します。以下は、`@OptionSet` の宣言の一部で、その役割の属性を含みます:

```swift
@attached(member)
@attached(conformance)
public macro OptionSet<RawType>() =
        #externalMacro(module: "SwiftMacros", type: "OptionSetMacro")
```

`attached` 属性は、この宣言で 2 回、各マクロの役割のために 1 回ずつ、現れます。最初の使用である `@attached(member)` は、マクロがそれを適用する型に新しいメンバーを追加することを示します。`OptionSet` マクロは、`OptionSet` プロトコルで必要な `init(rawValue:)` イニシャライザと、いくつかの追加メンバを追加します。2 つ目の使い方である `@attached(conformance)` は、`@OptionSet` が 1 つまたは複数のプロトコルへの準拠を追加することを示します。`OptionSet` マクロは、マクロを適用した型を展開し、`OptionSet` プロトコルへの準拠を追加します。
自立型マクロの場合、`@freestanding` 属性を記述してその役割を指定します:

```swift
@freestanding(expression)
public macro line<T: ExpressibleByIntegerLiteral>() -> T =
        /* ... マクロ実装の場所... */
```

上記の `#line` マクロは、`expression` の役割を担っています。式マクロは、値を生成したり、警告を生成するようなコンパイル時のアクションを実行します。
マクロの役割に加えて、マクロの宣言は、マクロが生成するシンボルの名前に関する情報を提供します。マクロ宣言で名前のリストを提供すると、その名前を使用する宣言だけが生成されることが保証され、生成されたコードの理解やデバッグに役立ちます。以下は、`@OptionSet` の完全な宣言です:

```swift 
@attached(member, names: named(RawValue), named(rawValue),
        named(`init`), arbitrary)
@attached(conformance)
public macro OptionSet<RawType>() =
        #externalMacro(module: "SwiftMacros", type: "OptionSetMacro")
```

上記の宣言では、`@attached(member)` マクロは、`@OptionSet` マクロが生成する各シンボルの `named:` ラベルの後に引数を含んでいます。マクロは、`RawValue`、`rawValue`、`init` という名前のシンボルに対する宣言を追加します --- これらの名前は前もってわかっているので、マクロ宣言はそれらを明示的にリストアップします。
また、マクロ宣言には名前のリストの後に `arbitrary` が含まれており、これは、マクロを使用するまで名前がわからない宣言をマクロで生成できるようにします。この場合、private な `Options` の `enum` の各 `case` に対して、`@OptionSet` は同じ名前を持つそれぞれの case に対応する宣言を生成します。
マクロの役割の全リストを含む詳細については、[Attributes]を参照してください。

## マクロ展開

多くの Swift のコードで、マクロを使用する Swift のコードを構築する一環として、コンパイラとマクロの実装は、マクロを展開するためにそのコードを前後に渡します。

マクロは以下のように展開されます:

1. コンパイラは、コードを読み、構文のメモリ内表現を作成する
2. コンパイラは、メモリ内表現の一部をマクロの実装に送信し、マクロの実装はマクロを展開する
3. コンパイラは、マクロの呼び出しを展開した形に置き換える
4. コンパイラは、展開されたソースコードを使用してコンパイルを続行する

これらのステップにより、以下のようにコードが変換されます:

![マクロを展開する4つのステップを示す図。入力はSwiftのソースコードです。これがツリーとなり、コードの構造を表す。マクロの実装は、ツリーにノードを追加します。その結果、コードが追加されたSwiftソースができあがります。](../assets/macro-expansion-full%402x.png)

具体的な手順を説明するために、次のように考えてみます:

```swift
let magicNumber = #fourCharacterCode("ABCD")
```

`fourCharacterCode` マクロは、4 文字の文字列を受け取り、その文字列の ASCII 値を足し合わせた符号なし 32 ビット整数を返します。ファイルフォーマットによっては、コンパクトな一方でデータをデバッガで読めるという理由から、データを識別するためにこのような整数を使用することがあります。以下の doc:Macros#Implementing-Macros のセクションで、このマクロを実装する方法を示します。

上記のコードでマクロを展開するために、コンパイラは Swift ファイルを読み、抽象構文木、または AST として知られているそのコードのメモリ内表現を作成します。AST は、コードの意味と構造を明確にし、コンパイラやマクロの実装のように、その構造とやり取りするコードを簡単に書けるようにします。上のコードの AST を、余分な部分を省いて少し簡略化して表現すると、次のようになります:

![定数をルート要素とするツリー図。定数は、名前、マジックナンバー、値があります。定数の値は、マクロの呼び出しです。マクロの呼び出しは、fourCharacterCodeという名前、および引数を持ちます。引数は、文字列リテラルABCDです。](../assets/macro-ast-original%402x.png)

上の図は、このコードの構造が、メモリ上でどう表現されるかを示しています。AST の各要素は、ソースコードの意味の一部に対応しています。「定数宣言 AST」要素には、その下に 2 つの子要素があり、定数宣言の 2 つの部分、つまり名前と値を表しています。「マクロ呼び出し」要素には、マクロの名前とマクロに渡される引数のリストを表す子要素があります。この AST を構築する一部として、コンパイラは、ソースコードが有効な Swift であることをチェックします。たとえば、`#fourCharacterCode` は、文字列でなければならない単一の引数を取ります。もし、整数の引数を渡そうとしたり、文字列リテラルの最後に引用符(`"`)を忘れたりすると、プロセスのこの時点でエラーが発生します。

コンパイラは、コードの中でマクロを呼び出す場所を見つけ、そのマクロを実装した外部バイナリをロードします。マクロを呼び出すたびに、コンパイラは AST の一部をそのマクロの実装に渡します。以下は、その部分的な AST の表現です:

![マクロコールをルート要素とするツリー図。マクロの呼び出しは、fourCharacterCodeという名前、および引数を持っています。引数は、文字列リテラル「ABCD」です。](../assets/macro-ast-input%402x.png)

`fourCharacterCode` マクロの実装は、マクロを展開する際に、この部分的な AST を入力として読み込みます。マクロの実装は、入力として受け取った部分的な AST に対してのみ動作します。つまり、マクロは、その前後のコードに関係なく、常に同じ方法で展開されます。この制限は、マクロの展開を理解しやすくするのに役立ち、Swift が変更されていないマクロの展開を避けることができるので、あなたのコードをより速く構築するのに役立ちます。

Swift は、マクロを実装するコードを制限することによって、マクロの作者が誤って他の入力を読み取れないようにできます:

- マクロの実装に渡される AST は、マクロを表す AST 要素のみを含み、その前後に来るコードは一切含まれない
- マクロ実装に渡される AST は、マクロを表す AST 要素のみを含み、その前後のコードは一切含まれない。マクロ実装は、ファイルシステムやネットワークにアクセスできないサンドボックス環境で実行される

これらの安全策に加えて、マクロの作者は、責任を持ってマクロの入力以外のものを読んだり変更しないようにします。たとえば、マクロの展開が現在の時刻に依存することはありません。

`fourCharacterCode` の実装は、展開されたコードを含む新しい AST を生成します。そのコードがコンパイラに返すものは以下の通りです：

![シングルノードが整数リテラル1145258561であるツリー図](../assets/macro-ast-output%402x.png)

コンパイラはこの展開を受け取ると、マクロの呼び出しを含む AST 要素を、マクロの展開を含む要素に置き換えます。マクロの展開の後、コンパイラはプログラムがまだ構文的に有効な Swift であり、すべての型が正しいことを確認するために再度チェックします。それにより、通常通りコンパイルできる最終的な AST が生成されます:

![定数をルート要素とするツリー図。定数には、名前、マジックナンバー、値を持ちます。定数の値は、整数リテラル1145258561です。](../assets/macro-ast-result%402x.png)

この AST は、次のような Swift のコードに対応します:

```swift
let magicNumber = 1145258561
```

この例では、入力ソースコードには 1 つのマクロしかありませんが、実際のプログラムでは、同じマクロの複数のインスタンスと異なるマクロへの複数の呼び出しがあるかもしれません。コンパイラは、マクロを 1 つずつ展開します。

あるマクロが別のマクロの中にある場合、外側のマクロが最初に展開されます。これにより、展開される前に、外側のマクロが内側のマクロを変更することができます。

## マクロの実装

マクロを実装するためには、2 つの部品を作ります: マクロを展開する型と、マクロを API として公開するためにマクロを宣言するライブラリです。これらの部品は、マクロとそのクライアントを一緒に開発している場合でも、マクロの実装はマクロのクライアントを構築する一部として実行されるので、マクロを使用するコードとは別に構築されます。

Swift パッケージマネージャを使用して新しいマクロを作成するには、`swift package init --type macro` これを実行すると、マクロの実装と宣言のためのテンプレートを含むいくつかのファイルが作成されます。

既存のプロジェクトにマクロを追加するには、マクロの実装のためのターゲットとマクロライブラリのためのターゲットを追加します。例えば、以下のようなものを `Package.swift` ファイルに追加し、プロジェクトに合わせて名前を変更します:

```swift
targets: [
    // ソース変換を行うマクロの実装
    .macro(
        name: "MyProjectMacros",
        dependencies: [
            .product(name: "SwiftSyntaxMacros", package: "swift-syntax"),
            .product(name: "SwiftCompilerPlugin", package: "swift-syntax")
        ]
    ),

    // APIの一部としてマクロを公開するライブラリ
    .target(name: "MyProject", dependencies: ["MyProjectMacros"]),
]
```

上記のコードでは、2 つのターゲットを定義しています: `MyProjectMacros` はマクロの実装を含み、`MyProject` はそれらのマクロを利用できるようにします。

マクロの実装は、抽象構文木(AST)を使用して、構造化された方法で Swift コードと対話するために `SwiftSyntax` モジュールを使用します。Swift パッケージマネージャで新しいマクロパッケージを作成した場合、生成された `Package.swift` ファイルは、自動的に `SwiftSyntax` への依存関係を含みます。既存のプロジェクトにマクロを追加する場合は、`Package.swift` ファイルに `SwiftSyntax` への依存関係を追加してください:

```swift
dependencies: [
    .package(url: "https://github.com/apple/swift-syntax.git", from: "some-tag"),
],
```

上記のコードの `some-tag` プレースホルダを、使用したい `SwiftSyntax` のバージョンの Git タグで置き換えてください。

マクロの役割に応じて、マクロの実装が準拠するべき `SwiftSystem` にある対応するプロトコルがあります。例えば、前のセクションの `#fourCharacterCode` を考えてみましょう。そのマクロを実装した構造体がこちらです:

```swift
public struct FourCharacterCode: ExpressionMacro {
    public static func expansion(
        of node: some FreestandingMacroExpansionSyntax,
        in context: some MacroExpansionContext
    ) throws -> ExprSyntax {
        guard let argument = node.argumentList.first?.expression,
              let segments = argument.as(StringLiteralExprSyntax.self)?.segments,
              segments.count == 1,
              case .stringSegment(let literalSegment)? = segments.first
        else {
            throw CustomError.message("Need a static string")
        }

        let string = literalSegment.content.text
        guard let result = fourCharacterCode(for: string) else {
            throw CustomError.message("Invalid four-character code")
        }

        return "\(raw: result)"
    }
}

private func fourCharacterCode(for characters: String) -> UInt32? {
    guard characters.count == 4 else { return nil }

    var result: UInt32 = 0
    for character in characters {
        result = result << 8
        guard let asciiValue = character.asciiValue else { return nil }
        result += UInt32(asciiValue)
    }
    return result.bigEndian
}
enum CustomError: Error { case message(String) }
```

`#fourCharacterCode` マクロは、式を生成する自立式マクロなので、これを実装した `FourCharacterCode` 型は、 `ExpressionMacro` プロトコルに準拠します。`ExpressionMacro` プロトコルの要件は 1 つで、AST を展開する `expansion(of:in:)` メソッドです。マクロの役割とそれに対応する `SwiftSystem` プロトコルのリストについては、[Attributes]の[Attributes attached]と [Attributes:freestanding]() を参照してください。

`#fourCharacterCode` マクロを展開するために、Swift はこのマクロを使用するコードの AST を、マクロの実装を含むライブラリに送信します。ライブラリの中で、Swift はメソッドの引数として AST とコンテキストを渡して `FourCharacterCode.expansion(of:in:)` を呼び出します。`expansion(of:in:)` の実装は、`#fourCharacterCode` に引数として渡された文字列を見つけ、対応する整数リテラルの値を計算します。

上記の例では、最初の `guard` ブロックが AST から文字列リテラルを取り出し、その AST 要素を `literalSegment` に代入しています。2 番目の `guard` ブロックは、private な `FourCharacterCode(for:)` 関数を呼び出します。これらのブロックはいずれも、マクロの使い方が間違っているとエラーを発生させます(エラーメッセージは、不正な呼び出し先でのコンパイラエラーになります)。例えば、マクロを `#fourCharacterCode("AB" + "CD")` として呼び出そうとすると、コンパイラは「静的な文字列が必要です(Need a static string)」というエラーを表示します。

`expansion(of:in:)` メソッドは、AST で式を表す `SwiftSyntax` からの型である `ExprSyntax` のインスタンスを返します。この型は、`StringLiteralConvertible` プロトコルに準拠しているので、マクロの実装は、その結果を作成するために、軽量な構文として文字列リテラルを使用します。マクロの実装から返す `SwiftSyntax` の型はすべて、 `StringLiteralConvertible` に準拠しているので、あらゆる種類のマクロを実装するときにこのアプローチを使用することができます。

## マクロの開発とデバッグ

マクロは、テストを使った開発に適しています。これは、マクロは、外部の状態に依存することなく、また外部の状態に変更を加えることなく、ある AST から別の AST に変換するからです。また、文字列リテラルから構文ノードを作成することができるので、ユニットテストの入力設定を簡略化することができます。また、AST の `description` プロパティを読み込んで、期待値と比較するための文字列を取得することも可能です。例えば、前のセクションで紹介した `#fourCharacterCode` マクロのテストは以下のとおりです:

```swift
let source: SourceFileSyntax =
    """
    let abcd = #fourCharacterCode("ABCD")
    """

let file = BasicMacroExpansionContext.KnownSourceFile(
    moduleName: "MyModule",
    fullFilePath: "test.swift"
)

let context = BasicMacroExpansionContext(sourceFiles: [source: file])

let transformedSF = source.expand(
    macros:["fourCharacterCode": FourCC.self],
    in: context
)

let expectedDescription =
    """
    let abcd = 1145258561
    """

precondition(transformedSF.description == expectedDescription)
```

上記の例では、`precondition` を使ってマクロをテストしていますが、代わりにテストフレームワークを使うこともできます。