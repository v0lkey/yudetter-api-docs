# [POST] /api/yudates/:id/reactions

> 認証: 要 | レート制限: なし

## 説明
指定された投稿に絵文字リアクションを追加します。既に同じ絵文字でリアクションしている場合は何も変更されません。

## リクエスト

### パスパラメータ

| 名前 | 型 | 必須 | 説明 |
|---|---|---|---|
| `id` | number | はい | リアクション対象の投稿ID |

### リクエストBody
```json
{
  "emoji": "🔥"
}
```

| 名前 | 型 | 必須 | 説明 |
|---|---|---|---|
| emoji | string | はい | 追加する絵文字（1文字推奨） |

## レスポンス
### 成功 (200)
```json
{
  "id": 1,
  "reactions": [
    { "emoji": "🔥", "count": 3, "me": true },
    { "emoji": "❤️", "count": 7, "me": false }
  ]
}
```
### エラー (400)
```json
{
  "error": "emojiは必須です"
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
fetch("/api/yudates/1/reactions", {
  method: "POST",
  credentials: "include",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({ emoji: "🔥" })
})
```

## 備考
- `me` フィールドで自分がそのリアクションをしたかどうかが分かります
- 対応絵文字はサーバー設定に依存します

---

# [DELETE] /api/yudates/:id/reactions/:emoji

> 認証: 要 | レート制限: なし

## 説明
指定された投稿から自分の絵文字リアクションを削除します。

## リクエスト

### パスパラメータ

| 名前 | 型 | 必須 | 説明 |
|---|---|---|---|
| `id` | number | はい | 投稿ID |
| `emoji` | string | はい | 削除する絵文字（URLエンコード必須） |

### リクエストBody

なし

## レスポンス
### 成功 (200)
```json
{
  "message": "リアクションを削除しました",
  "reactions": [
    { "emoji": "❤️", "count": 7, "me": false }
  ]
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
fetch("/api/yudates/1/reactions/🔥", { method: "DELETE", credentials: "include" })
```

## 備考
- `:emoji` パスパラメータはURLエンコードが必要です（例: `%F0%9F%94%A5`）
- リアクションしていない絵文字を削除しようとしてもエラーにはなりません
