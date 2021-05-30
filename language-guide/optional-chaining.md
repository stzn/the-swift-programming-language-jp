# Optional Chaining(オプショナルの連鎖)

最終更新日:

オプショナルの連鎖(*optional chaining*)は、現在 `nil` な可能性のある optioanl のプロパティ、メソッド、および subscript を照会して呼び出すためのプロセスです。optioanl に値が含まれている場合、プロパティ、メソッド、または subscript の呼び出しは成功します。optioanl が `nil` の場合、プロパティ、メソッド、または subscript の呼び出しは `nil` を返します。複数のクエリを一緒に連鎖させることができ、連鎖内のリンクが `nil` の場合、連鎖全体が失敗します。

> NOTE  
> Swift のオプショナルの連鎖は、Objective-C の `nil` のメッセージングに似ていますが、どの型にでも機能し、成功または失敗をチェックできます。

## Optional Chaining as an Alternative to Forced Unwrapping(強制アンラップの代替としてのオプショナル連鎖)


## Defining Model Classes for Optional Chaining(オプショナル連鎖モデルのクラスの定義)

## Accessing Properties Through Optional Chaining(オプショナル連鎖を通したプロパティへのアクセス)

## Calling Methods Through Optional Chaining(オプショナル連鎖を通したメソッドの呼び出し)

## Accessing Subscripts Through Optional Chaining(オプショナル連鎖を通したsubscriptへのアクセス)

### Accessing Subscripts of Optional Type(optional型のsubscriptへのアクセス)

## Linking Multiple Levels of Chaining(複数レベルの連鎖のリンク)

## Chaining on Methods with Optional Return Values(optionalの戻り値を持つメソッドの連鎖)