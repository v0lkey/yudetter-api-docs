# [GET] /api/users/me

> 認証: 要 | レート制限: なし

## 説明

現在認証中のユーザーの完全な User オブジェクトを返します。認証確認用としても使用可能です。Cookie に有効な BetterAuth セッション（`__Secure-better-auth.session_token`）が存在する場合のみ成功します。

## リクエスト

### パスパラメータ
なし

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

### エラー (401)
```json
{
  "error": "認証が必要です"
}
```

## 実行例

```bash
curl -X GET https://yudetter.com/api/users/me -b "cookie"
```

```javascript
fetch("/api/users/me", { credentials: "include" })
```

## 備考

- `email`, `birthday`, `yudedollar`, `clerkId` は機密情報です。レスポンスに含まれます。
- 認証が切れている場合は 401 が返ります。
- このエンドポイントが 200 を返せば認証済み、401 なら未認証の判定基準として利用できます。
