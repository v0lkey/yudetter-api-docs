# [POST] /api/yudates/:id/report

> 認証: 要 | レート制限: なし

## 説明
指定された投稿を通報します。クライアント側で確認ダイアログを表示してから実行してください。

## リクエスト

### パスパラメータ

| 名前 | 型 | 必須 | 説明 |
|---|---|---|---|
| `id` | number | はい | 通報する投稿ID |

### リクエストBody
```json
{
  "reason": "不適切な内容を含んでいます"
}
```

| 名前 | 型 | 必須 | 説明 |
|---|---|---|---|
| reason | string | いいえ | 通報理由（省略可） |

## レスポンス
### 成功 (200)
```json
{
  "message": "投稿を通報しました"
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
### エラー (409)
```json
{
  "error": "この投稿は既に通報済みです"
}
```

## 実行例
```javascript
if (confirm("この投稿を通報しますか？")) {
  fetch("/api/yudates/1/report", {
    method: "POST",
    credentials: "include",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({ reason: "不適切な内容です" })
  });
}
```

## 備考
- クライアント側で確認ダイアログを表示することが推奨されます
- 同じ投稿を複数回通報することはできません（409 Conflict）
- 通報は管理者に通知され、適宜対応が行われます
