# [GET, POST] /api/market/items/:id/comments

> 認証: 要 | レート制限: なし

## 説明

商品のコメント一覧の取得（GET）・追加（POST）を行います。

## リクエスト (GET)

### パスパラメータ

| 名前 | 型 | 必須 | 説明 |
|------|-----|------|------|
| `id` | `string` | はい | 商品ID |

## レスポンス (GET)

### 成功 (200)

```json
{
  "comments": [
    {
      "id": "cmt_001",
      "content": "素晴らしい商品ですね！",
      "user": {
        "id": "usr_001",
        "username": "commenter",
        "displayName": "コメント投稿者",
        "avatarUrl": "https://example.com/avatar.png"
      },
      "itemId": "abc123",
      "createdAt": "2026-07-12T10:00:00Z"
    }
  ]
}
```

## リクエスト (POST)

### パスパラメータ

| 名前 | 型 | 必須 | 説明 |
|------|-----|------|------|
| `id` | `string` | はい | 商品ID |

### リクエストBody

```json
{
  "content": "素晴らしい商品ですね！"
}
```

## レスポンス (POST)

### 成功 (200)

```json
{
  "id": "string",
  "content": "素晴らしい商品ですね！",
  "user": {
    "id": "string",
    "username": "commenter",
    "displayName": "コメント投稿者",
    "avatarUrl": "https://example.com/avatar.png"
  },
  "itemId": "abc123",
  "createdAt": "2026-07-12T10:00:00Z"
}
```

### エラー

```json
{
  "error": "商品が見つかりません"
}
```

```json
{
  "error": "コメント内容は必須です"
}
```

## 実行例

```javascript
// コメント一覧取得
fetch("/api/market/items/abc123/comments", {
  credentials: "include"
})

// コメント投稿
fetch("/api/market/items/abc123/comments", {
  method: "POST",
  credentials: "include",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({ content: "素晴らしい商品ですね！" })
})
```

## 備考

- `content` は必須です。空文字列の場合はエラーになります
