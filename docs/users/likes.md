# [GET] /api/users/:username/likes

> 認証: 不要 | レート制限: なし

## 説明

指定したユーザーがいいねした投稿一覧をページネーション付きで返します。

## リクエスト

### パスパラメータ
| 名前 | 型 | 必須 | 説明 |
|---|---|---|---|
| username | string | はい | 対象ユーザー名 |

### クエリパラメータ
| 名前 | 型 | 必須 | 説明 |
|---|---|---|---|
| cursor | string | いいえ | ページネーションカーソル |

### リクエストBody
なし

## レスポンス

### 成功 (200)
```json
{
  "items": [
    {
      "id": 200,
      "content": "いいねされた投稿の内容",
      "imageUrl": null,
      "author": {
        "id": 2,
        "clerkId": "user_other001",
        "username": "other_user",
        "displayName": "他人",
        "email": "other@example.com",
        "bio": null,
        "avatarUrl": null,
        "headerUrl": null,
        "birthday": "2002-12-25",
        "setupComplete": true,
        "followerCount": 30,
        "followingCount": 20,
        "yudateCount": 80,
        "isFollowing": false,
        "isFollowPending": false,
        "isPrivate": false,
        "isBlocking": false,
        "isBlockedBy": false,
        "pinnedYudateId": null,
        "yudedollar": 0,
        "badgeType": null,
        "isVerified": false,
        "consecutiveLoginDays": 2,
        "rankingOptIn": false,
        "createdAt": "2026-04-01T10:00:00.000Z"
      },
      "likeCount": 25,
      "reyudateCount": 1,
      "replyCount": 0,
      "isLiked": true,
      "isReyudated": false,
      "isSpoiler": false,
      "reactions": [],
      "quotedYudate": null,
      "replyToId": null,
      "superYudateAmount": 0,
      "createdAt": "2026-07-10T15:30:00.000Z"
    }
  ],
  "nextCursor": null
}
```

### エラー (404)
```json
{
  "error": "ユーザーが見つかりません"
}
```

## 実行例

```bash
curl -X GET https://yudetter.com/api/users/taro_yudetter/likes
curl -X GET "https://yudetter.com/api/users/taro_yudetter/likes?cursor=abc123"
```

```javascript
fetch("/api/users/taro_yudetter/likes")
  .then(res => res.json())
  .then(data => console.log(data.items))
```

## 備考

- レスポンスは `PaginatedResponse<Yudate>` 形式に従います。
- 各 Yudate の `author` は完全な User オブジェクトです（`email`, `birthday`, `yudedollar`, `clerkId` を含みます）。
- `isLiked` は常に `true` になります（いいねした投稿のみを返すため）。
- 非公開アカウントのいいね一覧はフォロワーのみ閲覧可能です。
