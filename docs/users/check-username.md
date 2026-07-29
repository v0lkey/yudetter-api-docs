# [GET] /api/users/setup/check-username

> 認証: 要 | レート制限: なし

## 説明

指定されたユーザー名が利用可能かどうかを確認します。アカウント作成時のユーザー名重複チェックに使用します。

## リクエスト

### パスパラメータ
なし

### クエリパラメータ
| 名前 | 型 | 必須 | 説明 |
|---|---|---|---|
| username | string | はい | チェックするユーザー名 |

### リクエストBody
なし

## レスポンス

### 成功 (200) — 利用可能
```json
{
  "available": true
}
```

### 成功 (200) — 既に使用中
```json
{
  "available": false
}
```

## 実行例

```bash
curl -X GET "https://yudetter.com/api/users/setup/check-username?username=taro_yudetter" -b "cookie"
```

```javascript
const checkName = async (name) => {
  const res = await fetch(`/api/users/setup/check-username?username=${encodeURIComponent(name)}`, {
    credentials: "include"
  });
  const data = await res.json();
  return data.available;
};
```

## 備考

- ⚠️ **レート制限は実装されていません。** 既存ユーザー名の存在確認が無制限に可能です。
- 英字3文字の組み合わせ（26^3 = 17,576通りのみ）を総当たりすることで、全ユーザー名のリストを取得するブルートフォース攻撃が理論上可能です。より長いユーザー名でも、辞書攻撃と組み合わせることで効率的に探索できます。
- 認証必須ですが、認証済みユーザーであれば誰でも任意のユーザー名をチェックできます。
- セットアップ画面でリアルタイムに利用可能かどうかを表示するために使用されます。
