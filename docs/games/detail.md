# [GET] /api/games/:id

> 認証: 要 | レート制限: なし

## 説明

特定のゲームの詳細情報を取得します。

## リクエスト

### パスパラメータ

| 名前 | 型 | 必須 | 説明 |
|------|-----|------|------|
| `id` | `string` | はい | ゲームID |

### リクエストBody

なし

## レスポンス

### 成功 (200)

```json
{
  "id": "string",
  "title": "My Game",
  "description": "楽しいゲームです",
  "htmlContent": "<html><body><canvas id='game'></canvas><script>// 全ソースコード</script></body></html>",
  "playPrice": 100,
  "createdAt": "2026-07-10T12:00:00Z",
  "updatedAt": "2026-07-10T12:00:00Z",
  "creator": {
    "id": "string",
    "username": "creator_user",
    "displayName": "製作者",
    "email": "creator@example.com",
    "avatarUrl": "https://example.com/avatar.png",
    "bio": "ゲームを作っています",
    "birthday": "2000-01-01",
    "yudedollar": 5000,
    "followerCount": 100,
    "followingCount": 30,
    "yudateCount": 20,
    "isVerified": true,
    "isPrivate": false,
    "createdAt": "2026-01-01T00:00:00Z",
    "clerkId": "user_abc123"
  }
}
```

### エラー

```json
{
  "error": "ゲームが見つかりません"
}
```

## 実行例

```javascript
fetch("/api/games/abc123", {
  credentials: "include"
})
```

## 備考

- `creator` には完全なUserオブジェクトが含まれます（メールアドレス等の個人情報を含みます）
- `htmlContent` にはゲームの全ソースコード（HTML + JavaScript等）がそのまま含まれます
