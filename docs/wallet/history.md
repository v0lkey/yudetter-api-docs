# [GET] /api/wallet/history

> 認証: 要 | レート制限: なし

## 説明
ウォレット（YD）の取引履歴をカーソルページネーションで取得します。YDの出入金、消費、スーパーユデート等の記録が表示されます。

## リクエスト
### クエリパラメータ
| 名前 | 型 | 必須 | 説明 |
|---|---|---|---|
| cursor | string | いいえ | 次ページを取得するためのカーソル |

## レスポンス
### 成功 (200)
```json
{
  "items": [
    {
      "id": 1,
      "type": "earn",
      "amount": 10,
      "description": "投稿がいいねされました",
      "createdAt": "2025-07-12T10:30:00Z"
    },
    {
      "id": 2,
      "type": "spend",
      "amount": -5,
      "description": "スーパーユデート",
      "createdAt": "2025-07-12T09:00:00Z"
    },
    {
      "id": 3,
      "type": "penalty",
      "amount": -5,
      "description": "返信あり投稿削除のペナルティ",
      "createdAt": "2025-07-11T18:00:00Z"
    }
  ],
  "nextCursor": "eyJpZCI6NH0="
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
fetch("/api/wallet/history?cursor=eyJpZCI6NH0=", { credentials: "include" })
```

## 備考
- 新しい取引が先頭に来ます（作成日時の降順）
- `type` の値例: `earn`（獲得）, `spend`（消費）, `penalty`（ペナルティ）, `purchase`（購入）
- `amount` が正の値は入金、負の値は出金を表します
- 現在の残高を取得するにはユーザー情報の `yudedollar` フィールドを参照してください
