# [POST] /api/mikancoin/exchange

> 認証: 要（Cookie + token + X-Yudetter-Web） | レート制限: なし

## 説明

Mikancoin（MKC）の取引を実行します。YDCの `/api/yudecoin/trade` とほぼ同等ですが、レスポンスのフィールド名が一部異なります。

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
| `amount` | number | はい | 取引量（buy=投入YD量, sell=投入MKC量） |
| `idempotencyKey` | string (UUID) | はい | 冪等性キー |
| `tradeToken` | string | はい | `/api/mikancoin/token` で取得したトークン |

## レスポンス

### 成功 (200)

```json
{
  "success": true,
  "transactionId": 56,
  "ydAmount": "100",
  "mkcAmount": "19",
  "feeAmount": "1",
  "mkcBalance": "19",
  "priceAtExecution": 5.015410145805078
}
```

| フィールド | 型 | YDCとの差 |
|---|---|---|
| `success` | boolean | 共通 |
| `transactionId` | number | YDCにはない |
| `ydAmount` | string | YDCは `executedYdAmount` |
| `mkcAmount` | string | YDCは `executedYudecoinAmount` |
| `feeAmount` | string | 共通 |
| `mkcBalance` | string | 総残高。YDCは `newBalance`（増分のみ） |
| `priceAtExecution` | number | 共通 |

### エラー (400)

```json
{
  "error": "残高不足"
}
```

### エラー (401)

```json
{
  "error": "認証が必要です"
}
```

## 取引フロー

```
1. POST /api/mikancoin/token → { token }
2. 1,050ms 待機
3. POST /api/mikancoin/exchange → 成約
4. GET /api/mikancoin/wallet + /api/mikancoin/market をリフェッチ
```

## YDC / MKC 比較

| 項目 | `/api/yudecoin/trade` | `/api/mikancoin/exchange` |
|---|---|---|
| 一回の取引上限 | 保有YD残高の10% | 保有YD残高の10% |
| 日次取引量制限 | 通貨ごとに100万YD分まで | 通貨ごとに100万YD分まで |
| 手数料 | 1% | 1% |
| サーキットブレーカー | あり（閾値非公開） | あり（閾値非公開） |
| 取引レスポンス | `executedYdAmount` / `executedYudecoinAmount` / `newBalance` | `ydAmount` / `mkcAmount` / `mkcBalance` / `transactionId` |
| 取引レスポンス | `executedYdAmount` / `executedYudecoinAmount` / `newBalance` | `ydAmount` / `mkcAmount` / `mkcBalance` / `transactionId` |

## 実行例

```javascript
const tokenRes = await fetch("/api/mikancoin/token", {
  method: "POST",
  headers: { "Content-Type": "application/json", "X-Yudetter-Web": "true" },
  credentials: "include"
})
const { token } = await tokenRes.json()

await new Promise(r => setTimeout(r, 1050))

const exchangeRes = await fetch("/api/mikancoin/exchange", {
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
