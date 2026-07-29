# [POST] /api/users/:username/ban

> 認証: 要 | レート制限: なし

## 説明

ユーザーをBANします。管理者権限が必要です。

## リクエスト

### パスパラメータ

| 名前 | 型 | 必須 | 説明 |
|------|-----|------|------|
| `username` | string | はい | BAN対象のユーザー名 |

### リクエストBody

```json
{
  "reason": "規約違反のため"
}
```

## レスポンス

### 成功 (200)

```json
{
  "success": true
}
```

### エラー (403)

```json
{
  "error": "権限がありません"
}
```

## 実行例

```javascript
fetch("/api/users/spammer/ban", {
  method: "POST",
  credentials: "include",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({ reason: "スパム行為のため" })
})
```
