# [GET] /api/market/installments/eligibility

> 認証: 要 | レート制限: なし

## 説明

分割払いの資格を確認します。

## リクエスト

### クエリパラメータ

| 名前 | 型 | 必須 | 説明 |
|---|---|---|---|
| `itemId` | string | はい | 購入予定の商品ID |

## レスポンス

### 成功 (200)

```json
{
  "eligible": true,
  "maxInstallments": 6
}
```

| フィールド | 型 | 説明 |
|---|---|---|
| `eligible` | boolean | 分割払いが利用可能か |
| `maxInstallments` | number | 最大分割回数 |

### エラー (401)

```json
{
  "error": "認証が必要です"
}
```

## 実行例

```javascript
fetch("/api/market/installments/eligibility?itemId=abc123", { credentials: "include" }).then(r => r.json())
```
