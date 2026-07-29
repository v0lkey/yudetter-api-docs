# [POST] /api/yudates/:id/like

> 認証: 要 | レート制限: なし

## 説明
指定された投稿に「いいね」をします。既にいいねしている場合はいいねが取り消されます（トグル動作）。

## リクエスト

### パスパラメータ

| 名前 | 型 | 必須 | 説明 |
|---|---|---|---|
| `id` | number | はい | いいねする投稿ID |

### リクエストBody

なし

## レスポンス
### 成功 (200)
```json
{
  "id": 1,
  "isLiked": true,
  "likeCount": 6
}
```
### エラー (401)
```json
{
  "error": "認証が必要です"
}
```
### エラー (404)
```json
{
  "error": "投稿が見つかりません"
}
```

## 実行例
```javascript
fetch("/api/yudates/1/like", { method: "POST", credentials: "include" })
```

## 備考
- トグル動作です。実行するたびに `isLiked` が反転します
- 自分の投稿にもいいねできます

---

# [POST] /api/yudates/:id/reyudate

> 認証: 要 | レート制限: なし

## 説明
指定された投稿をリユデート（引用拡散）します。自分のフォロワーのタイムラインに共有されます。

## リクエスト

### パスパラメータ

| 名前 | 型 | 必須 | 説明 |
|---|---|---|---|
| `id` | number | はい | リユデートする投稿ID |

### リクエストBody
```json
{
  "content": "この投稿すごい！"
}
```

| 名前 | 型 | 必須 | 説明 |
|---|---|---|---|
| content | string | いいえ | 引用コメント（空の場合は単純リユデート） |

## レスポンス
### 成功 (201)
```json
{
  "id": 456,
  "content": "この投稿すごい！",
  "author": { "id": 42, "username": "taro" },
  "quotedYudate": {
    "id": 1,
    "content": "元の投稿内容",
    "author": { "id": 7, "username": "hanako", "displayName": "花子" },
    "imageUrl": null,
    "likeCount": 5,
    "reyudateCount": 2,
    "replyCount": 1,
    "isLiked": false,
    "isReyudated": false,
    "isSpoiler": false,
    "reactions": [],
    "quotedYudate": null,
    "replyToId": null,
    "superYudateAmount": 0,
    "createdAt": "2026-07-11T09:00:00.000Z"
  },
  "likeCount": 0,
  "reyudateCount": 0,
  "replyCount": 0,
  "isLiked": false,
  "isReyudated": false,
  "reactions": [],
  "createdAt": "2025-07-12T11:00:00Z"
}
```
### エラー (401)
```json
{
  "error": "認証が必要です"
}
```
### エラー (404)
```json
{
  "error": "投稿が見つかりません"
}
```

## 実行例
```javascript
fetch("/api/yudates/1/reyudate", {
  method: "POST",
  credentials: "include",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({ content: "この投稿すごい！" })
})
```

## 備考
- リユデートは新しいYudateとして作成され、`quotedYudate` フィールドに元の投稿が格納されます
- content を空にすると引用コメントなしのリユデートになります
- すでにリユデートしているかの確認は GET /api/yudates/:id の `isReyudated` フィールドで行えます

---

# [DELETE] /api/yudates/:id/like

> 認証: 要 | レート制限: なし

## 説明

指定された投稿のいいねを取り消します。POST 版とは異なり、トグル動作ではなく明確にいいねを削除します。

## リクエスト

### パスパラメータ

| 名前 | 型 | 必須 | 説明 |
|---|---|---|---|
| `id` | number | はい | いいね解除する投稿ID |

### リクエストBody

なし

## レスポンス

### 成功 (200)

```json
{
  "id": 1,
  "isLiked": false,
  "likeCount": 5
}
```

### エラー (401)

```json
{
  "error": "認証が必要です"
}
```

## 実行例

```javascript
fetch("/api/yudates/1/like", { method: "DELETE", credentials: "include" })
```

---

# [DELETE] /api/yudates/:id/reyudate

> 認証: 要 | レート制限: なし

## 説明

指定された投稿のリユデート（拡散）を取り消します。

## リクエスト

### パスパラメータ

| 名前 | 型 | 必須 | 説明 |
|---|---|---|---|
| `id` | number | はい | リユデート解除する投稿ID |

### リクエストBody

なし

## レスポンス

### 成功 (200)

```json
{
  "message": "リユデートを取り消しました"
}
```

### エラー (401)

```json
{
  "error": "認証が必要です"
}
```

## 実行例

```javascript
fetch("/api/yudates/1/reyudate", { method: "DELETE", credentials: "include" })
```
