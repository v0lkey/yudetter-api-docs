# [POST] /api/users/:username/follow

> 認証: 要 | レート制限: なし

## 説明

指定したユーザーをフォローします。対象ユーザーが非公開アカウントの場合はフォローリクエスト（承認待ち）状態になります。

## リクエスト

### パスパラメータ
| 名前 | 型 | 必須 | 説明 |
|---|---|---|---|
| username | string | はい | フォロー対象のユーザー名 |

### クエリパラメータ
なし

### リクエストBody
なし

## レスポンス

### 成功 (200)
```json
{
  "message": "followed"
}
```

### エラー (400)
```json
{
  "error": "既にフォローしています"
}
```

### エラー (401)
```json
{
  "error": "認証が必要です"
}
```

### エラー (404)
```json
{
  "error": "ユーザーが見つかりません"
}
```

## 実行例

```bash
curl -X POST https://yudetter.com/api/users/other_user/follow -b "cookie"
```

```javascript
fetch("/api/users/other_user/follow", { method: "POST", credentials: "include" })
```

## 備考

- 自分自身をフォローすることはできません（その場合は 400 エラー）
- 既にフォローしている場合は 400 エラー
- ブロックしているユーザーをフォローすることはできません
- ブロックされているユーザーをフォローすることはできません
- 非公開アカウントの場合、即座にフォロー成立せず、フォローリクエスト（承認待ち）になります

---

# [DELETE] /api/users/:username/follow

> 認証: 要 | レート制限: なし

## 説明

指定したユーザーのフォローを解除します。

## リクエスト

### パスパラメータ

| 名前 | 型 | 必須 | 説明 |
|---|---|---|---|
| username | string | はい | フォロー解除対象のユーザー名 |

## レスポンス

### 成功 (200)

```json
{
  "message": "unfollowed"
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
fetch("/api/users/other_user/follow", { method: "DELETE", credentials: "include" })
```
