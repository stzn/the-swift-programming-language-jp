# Version Compatibility

この本は Xcode12.5 のデフォルトバージョンである Swift5.4 で記載されています。Xcode12.5 では、Swift5.4, Swift4.2, Swift4を build target に指定できます。

Swift4 または Swift4.2 のコードをビルドするのに Xcode12.5 を利用すると、たいていの Swift5.4 の機能は利用可能です。一方で、次の変更は Swift5.4 以降でのみ利用可能です。

- opaque typeを戻り値にする関数は Swift5.1 のruntimeが必要
- optional を戻り値をしている expression に try? を使用しても、追加で optional のネストは起きない
- 大きな値の integer リテラルの初期化時に、正しい integer が推論される。例えば、 `UInt64(0xffff_ffff_ffff_ffff)` はオーバーフローせず、正しい値に評価される

Swift5.4 で書かれた target は Swift4.2 または Swift4 で書かれた target に依存する場合があります(逆も同様)。すなわち、例えば複数のフレームワークに別れた大きなプロジェクトを扱っている場合、一度に1つのフレームワークを Swift4 から Swift5.4 に migrate することができます。