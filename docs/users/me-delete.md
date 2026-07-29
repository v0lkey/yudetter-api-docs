# [DELETE] /api/users/me

> 認証: 要 | レート制限: なし

## 説明

現在認証中のユーザーアカウントを完全に削除します。この操作は**復元できません**。ユーザーデータ、投稿、フォロー関係など全ての関連データが削除されます。

## リクエスト

### パスパラメータ
なし

### クエリパラメータ
なし

### リクエストBody
なし

## レスポンス

### 成功 (200)
```json
{
  "message": "deleted"
}
```

### エラー (401)
```json
{
  "error": "認証が必要です"
}
```

## 実行例

```bash
curl -X DELETE https://yudetter.com/api/users/me -b "cookie"
```

```javascript
fetch("/api/users/me", { method: "DELETE", credentials: "include" })
```

## 備考

- **この操作は元に戻せません。** 削除後にアカウントを復元する手段はありません。
- 削除が完了すると Cookie セッションは無効になります。
- 退会フローを実装する場合は、確認ダイアログを必ず表示してください。
