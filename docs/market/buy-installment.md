# [POST] /api/market/items/:id/buy-installment

> 認証: 要 | レート制限: なし

## 説明

商品を分割払いで購入します。事前に `/api/market/installments/eligibility` で資格確認が必要です。

## リクエスト

### パスパラメータ

| 名前 | 型 | 必須 | 説明 |
|---|---|---|---|
| `id` | string | はい | 購入する商品のID |

### リクエストBody

```json
{
  "installments": 3
}
```

| 名前 | 型 | 必須 | 説明 |
|---|---|---|---|
| `installments` | number | はい | 分割回数 |

## レスポンス

### 成功 (200)

```json
{
  "message": "分割払いが成立しました",
  "installments": 3,
  "amountPerPayment": 34
}
```

### エラー (400)

```json
{
  "error": "分割払いの資格がありません"
}
```

## 実行例

```javascript
fetch("/api/market/items/abc123/buy-installment", {
  method: "POST",
  credentials: "include",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({ installments: 3 })
})
```

## 備考

- 分割払いの資格は `/api/market/installments/eligibility?itemId=:id` で事前確認可能
