# データモデル定義

---

## YudecoinWallet（YDCウォレット）

```typescript
interface YudecoinWalletResponse {
  wallet: {
    userId: number;
    balance: string;           // 総YDC残高（BigInt文字列）
    unlockedBalance: string;   // 取引可能YDC残高
    updatedAt: string;         // ISO 8601
  };
  tradedAmountToday: string;   // 本日の取引累計量（YDCのみ）
}
```

---

## MikancoinWallet（MKCウォレット）

```typescript
interface MikancoinWalletResponse {
  wallet: {
    userId: number;
    balance: string;           // 総MKC残高（BigInt文字列）
    unlockedBalance: string;   // 取引可能MKC残高
    updatedAt: string;         // ISO 8601
  };
}
// tradedAmountToday は存在しない（MKCに日次制限なし）
```

---

## YudecoinMarket（YDCマーケット）

```typescript
interface YudecoinMarketResponse {
  price: number;               // 1YDCあたりのYD価格（ydReserve / yudecoinReserve）
  pool: {
    id: number;
    ydReserve: string;         // プール内YD残高（BigInt文字列）
    yudecoinReserve: string;   // プール内YDC残高
    kConstant: string;         // 定数積 x * y = k
    updatedAt: string;
  };
}
```

---

## MikancoinMarket（MKCマーケット）

```typescript
interface MikancoinMarketResponse {
  symbol: string;              // "MKC"
  name: string;                // "Mikancoin"
  initialPriceYd: number;      // 初期価格
  priceYd: number;             // 現在価格（1MKCあたりのYD価格）
  pool: {
    ydReserve: string;         // プール内YD残高
    mkcReserve: string;        // プール内MKC残高
    kConstant: string;         // 定数積
  };
}
```

---

## ChartCandle（チャートローソク足）

```typescript
interface ChartCandle {
  timestampBucket: string;     // ローソク足開始時刻（ISO 8601）
  open: number;                // 始値
  high: number;                // 高値
  low: number;                 // 低値
  close: number;               // 終値
  volumeYd: number;            // YD建て出来高
  volumeYudecoin: number;      // トークン建て出来高（YDC/MKC共通でこのフィールド名）
}
```

解像度別件数:
- `1m`: 15 candles
- `1h`: 24 candles
- `1d`: 30 candles

---

## YudecoinTradeResponse（YDC取引結果）

```typescript
interface YudecoinTradeResponse {
  success: boolean;
  executedYdAmount: string;         // 消費YD量
  executedYudecoinAmount: string;   // 取得YDC量
  feeAmount: string;                // 手数料（YD）
  priceAtExecution: number;         // 約定価格
  newBalance: string;               // 増分残高
}
```

---

## MikancoinExchangeResponse（MKC取引結果）

```typescript
interface MikancoinExchangeResponse {
  success: boolean;
  transactionId: number;            // トランザクションID（YDCにはない）
  ydAmount: string;                 // 消費YD量（YDCは executedYdAmount）
  mkcAmount: string;                // 取得MKC量（YDCは executedYudecoinAmount）
  feeAmount: string;                // 手数料（YD）
  mkcBalance: string;               // 総残高（YDCは newBalance: 増分のみ）
  priceAtExecution: number;         // 約定価格
}
```

---

## User（ユーザー）

```typescript
interface User {
  id: number;                    // 内部ユーザーID
  clerkId: string;               // BetterAuth認証ID（レガシー互換のフィールド名）
  username: string;              // ユーザー名（一意）
  displayName: string;           // 表示名
  email: string;                 // メールアドレス（機密）
  bio: string | null;            // 自己紹介
  avatarUrl: string | null;      // アバターURL
  headerUrl: string | null;      // ヘッダー画像URL
  birthday: string | null;       // 誕生日 "YYYY-MM-DD"（機密）
  setupComplete: boolean;        // 初回セットアップ完了
  followerCount: number;
  followingCount: number;
  yudateCount: number;
  isFollowing: boolean;
  isFollowPending: boolean;
  isPrivate: boolean;
  isBlocking: boolean;
  isBlockedBy: boolean;
  isBanned: boolean;
  isVip: boolean;
  vipAvatarFlameEnabled: boolean;
  vipPostFrameEnabled: boolean;
  vipNameGoldEnabled: boolean;
  pinnedYudateId: number | null;
  yudedollar: number;            // ウォレット残高（機密）
  badgeType: string | null;      // "gold" | "silver" | "bronze" | null
  isVerified: boolean;
  consecutiveLoginDays: number;
  rankingOptIn: boolean;
  createdAt: string;             // ISO 8601
}
```

> ⚠️ `email`, `birthday`, `yudedollar`, `clerkId` は機密情報です。
> 現在のAPIはこれらのフィールドを含む User オブジェクトを `creator`, `seller`, `author` 等の名目で複数エンドポイントが露出しています。

---

## Yudate（投稿）

```typescript
interface Yudate {
  id: number;
  content: string;
  imageUrl: string | null;
  author: User;                  // 著者（完全なUserオブジェクト）
  likeCount: number;
  reyudateCount: number;
  replyCount: number;
  isLiked: boolean;
  isReyudated: boolean;
  isSpoiler: boolean;
  reactions: Reaction[];         // [{emoji, count, isReacted}]
  quotedYudate: Yudate | null;  // 引用元
  reyudatedBy: User | null;     // リユデートしたユーザー
  reyudateCreatedAt: string | null;
  replyToId: number | null;
  superYudateAmount: number;    // スーパーユデート額（YD、0の場合はなし）
  visibility: string;           // "public" | ...
  scheduledFor: string | null;  // 予約投稿日時
  autoDeleteAt: string | null;  // 自動削除日時
  isAdvertisement: boolean;     // 広告投稿か
  createdAt: string;
}

interface Reaction {
  emoji: string;
  count: number;
  isReacted: boolean;
}
```

---

## Game（ゲーム）

```typescript
interface Game {
  id: number;
  creator: User;                 // 作成者（完全なUserオブジェクト）
  title: string;
  description: string | null;
  htmlContent: string;           // ゲームHTML（全ソースコード）
  playPrice: number;             // プレイ価格（YD）
  createdAt: string;
  updatedAt: string;
}
```

---

## MarketItem（マーケット商品）

```typescript
interface MarketItem {
  id: number;
  seller: User;                  // 出品者（完全なUserオブジェクト）
  buyer: User | null;            // 購入者（完全なUserオブジェクト）
  title: string;
  description: string;
  itemType: string;              // "image" | "audio" | "user_id"
  itemData: string;              // ファイルURL
  thumbnailUrl: string | null;
  price: number;
  saleType: string;              // "normal" | "auction"
  status: string;                // "selling" | "sold"
  stock: number;
  auctionEndAt: string | null;
  highestBid: number | null;
  highestBidder: User | null;    // 最高入札者（完全なUserオブジェクト）
  buyoutPrice: number | null;
  createdAt: string;
  updatedAt: string;
  likeCount: number;
  commentCount: number;
  isLiked: boolean;
  isBought: boolean;
}
```

---

## Transaction（取引履歴）

```typescript
interface Transaction {
  id: number;
  type: string;                  // "game_charge" | "market_purchase" | ...
  amount: number;
  description: string;
  createdAt: string;
}
```

---

## Notification（通知）

```typescript
interface Notification {
  id: number;
  type: string;                  // "like" | "follow" | "reyudate" | "reply" | "super_yudate" | ...
  actor: User;                   // アクションを起こしたユーザー（完全なUserオブジェクト）
  yudate: Yudate | null;        // 対象の投稿
  actionYudate: Yudate | null;  // アクションによって作成された投稿（リユデート等）
  itemId: number | null;        // 関連するアイテムID（マーケット等）
  detail: string | null;        // 追加詳細
  read: boolean;
  createdAt: string;
}
```

> ⚠️ REST API のレスポンスは上記の形式ですが、SSEストリーム（`/api/notifications/stream`）では異なるフィールド名で配信されます。

---

## RankingEntry（ランキング）

```typescript
interface RankingEntry {
  user: User;                    // ユーザー（完全なUserオブジェクト）
  score: number;
}

interface RankingsResponse {
  weekly: {
    post: RankingEntry[];
    follower: RankingEntry[];
  };
  allTime: {
    post: RankingEntry[];
    follower: RankingEntry[];
  };
}
```

---

## Trend（トレンド）

```typescript
interface Trend {
  topic: string;  // トレンドキーワード
  posts: string;  // 投稿数（文字列）
}
```

---

## PaginatedResponse（ページネーション共通）

```typescript
interface PaginatedResponse<T> {
  items: T[];
  nextCursor: number | null;    // 数値カーソル。null = 最終ページ
}
```
