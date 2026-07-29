# [GET] /api/explore/popular

> 認証: 不要 | レート制限: なし

## 説明
人気の投稿一覧を取得します。いいね数やリユデート数などに基づいてランク付けされます。

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
      "id": 50,
      "content": "たくさんいいねされた投稿",
      "imageUrl": "https://example.com/popular.jpg",
      "author": {
        "id": 7,
        "username": "hanako",
        "displayName": "花子",
        "avatarUrl": "https://example.com/avatar2.jpg",
        "headerUrl": null,
        "bio": "花子です",
        "isVerified": true,
        "isPrivate": false,
        "followerCount": 120,
        "followingCount": 80
      },
      "likeCount": 342,
      "reyudateCount": 89,
      "replyCount": 15,
      "isLiked": false,
      "isReyudated": false,
      "isSpoiler": false,
      "reactions": [
        { "emoji": "🔥", "count": 12, "isReacted": false },
        { "emoji": "❤️", "count": 45, "isReacted": true }
      ],
      "quotedYudate": null,
      "replyToId": null,
      "superYudateAmount": 0,
      "createdAt": "2025-07-11T08:00:00Z"
    }
  ],
  "nextCursor": "eyJpZCI6NDB9"
}
```

## 実行例
```javascript
fetch("/api/explore/popular?cursor=eyJpZCI6NDB9")
```

## 備考
- 人気度の計算アルゴリズムはサーバー側で定義されます（いいね・リユデート・リアクションを複合的に評価）
- 一定期間内の投稿が対象です
