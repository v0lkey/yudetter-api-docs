# [GET] /api/mikancoin/wallet

> 認証: 要 | レート制限: なし

## 説明

ログインユーザーのMikancoin（MKC）ウォレット残高を取得します。

## リクエスト

### クエリパラメータ

なし

### リクエストBody

なし

## レスポンス

### 成功 (200)

```json
{
  "wallet": {
    "userId": 88,
    "balance": "2040978",
    "unlockedBalance": "2040978",
    "updatedAt": "2026-07-27T12:14:19.306Z"
  }
}
```

| フィールド | 型 | 説明 |
|---|---|---|
| `wallet.balance` | string | 総MKC残高 |
| `wallet.unlockedBalance` | string | 取引可能MKC残高 |

### YDCとの差

- `tradedAmountToday` フィールドが存在しません（MKCに日次取引量制限なし）

### エラー (401)

```json
{
  "error": "認証が必要です"
}
```

## 実行例

```bash
curl -s https://yudetter.com/api/mikancoin/wallet -b "cookie"
```

```javascript
const wallet = await fetch("/api/mikancoin/wallet", { credentials: "include" }).then(r => r.json())
```
