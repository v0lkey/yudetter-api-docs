# [POST] /api/notifications/read

> 認証: 要 | レート制限: なし

## 説明
全ての未読通知を既読にします。個別指定はできません。

## リクエスト
リクエストボディは不要です。

## レスポンス
### 成功 (200)
```json
{
  "message": "全ての通知を既読にしました",
  "unreadCount": 0
}
```
### エラー (401)
```json
{
  "error": "認証が必要です"
}
```

## 実行例
```javascript
fetch("/api/notifications/read", { method: "POST", credentials: "include" })
```

## 備考
- この操作は元に戻せません
- 現在ログイン中のユーザーの全通知が既読になります
- 既読にしたあと新しく通知が届くと、`unreadCount` は再び増加します
