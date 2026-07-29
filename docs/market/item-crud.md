# [GET / PUT / DELETE] /api/market/items/:id

> 認証: 要 | レート制限: なし

## 説明

特定の商品を取得・更新・削除します。更新と削除は出品者のみ実行可能です。

## リクエスト

### パスパラメータ

| 名前 | 型 | 必須 | 説明 |
|------|-----|------|------|
| `id` | `string` | はい | 商品ID |

---

### GET: 商品詳細

#### リクエストBody

なし

#### レスポンス: 成功 (200)

```json
{
  "id": "string",
  "title": "string",
  "description": "string",
  "itemType": "image",
  "itemData": "string (URL)",
  "price": 100,
  "saleType": "normal",
  "status": "selling",
  "stock": 1,
  "auctionEndAt": "2026-07-13T00:00:00Z",
  "highestBid": 150,
  "buyoutPrice": 500,
  "likeCount": 10,
  "commentCount": 3,
  "isLiked": false,
  "isBought": false,
  "createdAt": "2026-07-10T12:00:00Z",
  "updatedAt": "2026-07-10T12:00:00Z",
  "seller": {
    "id": "string",
    "username": "seller_user",
    "displayName": "出品者",
    "email": "seller@example.com",
    "avatarUrl": "https://example.com/avatar.png",
    "bio": "こんにちは",
    "birthday": "2000-01-01",
    "yudedollar": 5000,
    "followerCount": 42,
    "followingCount": 15,
    "yudateCount": 7,
    "isVerified": false,
    "isPrivate": false,
    "createdAt": "2026-01-01T00:00:00Z",
    "clerkId": "user_abc123"
  },
  "buyer": null,
  "highestBidder": null
}
```

---

### PUT: 商品更新

#### リクエストBody

```json
{
  "title": "更新後のタイトル",
  "description": "更新後の説明",
  "price": 200,
  "saleType": "auction",
  "buyoutPrice": 1000,
  "stock": 2
}
```

#### レスポンス: 成功 (200)

```json
{
  "id": "string",
  "title": "更新後のタイトル",
  "description": "更新後の説明",
  "itemType": "image",
  "itemData": "string (URL)",
  "price": 200,
  "saleType": "auction",
  "status": "selling",
  "stock": 2,
  "auctionEndAt": "2026-07-13T00:00:00Z",
  "highestBid": 150,
  "buyoutPrice": 1000,
  "likeCount": 10,
  "commentCount": 3,
  "isLiked": false,
  "isBought": false,
  "createdAt": "2026-07-10T12:00:00Z",
  "updatedAt": "2026-07-12T10:00:00Z",
  "seller": { "...User..." },
  "buyer": null,
  "highestBidder": null
}
```

#### エラー

```json
{
  "error": "あなたの出品した商品のみ編集できます"
}
```

---

### DELETE: 商品削除

#### リクエストBody

なし

#### レスポンス: 成功 (200)

```json
{
  "message": "商品を削除しました"
}
```

#### エラー

```json
{
  "error": "あなたの出品した商品のみ削除できます"
}
```

## 実行例

```javascript
// GET
fetch("/api/market/items/abc123", {
  credentials: "include"
})

// PUT
fetch("/api/market/items/abc123", {
  method: "PUT",
  credentials: "include",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({ title: "新しいタイトル", price: 300 })
})

// DELETE
fetch("/api/market/items/abc123", {
  method: "DELETE",
  credentials: "include"
})
```

## 備考

- レスポンスに含まれる `seller`, `buyer`, `highestBidder` は完全なUserオブジェクトです（メールアドレス等の個人情報を含みます）
- PUT・DELETE は出品者のみ実行可能で、他のユーザーが実行すると403エラーとなります
- PUTのbodyは部分更新が可能で、指定したフィールドのみ上書きされます
