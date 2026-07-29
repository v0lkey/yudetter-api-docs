# [POST] /api/users/request-email-change

> 認証: 要 | レート制限: なし

## 説明

メールアドレスの変更をリクエストします。

## リクエスト

### リクエストBody

```json
{
  "newEmail": "new-email@example.com"
}
```

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
  "error": "このメールアドレスは既に使用されています"
}
```

## 実行例

```javascript
fetch("/api/users/request-email-change", {
  method: "POST",
  credentials: "include",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({ newEmail: "new-email@example.com" })
})
```
