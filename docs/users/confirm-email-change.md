# [POST] /api/users/confirm-email-change

> 認証: 要 | レート制限: なし

## 説明

メールアドレス変更を確定します。`/api/users/request-email-change` で発行された確認用URLに含まれるトークンとユーザーIDを送信します。

## リクエスト

### リクエストBody

```json
{
  "token": "confirm-token",
  "uid": 42
}
```

| 名前 | 型 | 必須 | 説明 |
|---|---|---|---|
| token | string | はい | 確認メールに含まれるトークン |
| uid | number | はい | ユーザーID |

## レスポンス

### 成功 (200)

```json
{
  "success": true
}
```

### エラー

```json
{
  "error": "トークンが無効か期限切れです"
}
```

## 実行例

```javascript
fetch("/api/users/confirm-email-change", {
  method: "POST",
  credentials: "include",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({ token: "confirm-token", uid: 42 })
})
```

## 備考

- 確認メールのリンク（`/confirm-email-change?token=...&uid=...`）から呼び出されます
- トークンとユーザーIDの両方が一致する必要があります
