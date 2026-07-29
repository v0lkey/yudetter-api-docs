# [POST] /api/yudecoin/trade

> 認証: 要（Cookie + token + X-Yudetter-Web） | レート制限: なし

## 説明

Yudecoin（YDC）の取引を実行します。事前に `/api/yudecoin/token` で取得した tradeToken が必要です。
取引は Uniswap V2 スタイルの Constant Product AMM により即時約定します。

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
  "tradeToken": "eyJhbGciOiJIUzI1NiIs..."
}
```

| 名前 | 型 | 必須 | 説明 |
|---|---|---|---|
| `type` | `"buy" \| "sell"` | はい | 売買方向 |
| `amount` | number | はい | 取引量（buy=投入YD量, sell=投入YDC量） |
| `idempotencyKey` | string (UUID) | はい | 冪等性キー（二重実行防止） |
| `tradeToken` | string | はい | `/api/yudecoin/token` で取得したワンタイムトークン |

## レスポンス

### 成功 (200)

```json
{
  "success": true,
  "executedYdAmount": "100",
  "executedYudecoinAmount": "534",
  "feeAmount": "1",
  "priceAtExecution": 0.18560839796801493,
  "newBalance": "534"
}
```

| フィールド | 型 | 説明 |
|---|---|---|
| `success` | boolean | 成約したか |
| `executedYdAmount` | string | 消費YD量 |
| `executedYudecoinAmount` | string | 取得YDC量 |
| `feeAmount` | string | 手数料（YD、固定1%） |
| `priceAtExecution` | number | 約定価格 |
| `newBalance` | string | 増分残高 |

### エラー (400)

```json
{
  "error": "残高不足"
}
```

```json
{
  "error": "注文上限を超えています"
}
```

```json
{
  "error": "サーキットブレーカーが作動しました"
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

## 取引フロー

```
1. POST /api/yudecoin/token → { token }
2. 1,050ms 待機
3. POST /api/yudecoin/trade → 成約
4. GET /api/yudecoin/wallet + /api/yudecoin/market をリフェッチ
```

## 制限

| 項目 | 値 |
|---|---|
| 一回の取引上限 | 保有YD残高の10% |
| 日次取引量制限 | 通貨ごとに100万YD分まで |
| 手数料 | 1%（固定） |
| サーキットブレーカー | あり（閾値は非公開） |

## 実行例

```javascript
const tokenRes = await fetch("/api/yudecoin/token", {
  method: "POST",
  headers: { "Content-Type": "application/json", "X-Yudetter-Web": "true" },
  credentials: "include"
})
const { token } = await tokenRes.json()

await new Promise(r => setTimeout(r, 1050))

const tradeRes = await fetch("/api/yudecoin/trade", {
  method: "POST",
  headers: { "Content-Type": "application/json", "X-Yudetter-Web": "true" },
  credentials: "include",
  body: JSON.stringify({
    type: "buy",
    amount: 100,
    idempotencyKey: crypto.randomUUID(),
    tradeToken: token
  })
})
```

## 備考

- Mikancoinの `/api/mikancoin/exchange` とほぼ同等ですが、レスポンスのフィールド名が異なります
- 両通貨とも同じ制限（一回の取引上限=保有YD10%、日次100万YD分まで）が適用されます。サーキットブレーカーはYDCのみ実装
- 冪等性キーにより、同一キーでの重複実行は防止されます
