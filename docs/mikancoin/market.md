# [GET] /api/mikancoin/market

> 認証: 不要 | レート制限: なし

## 説明

Mikancoin（MKC）の現在価格と流動性プールの状態を取得します。

## リクエスト

### クエリパラメータ

なし

### リクエストBody

なし

## レスポンス

### 成功 (200)

```json
{
  "symbol": "MKC",
  "name": "Mikancoin",
  "initialPriceYd": 3,
  "priceYd": 4.983355646552585,
  "pool": {
    "ydReserve": "248257099",
    "mkcReserve": "49817255",
    "kConstant": "12367487100000000"
  }
}
```

| フィールド | 型 | 説明 |
|---|---|---|
| `symbol` | string | 銘柄シンボル（YDCにはない） |
| `name` | string | 銘柄名 |
| `initialPriceYd` | number | 初期価格（MKCのみ） |
| `priceYd` | number | 現在価格（YDCの `price` に相当） |
| `pool.ydReserve` | string | プール内YD残高 |
| `pool.mkcReserve` | string | プール内MKC残高（YDCは `yudecoinReserve`） |
| `pool.kConstant` | string | AMM定数積 |

## 実行例

```bash
curl -s https://yudetter.com/api/mikancoin/market
```

```javascript
const data = await fetch("/api/mikancoin/market").then(r => r.json())
```

## 備考

- YDCの `/api/yudecoin/market` と比べて `symbol`, `name`, `initialPriceYd` フィールドが追加されています
- 価格フィールド名は `priceYd`（YDCは `price`）
