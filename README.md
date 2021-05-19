# The Swift Programming Language\(日本語版\)

[The Swift Programming Language](https://docs.swift.org/swift-book/)の日本語版です。

## オンラインドキュメント

[Gitbook](https://www.gitbook.com)を使用しています

## バージョン履歴

## 開発ガイド

### 参加方法

1. [GitHub Issues](https://github.com/stzn/the-swift-programming-language-jp/issues)からアサインされていない issues 一覧からやりたい issue を選択します
2. 選択した issue で、「翻訳やります」的なコメントをお願いします
3. このリポジトリをフォークします
4. 変更をコミットします: コミットメッセージの先頭に翻訳したページのタイトルを\[XXXX\]のような形で入れてもらえると助かります
5. lint で引っかかる場合は再度修正を行いコミットします
6. ページ冒頭の最終更新日を更新しているか確認をお願いします
7. フォークした自分のリポジトリに Push します: `git push origin my-branch`
8. 問題がなければプルリクエストを送ります
9. レビューで指摘事項があったら修正し、再度 Push します
10. レビュー OK ならば、マージされて内容がドキュメントに反映されます

### 開発準備

まず始めに実行してください。

```text
npm install
```

## 翻訳ルール

### 翻訳のゆらぎ & トーン

#### **文体**

`「だである」` ではなく `「ですます」` 調

> Swift is a fantastic way to write software.

❌ Swift はソフトウェアを書くための素晴らしい手段 `である`。

⭕️ Swift はソフトウェアを書くための素晴らしい手段 `です`。

#### **半角スペースを半角文字の両端に入れて読みやすく**

> Swift has struct and class.

❌ Swift5.5 には、struct と class があります。

⭕️ Swift5.5 には、struct と class があります。

例外として、句読点の前後に半角文字がある場合は、スペースを入れなくても大丈夫です。

読点: Swift には**、struct** と class、 があります。

※ commit 時にチェックしていますが、一部コード内のコメントや引用コメントでは有効ではありません。\(方法を探し中\)

#### **原則、一語一句翻訳、ただ日本語として分かりにくい場合は読みやすさを優先**

> The compiler is optimized for performance and the language is optimized for development, without compromising on either.

❌ コンパイラはパフォーマンスが最適化され、言語は開発に最適化されています。どちらも妥協することなく。

⭕️ コンパイラによるパフォーマンスの最適化、言語による開発のしやすさの最適化、どちらも実現できるようにしています。

※ 直訳だと意味がわかりにく箇所が結構あると思うので、意味が変わらない限りは、読みやすさを優先したいと思います。

#### **原文に使われる ':' や '!' や '?' などの記号は極力残して翻訳**

> Example:

❌ 例

⭕️ 例:

ただし、文の途中にハイフン `-` やセミコロン `;` がある場合は、その記号があると理解しづらい訳になる場合は、例外として削除してもよいです。

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

#### 翻訳チェック

[textlint](https://github.com/textlint/textlint)に

* [textlint-rule-preset-JTF-style](https://github.com/textlint-ja/textlint-rule-preset-JTF-style)
* [textlint-rule-period-in-list-item](https://github.com/textlint-rule/textlint-rule-period-in-list-item)
* [textlint-rule-no-mix-dearu-desumasu](https://github.com/textlint-ja/textlint-rule-no-mix-dearu-desumasu)
* [textlint-rule-detect-bad-chars](https://github.com/magitek-telescope/textlint-rule-detect-bad-chars)
* [textlint-rule-preset-ja-spacing](https://github.com/textlint-ja/textlint-rule-preset-ja-spacing)
* [textlint-rule-proofdict](https://proofdict.github.io)

を適用してチェックしています。

※ 一部チェック項目を除外しています。内容は[.textlintrc](https://github.com/stzn/the-swift-programming-language-jp/blob/master/.textlintrc)をご参照ください。

手動で lint を行う場合は下記を実行してください。

```text
npm run lint
```

