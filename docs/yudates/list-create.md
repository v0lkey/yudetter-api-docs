# [GET] /api/yudates

> 認証: 不要 | レート制限: なし

## 説明
タイムラインの投稿一覧をカーソルページネーションで取得します。フォロー中のユーザーの投稿が時系列で表示されます。

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
  ],
  "nextCursor": "eyJpZCI6MjB9"
}
```
## 実行例
```javascript
fetch("/api/yudates?cursor=eyJpZCI6MjB9")
```

## 備考
- 未認証でも閲覧可能ですが、`isLiked` / `isReyudated` は常に `false` になります
- `nextCursor` が `null` の場合は最終ページです

---

# [POST] /api/yudates

> 認証: 要 | レート制限: なし

## 説明
新しい投稿（ユデート）を作成します。画像の添付や既存投稿への返信が可能です。

## リクエスト
### リクエストBody
```json
{
  "content": "こんにちは！初投稿です",
  "imageUrl": "https://example.com/photo.jpg",
  "replyToId": null
}
```

| 名前 | 型 | 必須 | 説明 |
|---|---|---|---|
| content | string | はい | 投稿内容（最大文字数はサーバー設定に依存） |
| imageUrl | string | いいえ | 添付画像のURL |
| replyToId | number | いいえ | 返信先の投稿ID |

## レスポンス
### 成功 (201)
```json
{
  "id": 123,
  "content": "こんにちは！初投稿です",
  "imageUrl": "https://example.com/photo.jpg",
  "author": {
    "id": 42,
    "username": "taro",
    "displayName": "太郎",
    "avatarUrl": "https://example.com/avatar.jpg",
    "isVerified": false
  },
  "likeCount": 0,
  "reyudateCount": 0,
  "replyCount": 0,
  "isLiked": false,
  "isReyudated": false,
  "reactions": [],
  "quotedYudate": null,
  "replyToId": null,
  "superYudateAmount": 0,
  "createdAt": "2025-07-12T10:30:00Z"
}
```
### エラー (400)
```json
{
  "error": "contentは必須です"
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
fetch("/api/yudates", {
  method: "POST",
  credentials: "include",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({ content: "こんにちは！初投稿です" })
})
```

## 備考
- `replyToId` を指定すると、指定された投稿への返信として作成されます
- 返信先が存在しない場合は400エラーが返ります
