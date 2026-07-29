# [POST] /api/users/:username/follow/approve

> 認証: 要 | レート制限: なし

## 説明

自分へのフォローリクエストを承認します。非公開アカウントの場合にのみ有効です。

## リクエスト

### パスパラメータ

| 名前 | 型 | 必須 | 説明 |
|---|---|---|---|
| username | string | はい | 承認するユーザー名 |

### リクエストBody

なし

## レスポンス

### 成功 (200)

```json
{
  "message": "フォローリクエストを承認しました"
}
```

### エラー (400)

```json
{
  "error": "フォローリクエストが見つかりません"
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
fetch("/api/users/other_user/follow/approve", { method: "POST", credentials: "include" })
```
