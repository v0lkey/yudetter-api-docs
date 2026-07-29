# [GET] /api/mikancoin/chart

> 認証: 不要 | レート制限: なし

## 説明

Mikancoin（MKC）のOHLCVチャートデータを取得します。YDCのチャートと同一レスポンス形式です。

## リクエスト

### クエリパラメータ

| 名前 | 型 | 必須 | 説明 |
|---|---|---|---|
| `resolution` | `"1m" \| "1h" \| "1d"` | いいえ | 解像度（デフォルト: `1m`） |

解像度別件数:
- `1m`: 15 candles
- `1h`: 24 candles
- `1d`: 30 candles

## レスポンス

### 成功 (200)

```json
[
  {
    "timestampBucket": "2026-07-24T11:15:00.000Z",
    "open": 0.107311435,
    "high": 0.107311435,
    "low": 0.107311435,
    "close": 0.107311435,
    "volumeYd": 0,
    "volumeYudecoin": 0
  }
]
```

フィールド名はYDCと同じ `volumeYudecoin` です（`volumeMkc` ではありません）。

## 実行例

```bash
curl -s "https://yudetter.com/api/mikancoin/chart?resolution=1d"
```

```javascript
const candles = await fetch("/api/mikancoin/chart?resolution=1h").then(r => r.json())
```
