# [POST] /api/games/:id/charge

> 認証: 要 | レート制限: なし

## 説明

ゲーム内課金処理を行います。`playPrice`（プレイ料金）とは別の、ゲーム内アイテム購入やスコアアップなどの課金システムです。

## リクエスト

### パスパラメータ

| 名前 | 型 | 必須 | 説明 |
|------|-----|------|------|
| `id` | `string` | はい | 課金対象のゲームID |

### リクエストBody

```json
{
  "amount": 100,
  "description": "ゲーム内アイテム購入"
}
```

## レスポンス

### 成功 (200)

```json
{
  "message": "課金が完了しました",
  "charged": 100,
  "balance": 4900
}
```

### エラー

```json
{
  "error": "残高不足"
}
```

```json
{
  "error": "ゲームが見つかりません"
}
```

```json
{
  "error": "課金額は正の数である必要があります"
}
```

## 実行例

```javascript
fetch("/api/games/abc123/charge", {
  method: "POST",
  credentials: "include",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({ amount: 100, description: "ゲーム内アイテム購入" })
})
```

## 備考

- `playPrice`（ゲームプレイ時の課金）とは異なる独立した課金システムです
- `amount` は正の整数である必要があります
- Yudedollar残高が不足している場合は `400 Bad Request` が返ります
- `balance` は課金後の残高です
