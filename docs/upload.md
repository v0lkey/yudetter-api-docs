# [POST] /api/upload

> 認証: 要 | レート制限: なし

## 説明
画像や音声などのファイルを Supabase Storage にアップロードします。`multipart/form-data`（FormData）で送信します。

## リクエスト
### リクエストBody (multipart/form-data)
| フィールド | 型 | 必須 | 説明 |
|---|---|---|---|
| file | File | はい | アップロードするファイル（画像・音声等） |

### Content-Type
`multipart/form-data`（FormData を使用してください）

## レスポンス
### 成功 (200)
```json
{
  "url": "https://supabase.storage.example.com/yudetter/files/abc123.jpg"
}
```
### エラー (400)
```json
{
  "error": "アップロードに失敗しました"
}
```
### エラー (400 - ファイルなし)
```json
{
  "error": "ファイルが指定されていません"
}
```
### エラー (400 - サイズ超過)
```json
{
  "error": "ファイルサイズが制限を超えています"
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
const formData = new FormData();
formData.append("file", fileInput.files[0]);

fetch("/api/upload", {
  method: "POST",
  credentials: "include",
  body: formData
}).then(res => res.json()).then(data => {
  console.log("アップロードURL:", data.url);
});
```

## 備考
- アップロード先は Supabase Storage です
- 対応形式: 画像（JPEG, PNG, GIF, WebP）、音声（MP3, WAV, AAC 等）— サーバー設定に依存
- ファイルサイズ制限はサーバー設定に依存します
- `Content-Type` ヘッダーは手動で設定しないでください（`multipart/form-data` はブラウザが自動設定します）
- 返却されたURLを Yudate 作成時の `imageUrl` 等に使用します
