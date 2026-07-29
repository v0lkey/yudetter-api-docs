# [POST] /api/mikancoin/token

> 認証: 要（Cookie + X-Yudetter-Web） | レート制限: なし

## 説明

Mikancoinの取引に必要なワンタイム tradeToken を発行します。YDCと同一フローです。

## リクエスト

### ヘッダー

| 名前 | 必須 | 説明 |
|---|---|---|
| `Cookie` | はい | `__Secure-better-auth.session_token` |
| `X-Yudetter-Web` | はい | `true` |

### リクエストBody

なし

## レスポンス

### 成功 (200)

```json
{
  "token": "eyJhbGciOiJIUzI1NiIs..."
}
```

### エラー (403)

```json
{
  "error": "Invalid or missing X-Yudetter-Web header"
}
```

## 実行例

```javascript
const res = await fetch("/api/mikancoin/token", {
  method: "POST",
  headers: {
    "Content-Type": "application/json",
    "X-Yudetter-Web": "true"
  },
  credentials: "include"
})
const { token } = await res.json()
```
