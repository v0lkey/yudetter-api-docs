# [POST] /api/market/items/:id/like

> 認証: 要 | レート制限: なし

## 説明

商品に「いいね」を付けます。トグル動作（既にいいね済みの場合は取り消し）になります。

## リクエスト

### パスパラメータ

| 名前 | 型 | 必須 | 説明 |
|------|-----|------|------|
| `id` | `string` | はい | 商品ID |

### リクエストBody

なし

## レスポンス

### 成功 (200)

```json
{
  "message": "いいねしました",
  "isLiked": true,
  "likeCount": 11
}
```

### 取り消し時 (200)

```json
{
  "message": "いいねを取り消しました",
  "isLiked": false,
  "likeCount": 10
}
```

### エラー

```json
{
  "error": "商品が見つかりません"
}
```

## 実行例

```javascript
fetch("/api/market/items/abc123/like", {
  method: "POST",
  credentials: "include"
})
```

## 備考

- いいねはトグル動作です。既にいいねしている状態で再度リクエストすると取り消しになります
- `isLiked` フィールドで現在のいいね状態を確認できます
- `likeCount` は変更後の総いいね数です

---

# [DELETE] /api/market/items/:id/like

> 認証: 要 | レート制限: なし

## 説明

商品のいいねを明確に取り消します（トグル動作ではなく削除専用）。

## リクエスト

### パスパラメータ

| 名前 | 型 | 必須 | 説明 |
|---|---|---|---|
| `id` | `string` | はい | 商品ID |

### リクエストBody

なし

## レスポンス

### 成功 (200)

```json
{
  "message": "いいねを取り消しました",
  "isLiked": false,
  "likeCount": 10
}
```

### エラー

```json
{
  "error": "商品が見つかりません"
}
```

## 実行例

```javascript
fetch("/api/market/items/abc123/like", {
  method: "DELETE",
  credentials: "include"
})
```
