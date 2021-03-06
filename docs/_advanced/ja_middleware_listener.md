---
title: リスナーミドルウェア
lang: ja-jp
slug: listener-middleware
order: 5
---

<div class="section-content">
リスナーミドルウェアは、全てではありませんが多くのリスナー関数を対象としたロジックに使用され、組み込みメソッド内のリスナー関数より先に引数として追加されます。ここでは任意の数のリスナーミドルウェアを追加することができます。

組み込みリスナーミドルウェアはいくつか用意されており、例えば、メッセージのサブタイプをフィルタリングする `subtype()` や、ボットに直接 @ メンションしないメッセージを除外する `directMention()` のように使用することができます。

ただしもちろん、よりカスタマイズされた機能を追加するために、独自のミドルウェアを作成することもできます。独自のミドルウェアを記述する際には、関数で `await next()` を呼び出して制御を次のミドルウェアに渡すか、`throw` を呼び出して以前に実行されたミドルウェアチェーンにエラーを渡す必要があります。

たとえば、リスナーが人間からのメッセージのみを扱うのであれば、ボットメッセージを除外するリスナーミドルウェアを作成できます。

*注: Bolt 2.x からミドルウェアが `async` 関数をサポートしました！この変更については [2.x マイグレーションガイド](https://slack.dev/bolt/ja-jp/tutorial/migration-v2)を参照してください。*
</div>

```javascript
// 'bot_message' サブタイプを持つメッセージをフィルタリングするリスナーミドルウェア
async function noBotMessages({ message, next }) {
  if (!message.subtype || message.subtype !== 'bot_message') {
    await next();
  }
}

// ボットではなく人間からのメッセージのみを受信するリスナー
app.message(noBotMessages, ({ message }) => console.log(
  `(MSG) User: ${message.user}
   Message: ${message.text}`
));
```
