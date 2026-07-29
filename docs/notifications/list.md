# [GET] /api/notifications

> 認証: 要 | レート制限: なし

## 説明
ログインユーザー宛の通知一覧をカーソルページネーションで取得します。

## リクエスト
### クエリパラメータ
| 名前 | 型 | 必須 | 説明 |
|---|---|---|---|
| cursor | number | いいえ | 次ページを取得するためのカーソル |

## レスポンス
### 成功 (200)
```json
{
  "items": [
    {
      "id": 1,
      "type": "like",
      "actor": {
        "id": 42,
        "username": "hanako",
        "displayName": "花子",
        "avatarUrl": "https://example.com/avatar2.jpg",
        "isVerified": false
      },
      "yudate": {
        "id": 100,
        "content": "対象の投稿内容",
        "author": { "id": 7, "username": "taro" }
      },
      "actionYudate": null,
      "itemId": null,
      "detail": null,
      "read": false,
      "createdAt": "2025-07-12T10:30:00Z"
    },
    {
      "id": 2,
      "type": "follow",
      "actor": {
        "id": 99,
        "username": "john",
        "displayName": "John",
        "avatarUrl": "https://example.com/avatar3.jpg",
        "isVerified": false
      },
      "yudate": null,
      "actionYudate": null,
      "itemId": null,
      "detail": null,
      "read": true,
      "createdAt": "2025-07-12T09:00:00Z"
    }
  ],
  "nextCursor": null
}
```

| フィールド | 型 | 説明 |
|---|---|---|
| `items[].type` | string | `"like"`, `"follow"`, `"reyudate"`, `"reply"`, `"super_yudate"` 等 |
| `items[].actor` | User | アクションを起こしたユーザー（完全なUserオブジェクト） |
| `items[].yudate` | Yudate \| null | 対象の投稿 |
| `items[].actionYudate` | Yudate \| null | アクションで作成された投稿（リユデート等） |
| `items[].itemId` | number \| null | 関連アイテムID（マーケット等） |
| `items[].detail` | string \| null | 追加詳細情報 |
| `items[].read` | boolean | 既読状態 |
| `nextCursor` | number \| null | 次ページカーソル（null=最終ページ） |

### エラー (401)
```json
{
  "error": "認証が必要です"
}
```

## 実行例
```javascript
fetch("/api/notifications?cursor=12345", { credentials: "include" })
```

## 備考
- 新しい通知が先頭に来ます（作成日時の降順）
- `actor` は完全なUserオブジェクトです（`email`, `birthday`, `yudedollar` 等の機密情報を含みます）
- SSEストリーム（`/api/notifications/stream`）では異なる形状でデータが配信されます
