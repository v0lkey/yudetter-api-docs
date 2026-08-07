# [GET] /api/games/:id/reward-requests ／ [POST] /api/games/:id/reward-requests ／ [POST] /api/games/:id/reward-requests/:requestId/:action

> 認証: 要 | レート制限: なし

## 説明

ゲームの報酬（リワード）申請の作成・一覧取得と、承認・却下を行います。クリエイターがゲームから送られた報酬支払いリクエストを管理するために使用します。

---

# [POST] /api/games/:id/reward-requests

報酬申請を作成します。ゲーム内で報酬の受け取り（出金）が発生した際に、金額と説明を添えて申請します。申請はゲーム作成者側の一覧に `pending` として登録され、承認されると作成者のYD残高から支払われます。

## パスパラメータ

| 名前 | 型 | 必須 | 説明 |
|---|---|---|---|
| `id` | string | はい | ゲームID |

## リクエストBody

| 名前 | 型 | 必須 | 説明 |
|---|---|---|---|
| `amount` | number | はい | 報酬額（YD） |
| `description` | string | いいえ | 報酬の説明（最大200文字） |

```json
{
  "amount": 500,
  "description": "スコア報酬"
}
```

## レスポンス

### 成功 (200)

```json
{
  "requestId": 123,
  "status": "pending"
}
```

### エラー (400)

```json
{
  "error": "報酬申請を作成できません"
}
```

## 実行例

```javascript
fetch("/api/games/abc123/reward-requests", {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  credentials: "include",
  body: JSON.stringify({ amount: 500, description: "スコア報酬" })
})
```

---

# [GET] /api/games/:id/reward-requests

報酬申請の一覧を取得します。

## パスパラメータ

| 名前 | 型 | 必須 | 説明 |
|---|---|---|---|
| `id` | string | はい | ゲームID |

## レスポンス

### 成功 (200)

```json
{
  "requests": [
    {
      "id": 123,
      "gameId": "abc123",
      "userId": 42,
      "username": "taro",
      "amount": 500,
      "description": "スコア報酬",
      "status": "pending",
      "createdAt": "2026-08-07T00:00:00Z"
    }
  ]
}
```

| フィールド | 型 | 説明 |
|---|---|---|
| `requests[].id` | number | 申請ID |
| `requests[].status` | `"pending" \| "approved" \| "rejected"` | 申請状態 |
| `requests[].amount` | number | 報酬額（YD） |

## 実行例

```javascript
fetch("/api/games/abc123/reward-requests", { credentials: "include" })
```

---

# [POST] /api/games/:id/reward-requests/:requestId/:action

報酬申請を承認または却下します。

## パスパラメータ

| 名前 | 型 | 必須 | 説明 |
|---|---|---|---|
| `id` | string | はい | ゲームID |
| `requestId` | number | はい | 申請ID |
| `action` | `"approve" \| "reject"` | はい | 承認または却下 |

## リクエストBody

なし

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
  "error": "報酬申請を承認できません"
}
```

## 実行例

```javascript
fetch("/api/games/abc123/reward-requests/123/approve", {
  method: "POST",
  credentials: "include"
})
```

## 備考

- `action=approve` で承認、`action=reject` で却下
- 承認時はリクエスト一覧から当該申請が除去されます