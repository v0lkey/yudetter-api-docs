# [POST] /api/users/request-password-reset

> 認証: 要 | レート制限: なし

## 説明

パスワードリセットをリクエストします。

## リクエスト

### リクエストBody

```json
{
  "email": "user@example.com"
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
  "error": "ユーザーが見つかりません"
}
```

## 実行例

```javascript
fetch("/api/users/request-password-reset", {
  method: "POST",
  credentials: "include",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({ email: "user@example.com" })
})
```
