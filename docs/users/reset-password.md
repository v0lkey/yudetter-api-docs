# [POST] /api/users/reset-password

> 認証: 不要 | レート制限: なし

## 説明

パスワードリセットを実行します。`/api/users/request-password-reset` で発行されたトークンと新しいパスワードを送信します。

## リクエスト

### リクエストBody

```json
{
  "token": "reset-token",
  "newPassword": "new-password-123"
}
```

| 名前 | 型 | 必須 | 説明 |
|---|---|---|---|
| token | string | はい | メール送付されたリセットトークン |
| newPassword | string | はい | 新しいパスワード |

## レスポンス

### 成功 (200)

```json
{
  "success": true
}
```

### エラー

```json
{
  "error": "トークンが無効か期限切れです"
}
```

## 実行例

```javascript
fetch("/api/users/reset-password", {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({ token: "reset-token", newPassword: "new-password-123" })
})
```

## 備考

- 認証不要です（トークンが本人確認の役割を担います）
- トークンは `/api/users/request-password-reset` で発行されます
