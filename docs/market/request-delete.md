# [POST] /api/market/items/:id/request-delete

> 認証: 要 | レート制限: なし

## 説明

出品者に対して商品の削除をリクエストします。購入者が不適切な商品に遭遇した場合などに使用します。

## リクエスト

### パスパラメータ

| 名前 | 型 | 必須 | 説明 |
|---|---|---|---|
| `id` | string | はい | 削除リクエストする商品のID |

### リクエストBody

なし

## レスポンス

### 成功 (200)

```json
{
  "message": "削除リクエストを送信しました"
}
```

### エラー

```json
{
  "error": "認証が必要です"
}
```

```json
{
  "error": "商品が見つかりません"
}
```

## 実行例

```javascript
fetch("/api/market/items/abc123/request-delete", {
  method: "POST",
  credentials: "include"
})
```
