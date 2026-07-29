# Yudetter API ドキュメント

> 解析日: 2026-07-29
> バージョン: 公開前（yudetter.com）
> 全エンドポイント数: **87**

---

## 共通仕様

| ドキュメント | 内容 |
|---|---|
| [overview.md](overview.md) | 認証・ページネーション・エラー形式・CORS等の共通仕様 |
| [models.md](models.md) | 全データモデル定義（User, Yudate, Game, MarketItem...） |

---

## エンドポイント一覧

### 👤 ユーザー系 `/api/users/*`

| メソッド | パス | ファイル | 認証 |
|---|---|---|---|
| GET | `/api/users/me` | [me-get.md](users/me-get.md) | 要 |
| DELETE | `/api/users/me` | [me-delete.md](users/me-delete.md) | 要 |
| GET | `/api/users/me/blocks` | [me-blocks.md](users/me-blocks.md) | 要 |
| GET | `/api/users/:username` | [user-get.md](users/user-get.md) | 不要 |
| POST | `/api/users/:username/follow` | [follow.md](users/follow.md) | 要 |
| DELETE | `/api/users/:username/follow` | [follow.md](users/follow.md) | 要 |
| POST | `/api/users/:username/block` | [block.md](users/block.md) | 要 |
| DELETE | `/api/users/:username/block` | [block.md](users/block.md) | 要 |
| GET | `/api/users/:username/followers` | [followers.md](users/followers.md) | 不要 |
| GET | `/api/users/:username/following` | [following.md](users/following.md) | 不要 |
| GET | `/api/users/:username/yudates` | [yudates.md](users/yudates.md) | 不要 |
| GET | `/api/users/:username/likes` | [likes.md](users/likes.md) | 不要 |
| GET | `/api/users/me/follow-requests` | [me-follow-requests.md](users/me-follow-requests.md) | 要 |
| POST | `/api/users/:username/follow/approve` | [follow-approve.md](users/follow-approve.md) | 要 |
| POST | `/api/users/:username/follow/reject` | [follow-reject.md](users/follow-reject.md) | 要 |
| POST | `/api/users/lookup-email` | [lookup-email.md](users/lookup-email.md) | 要 |
| POST | `/api/users/sign-in-by-username` | [sign-in-by-username.md](users/sign-in-by-username.md) | 🟢不要 |
| GET | `/api/users/setup/check-username` | [check-username.md](users/check-username.md) | 要 |
| POST | `/api/users/:username/ban` | [ban.md](users/ban.md) | 要 |
| POST | `/api/users/request-email-change` | [request-email-change.md](users/request-email-change.md) | 要 |
| POST | `/api/users/request-password-reset` | [request-password-reset.md](users/request-password-reset.md) | 要 |

### 📝 ユデート（投稿）系 `/api/yudates/*`

| メソッド | パス | ファイル | 認証 |
|---|---|---|---|
| GET | `/api/yudates` | [list-create.md](yudates/list-create.md) | 不要 |
| POST | `/api/yudates` | [list-create.md](yudates/list-create.md) | 要 |
| GET | `/api/yudates/:id` | [get-patch-delete.md](yudates/get-patch-delete.md) | 不要 |
| PATCH | `/api/yudates/:id` | [get-patch-delete.md](yudates/get-patch-delete.md) | 要 |
| DELETE | `/api/yudates/:id` | [get-patch-delete.md](yudates/get-patch-delete.md) | 要 |
| POST | `/api/yudates/:id/pin` | [pin.md](yudates/pin.md) | 要 |
| POST | `/api/yudates/:id/unpin` | [pin.md](yudates/pin.md) | 要 |
| POST | `/api/yudates/:id/like` | [like-reyudate.md](yudates/like-reyudate.md) | 要 |
| DELETE | `/api/yudates/:id/like` | [like-reyudate.md](yudates/like-reyudate.md) | 要 |
| POST | `/api/yudates/:id/reyudate` | [like-reyudate.md](yudates/like-reyudate.md) | 要 |
| DELETE | `/api/yudates/:id/reyudate` | [like-reyudate.md](yudates/like-reyudate.md) | 要 |
| POST | `/api/yudates/:id/reactions` | [reactions.md](yudates/reactions.md) | 要 |
| DELETE | `/api/yudates/:id/reactions/:emoji` | [reactions.md](yudates/reactions.md) | 要 |
| GET | `/api/yudates/:id/replies` | [replies.md](yudates/replies.md) | 不要 |
| POST | `/api/yudates/:id/report` | [report.md](yudates/report.md) | 要 |
| GET | `/api/yudates/scheduled` | [scheduled.md](yudates/scheduled.md) | 要 |
| DELETE | `/api/yudates/scheduled/:id` | [scheduled.md](yudates/scheduled.md) | 要 |

### 🔍 探索・検索系 `/api/explore/*`

| メソッド | パス | ファイル | 認証 |
|---|---|---|---|
| GET | `/api/explore` | [list.md](explore/list.md) | 不要 |
| GET | `/api/explore/popular` | [popular.md](explore/popular.md) | 不要 |
| GET | `/api/explore/trends` | [trends.md](explore/trends.md) | 不要 |
| GET | `/api/explore/search` | [search.md](explore/search.md) | 不要 |

### 🔔 通知系 `/api/notifications/*`

| メソッド | パス | ファイル | 認証 |
|---|---|---|---|
| GET | `/api/notifications` | [list.md](notifications/list.md) | 要 |
| GET | `/api/notifications/unread-count` | [unread-count.md](notifications/unread-count.md) | 要 |
| POST | `/api/notifications/read` | [read.md](notifications/read.md) | 要 |
| GET | `/api/notifications/stream` | [stream.md](notifications/stream.md) | 要 |

### 💰 ウォレット系 `/api/wallet/*`

| メソッド | パス | ファイル | 認証 |
|---|---|---|---|
| GET | `/api/wallet/history` | [history.md](wallet/history.md) | 要 |

### 🏪 マーケット系 `/api/market/*`

| メソッド | パス | ファイル | 認証 |
|---|---|---|---|
| GET | `/api/market/items` | [items.md](market/items.md) | 要 |
| GET | `/api/market/items/:id` | [item-crud.md](market/item-crud.md) | 要 |
| PUT | `/api/market/items/:id` | [item-crud.md](market/item-crud.md) | 要 |
| DELETE | `/api/market/items/:id` | [item-crud.md](market/item-crud.md) | 要 |
| POST | `/api/market/items/:id/purchase` | [purchase.md](market/purchase.md) | 要 |
| POST | `/api/market/items/:id/like` | [like.md](market/like.md) | 要 |
| DELETE | `/api/market/items/:id/like` | [like.md](market/like.md) | 要 |
| GET | `/api/market/items/:id/comments` | [comments.md](market/comments.md) | 要 |
| POST | `/api/market/items/:id/comments` | [comments.md](market/comments.md) | 要 |
| POST | `/api/market/items/:id/buy` | [buy.md](market/buy.md) | 要 |
| POST | `/api/market/items/:id/buy-installment` | [buy-installment.md](market/buy-installment.md) | 要 |
| POST | `/api/market/items/:id/bid` | [bid.md](market/bid.md) | 要 |
| POST | `/api/market/items/:id/report` | [report.md](market/report.md) | 要 |
| POST | `/api/market/items/:id/request-delete` | [request-delete.md](market/request-delete.md) | 要 |
| POST | `/api/market/items/:id/apply-user-id` | [apply-user-id.md](market/apply-user-id.md) | 要 |
| POST | `/api/market/items/:id/claim-id` | [claim-id.md](market/claim-id.md) | 要 |
| GET | `/api/market/installments/eligibility` | [installments-eligibility.md](market/installments-eligibility.md) | 要 |

### 🎮 ゲーム系 `/api/games/*`

| メソッド | パス | ファイル | 認証 |
|---|---|---|---|
| GET | `/api/games` | [list-create.md](games/list-create.md) | 要 |
| POST | `/api/games` | [list-create.md](games/list-create.md) | 要 |
| GET | `/api/games/:id` | [detail.md](games/detail.md) | 要 |
| PUT | `/api/games/:id` | [update.md](games/update.md) | 要 |
| POST | `/api/games/:id/charge` | [charge.md](games/charge.md) | 要 |
| DELETE | `/api/games/:id` | [update.md](games/update.md) | 要 |
| POST | `/api/games/auth/verify` | [auth-verify.md](games/auth-verify.md) | 要 |

### 🏆 ランキング系 `/api/rankings/*`

| メソッド | パス | ファイル | 認証 |
|---|---|---|---|
| GET | `/api/rankings` | [get.md](rankings/get.md) | 要 |
| POST | `/api/rankings/opt-in` | [opt-in.md](rankings/opt-in.md) | 要 |
| POST | `/api/rankings/opt-out` | [opt-out.md](rankings/opt-out.md) | 要 |

### 🔐 認証系

| メソッド | パス | ファイル | 認証 |
|---|---|---|---|
| GET | `/api/auth/get-session` | 認証確認（_overview.md参照） | 🟢不要 |

### 👑 VIP系 `/api/vip/*`

| メソッド | パス | ファイル | 認証 |
|---|---|---|---|
| GET | `/api/vip/casino-access` | [casino-access.md](vip/casino-access.md) | 要 |

### 📤 アップロード系

| メソッド | パス | ファイル | 認証 |
|---|---|---|---|
| POST | `/api/upload` | [upload.md](upload.md) | 要 |

### 🪙 Yudecoin系 `/api/yudecoin/*`

| メソッド | パス | ファイル | 認証 |
|---|---|---|---|
| GET | `/api/yudecoin/market` | [market.md](yudecoin/market.md) | 不要 |
| GET | `/api/yudecoin/chart` | [chart.md](yudecoin/chart.md) | 不要 |
| GET | `/api/yudecoin/wallet` | [wallet.md](yudecoin/wallet.md) | 要 |
| POST | `/api/yudecoin/token` | [token.md](yudecoin/token.md) | 要 |
| POST | `/api/yudecoin/trade` | [trade.md](yudecoin/trade.md) | 要 |

### 🍊 Mikancoin系 `/api/mikancoin/*`

| メソッド | パス | ファイル | 認証 |
|---|---|---|---|
| GET | `/api/mikancoin/market` | [market.md](mikancoin/market.md) | 不要 |
| GET | `/api/mikancoin/chart` | [chart.md](mikancoin/chart.md) | 不要 |
| GET | `/api/mikancoin/wallet` | [wallet.md](mikancoin/wallet.md) | 要 |
| POST | `/api/mikancoin/token` | [token.md](mikancoin/token.md) | 要 |
| POST | `/api/mikancoin/exchange` | [exchange.md](mikancoin/exchange.md) | 要 |

---

## 凡例

| 記号 | 意味 |
|---|---|
| 🔴 **要** | 認証必須（BetterAuthセッションCookieが必要） |
| 🟢 **不要** | 認証不要（未ログインでもアクセス可能） |
| ⚠️ | 機密情報を含むレスポンス（注意） |

## 統計

| 項目 | 値 |
|---|---|
| 総エンドポイント数 | 87 |
| 総ファイル数 | 69 |
| 認証要エンドポイント | 65 |
| 認証不要エンドポイント | 22 |
