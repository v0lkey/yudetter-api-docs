# [POST] /api/market/items/:id/report

> 認証: 要 | レート制限: なし

## 説明

不適切な商品を報告します。

## リクエスト

### パスパラメータ

| 名前 | 型 | 必須 | 説明 |
|---|---|---|---|
| `id` | string | はい | 報告する商品のID |

### リクエストBody

```json
{
  "reason": "不適切な内容"
}
```

| 名前 | 型 | 必須 | 説明 |
|---|---|---|---|
| `reason` | string | はい | 報告理由 |

## レスポンス

### 成功 (200)

```json
{
  "message": "報告しました"
}
```

### エラー

```json
{
  "error": "認証が必要です"
}
```

```json
{
  "error": "商品が見つかりません"
}
```

## 実行例

```javascript
fetch("/api/market/items/abc123/report", {
  method: "POST",
  credentials: "include",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({ reason: "不適切な内容" })
})
```
