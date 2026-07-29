# [GET] /api/explore

> 認証: 不要 | レート制限: なし

## 説明
最新の投稿一覧を取得します。Explore画面のデフォルトタブで表示される内容です。

## リクエスト
### クエリパラメータ
| 名前 | 型 | 必須 | 説明 |
|---|---|---|---|
| cursor | string | いいえ | 次ページを取得するためのカーソル |

## レスポンス
### 成功 (200)
```json
{
  "items": [
    {
      "id": 100,
      "content": "最新の投稿です",
      "imageUrl": null,
      "author": {
        "id": 42,
        "username": "taro",
        "displayName": "太郎",
        "avatarUrl": "https://example.com/avatar.jpg",
        "bio": "こんにちは",
        "isVerified": false,
        "isPrivate": false
      },
      "likeCount": 0,
      "reyudateCount": 0,
      "replyCount": 0,
      "isLiked": false,
      "isReyudated": false,
      "reactions": [],
      "quotedYudate": null,
      "replyToId": null,
      "superYudateAmount": 0,
      "isSpoiler": false,
      "createdAt": "2025-07-12T12:00:00Z"
    }
  ],
  "nextCursor": "eyJpZCI6OTB9"
}
```

## 実行例
```javascript
fetch("/api/explore?cursor=eyJpZCI6OTB9")
```

## 備考
- ⚠️ `items[].author` には User オブジェクトが含まれます。`bio` などの情報も出力されるため、クライアント側で機密情報の取り扱いに注意してください
- 未認証でも全件閲覧可能です
