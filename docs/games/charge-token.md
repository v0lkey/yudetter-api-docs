# [POST] /api/games/:id/charge-token

> 認証: 要 | レート制限: なし

## パスパラメータ

| 名前 | 型 | 必須 | 説明 |
|---|---|---|---|
| `id` | string | はい | 課金対象のゲームID |

## 説明

ゲーム内課金（`/api/games/:id/charge`）を実行するためのワンタイムトークンを発行します。金額と用途を指定し、発行されたトークンを使ってゲーム側から課金を確定するフローで使用します。

## リクエストBody

```json
{
  "amount": 100,
  "description": "ゲーム内アイテム購入"
}
```

| 名前 | 型 | 必須 | 説明 |
|---|---|---|---|
| `amount` | number | はい | 課金額（正の整数、YD） |
| `description` | string | はい | 課金用途説明 |

## レスポンス

### 成功 (200)

```json
{
  "token": "charge-token-xxx"
}
```

### エラー

```json
{
  "error": "残高不足"
}
```

## 実行例

```javascript
fetch("/api/games/abc123/charge-token", {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  credentials: "include",
  body: JSON.stringify({ amount: 100, description: "ゲーム内アイテム購入" })
})
```

## 備考

- `amount` は正の整数である必要があります
- ワンタイムトークンであり、消費後の再利用はできません