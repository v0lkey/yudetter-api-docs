# [GET] /api/ykc/market

> 認証: 不要 | レート制限: なし

## 説明

Ykc（YKC）の現在価格を取得します。Mikancoin（MKC）と同一の取引所UI・価格決定ロジックを共有する通貨です。

## リクエスト

### クエリパラメータ

なし

### リクエストBody

なし

## レスポンス

### 成功 (200)

```json
{
  "symbol": "YKC",
  "name": "Ykc",
  "priceYd": 1.235
}
```

| フィールド | 型 | 説明 |
|---|---|---|
| `symbol` | string | 銘柄シンボル |
| `name` | string | 銘柄名 |
| `priceYd` | number | 現在価格（YD建て） |

## 実行例

```bash
curl -s https://yudetter.com/api/ykc/market
```

```javascript
const data = await fetch("/api/ykc/market").then(r => r.json())
```

## 備考

- フロントエンドは5秒ごとに本エンドポイントと `/api/ykc/wallet` をリフェッチします
- 取引ページの表示通貨はYkc / Mikancoinの2種類で、`Ct` 変数（`"ykc" | "mikancoin"`）により `/api/${Ct}/token` 等の共通APIが呼び分けられます