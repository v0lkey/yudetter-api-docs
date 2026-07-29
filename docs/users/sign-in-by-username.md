# [POST] /api/users/sign-in-by-username

> 認証: 不要 | レート制限: なし

## 説明

ユーザー名とパスワードでログインするエンドポイントです。
内部で username → email の逆引き (`SELECT email FROM users WHERE username = $1`) を行った後、その email を使って BetterAuth のパスワード認証を実行します。

## リクエスト

### パスパラメータ
なし

### クエリパラメータ
なし

### リクエストBody
```json
{
  "username": "test",
  "password": "testtest"
}
```

## レスポンス

### 成功 (200)
```json
{
  "redirect": false,
  "token": "VyDZavcrzbXoR1Bq62S3qdKLE0wJhloo",
  "user": {
    "name": "test",
    "email": "test@example.com",
    "emailVerified": false,
    "image": null,
    "createdAt": "2026-07-14T02:33:17.581Z",
    "updatedAt": "2026-07-16T12:25:45.481Z",
    "username": "test",
    "displayName": "test"
  }
}
```

Set-Cookie も同時に発行されます:
```
__Secure-better-auth.session_token=<token>; Max-Age=604800; Path=/; HttpOnly; Secure; SameSite=Lax
```

### エラー (400) — ユーザー名またはパスワード不一致
```json
{
  "error": "ユーザーIDまたはパスワードが間違っています"
}
```

### エラー (401) — email解決後、BetterAuth認証失敗
```json
{
  "message": "Invalid email or password",
  "code": "INVALID_EMAIL_OR_PASSWORD"
}
```

### エラー (400) — 空文字
```json
{
  "error": "ユーザーIDとパスワードが必要です"
}
```

## 実行例

```bash
curl -X POST https://yudetter.com/api/users/sign-in-by-username \
  -H "Content-Type: application/json" \
  -d '{"username":"test","password":"testtest"}'
```

```javascript
const login = async (username, password) => {
  const res = await fetch("/api/users/sign-in-by-username", {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({ username, password }),
    credentials: "include"
  });
  const data = await res.json();
  return data.token;
};
```

## 内部処理フロー

```
1. username を受け取る
2. SELECT email FROM users WHERE username = $1 LIMIT 1 を実行
   → 存在しないユーザー名の場合は 400 を返す
3. 取得した email + password で BetterAuth signIn を呼び出す
4. 成功時 → Set-Cookie 発行 + token/user を JSON で返却
5. 失敗時 → BetterAuth のエラーをそのまま返却
```

## 備考

- ⚠️ **認証不要**。誰でもログイン試行が可能です。
- ⚠️ **レート制限は実装されていません。** ブルートフォース攻撃に悪用される可能性があります。
- ⚠️ username に NULLバイト (`\u0000`) を送ると内部SQLがエラーレスポンスに漏洩します（脆弱性）。
- ⚠️ username に数値・配列・オブジェクト・真偽値を送ると JS ランタイムエラー (`username.trim is not a function`) がそのままレスポンスになります。
- 存在しないユーザー名の場合と、存在するがパスワード不一致の場合で返るエラーが異なります（前者: 日本語400 / 後者: BetterAuth標準401）。これにより**ユーザー名の存在確認が可能**です。
- 成功時に返る token は `GET /api/auth/list-sessions` で確認できるセッショントークンと同一です。
- Cookie の有効期限は 7日間、24時間ごとにスライディング更新されます。
