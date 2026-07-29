# [GET] /api/yudecoin/market

> 認証: 不要 | レート制限: なし

## 説明

Yudecoin（YDC）の現在価格と流動性プールの状態を取得します。

## リクエスト

### クエリパラメータ

なし

### リクエストBody

なし

## レスポンス

### 成功 (200)

```json
{
  "price": 0.12712976662241507,
  "pool": {
    "id": 1,
    "ydReserve": "33825551",
    "yudecoinReserve": "266071054",
    "kConstant": "9000000000000000",
    "updatedAt": "2026-07-24T14:39:16.867Z"
  }
}
```

| フィールド | 型 | 説明 |
|---|---|---|
| `price` | number | `ydReserve / yudecoinReserve` = 1YDCあたりのYD価格 |
| `pool.ydReserve` | string | プール内YD残高（整数文字列、BigInt互換） |
| `pool.yudecoinReserve` | string | プール内YDC残高 |
| `pool.kConstant` | string | AMM定数積 x * y = k |

## 実行例

```bash
curl -s https://yudetter.com/api/yudecoin/market
```

```javascript
const data = await fetch("/api/yudecoin/market").then(r => r.json())
```

## 備考

- `price` はフル精度（小数点6桁以上）で返されます
- フロントエンドでは `.toFixed(2)` で丸められているため表示と実値に乖離があります
- `kConstant` は理論上の `ydReserve * yudecoinReserve` と一致しないことがあります（サーバー側の丸め）
