# [GET] /api/ykc/wallet

> 認証: 要 | レート制限: なし

## 説明

Ykc（YKC）の保有残高と当日の取引実績・制限値を取得します。

## リクエスト

### リクエストBody

なし

## レスポンス

### 成功 (200)

```json
{
  "wallet": {
    "balance": 1234.5,
    "unlockedBalance": 1200
  },
  "purchasedYdToday": 50000,
  "soldYdToday": 30000,
  "dailyBuyLimitYd": 1000000,
  "dailySellLimitYd": 1000000,
  "dailyLimitsExempt": false
}
```

| フィールド | 型 | 説明 |
|---|---|---|
| `wallet.balance` | number | YKC保有残高 |
| `wallet.unlockedBalance` | number | アンロック済み残高 |
| `purchasedYdToday` | number | 今日の購入に使ったYD量 |
| `soldYdToday` | number | 今日の売却YD量（`tradedAmountToday` という名前で返る場合あり） |
| `dailyBuyLimitYd` | number | 日次購入上限YD（デフォルト 1,000,000） |
| `dailySellLimitYd` | number | 日次売却上限YD（デフォルト 1,000,000） |
| `dailyLimitsExempt` | boolean | 制限免除フラグ（管理者等） |

## 実行例

```javascript
fetch("/api/ykc/wallet", { credentials: "include" })
```

## 備考

- 取引ページでは本エンドポイントと `/api/ykc/market` を5秒ごとにリフェッチします