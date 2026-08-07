# [GET] /api/ykc/quote

> 認証: 不要 | レート制限: なし

## 説明

Ykc（YKC）の売買見積り（クオート）を取得します。取引実行前の価格確認と、価格バージョン（`priceVersion`）の取得に使用します。

## リクエスト

### クエリパラメータ

| 名前 | 型 | 必須 | 説明 |
|---|---|---|---|
| `type` | `"buy" \| "sell"` | はい | 売買方向 |
| `amount` | number | はい | 取引量（buy=投入YD量, sell=売却YKC量） |

## レスポンス

### 成功 (200)

```json
{
  "canBuy": true,
  "canSell": false,
  "priceVersion": 42,
  "price": 1.235
}
```

| フィールド | 型 | 説明 |
|---|---|---|
| `canBuy` | boolean | 買いが現在の価格・残高・上限内で可能か |
| `canSell` | boolean | 売りが可能か |
| `priceVersion` | number | 取引実行時に `priceVersion` として返送する価格バージョン |
| `message` | string | 不可の場合の理由（エラー時） |

### エラー (400)

```json
{
  "message": "現在の価格・残高・上限では取引できません"
}
```

## 実行例

```javascript
const quote = await fetch(`/api/ykc/quote?type=buy&amount=1000`).then(r => r.json())
if (quote.canBuy) {
  // /api/ykc/exchange を priceVersion 付きで実行
}
```

## 備考

- クオートは取引前の必須フローです（実行時価格との乖離を `priceVersion` で検証）
- 取引不可（上限超過・残高不足等）の場合は `canBuy` / `canSell` が `false` になり、`message` に理由が入ります