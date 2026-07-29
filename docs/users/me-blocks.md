# [GET] /api/users/me/blocks

> 認証: 要 | レート制限: なし

## 説明

現在認証中のユーザーがブロックしているユーザーの一覧をページネーション付きで返します。

## リクエスト

### パスパラメータ
なし

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
      "id": 5,
      "clerkId": "user_xyz789",
      "username": "blocked_user",
      "displayName": "ブロック済み",
      "email": "blocked@example.com",
      "bio": null,
      "avatarUrl": null,
      "headerUrl": null,
      "birthday": "1999-05-20",
      "setupComplete": true,
      "followerCount": 10,
      "followingCount": 5,
      "yudateCount": 30,
      "isFollowing": false,
      "isFollowPending": false,
      "isPrivate": false,
      "isBlocking": true,
      "isBlockedBy": false,
      "pinnedYudateId": null,
      "yudedollar": 100,
      "badgeType": null,
      "isVerified": false,
      "consecutiveLoginDays": 1,
      "rankingOptIn": false,
      "createdAt": "2026-02-20T12:00:00.000Z"
    }
  ],
  "nextCursor": null
}
```

### エラー (401)
```json
{
  "error": "認証が必要です"
}
```

## 実行例

```bash
curl -X GET https://yudetter.com/api/users/me/blocks -b "cookie"
```

```javascript
fetch("/api/users/me/blocks", { credentials: "include" })
  .then(res => res.json())
  .then(data => console.log(data.items))
```

## 備考

- レスポンスは `PaginatedResponse<User>` 形式に従います。
- ブロック中のユーザー数が多い場合は `cursor` を使用してページングしてください。
- User オブジェクトの `isBlocking` が `true` になります。
