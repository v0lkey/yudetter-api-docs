# [POST] /api/users/setup

> 認証: 要 | レート制限: なし

## 説明

アカウント作成直後のオンボーディング（ユーザー名・表示名・生年月日の設定）を完了します。設定済みの場合はこの画面は表示されません。

## リクエスト

### リクエストBody

```json
{
  "username": "taro_yudetter",
  "displayName": "太郎",
  "birthday": "2010-01-01"
}
```

| 名前 | 型 | 必須 | 説明 |
|---|---|---|---|
| username | string | はい | 半角英数字とアンダースコア（`/^[a-zA-Z0-9_]+$/`）のみ。事前に `/api/users/setup/check-username` で利用可否を確認する |
| displayName | string | はい | 表示名 |
| birthday | string | はい | 生年月日（`YYYY-MM-DD` 形式）。13歳以上である必要がある（生年月日から13年前が上限） |

## レスポンス

### 成功 (200)

```json
{
  "success": true
}
```

### エラー

```json
{
  "error": "このIDは既に使われています"
}
```

## 実行例

```javascript
fetch("/api/users/setup", {
  method: "POST",
  credentials: "include",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({ username: "taro_yudetter", displayName: "太郎", birthday: "2010-01-01" })
})
```

## 備考

- ユーザー名の重複チェックは `/api/users/setup/check-username` で行います
- 成功後はホーム画面へリダイレクトされ、リロードされます
