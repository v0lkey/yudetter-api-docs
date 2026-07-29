# Yudetter API 共通仕様

## Base URL

```
https://yudetter.com
```

## 認証

- **方式:** BetterAuth（セルフホスト認証）
- **メカニズム:** HTTPOnly Cookie（自動送信、`__Secure-better-auth.session_token`）
- **ログイン方法:** `POST /api/auth/sign-in/email`（email認証）または `POST /api/users/sign-in-by-username`（ユーザー名認証）
- **セッション確認:** `GET /api/auth/get-session` または `GET /api/users/me`（200=認証済み, 401=未認証）

認証が必要なエンドポイントは「認証: 要」と記載。
認証不要のエンドポイントは「認証: 不要」と記載。

## コンテンツタイプ

全てのリクエスト・レスポンスは `application/json`（一部 `FormData` を除く）。

```
Content-Type: application/json
```

## ページネーション

リスト系APIはカーソルベースページネーションを採用:

```typescript
interface PaginatedResponse<T> {
  items: T[];
  nextCursor: number | null;  // null = 最終ページ
}
```

クエリパラメータ例: `GET /api/explore?cursor=<opaque_cursor>`

## 日付形式

全ての日付は ISO 8601 形式:

```
"2026-07-11T09:26:43.908Z"
```

誕生日のみ `YYYY-MM-DD` 形式:

```
"2000-08-03"
```

## HTTPステータスコード

| コード | 意味 |
|---|---|
| 200 | 成功 |
| 201 | 作成成功（POST） |
| 204 | 削除成功（DELETE） |
| 400 | バリデーションエラー |
| 401 | 認証が必要 |
| 403 | 権限なし |
| 404 | リソースなし |
| 500 | サーバーエラー |

## エラーレスポンス形式

```json
{
  "error": "エラーメッセージ（日本語）"
}
```

## レート制限

**現状: なし**
全てのエンドポイントにレート制限は実装されていません。

## CSRF対策

**現状: なし**
CSRFトークンは実装されていません。Cookieベース認証のため、SameSite=Lax以上に依存しています。

## 特別ヘッダー

一部のエンドポイント（Yudecoin/Mikancoin系）では以下のカスタムヘッダーが必須:

| ヘッダー | 必須エンドポイント | 内容 |
|---|---|---|
| `X-Yudetter-Web: true` | `POST /api/yudecoin/*`, `POST /api/mikancoin/*` | Webクライアント識別用。不足すると403

## CORS

- `Access-Control-Allow-Credentials: true`
- `Vary: Origin`（Origin検証は動的）

## ストレージ

画像等のアップロード先は Supabase Storage:

```
https://yaubmafsnsufjasndnft.supabase.co/storage/v1/object/public/yudetter-bucket/{timestamp}-{random}.{ext}
```

## スクリプト

全JSバンドルは Vite でビルドされ、動的インポートで分割されています。

| バンドル | 内容 |
|---|---|
| `index-Di8x5gxm.js` | メインバンドル（全機能・ルーター・BetterAuth統合） |
| `vendor-ui-56RSVuBV.js` | UIコンポーネント |
| `vendor-icons-D0ikjflo.js` | アイコン |
| 他 lazy chunks | 各ルートのページコンポーネント |
