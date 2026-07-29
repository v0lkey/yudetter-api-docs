# [POST] /api/market/items/:id/buy

> 認証: 要 | レート制限: なし

## 説明

商品を直接購入します（通常販売のみ）。購入者のYudedollarから商品価格分が差し引かれます。

## リクエスト

### パスパラメータ

| 名前 | 型 | 必須 | 説明 |
|---|---|---|---|
| `id` | string | はい | 購入する商品のID |

### リクエストBody

なし

## レスポンス

### 成功 (200)

```json
{
  "message": "購入が完了しました"
}
```

### エラー (400)

```json
{
  "error": "残高不足"
}
```

```json
{
  "error": "この商品は既に売却済みです"
}
```

## 実行例

```javascript
fetch("/api/market/items/abc123/buy", {
  method: "POST",
  credentials: "include"
})
```

## 備考

- `/api/market/items/:id/purchase` とほぼ同等の機能
- 自分の出品した商品は購入できません
