# [GET] /api/vip/casino-access

> 認証: 要 | レート制限: なし

## 説明

VIP会員のカジノアクセス権限を確認します。

## レスポンス

### 成功 (200)

```json
{
  "hasAccess": true,
  "vipLevel": "gold",
  "expiresAt": "2026-12-31T23:59:59Z"
}
```

### エラー (403)

```json
{
  "error": "VIP会員のみアクセス可能です"
}
```

## 実行例

```javascript
fetch("/api/vip/casino-access", {
  credentials: "include"
})
```
