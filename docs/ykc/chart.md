# [GET] /api/ykc/chart

> 認証: 不要 | レート制限: なし

## 説明

Ykc（YKC）のOHLCVチャートデータを取得します。Mikancoin / Yudecoin のチャートと同一レスポンス形式です。

## リクエスト

### クエリパラメータ

| 名前 | 型 | 必須 | 説明 |
|---|---|---|---|
| `resolution` | `"1m" \| "1h" \| "1d"` | いいえ | 解像度（デフォルト: `1m`） |

## レスポンス

### 成功 (200)

```json
[
  {
    "timestampBucket": "2026-08-07T00:00:00.000Z",
    "open": 1.2,
    "high": 1.25,
    "low": 1.18,
    "close": 1.235,
    "volumeYd": 120000,
    "volumeYudecoin": 100000
  }
]
```

## 実行例

```bash
curl -s "https://yudetter.com/api/ykc/chart?resolution=1h"
```

```javascript
const candles = await fetch("/api/ykc/chart?resolution=1d").then(r => r.json())
```