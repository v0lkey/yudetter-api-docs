# [POST] /api/yudecoin/token

> 認証: 要（Cookie + X-Yudetter-Web） | レート制限: なし

## 説明

Yudecoinの取引に必要なワンタイム tradeToken を発行します。フロントランニング防止のため、トークン発行から1,050ms待機後に取引を実行します。

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

### エラー (401)

```json
{
  "error": "認証が必要です"
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
const res = await fetch("/api/yudecoin/token", {
  method: "POST",
  headers: {
    "Content-Type": "application/json",
    "X-Yudetter-Web": "true"
  },
  credentials: "include"
})
const { token } = await res.json()
```

## 備考

- `X-Yudetter-Web: true` ヘッダーがないと403エラー
- 発行されたトークンは1回限り有効
- Mikancoinも同様のフロー（`/api/mikancoin/token`）
