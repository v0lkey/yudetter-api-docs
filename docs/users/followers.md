# [GET] /api/users/:username/followers

> 認証: 不要 | レート制限: なし

## 説明

指定したユーザーのフォロワー一覧をページネーション付きで返します。

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
      "id": 10,
      "clerkId": "user_fan001",
      "username": "fan_taro",
      "displayName": "タロウファン",
      "email": "fan@example.com",
      "bio": "タロウの大ファンです",
      "avatarUrl": null,
      "headerUrl": null,
      "birthday": "2001-04-15",
      "setupComplete": true,
      "followerCount": 5,
      "followingCount": 100,
      "yudateCount": 20,
      "isFollowing": false,
      "isFollowPending": false,
      "isPrivate": false,
      "isBlocking": false,
      "isBlockedBy": false,
      "pinnedYudateId": null,
      "yudedollar": 200,
      "badgeType": null,
      "isVerified": false,
      "consecutiveLoginDays": 3,
      "rankingOptIn": false,
      "createdAt": "2026-03-10T09:00:00.000Z"
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
curl -X GET https://yudetter.com/api/users/taro_yudetter/followers
curl -X GET "https://yudetter.com/api/users/taro_yudetter/followers?cursor=abc123"
```

```javascript
fetch("/api/users/taro_yudetter/followers?cursor=abc123")
  .then(res => res.json())
  .then(data => console.log(data.items))
```

## 備考

- レスポンスは `PaginatedResponse<User>` 形式に従います。
- items は User オブジェクトの配列です（`email`, `birthday`, `yudedollar`, `clerkId` を含みます）。
- 認証不要ですが、認証済みの場合は `isFollowing` 等が設定されます。
