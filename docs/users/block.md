# [POST/DELETE] /api/users/:username/block

> 認証: 要 | レート制限: なし

## 説明

POST で指定ユーザーをブロック、DELETE でブロックを解除します。

ブロックすると、相手の投稿が自分のタイムラインから表示されなくなります。また、相手からも自分の投稿が見えなくなります。

## リクエスト

### パスパラメータ
| 名前 | 型 | 必須 | 説明 |
|---|---|---|---|
| username | string | はい | ブロック/解除対象のユーザー名 |

### クエリパラメータ
なし

### リクエストBody
なし

## レスポンス

### 成功: ブロック (200)
```json
{
  "message": "blocked"
}
```

### 成功: ブロック解除 (200)
```json
{
  "message": "unblocked"
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
  "error": "ユーザーが見つかりません"
}
```

## 実行例

```bash
curl -X POST https://yudetter.com/api/users/spam_user/block -b "cookie"
curl -X DELETE https://yudetter.com/api/users/spam_user/block -b "cookie"
```

```javascript
// ブロック
fetch("/api/users/spam_user/block", { method: "POST", credentials: "include" })

// ブロック解除
fetch("/api/users/spam_user/block", { method: "DELETE", credentials: "include" })
```

## 備考

- ブロック成功時に、該当ユーザーの投稿がタイムラインから自動的に除外されます。
- ブロックされると、お互いのプロフィールが表示できなくなります。
- ブロック中はフォロー関係も自動的に解除されます。
- 自分自身をブロックすることはできません。
