# [POST] /api/games/:id/auth-request

> 認証: 要 | レート制限: なし

## パスパラメータ

| 名前 | 型 | 必須 | 説明 |
|---|---|---|---|
| `id` | string | はい | ゲームID |

## 説明

ゲームを起動するための認証セッションをリクエストします。Studioで編集したゲームを作者が開始する際に使用します。返却されたトークンはゲーム側の起動認証で利用します。

## リクエストBody

なし

## レスポンス

### 成功 (200)

```json
{
  "token": "game-auth-token-xxx",
  "gameId": "abc123",
  "sessionId": "..."
}
```

### エラー

```json
{
  "error": "ゲームが見つかりません"
}
```

```json
{
  "error": "このゲームを起動する権限がありません"
}
```

## 実行例

```javascript
fetch("/api/games/abc123/auth-request", {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  credentials: "include"
})
```

## 備考

- 認証が必要です（クリエイター本人のセッション）
- 発行したトークンは `/api/games/auth/verify` で検証します