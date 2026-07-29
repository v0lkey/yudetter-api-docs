# [GET] /api/users/me/follow-requests

> 認証: 要 | レート制限: なし

## 説明

自分宛の未承認フォローリクエスト一覧を取得します。非公開アカウントの場合にのみ意味があります。

## リクエスト

### クエリパラメータ

なし

### リクエストBody

なし

## レスポンス

### 成功 (200)

```json
[
  {
    "id": 1,
    "username": "other_user",
    "displayName": "その他ユーザー",
    "avatarUrl": null,
    "bio": "プロフィール",
    "followerCount": 10,
    "followingCount": 5,
    "isVerified": false,
    "isPrivate": false
  }
]
```

リクエストしてきたユーザーの簡略Userオブジェクトの配列。

### エラー (401)

```json
{
  "error": "認証が必要です"
}
```

## 実行例

```javascript
fetch("/api/users/me/follow-requests", { credentials: "include" })
```
