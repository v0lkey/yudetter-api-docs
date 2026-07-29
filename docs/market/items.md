# [GET] /api/market/items

> 認証: 要 | レート制限: なし

## 説明

マーケットに出品されている全商品を一覧で取得します。ページネーションはなく、全商品が配列として返されます。

## リクエスト

### パスパラメータ

なし

### クエリパラメータ

| 名前 | 型 | 必須 | 説明 |
|------|-----|------|------|
| `itemType` | `"image" \| "audio" \| "user_id"` | いいえ | 商品タイプでフィルタリング |
| `saleType` | `"normal" \| "auction"` | いいえ | 販売種別でフィルタリング |
| `status` | `"selling" \| "sold"` | いいえ | ステータスでフィルタリング |

### リクエストBody

なし

## レスポンス

### 成功 (200)

```json
[
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
]
```

### エラー

```json
{
  "error": "認証が必要です"
}
```

## 実行例

```javascript
fetch("/api/market/items", {
  credentials: "include"
})
```

## 備考

- `seller`, `buyer`, `highestBidder` には完全なUserオブジェクトが含まれます（メールアドレス等の個人情報も露出します）
- 該当するプロパティがない場合は `null` となります（例：`buyer` は未購入時 `null`）
- ページネーションは実装されておらず、全商品が一括で返却されます
