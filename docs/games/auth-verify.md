# [POST] /api/games/auth/verify

> 認証: 要 | レート制限: なし

## 説明

ゲームの認証トークンを検証します。スタジオ機能で使用します。

## リクエスト

### リクエストBody

```json
{
  "token": "game-auth-token-xxx"
}
```

## レスポンス

### 成功 (200)

```json
{
  "valid": true,
  "gameId": "abc123",
  "userId": 42
}
```

### エラー

```json
{
  "error": "無効なトークンです"
}
```

## 実行例

```javascript
fetch("/api/games/auth/verify", {
  method: "POST",
  credentials: "include",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({ token: "game-auth-token-xxx" })
})
```
