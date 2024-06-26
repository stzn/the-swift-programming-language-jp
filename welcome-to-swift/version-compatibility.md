# バージョン互換性\(Version Compatibility\)

最終更新日: 2024/6/23  
原文: https://docs.swift.org/swift-book/GuidedTour/Compatibility.html

どの機能が古い言語モードで利用可能かを知る。

本書では、Xcode 16 に含まれる Swift のデフォルトバージョンである Swift 6 について説明します。Swift 6 コンパイラは、Swift 6、Swift 5、Swift 4.2、または Swift 4 で書かれたコードをビルドするために使用できます。

Swift 5 言語モードを使用するコードをビルドするために Swift 6 コンパイラを使用する場合、Swift 6 からの新機能を使用できます。しかし、Strict Concurrency チェックを有効にするには、Swift 6 言語モードにアップグレードする必要があります。

さらに、Swift 4 と Swift 4.2 のコードをビルドするために Xcode 15.3 を使用する場合、ほとんどの Swift 5 の機能はまだ利用可能です。一方、以下の変更は、Swift 5 言語モードを使用するコードでのみ利用可能です:

* Opaque 型を戻り値にする関数は Swift5.1 の runtime が必要です
* オプショナルを戻り値をしている式に `try?` を使用しても、追加でオプショナルのネストは起きません
* 大きな値の整数リテラルの初期化時に、正しい整数が推論されます。例えば、 `UInt64(0xffff_ffff_ffff_ffff)` はオーバーフローせず、正しい値に評価されます

Swift Concurrency には、Swift 5 言語モードと、対応する同時実行型を提供する Swift 標準ライブラリのバージョンが必要です。Apple プラットフォームでは、少なくとも iOS 13、macOS 10.15、tvOS 13、 watchOS 6 または visionOS 1 をデプロイメントターゲットに設定します。

Swift 6 で書かれたターゲットは、Swift 5、Swift 4.2、または Swift 4 で書かれたターゲットに依存でき、その逆も同様です。これは、複数のフレームワークに分割された大規模なプロジェクトを持っている場合、一度に 1 つのフレームワークで新しい言語バージョンにコードを移行できるということです。