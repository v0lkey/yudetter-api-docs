# [GET] /api/users/:username/yudates

> 認証: 不要 | レート制限: なし

## 説明

指定したユーザーの投稿（Yudate）一覧をページネーション付きで返します。自分の投稿は全て表示され、他ユーザーの投稿は公開設定のもののみ表示されます。

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
      "id": 100,
      "content": "今日のゆで卵最高だった #yudette",
      "imageUrl": null,
      "author": {
        "id": 1,
        "clerkId": "user_2abc123def456",
        "username": "taro_yudetter",
        "displayName": "タロウ",
        "email": "taro@example.com",
        "bio": "ゆで太郎です",
        "avatarUrl": "https://yaubmafsnsufjasndnft.supabase.co/storage/v1/object/public/yudetter-bucket/1712345678-abc.jpg",
        "headerUrl": null,
        "birthday": "2000-08-03",
        "setupComplete": true,
        "followerCount": 42,
        "followingCount": 18,
        "yudateCount": 156,
        "isFollowing": false,
        "isFollowPending": false,
        "isPrivate": false,
        "isBlocking": false,
        "isBlockedBy": false,
        "pinnedYudateId": null,
        "yudedollar": 5000,
        "badgeType": "gold",
        "isVerified": false,
        "consecutiveLoginDays": 7,
        "rankingOptIn": true,
        "createdAt": "2026-01-15T08:30:00.000Z"
      },
      "likeCount": 12,
      "reyudateCount": 3,
      "replyCount": 2,
      "isLiked": false,
      "isReyudated": false,
      "isSpoiler": false,
      "reactions": [
        { "emoji": "🔥", "count": 5, "isReacted": false },
        { "emoji": "❤️", "count": 3, "isReacted": false }
      ],
      "quotedYudate": null,
      "replyToId": null,
      "superYudateAmount": 0,
      "createdAt": "2026-07-11T09:26:43.908Z"
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
curl -X GET https://yudetter.com/api/users/taro_yudetter/yudates
curl -X GET "https://yudetter.com/api/users/taro_yudetter/yudates?cursor=abc123"
```

```javascript
fetch("/api/users/taro_yudetter/yudates")
  .then(res => res.json())
  .then(data => data.items.forEach(y => console.log(y.content)))
```

## 備考

- レスポンスは `PaginatedResponse<Yudate>` 形式に従います。
- 各 Yudate の `author` は完全な User オブジェクトです（`email`, `birthday`, `yudedollar`, `clerkId` を含みます）。
- 非公開アカウントの投稿はフォロワーのみ閲覧可能です。
- ブロックされているユーザーからは閲覧できません。
