# [POST] /api/market/items/:id/purchase

> 認証: 要 | レート制限: なし

## 説明

商品を購入します。購入者のYudedollarから商品価格分が差し引かれます。

## リクエスト

### パスパラメータ

| 名前 | 型 | 必須 | 説明 |
|------|-----|------|------|
| `id` | `string` | はい | 購入する商品のID |

### リクエストBody

なし

## レスポンス

### 成功 (200)

```json
{
  "message": "購入が完了しました",
  "item": {
    "id": "string",
    "title": "string",
    "price": 100,
    "itemType": "image",
    "status": "sold",
    "seller": { "...User..." },
    "buyer": { "...User..." },
    "highestBidder": null
  }
}
```

### エラー

```json
{
  "error": "残高不足"
}
```

```json
{
  "error": "商品が見つかりません"
}
```

```json
{
  "error": "自分の商品は購入できません"
}
```

```json
{
  "error": "この商品は既に売却済みです"
}
```

## 実行例

```javascript
fetch("/api/market/items/abc123/purchase", {
  method: "POST",
  credentials: "include"
})
```

## 備考

- 自分の出品した商品は購入できません
- 残高が不足している場合は `400 Bad Request` が返ります
- 購入後は `status` が `"sold"` に変更され、`buyer` に購入者の情報がセットされます
- `item.buyer` には完全なUserオブジェクトが含まれます
