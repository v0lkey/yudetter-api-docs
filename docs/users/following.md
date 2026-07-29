# [GET] /api/users/:username/following

> 認証: 不要 | レート制限: なし

## 説明

指定したユーザーがフォローしているユーザーの一覧をページネーション付きで返します。

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
      "id": 3,
      "clerkId": "user_celeb002",
      "username": "celeb_user",
      "displayName": "セレブ",
      "email": "celeb@example.com",
      "bio": "有名人です",
      "avatarUrl": "https://yaubmafsnsufjasndnft.supabase.co/storage/v1/object/public/yudetter-bucket/1712345999-def.jpg",
      "headerUrl": null,
      "birthday": "1995-11-30",
      "setupComplete": true,
      "followerCount": 10000,
      "followingCount": 50,
      "yudateCount": 500,
      "isFollowing": false,
      "isFollowPending": false,
      "isPrivate": false,
      "isBlocking": false,
      "isBlockedBy": false,
      "pinnedYudateId": null,
      "yudedollar": 99999,
      "badgeType": "gold",
      "isVerified": true,
      "consecutiveLoginDays": 365,
      "rankingOptIn": true,
      "createdAt": "2025-06-01T00:00:00.000Z"
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
curl -X GET https://yudetter.com/api/users/taro_yudetter/following
curl -X GET "https://yudetter.com/api/users/taro_yudetter/following?cursor=abc123"
```

```javascript
fetch("/api/users/taro_yudetter/following")
  .then(res => res.json())
  .then(data => console.log(data.items))
```

## 備考

- レスポンスは `PaginatedResponse<User>` 形式に従います。
- items は User オブジェクトの配列です。
- `followingCount` が多いユーザーの場合は `cursor` を使用してページングしてください。
