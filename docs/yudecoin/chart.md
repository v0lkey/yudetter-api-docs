# [GET] /api/yudecoin/chart

> 認証: 不要 | レート制限: なし

## 説明

Yudecoin（YDC）のOHLCVチャートデータを取得します。

## リクエスト

### クエリパラメータ

| 名前 | 型 | 必須 | 説明 |
|---|---|---|---|
| `resolution` | `"1m" \| "1h" \| "1d"` | いいえ | 解像度（デフォルト: `1m`） |

解像度別件数:
- `1m`: 15 candles（最近15分）
- `1h`: 24 candles（最近24時間）
- `1d`: 30 candles（最近30日）

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

| フィールド | 型 | 説明 |
|---|---|---|
| `timestampBucket` | string | ローソク足開始時刻（ISO 8601） |
| `open` / `high` / `low` / `close` | number | 始値 / 高値 / 安値 / 終値 |
| `volumeYd` | number | YD建て出来高 |
| `volumeYudecoin` | number | YDC建て出来高 |

## 実行例

```bash
curl -s "https://yudetter.com/api/yudecoin/chart?resolution=1h"
```

```javascript
const candles = await fetch("/api/yudecoin/chart?resolution=1d").then(r => r.json())
```

## 備考

- APIはフル精度を返しますが、フロントエンドで `.toFixed(2)` により丸められます
- Mikancoinのチャートも同一レスポンス形式です（`volumeYudecoin` フィールド名は共通）
