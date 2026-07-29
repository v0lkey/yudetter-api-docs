# [POST] /api/market/items/:id/bid

> 認証: 要 | レート制限: なし

## 説明

オークション出品中の商品に入札します。

## リクエスト

### パスパラメータ

| 名前 | 型 | 必須 | 説明 |
|---|---|---|---|
| `id` | string | はい | 入札する商品のID |

### リクエストBody

```json
{
  "amount": 200
}
```

| 名前 | 型 | 必須 | 説明 |
|---|---|---|---|
| `amount` | number | はい | 入札額。現在の最高入札額より高い必要があります |

## レスポンス

### 成功 (200)

```json
{
  "message": "入札しました",
  "highestBid": 200,
  "highestBidder": { "...User..." }
}
```

### エラー (400)

```json
{
  "error": "入札額が現在の最高入札額を下回っています"
}
```

## 実行例

```javascript
fetch("/api/market/items/abc123/bid", {
  method: "POST",
  credentials: "include",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({ amount: 200 })
})
```
