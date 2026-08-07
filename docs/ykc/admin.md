# Ykc 管理 API（/api/ykc/admin/*）

> 認証: 要（管理者セッション） | レート制限: なし

## 説明

Ykc（YKC）の運営管理用エンドポイント群です。`ykc-admin` チャンクの管理パネルから呼び出されます。**一般ユーザーからはアクセスできません（管理者認証必須）。**

先頭に管理者チェックがあり、認証に失敗すると `400 / 403` と「管理者認証が必要です」が返ります。

## エンドポイント一覧

| メソッド | パス | 説明 |
|---|---|---|
| GET | `/api/ykc/admin/status` | 現在の価格・状態サマリー |
| GET | `/api/ykc/admin/price-updates` | 価格更新履歴一覧 |
| GET | `/api/ykc/admin/anomalies` | 異常検知一覧 |
| POST | `/api/ykc/admin/price-update` | 価格強制更新を実行（任意で `reason`） |
| POST | `/api/ykc/admin/restore-previous-price` | 前回価格へ復元（`reason` 必須） |
| PATCH | `/api/ykc/admin/settings` | 設定変更（`reason` 必須） |

---

# [GET] /api/ykc/admin/status

## レスポンス (200)

```json
{
  "price": 1.235,
  "status": "normal",
  "updatedAt": "2026-08-07T00:00:00Z"
}
```

---

# [GET] /api/ykc/admin/price-updates

価格更新の履歴を返します。

## レスポンス (200)

```json
{
  "items": [
    {
      "id": 1,
      "priceYd": 1.235,
      "reason": "市場変動",
      "createdAt": "2026-08-07T00:00:00Z"
    }
  ]
}
```

---

# [GET] /api/ykc/admin/anomalies

価格の異常を検知したイベント一覧を返します。

## レスポンス (200)

```json
{
  "items": [
    {
      "id": 3,
      "type": "price_spike",
      "message": "1分で+20%の変動",
      "createdAt": "2026-08-07T00:00:00Z"
    }
  ]
}
```

---

# [POST] /api/ykc/admin/price-update

価格の強制更新を実行します。

## リクエストBody

```json
{
  "reason": "市場急変への対応"
}
```

`reason` は任意（変更理由が3文字以上のとき送信）。

## レスポンス (200)

```json
{
  "status": "価格更新を実行しました"
}
```

---

# [POST] /api/ykc/admin/restore-previous-price

価格を前回値に復元します。

## リクエストBody

```json
{
  "reason": "誤処理のロールバック"
}
```

**`reason` は必須**（変更理由が3文字以上のエラー時に弾かれます）。

## レスポンス (200)

```json
{
  "status": "前の価格へ復元しました"
}
```

---

# [PATCH] /api/ykc/admin/settings

Ykcの管理設定を変更します。

## リクエストBody

```json
{
  "reason": "設定変更",
  "minimumPriceChange": 0.001
}
```

`reason` は必須（変更理由が3文字以上必要）。

## レスポンス (200)

```json
{
  "status": "設定を更新しました"
}
```

---

## 共通の備考

- 全エンドポイントで管理者セッションが必要です（`X-Yudetter-Web` ヘッダに相当）
- 変更系（`price-update` / `restore-previous-price` / `settings`）は操作理由を記録する設計です
- レスポンスのフィールド名はUI実装に基づく想定であり、実際のレスポンスは実装により変わることがあります