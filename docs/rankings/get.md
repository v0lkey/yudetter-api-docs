# [GET] /api/rankings

> 認証: 要 | レート制限: なし

## 説明

現在のランキング一覧を取得します。投稿数・フォロワー数の週間・全期間の4カテゴリのランキングが返却されます。

## リクエスト

### パスパラメータ

なし

### リクエストBody

なし

## レスポンス

### 成功 (200)

```json
{
  "weekly": {
    "post": [
      {
        "user": {
          "id": "string",
          "username": "top_poster",
          "displayName": "投稿王",
          "email": "top@example.com",
          "avatarUrl": "https://example.com/avatar.png",
          "bio": "たくさん投稿します",
          "birthday": "2000-01-01",
          "yudedollar": 10000,
          "followerCount": 200,
          "followingCount": 50,
          "yudateCount": 150,
          "isVerified": true,
          "isPrivate": false,
          "createdAt": "2026-01-01T00:00:00Z",
          "clerkId": "user_abc123"
        },
        "score": 42
      }
    ],
    "follower": [
      {
        "user": { "...User..." },
        "score": 500
      }
    ]
  },
  "allTime": {
    "post": [
      {
        "user": { "...User..." },
        "score": 1000
      }
    ],
    "follower": [
      {
        "user": { "...User..." },
        "score": 5000
      }
    ]
  }
}
```

### エラー

```json
{
  "error": "認証が必要です"
}
```

## 実行例

```javascript
fetch("/api/rankings", {
  credentials: "include"
})
```

## 備考

- 4カテゴリのランキングが同時に返却されます
  - `weekly.post`: 週間投稿数ランキング
  - `weekly.follower`: 週間フォロワー増加数ランキング
  - `allTime.post`: 全期間投稿数ランキング
  - `allTime.follower`: 全期間フォロワー数ランキング
- ⚠️ 各エントリの `user` には完全なUserオブジェクトが含まれます（`email`, `birthday`, `yudedollar`, `clerkId` 等の個人情報を含みます）
- `score` はランキングのスコア値です
- ランキングに参加していないユーザーは結果に含まれません（参加には `/api/rankings/opt-in` が必要）
