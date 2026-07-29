# [GET] /api/notifications/unread-count

> 認証: 要 | レート制限: なし

## 説明

未読通知の件数を取得します。フロントエンドでは30秒間隔でポーリングされ、通知バッジの表示に使用されます。

## リクエスト

### クエリパラメータ

なし

### リクエストBody

なし

## レスポンス

### 成功 (200)

```json
{
  "count": 5
}
```

### エラー (401)

```json
{
  "error": "認証が必要です"
}
```

## 実行例

```javascript
fetch("/api/notifications/unread-count", { credentials: "include" }).then(r => r.json())
```

## 備考

- フロントエンドは `refetchInterval: 30000` で定期的にポーリングしています
- 認証不要の場合は401が返ります
