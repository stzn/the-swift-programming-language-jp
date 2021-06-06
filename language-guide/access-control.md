# Access Control

最終更新日:

アクセス制御は、他のソースファイルやモジュールのコードからコードの一部へのアクセスを制限します。この機能を使用すると、コードの実装の詳細を非表示にし、そのコードにアクセスして使用するための望ましいインターフェイスを指定できます。

個々の型(クラス、構造体、および列挙型)、およびそれらの型に属するプロパティ、メソッド、イニシャライザ、および subscript に特定のアクセスレベルを割り当てることができます。プロトコルは、グローバル定数、変数、および関数と同様に、特定のコンテキストに制限できます。

様々なレベルのアクセス制御を提供することに加えて、Swift は、一般的なシナリオのデフォルトのアクセスレベルを提供することにより、明示的なアクセス制御レベルを指定する手間を減らします。実際、単一のターゲットアプリを作成している場合は、明示的なアクセス制御レベルをまったく指定する必要がない場合もあります。

> NOTE  
> アクセス制御を適用できるコードの様々な側面(プロパティ、型、関数など)は、簡潔にするために、以下のセクションでは「エンティティ」と呼びます。

## Modules and Source Files(モジュールとソースファイル)

Swift のアクセス制御モデルは、モジュールとソースファイルの概念に基づいています。

モジュールは、コード配布の単位で、単体で構築および出荷され、`import` キーワードを使用して別のモジュールによってインポートできるフレームワークまたはアプリケーションです。

Xcode の各ビルドターゲット(アプリバンドルやフレームワークなど) は、Swift では個別のモジュールとして扱われます。アプリのコードの様々な側面を独立したフレームワークとしてグループ化する場合(おそらく、そのコードを複数のアプリケーションでカプセル化して再利用する場合)、そのフレームワーク内で定義するものは全て、アプリ内でインポートされて使用される場合や、別のフレームワーク内で使用されている場合に、別のモジュールの一部になります。

ソースファイルは、モジュール内の Swift ソースコードファイルです(実際には、アプリまたはフレームワーク内の単一のファイル)。個別のソースファイルで個々の型を定義するのが一般的ですが、1 つのソースファイルに複数の型や関数などの定義を含めることができます。

## Access Levels(アクセスレベル)

### Guiding Principle of Access Levels(アクセスレベルの指針)

### Default Access Levels(デフォルトのアクセスレベル)

### Access Levels for Single-Target Apps(単一ターゲットアプリのアクセスレベル)

### Access Levels for Frameworks(フレームワークのアクセスレベル)

### Access Levels for Unit Test Targets(単体テストターゲットのアクセスレベル)

## Access Control Syntax(アクセス制御構文)

## Custom Types(独自型)

### Tuple Types(タプル型)

### Function Types(関数型)

### Enumeration Types(列挙型)

#### Raw Values and Associated Values(Raw Valueと関連値)

---

### Nested Types(ネストされた型)

## Subclassing(サブクラス)

## Constants, Variables, Properties, and Subscripts(定数、変数、プロパティ、subscript)

### Getters and Setters(get と set)

## Initializers(初期化)

### Default Initializers(デフォルトイニシャライザ)

### Default Memberwise Initializers for Structure Types(構造体のデフォルトのメンバごとのイニシャライザ)

## Protocols(プロトコル)

### Protocol Inheritance(プロトコル継承)

### Protocol Conformance(プロトコル準拠)

## Extensions(拡張)

### Private Members in Extensions(extensionのprivateメンバ)

## Generics(ジェネリクス)

## Type Aliases(型エイリアス)
