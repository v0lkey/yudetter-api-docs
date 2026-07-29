# [GET] /api/yudecoin/wallet

> 認証: 要 | レート制限: なし

## 説明

ログインユーザーのYudecoin（YDC）ウォレット残高を取得します。

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
    "userId": 93,
    "balance": "0",
    "unlockedBalance": "0",
    "updatedAt": "2026-07-24T15:17:09.565Z"
  },
  "tradedAmountToday": "0"
}
```

| フィールド | 型 | 説明 |
|---|---|---|
| `wallet.balance` | string | 総YDC残高 |
| `wallet.unlockedBalance` | string | 取引可能YDC残高（ロック中のYDCは差し引かれる） |
| `tradedAmountToday` | string | 本日の取引累計量（日次上限の監視に使用） |

### エラー (401)

```json
{
  "error": "認証が必要です"
}
```

## 実行例

```bash
curl -s https://yudetter.com/api/yudecoin/wallet -b "cookie"
```

```javascript
const wallet = await fetch("/api/yudecoin/wallet", { credentials: "include" }).then(r => r.json())
```

## 備考

- 認証必須（`__Secure-better-auth.session_token` Cookie）
- Mikancoinのwalletとは異なり `tradedAmountToday` フィールドが存在します
- 取引実行後、フロントエンドはこのエンドポイントを自動リフェッチして残高を更新します
