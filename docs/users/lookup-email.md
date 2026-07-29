# [POST] /api/users/lookup-email

> 認証: 要 | レート制限: なし

## 説明

ユーザー名からメールアドレスを取得する内部APIです。サインインフォームにおいて「ユーザー名ログイン」を可能にするために使用されます。(脆弱性により、エンドポイントが閉じられました)

## リクエスト

### パスパラメータ
なし

### クエリパラメータ
なし

### リクエストBody
```json
{
  "username": "taro_yudetter"
}
```

## レスポンス

### 成功 (200)
```json
{
  "email": "taro@example.com"
}
```

### エラー (404)
```json
{
  "error": "ユーザーIDが見つかりません"
}
```

### エラー (401)
```json
{
  "error": "認証が必要です"
}
```

## 実行例

```bash
curl -X POST https://yudetter.com/api/users/lookup-email \
  -H "Content-Type: application/json" \
  -d '{"username":"taro_yudetter"}' \
  -b "cookie"
```

```javascript
fetch("/api/users/lookup-email", {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({ username: "taro_yudetter" }),
  credentials: "include"
})
```

## 備考

- ⚠️ **レート制限は実装されていません。** ユーザー名からメールアドレスを無制限に取得可能です。
- このAPIはサインインフォームで「ユーザー名ログイン」を可能にするための内部APIです。
- 認証必須ですが、認証済みユーザーであれば誰でも任意のユーザー名で検索できます。
- 存在しないユーザー名の場合は 404 エラーが返ります。
