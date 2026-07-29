# [GET] /api/yudates/:id

> 認証: 不要 | レート制限: なし

## 説明
指定されたIDの投稿を単体で取得します。

## リクエスト

### パスパラメータ

| 名前 | 型 | 必須 | 説明 |
|---|---|---|---|
| `id` | number | はい | 取得する投稿ID |

### クエリパラメータ

なし

### リクエストBody

なし

## レスポンス
### 成功 (200)
```json
{
  "id": 1,
  "content": "今日も良い天気ですね",
  "imageUrl": null,
  "author": {
    "id": 42,
    "username": "taro",
    "displayName": "太郎",
    "avatarUrl": "https://example.com/avatar.jpg",
    "isVerified": false
  },
  "likeCount": 5,
  "reyudateCount": 2,
  "replyCount": 1,
  "isLiked": false,
  "isReyudated": false,
  "reactions": [],
  "quotedYudate": null,
  "replyToId": null,
  "superYudateAmount": 0,
  "isSpoiler": false,
  "createdAt": "2025-07-12T10:00:00Z"
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
fetch("/api/yudates/1")
```

## 備考
- 削除済みの投稿にはアクセスできません（404）

---

# [PATCH] /api/yudates/:id

> 認証: 要 | レート制限: なし

## 説明
自分の投稿の内容を編集します。他人の投稿は編集できません。

## リクエスト
### リクエストBody
```json
{
  "content": "編集後の内容です"
}
```

| 名前 | 型 | 必須 | 説明 |
|---|---|---|---|
| content | string | はい | 新しい投稿内容 |

## レスポンス
### 成功 (200)
```json
{
  "id": 1,
  "content": "編集後の内容です",
  "imageUrl": null,
  "author": { "id": 42, "username": "taro" },
  "likeCount": 5,
  "reyudateCount": 2,
  "replyCount": 1,
  "isLiked": false,
  "isReyudated": false,
  "reactions": [],
  "quotedYudate": null,
  "replyToId": null,
  "superYudateAmount": 0,
  "isSpoiler": false,
  "createdAt": "2025-07-12T10:00:00Z"
}
```
### エラー (403)
```json
{
  "error": "自分の投稿のみ編集可能です"
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
fetch("/api/yudates/1", {
  method: "PATCH",
  credentials: "include",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({ content: "編集後の内容です" })
})
```

## 備考
- 自分の投稿のみ編集可能です
- 編集後も `createdAt` は変更されません

---

# [DELETE] /api/yudates/:id

> 認証: 要 | レート制限: なし

## 説明
自分の投稿を削除します。投稿に返信が存在する場合、5YDが差し引かれます。

## レスポンス
### 成功 (200)
```json
{
  "message": "投稿を削除しました",
  "penalty": 0
}
```
### 成功（返信あり・減額あり） (200)
```json
{
  "message": "投稿を削除しました（返信があるため5YD減額されました）",
  "penalty": 5
}
```
### エラー (403)
```json
{
  "error": "自分の投稿のみ削除可能です"
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
fetch("/api/yudates/1", { method: "DELETE", credentials: "include" })
```

## 備考
- 返信がある投稿を削除すると5YDがウォレットから差し引かれます
- 残高不足の場合は削除できません
- 返信がない場合は無料で削除できます
