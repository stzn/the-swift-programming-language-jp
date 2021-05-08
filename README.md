# The Swift Programming Language\(日本語版\)

[The Swift Programming Language](https://docs.swift.org/swift-book/)の日本語版です。

## オンラインドキュメント

[Gitbook](https://www.gitbook.com)を使用しています

## バージョン履歴

## 翻訳上のルール

### 翻訳のゆらぎ & トーン

#### **文体**

「だである」ではなく「ですます」調

> Swift is a fantastic way to write software.

* ❌ Swift はソフトウェアを書くための素晴らしい手段である。
* ⭕️ Swift はソフトウェアを書くための素晴らしい手段です。

**半角スペースでアルファベット両端を入れて読みやすく！**

> Swift has struct and class.

* ❌ Swiftには、structとclassがあります。
* ⭕️ Swift には、struct と class があります。

例外として、句読点の前後にアルファベットがある場合は、スペースを入れなくてもいいです。

* 読点: Swift には**、struct** と class があります。

#### **原則、一語一句翻訳、ただ日本語として分かりにくい場合は読みやすさを優先**

> The compiler is optimized for performance and the language is optimized for development, without compromising on either.

* ❌ コンパイラはパフォーマンスが最適化され、言語は開発に最適化されています。どちらも妥協することなく。
* ⭕️ コンパイラによるパフォーマンスの最適化、言語による開発のしやすさの最適化、どちらも実現できるようにしています。

#### **原文に使われる ':' や '!' や '?' などの記号は極力残して翻訳**

> Example:

* ❌ 例
* ⭕️ 例:

ただし、文の途中にハイフン`-` や セミコロン`;` がある場合は、その記号があると理解しづらい訳になる場合は、例外として削除してもよいです。

* 原文:

  > There’s an even simpler way to include values in strings: Write the value in parentheses, and write a backslash \(\) before the parentheses.

* 訳文:

  > もっと簡単な方法で文字列の中に値を含めることができます。値を括弧で囲み、括弧の前にバックスラッシュをつけます。

#### **単語の統一 \(特に技術用語\)**

* 技術用語は基本英語、ただ日本語で一般的に使われている場合は日本語 OK
  * 例: 英語の filter 、日本語のフィルタ
* 和訳に困った、とりあえず英語
  * 例: expression -&gt; 式、表現
* 和訳にして分かりづらい場合は、翻訳と英語\(どちらかに括弧付け\)でも OK
  * 例: Two way -&gt; Two way \(双方向\)

#### **長音訳のついて**

原則、**長音なし**で翻訳する。

* ❌ コンピューター
* ⭕️ コンピュータ

ただし、長音なしで訳した場合、**意味が分かりにくいものは、例外として長音あり**で訳してもよいです。

> Pull Request flow

* ❌ プルリクエストフロ
* ⭕️ プルリクエストフロー

#### **長音訳例外リスト\(随時更新\)**

* error: エラー
* throw: スロー
* flow: フロー

#### 翻訳チェック

[textlint](https://github.com/textlint/textlint)に
- [textlint-rule-preset-JTF-style](https://github.com/textlint-ja/textlint-rule-preset-JTF-style)
- [textlint-rule-period-in-list-item](https://github.com/textlint-rule/textlint-rule-period-in-list-item)
を適用してチェックしています。

※ 一部チェック項目を除外しています。内容は[.textlintrc](https://github.com/stzn/the-swift-programming-language-jp/blob/master/.textlintrc)をご参照ください。

#### **実行方法**

```text
npm install
npm run textlint <対象のファイル>
```
