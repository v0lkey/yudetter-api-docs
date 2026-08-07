# [POST] /api/ykc/exchange

> 認証: 要（Cookie + token + X-Yudetter-Web） | レート制限: なし

## 説明

Ykc（YKC）の取引を実行します。Mikancoinの `/api/mikancoin/exchange` と同一フローですが、取引前に `quote` で取得した `priceVersion` を送信します。

## リクエスト

### ヘッダー

| 名前 | 必須 | 説明 |
|---|---|---|
| `Cookie` | はい | `__Secure-better-auth.session_token` |
| `X-Yudetter-Web` | はい | `true` |

### リクエストBody

```json
{
  "type": "buy",
  "amount": 100,
  "idempotencyKey": "550e8400-e29b-41d4-a716-446655440000",
  "tradeToken": "eyJhbGciOiJIUzI1NiIs...",
  "priceVersion": 42
}
```

| 名前 | 型 | 必須 | 説明 |
|---|---|---|---|
| `type` | `"buy" \| "sell"` | はい | 売買方向 |
| `amount` | number | はい | 取引量（buy=投入YD量, sell=売却YKC量） |
| `idempotencyKey` | string (UUID) | はい | 冪等性キー |
| `tradeToken` | string | はい | `/api/ykc/token` で取得したトークン |
| `priceVersion` | number | はい | `/api/ykc/quote` で取得した価格バージョン（Ykc固有） |

## レスポンス

### 成功 (200)

```json
{
  "success": true,
  "transactionId": 58,
  "ydAmount": "100",
  "ykcAmount": "80",
  "feeAmount": "1",
  "ykcBalance": "80",
  "priceAtExecution": 1.235
}
```

### エラー (400)

```json
{
  "error": "残高不足"
}
```

### エラー (409)

```json
{
  "error": "価格が変動しました。再クオートしてください"
}
```

## 取引フロー

```
1. GET /api/ykc/quote?type&amount → { canBuy, priceVersion }
2. POST /api/ykc/token → { token }
3. 1,050ms 待機
4. POST /api/ykc/exchange → 成約
5. GET /api/ykc/wallet + /api/ykc/market をリフェッチ
```

## 実行例

```javascript
const res = await fetch("/api/ykc/exchange", {
  method: "POST",
  headers: { "Content-Type": "application/json", "X-Yudetter-Web": "true" },
  credentials: "include",
  body: JSON.stringify({
    type: "buy",
    amount: 100,
    idempotencyKey: crypto.randomUUID(),
    tradeToken: token,
    priceVersion
  })
})
```