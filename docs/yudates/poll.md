# [PATCH] /api/yudates/:id/poll ／ [POST] /api/yudates/:id/poll/vote

> 認証: 要 | レート制限: なし

## 説明

投稿に付属するアンケート（Poll）の設定変更と投票を行います。アンケート自体はユデート投稿時（`POST /api/yudates`）に `poll: { options, voterVisibility }` を含めることで作成されます。

---

# [PATCH] /api/yudates/:id/poll

投票者の表示設定（匿名化）を変更します。

## パスパラメータ

| 名前 | 型 | 必須 | 説明 |
|---|---|---|---|
| `id` | number | はい | アンケートを持つユデートID |

## リクエストBody

```json
{
  "voterVisibility": "anonymous"
}
```

| 名前 | 型 | 必須 | 説明 |
|---|---|---|---|
| `voterVisibility` | `"visible" \| "anonymous"` | はい | 投票者を公開するか匿名にするか |

## レスポンス

### 成功 (200)

変更後のアンケート付きユデートが返ります。

```json
{
  "id": 10,
  "content": "どっちが好き？",
  "poll": {
    "options": [],
    "totalVotes": 20,
    "voterVisibility": "anonymous",
    "selectedOptionId": null
  }
}
```

### エラー (403)

```json
{
  "error": "アンケートの設定を変更できません"
}
```

## 実行例

```javascript
fetch(`/api/yudates/${yudateId}/poll`, {
  method: "PATCH",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({ voterVisibility: "anonymous" })
})
```

---

# [POST] /api/yudates/:id/poll/vote

アンケートに投票します。

## パスパラメータ

| 名前 | 型 | 必須 | 説明 |
|---|---|---|---|
| `id` | number | はい | 投票するユデートID |

## リクエストBody

```json
{
  "optionId": 3
}
```

| 名前 | 型 | 必須 | 説明 |
|---|---|---|---|
| `optionId` | number | はい | 投票先の選択肢ID |

## レスポンス

### 成功 (200)

投票後のアンケート状態（各選択肢の `voteCount` 等）が返ります。

```json
{
  "id": 1,
  "totalVotes": 21,
  "voterVisibility": "visible",
  "selectedOptionId": 3,
  "options": [
    { "id": 1, "text": "選択肢A", "voteCount": 10 },
    { "id": 2, "text": "選択肢B", "voteCount": 5 },
    { "id": 3, "text": "選択肢C", "voteCount": 6 }
  ]
}
```

### エラー (400)

```json
{
  "error": "投票に失敗しました"
}
```

## 実行例

```javascript
fetch(`/api/yudates/${yudateId}/poll/vote`, {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({ optionId: 3 })
})
```

## 共通の備考

- アンケートの表示用フィールド: `options[]`（`id` / `text` / `vote_count` / `voters`）、`total_votes`、`voter_visibility`、`selected_option_id`
- `voterVisibility: "visible"` の場合のみ各選択肢に `voters` 配列が含まれます
- 有効な投票は1人1つ（投票済みなら `selected_option_id` が入る）
- 3つを超える選択肢は省略表示され、開くことで全選択肢が表示されます