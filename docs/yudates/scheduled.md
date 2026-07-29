# [GET, DELETE] /api/yudates/scheduled

> 認証: 要 | レート制限: なし

## 説明

予約投稿の一覧取得（GET）・削除（DELETE）を行います。

## GET レスポンス

### 成功 (200)

```json
{
  "items": [
    {
      "id": 100,
      "content": "明日投稿する内容",
      "scheduledFor": "2026-07-30T09:00:00.000Z",
      "createdAt": "2026-07-29T12:00:00.000Z"
    }
  ],
  "nextCursor": null
}
```

## DELETE - /api/yudates/scheduled/:id

### パスパラメータ

| 名前 | 型 | 必須 | 説明 |
|------|-----|------|------|
| `id` | number | はい | 予約投稿ID |

### 成功 (204)

レスポンスボディなし。

### エラー

```json
{
  "error": "予約投稿が見つかりません"
}
```

## 実行例

```javascript
// 予約一覧取得
fetch("/api/yudates/scheduled", {
  credentials: "include"
})

// 予約削除
fetch("/api/yudates/scheduled/100", {
  method: "DELETE",
  credentials: "include"
})
```
