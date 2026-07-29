# [GET] /api/notifications/stream

> 認証: 要（withCredentials: true） | レート制限: なし

## 説明
Server-Sent Events (SSE) を使用してリアルタイムに通知を受信します。接続を維持することで、新しい通知が発生するたびにサーバーからプッシュ配信されます。

## リクエスト
### ヘッダー
| 名前 | 値 | 必須 | 説明 |
|---|---|---|---|
| Cookie | （セッションクッキー） | はい | `withCredentials: true` で認証情報を送信 |

クエリパラメータはありません。

## レスポンス
### 成功 (SSEストリーム)
```
data: {"actorName":"花子","actionMessage":"花子さんがあなたの投稿にいいねしました","actorAvatar":"https://example.com/avatar2.jpg","targetUrl":"/yudates/100","type":"like","createdAt":"2025-07-12T10:30:00Z"}

data: {"actorName":"John","actionMessage":"Johnさんがあなたをフォローしました","actorAvatar":"https://example.com/avatar3.jpg","targetUrl":"/users/99","type":"follow","createdAt":"2025-07-12T10:31:00Z"}
```

| フィールド | 型 | 説明 |
|---|---|---|
| `actorName` | string | アクションを起こしたユーザーの表示名 |
| `actionMessage` | string | 通知メッセージ（例: 「花子さんがあなたの投稿にいいねしました」） |
| `actorAvatar` | string | アクションを起こしたユーザーのアバターURL |
| `targetUrl` | string | 通知対象へのリンクパス（例: `/yudates/100`） |
| `type` | string | 通知種別（`like`, `follow`, `reyudate`, `reply`, `super_yudate` 等） |
| `createdAt` | string | 作成日時（ISO 8601） |

> REST API（`GET /api/notifications`）とはフィールド名・構造が異なります。SSEは軽量な通知専用形式です。

### エラー (401)
```json
{
  "error": "認証が必要です"
}
```

## 実行例
```javascript
const eventSource = new EventSource("/api/notifications/stream", {
  withCredentials: true
});

eventSource.onmessage = (event) => {
  const notification = JSON.parse(event.data);
  console.log(notification.actorName, notification.actionMessage);
};

eventSource.onerror = (err) => {
  console.error("SSE接続エラー", err);
};
```

## 備考
- 接続は持続的ですが、ネットワーク切断時は自動的に再接続を試みます（ブラウザのEventSourceのデフォルト動作）
- 認証にはクッキーベースのセッションを使用します。`withCredentials: true` が必須です
- 各イベントは `data:` プレフィックス付きのJSONテキスト行として配信されます
- 空行（ダブル改行）がイベントの区切りです
- 接続を閉じるには `eventSource.close()` を呼び出します
