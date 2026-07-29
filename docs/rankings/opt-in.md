# [POST] /api/rankings/opt-in

> 認証: 要 | レート制限: なし

## 説明

ランキングに参加します。参加後はランキング集計の対象となり、`/api/rankings` の結果に自身のスコアが表示されるようになります。

## リクエスト

### パスパラメータ

なし

### リクエストBody

なし

## レスポンス

### 成功 (200)

```json
{
  "message": "ランキングに参加しました"
}
```

### エラー

```json
{
  "error": "既にランキングに参加しています"
}
```

```json
{
  "error": "認証が必要です"
}
```

## 実行例

```javascript
fetch("/api/rankings/opt-in", {
  method: "POST",
  credentials: "include"
})
```

## 備考

- 参加後は週次・全期間の両方のランキングで集計対象となります
- 既に参加している場合はエラーが返ります
- ランキングから離脱するには `/api/rankings/opt-out` を使用します
