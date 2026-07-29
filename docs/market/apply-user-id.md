# [POST] /api/market/items/:id/apply-user-id

> 認証: 要 | レート制限: なし

## 説明

購入した user_id タイプの商品を自分のアカウントに適用します。

## リクエスト

### パスパラメータ

| 名前 | 型 | 必須 | 説明 |
|---|---|---|---|
| `id` | string | はい | 購入した商品のID |

### リクエストBody

なし

## レスポンス

### 成功 (200)

```json
{
  "message": "ユーザーIDを適用しました"
}
```

### エラー (400)

```json
{
  "error": "この商品は user_id タイプではありません"
}
```

## 実行例

```javascript
fetch("/api/market/items/abc123/apply-user-id", {
  method: "POST",
  credentials: "include"
})
```
