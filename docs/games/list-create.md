# [GET / POST] /api/games

> 認証: 要 | レート制限: なし

## 説明

全ゲーム一覧の取得と、新規ゲームの作成を行います。

## リクエスト

### パスパラメータ

なし

---

### GET: 全ゲーム一覧

#### クエリパラメータ

なし（ページネーションなし）

#### リクエストBody

なし

#### レスポンス: 成功 (200)

```json
[
  {
    "id": "string",
    "title": "My Game",
    "description": "楽しいゲームです",
    "htmlContent": "<html>...</html>",
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
]
```

---

### POST: 新規ゲーム作成

#### リクエストBody

```json
{
  "title": "My Game",
  "description": "楽しいゲームです",
  "playPrice": 100,
  "htmlContent": "<html><body><canvas id='game'></canvas><script>...</script></body></html>"
}
```

#### レスポンス: 成功 (201)

```json
{
  "id": "string",
  "title": "My Game",
  "description": "楽しいゲームです",
  "htmlContent": "<html><body><canvas id='game'></canvas><script>...</script></body></html>",
  "playPrice": 100,
  "createdAt": "2026-07-12T10:00:00Z",
  "updatedAt": "2026-07-12T10:00:00Z",
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

#### エラー

```json
{
  "error": "タイトルとHTMLコンテンツは必須です"
}
```

## 実行例

```javascript
// GET
fetch("/api/games", {
  credentials: "include"
})

// POST
fetch("/api/games", {
  method: "POST",
  credentials: "include",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({
    title: "My Game",
    description: "楽しいゲームです",
    playPrice: 100,
    htmlContent: "<html>..."
  })
})
```

## 備考

- GETのレスポンスに含まれる `creator` は完全なUserオブジェクトです（メールアドレス等の個人情報を含みます）
- POST の成功ステータスは `201 Created` です
- POST で作成されたGameの `creator` にも完全なUserオブジェクトが含まれます
- DELETE エンドポイントは存在しません
- `description` は任意、`htmlContent` は必須です
