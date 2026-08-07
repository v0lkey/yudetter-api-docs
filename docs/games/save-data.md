# [GET] /api/games/:id/save-data?key=

> 認証: 要 | レート制限: なし

## 説明

ゲームセーブデータをキー単位で読み書きします。ゲーム側の進行状況をサーバーに保存するために使用します。

---

# [GET] /api/games/:id/save-data?key=...

## クエリパラメータ

| 名前 | 型 | 必須 | 説明 |
|---|---|---|---|
| `key` | string | はい | セーブデータのキー（URLエンコード必須） |

## レスポンス

### 成功 (200)

```json
{
  "key": "progress",
  "value": "{\"level\":3,\"score\":1200}"
}
```

## 実行例

```javascript
fetch(`/api/games/abc123/save-data?key=${encodeURIComponent("progress")}`, { credentials: "include" })
```

---

# [POST] /api/games/:id/save-data

セーブデータを保存します。

## リクエストBody

```json
{
  "key": "progress",
  "value": "{\"level\":3,\"score\":1200}"
}
```

| 名前 | 型 | 必須 | 説明 |
|---|---|---|---|
| `key` | string | はい | セーブデータのキー |
| `value` | string | はい | セーブデータ内容（文字列。JSONを文字列化して保存） |

## レスポンス

### 成功 (200)

```json
{
  "success": true
}
```

### エラー (400)

```json
{
  "error": "セーブに失敗しました"
}
```

## 実行例

```javascript
fetch("/api/games/abc123/save-data", {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  credentials: "include",
  body: JSON.stringify({ key: "progress", value: JSON.stringify({ level: 1, score: 1200 }) })
})
```

## 備考

- `value` は文字列として保存されます（オブジェクトは `JSON.stringify` して渡す）
- キーはゲームごとに独立して保持されます