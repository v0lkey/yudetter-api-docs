# [GET] /api/yudates/:id/replies

> 認証: 不要 | レート制限: なし

## 説明
指定された投稿に対する返信一覧をカーソルページネーションで取得します。

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
      "id": 10,
      "content": "返信コメントです",
      "imageUrl": null,
      "author": {
        "id": 7,
        "username": "hanako",
        "displayName": "花子",
        "avatarUrl": "https://example.com/avatar2.jpg",
        "isVerified": true
      },
      "likeCount": 3,
      "reyudateCount": 0,
      "replyCount": 0,
      "isLiked": false,
      "isReyudated": false,
      "reactions": [],
      "quotedYudate": null,
      "replyToId": 1,
      "superYudateAmount": 0,
      "isSpoiler": false,
      "createdAt": "2025-07-12T10:05:00Z"
    }
  ],
  "nextCursor": null
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
fetch("/api/yudates/1/replies?cursor=eyJpZCI6MTB9")
```

## 備考
- 返信は作成日時の昇順（古い順）で返却されます
- `replyToId` が元の投稿IDと一致します
- 未認証でも閲覧可能です
