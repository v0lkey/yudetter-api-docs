# YGS SDK（Yudetter Game SDK）リファレンス

> ゲーム内から課金決済・報酬出金・ユーザー認証・クラウドセーブを行うための開発者向けSDKです。

## 概要

Yudetterのゲームはユーザーが作成したHTML/JSを yudetter.com の **iframe 内で実行**します。ゲームは `window.parent.postMessage()` でサイト本体と通信し、課金決済・報酬出金・ユーザー認証・クラウドセーブを実行できます。

- 通信は `type` が `YGS_*_REQUEST` / `YGS_*_RESPONSE` のメッセージを `postMessage` で行います
- レスポンスはリクエスト元（`e.source`）へ `targetOrigin: "*"` で返されます
- 確認ダイアログは**同時に1つまで**。別のダイアログが開いている間は `success: false` が返ります

## 関数一覧

| 関数 | 概要 | 返り値 |
|---|---|---|
| `requestYdPayment(amount, description)` | プレイヤーからYDを徴収（開発者への入金） | `transactionId` |
| `requestYdWithdraw(amount, description)` | プレイヤーにYDを支払う（開発者からの出金・残高不足時はエラー） | `pending` / `requestId` |
| `requestYdAuth()` | プレイヤーの認証情報を取得（OAuthライク・ユーザー承認が必要） | `{ id, username, displayName, avatarUrl }` |
| `requestYdSaveData(key, value)` | アカウントにデータを保存（クラウドセーブ） | `{ key, updatedAt }` |
| `requestYdLoadData(key)` | アカウントからデータを読み込む | `value`（文字列または `null`） |

## SDKコード

```javascript
// ===== YGS SDK =====
// 【重要】localStorage にデータを保存すると端末ローカル（ブラウザ単位）に保存されるため、
// アカウントを切り替えたり他端末からログインした際にデータが同期されません。
// アカウントごとにセーブデータを同期させるには requestYdSaveData() / requestYdLoadData() を使用してください。

// プレイヤーに課金する（プレイヤーから開発者へYDを移動）
function requestYdPayment(amount, description) {
  return new Promise((resolve, reject) => {
    window.parent.postMessage({ type: 'YGS_PAYMENT_REQUEST', amount, description }, '*');
    const handler = (e) => {
      if (e.data && e.data.type === 'YGS_PAYMENT_RESPONSE') {
        window.removeEventListener('message', handler);
        if (e.data.success) {
          resolve(e.data.transactionId);
        } else {
          reject(new Error(e.data.error || '決済失敗'));
        }
      }
    };
    window.addEventListener('message', handler);
  });
}

// プレイヤーに報酬を支払う（開発者からプレイヤーへYDを移動）
// 注意: 実装では出金はゲーム作成者の承認待ち（pending）となり、
//       即時支払いではなく requestId が返ります（transactionId は返りません）
function requestYdWithdraw(amount, description) {
  return new Promise((resolve, reject) => {
    window.parent.postMessage({ type: 'YGS_WITHDRAW_REQUEST', amount, description }, '*');
    const handler = (e) => {
      if (e.data && e.data.type === 'YGS_WITHDRAW_RESPONSE') {
        window.removeEventListener('message', handler);
        if (e.data.success) {
          resolve({ pending: e.data.pending, requestId: e.data.requestId });
        } else {
          reject(new Error(e.data.error || '出金失敗'));
        }
      }
    };
    window.addEventListener('message', handler);
  });
}

// プレイヤーの認証情報を取得する（OAuthライク）
// 戻り値: { id, username, displayName, avatarUrl }
function requestYdAuth() {
  return new Promise((resolve, reject) => {
    window.parent.postMessage({ type: 'YGS_AUTH_REQUEST' }, '*');
    const handler = (e) => {
      if (e.data && e.data.type === 'YGS_AUTH_RESPONSE') {
        window.removeEventListener('message', handler);
        if (e.data.success) {
          resolve(e.data.user);
        } else {
          reject(new Error(e.data.error || '認証失敗'));
        }
      }
    };
    window.addEventListener('message', handler);
  });
}

// プレイヤーのアカウントにデータを保存する (クラウドセーブデータ)
// key: 任意の識別キー (例: "default", "save_1", "score"), value: 保存する文字列(JSONなど)
function requestYdSaveData(key, value) {
  return new Promise((resolve, reject) => {
    window.parent.postMessage({ type: 'YGS_SAVE_DATA_REQUEST', key, value }, '*');
    const handler = (e) => {
      if (e.data && e.data.type === 'YGS_SAVE_DATA_RESPONSE') {
        window.removeEventListener('message', handler);
        if (e.data.success) {
          resolve({ key: e.data.key, updatedAt: e.data.updatedAt });
        } else {
          reject(new Error(e.data.error || 'データ保存失敗'));
        }
      }
    };
    window.addEventListener('message', handler);
  });
}

// プレイヤーのアカウントからデータを読み込む (クラウドセーブデータ)
// 戻り値: value (データ文字列またはnull)
function requestYdLoadData(key) {
  return new Promise((resolve, reject) => {
    window.parent.postMessage({ type: 'YGS_LOAD_DATA_REQUEST', key }, '*');
    const handler = (e) => {
      if (e.data && e.data.type === 'YGS_LOAD_DATA_RESPONSE') {
        window.removeEventListener('message', handler);
        if (e.data.success) {
          resolve(e.data.value);
        } else {
          reject(new Error(e.data.error || 'データ読み込み失敗'));
        }
      }
    };
    window.addEventListener('message', handler);
  });
}
```

## メッセージ仕様

### YGS_PAYMENT_REQUEST

ゲームがプレイヤーに課金します。プレイヤーの確認ダイアログ後、プレイヤーのYD残高からゲーム開発者へ移動します。

| 項目 | 型 | 説明 |
|---|---|---|
| `amount` | number | 課金額（YD）。`0` より大きい必要があります |
| `description` | string | 課金の説明（例: "アイテム購入"） |

レスポンス `YGS_PAYMENT_RESPONSE`:

```json
{ "success": true, "transactionId": "tx-123" }
```

| フィールド | 型 | 説明 |
|---|---|---|
| `success` | boolean | 成否 |
| `transactionId` | string | 取引ID（成功時） |
| `error` | string | エラーメッセージ（失敗時） |

### YGS_WITHDRAW_REQUEST

ゲームがプレイヤーに報酬を支払います。**即時支払いではなく、ゲーム作成者への報酬申請（pending）として登録**され、作成者が通知から承認すると作成者のYD残高から支払われます。

| 項目 | 型 | 説明 |
|---|---|---|
| `amount` | number | 報酬額（YD）。安全な整数（`Number.isSafeInteger`）かつ `1` 以上である必要があります |
| `description` | string | 報酬の説明（最大200文字） |

レスポンス `YGS_WITHDRAW_RESPONSE`:

```json
{ "success": true, "pending": true, "requestId": 123 }
```

| フィールド | 型 | 説明 |
|---|---|---|
| `success` | boolean | 成否 |
| `pending` | boolean | 承認待ちフラグ（常に `true`） |
| `requestId` | number | 報酬申請ID（承認・却下は [reward-requests.md](reward-requests.md) で管理） |
| `error` | string | エラーメッセージ（失敗時）。残高不足時はその旨のエラーが返ります |

### YGS_AUTH_REQUEST

プレイヤーの認証情報を取得します。ユーザーの承認ダイアログ後に認証セッションが発行され、ゲーム内の通信認証（自前サーバーとの連携など）に使用できます。

リクエストペイロードはありません。

レスポンス `YGS_AUTH_RESPONSE`:

```json
{
  "success": true,
  "user": { "id": 42, "username": "taro", "displayName": "たろう", "avatarUrl": "https://..." },
  "token": "game-auth-token-xxx"
}
```

| フィールド | 型 | 説明 |
|---|---|---|
| `success` | boolean | 成否 |
| `user` | object | プレイヤー情報（`id`, `username`, `displayName`, `avatarUrl`） |
| `token` | string | 認証トークン（自前サーバーでの検証に使用。 [auth-verify.md](auth-verify.md) 参照） |
| `error` | string | エラーメッセージ（失敗時） |

### YGS_SAVE_DATA_REQUEST

プレイヤーのアカウントにデータを保存します（クラウドセーブ）。

| 項目 | 型 | 説明 |
|---|---|---|
| `key` | string | 識別キー（例: `"default"`, `"save_1"`, `"score"`） |
| `value` | string | 保存する文字列（JSONなど） |

レスポンス `YGS_SAVE_DATA_RESPONSE`:

```json
{ "success": true, "key": "save_1", "updatedAt": "2026-08-07T00:00:00Z" }
```

### YGS_LOAD_DATA_REQUEST

プレイヤーのアカウントからデータを読み込みます。

| 項目 | 型 | 説明 |
|---|---|---|
| `key` | string | 識別キー |

レスポンス `YGS_LOAD_DATA_RESPONSE`:

```json
{ "success": true, "key": "save_1", "value": "{\"score\":1000}", "updatedAt": "2026-08-07T00:00:00Z" }
```

`value` は保存データの文字列、未保存の場合は `null` です。

## 外部DB / 自前サーバー連携

大容量データや自前DB管理（Supabase, Firebase, 独自API等）を行う場合、以下の手順で安全に認証連携します。

1. **[HTMLゲーム内]** `requestYdAuth()` を呼んでトークンを取得し、自前サーバーへ送信:

```javascript
const authResult = await requestYdAuth();
const clientToken = authResult.token;
```

2. **[自前サーバー側 (Node.js/Python等)]** Yudetter 検証APIでプレイヤー本人確認:

```javascript
const res = await fetch("https://yudetter.com/api/games/auth/verify", {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({ token: clientToken })
});
const { success, user } = await res.json();
// 検証成功後、user.id をキーにして Supabase / Firebase / 自前DB にデータを保存
```

## 使用例

```javascript
// 認証してユーザー情報を取得
const user = await requestYdAuth();
console.log("ログインユーザー:", user.username);

// 100YDのアイテム購入（プレイヤー→開発者）
const txId = await requestYdPayment(100, "スキン購入");

// 50YDの報酬支払い（開発者→プレイヤー）
// 申請は承認待ち（pending）となり、開発者の残高が不足している場合はエラー
const reward = await requestYdWithdraw(50, "ステージクリア報酬");
```

## 注意点

- **`localStorage` は同期されません**: 端末ローカル（ブラウザ単位）に保存されるため、アカウント切り替えや他端末ログインでデータが消えます。アカウント単位で同期するには `requestYdSaveData()` / `requestYdLoadData()` を使用してください
- **出金は承認制**: `requestYdWithdraw()` は即時支払いではなくゲーム作成者の承認待ちになります。報酬申請の管理は [reward-requests.md](reward-requests.md) を参照してください
- **金額はゲーム側の自己申告**: `amount` の正しさはクライアント側のサニティチェック（整数・正数・説明文の長さ）のみで検証されます
- **確認ダイアログは同時に1つ**: 他ダイアログ表示中は `success: false` が返ります

## 関連API

| エンドポイント | 用途 |
|---|---|
| [POST /api/games/:id/auth-request](auth-request.md) | 認証セッション発行 |
| [POST /api/games/auth/verify](auth-verify.md) | トークン検証（自前サーバー連携） |
| [POST /api/games/:id/charge-token](charge-token.md) / [POST /api/games/:id/charge](charge.md) | 課金（チャージ）実行 |
| [GET/POST /api/games/:id/reward-requests](reward-requests.md) | 報酬申請の一覧・作成・承認・却下 |
| [GET/POST /api/games/:id/save-data](save-data.md) | クラウドセーブの読み書き |
