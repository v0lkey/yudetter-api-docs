# [GET] /api/explore/search

> 認証: 不要 | レート制限: なし

## 説明
投稿とユーザーを横断して検索します。クエリパラメータ `q` は必須です。

## リクエスト
### クエリパラメータ
| 名前 | 型 | 必須 | 説明 |
|---|---|---|---|
| q | string | はい | 検索クエリ（URLエンコード必須） |

## レスポンス
### 成功 (200)
```json
{
  "yudates": [
    {
      "id": 10,
      "content": "Yudetterの新機能について",
      "imageUrl": null,
      "author": {
        "id": 1,
        "username": "yudetter",
        "displayName": "Yudetter公式",
        "avatarUrl": "https://example.com/avatar.jpg",
        "isVerified": true
      },
      "likeCount": 20,
      "reyudateCount": 5,
      "replyCount": 3,
      "isLiked": false,
      "isReyudated": false,
      "reactions": [],
      "quotedYudate": null,
      "replyToId": null,
      "superYudateAmount": 0,
      "isSpoiler": false,
      "createdAt": "2025-07-10T09:00:00Z"
    }
  ],
  "users": [
    {
      "id": 42,
      "username": "taro",
      "displayName": "太郎",
      "avatarUrl": "https://example.com/avatar.jpg",
      "bio": "Yudetter大好き",
      "followerCount": 120,
      "followingCount": 80,
      "yudateCount": 45,
      "isVerified": false,
      "isPrivate": false
    }
  ]
}
```
### エラー (400)
```json
{
  "error": "検索クエリ(q)は必須です"
}
```

## 実行例
```javascript
fetch("/api/explore/search?q=Yudetter")
```

## 備考
- `q` はURLエンコードが必要です（例: `?q=%E6%A4%9C%E7%B4%A2`）
- 空文字列の場合は400エラーが返ります
- 部分一致検索です
- ユーザー検索は username / displayName の両方が対象です
