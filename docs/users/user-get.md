# [GET] /api/users/:username

> 認証: 不要 | レート制限: なし

## 説明

指定されたユーザーのプロフィール情報を取得します。認証済みの場合は、フォロー状態などの追加情報も含まれます。

`:username` パスパラメータは **username・id** のいずれでも検索可能です（レガシー互換で clerkId も可能な場合があります）。優先順位は `username > id` です。

## リクエスト

### パスパラメータ
| 名前 | 型 | 必須 | 説明 |
|---|---|---|---|
| username | string | はい | ユーザー名または内部ID |

### クエリパラメータ
なし

### リクエストBody
なし

## レスポンス

### 成功 (200)
```json
{
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
curl -X GET https://yudetter.com/api/users/taro_yudetter
curl -X GET https://yudetter.com/api/users/1
curl -X GET https://yudetter.com/api/users/user_2abc123def456
```

```javascript
fetch("/api/users/taro_yudetter")
  .then(res => res.json())
  .then(user => console.log(user.displayName))
```

## 備考

- `email`, `birthday`, `yudedollar`, `clerkId` は機密情報ですが、現在の実装ではレスポンスに含まれます。(現在はログイン中の自分のアカウントしか表示されなくなりました。他人のアカウントは全部空文字になります)
- 認証不要のエンドポイントですが、認証済みの場合は `isFollowing`, `isFollowPending`, `isBlocking`, `isBlockedBy` が適切に設定されます。
- 未認証の場合はこれらのフォロー関連フィールドはデフォルト値（`false`）になります。
- パスパラメータの検索優先順位: 数値のみ → id検索、英数字 → username 優先。
