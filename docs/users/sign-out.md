# [POST] /api/users/sign-out

> 認証: 要 | レート制限: なし

## 説明

現在のBearerセッションを破棄してログアウトします。フロントエンドは失敗時のフォールバックとして BetterAuth 標準の `signOut` を実行します。

## リクエスト

### リクエストBody

なし

## レスポンス

### 成功 (200)

```json
{
  "success": true
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
fetch("/api/users/sign-out", {
  method: "POST",
  credentials: "include",
  headers: { "Accept": "application/json" }
})
```

## 備考

- クライアントはこのエンドポイントが `!ok` を返した場合、BetterAuth 標準の `signOut()` でセッション破棄を行います
- ログアウト後は `GET /api/auth/get-session` が `null` を返します