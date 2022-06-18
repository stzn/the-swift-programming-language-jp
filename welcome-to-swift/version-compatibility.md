# バージョン互換性\(Version Compatibility\)

最終更新日: 2021/2/1  
原文: https://docs.swift.org/swift-book/GuidedTour/Compatibility.html

この本は Xcode13 のデフォルトバージョン Swift5.6 で記載されています。Xcode13 では、Swift5.6, Swift4.2, Swift4 を build target に指定できます。

Swift4 または Swift4.2 のコードをビルドするのに Xcode13 を利用すると、たいていの Swift5.6 の機能は利用可能です。一方で、次の変更は Swift5.6 以降でのみ利用可能です。

* Opaque 型を戻り値にする関数は Swift5.1 の runtime が必要です
* オプショナルを戻り値をしている式に `try?` を使用しても、追加でオプショナルのネストは起きません
* 大きな値の整数リテラルの初期化時に、正しい整数が推論されます。例えば、 `UInt64(0xffff_ffff_ffff_ffff)` はオーバーフローせず、正しい値に評価されます

Concurrency は、Swift 5.6 以降、およびそれに対応する Concurrency の型を提供する Swift 標準ライブラリのバージョンを必要とします。Apple プラットフォームでは、少なくとも iOS 15、macOS 12、tvOS 15、または watchOS 8.0 のデプロイターゲットが必要です。

Swift5.6 で書かれたターゲットは Swift4.2 または Swift4 で書かれたターゲットに依存する場合があります\(逆も同様\)。すなわち、例えば複数のフレームワークに別れた大きなプロジェクトを扱っている場合、一度に 1 つのフレームワークを Swift4 から Swift5.6 に切り換えることができます。
