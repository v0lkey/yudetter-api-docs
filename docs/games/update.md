# [PUT] /api/games/:id | [DELETE] /api/games/:id

> 認証: 要 | レート制限: なし

## 説明

ゲーム情報の更新（PUT）・削除（DELETE）を行います。自分の作成したゲームのみ操作可能です。

## リクエスト

### パスパラメータ

| 名前 | 型 | 必須 | 説明 |
|------|-----|------|------|
| `id` | `string` | はい | 更新するゲームID |

### リクエストBody

すべて任意（部分更新対応）

```json
{
  "title": "更新後のタイトル",
  "description": "更新後の説明",
  "playPrice": 200,
  "htmlContent": "<html><body>...更新後のソースコード...</body></html>"
}
```

## レスポンス

### 成功 (200)

```json
{
  "id": "string",
  "title": "更新後のタイトル",
  "description": "更新後の説明",
  "htmlContent": "<html><body>...更新後のソースコード...</body></html>",
  "playPrice": 200,
  "createdAt": "2026-07-10T12:00:00Z",
  "updatedAt": "2026-07-12T10:00:00Z",
  "creator": { "...User..." }
}
```

### エラー

```json
{
  "error": "タイトルとHTMLコンテンツは必須です"
}
```

```json
{
  "error": "あなたが作成したゲームのみ編集できます"
}
```

```json
{
  "error": "ゲームが見つかりません"
}
```

## 実行例

```javascript
fetch("/api/games/abc123", {
  method: "PUT",
  credentials: "include",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({ title: "更新後のタイトル", playPrice: 200 })
})
```

## 備考

- 自分の作成したゲームのみ編集可能です。他のユーザーのゲームを編集しようとすると `403 Forbidden` が返ります
- リクエストBodyはすべて任意ですが、`title` または `htmlContent` を含む場合は両方必須です（片方だけではエラー）
- 実際に変更のあったフィールドのみが更新されます（部分更新）

## DELETE レスポンス

### 成功 (204)

レスポンスボディなし。

### エラー

```json
{
  "error": "あなたが作成したゲームのみ削除できます"
}
```

## DELETE 実行例

```javascript
fetch("/api/games/abc123", {
  method: "DELETE",
  credentials: "include"
})
```
