# バージョン互換性\(Version Compatibility\)

<!--最終更新日: 2022/12/3  -->
<!--原文: https://docs.swift.org/swift-book/GuidedTour/Compatibility.html -->

どの機能が古い言語モードで利用可能かを知る。

この本は Xcode14 のデフォルトバージョン Swift5.7 で記載されています。Xcode14 では、Swift5.7, Swift4.2, Swift4 を build target に指定できます。

Swift4 または Swift4.2 のコードをビルドするのに Xcode14 を利用すると、たいていの Swift5.7 の機能は利用可能です。一方で、次の変更は Swift5.7 以降でのみ利用可能です。

* Opaque 型を戻り値にする関数は Swift5.1 の runtime が必要です
* オプショナルを戻り値をしている式に `try?` を使用しても、追加でオプショナルのネストは起きません
* 大きな値の整数リテラルの初期化時に、正しい整数が推論されます。例えば、 `UInt64(0xffff_ffff_ffff_ffff)` はオーバーフローせず、正しい値に評価されます

Concurrency は、Swift 5.7 以降、およびそれに対応する Concurrency の型を提供する Swift 標準ライブラリのバージョンを必要とします。Apple プラットフォームでは、少なくとも iOS 13、macOS 10.5、tvOS 13、または watchOS 6.0 のデプロイターゲットが必要です。

Swift5.7 で書かれたターゲットは Swift4.2 または Swift4 で書かれたターゲットに依存する場合があります\(逆も同様\)。すなわち、例えば複数のフレームワークに別れた大きなプロジェクトを扱っている場合、一度に 1 つのフレームワークを Swift4 から Swift5.7 に切り換えることができます。
