# Money Planner 💰

個人資産管理・シミュレーションのための PWA アプリ。収支記録から将来の資産予測まで、ひとつのアプリで完結します。

---

## 技術スタック

| 項目 | 内容 |
|------|------|
| フレームワーク | React 18 |
| ビルドツール | Vite 5 |
| スタイリング | Tailwind CSS 3 |
| グラフ | Recharts 2 |
| アイコン | Lucide React |
| データ保存 | localStorage（Capacitor Preferences に移行可能な抽象化済み） |
| 配布形式 | PWA（スタンドアロン表示対応） |

---

## セットアップ

```bash
npm install
npm run dev      # 開発サーバー起動
npm run build    # 本番ビルド
npm run preview  # ビルド結果をローカルでプレビュー
```

---

## 主な機能

### 🏠 ホームタブ
- 収支トランザクションの記録（支出・収入）
- 支払い方法の選択：現金 / クレジットカード / 電子マネー
- 割り勘機能（複数人で金額を分割）
- 固定費・定期支出の登録と自動記録
- トランザクションテンプレート（よく使う支出をワンタップで入力）
- 月次締め処理と履歴管理
- PayPay 明細のインポート

### 📅 カレンダータブ
- 月間収支カレンダー表示
- 日別のトランザクション詳細確認
- 収支の棒グラフ表示

### 📊 資産タブ
- 貯金・課税口座投資・NISA・待機資金（ドライパウダー）の資産内訳
- 月次予算との比較分析
- 同世代平均との資産ベンチマーク比較
- 資産の手動編集・投資記録

### 📈 シミュレーションタブ
- 投資スタイル選択（保守的 / 標準的 / 積極的）
- N年後の資産予測グラフ（積み上げエリアチャート）
- モンテカルロシミュレーション（100通りの確率的未来予測）
- 目標達成率・追加必要投資額の表示
- NISA 節税効果の試算
- ライフイベント（結婚・住宅購入・教育費など）の影響シミュレーション

### ⚙️ 設定タブ
- ダークモード切替
- プロフィール設定（名前・年齢）
- 月間予算・カテゴリ別予算スライダー
- 積立・投資目標の設定（目標金額・運用期間・利回り・貯金額）
- 新 NISA 設定
  - つみたて投資枠：月々の積立額
  - 成長投資枠：投資月と1回あたりの金額を個別設定
- カテゴリ管理（追加・リネーム・削除・並び替え）
- クレジットカード管理（締め日・支払日）
- 電子マネー・残高管理（PayPay / Suica / PASMO など、カスタム追加も可能）
- データエクスポート（JSON）・全データリセット

---

## データ永続化

全データは `localStorage` に保存されます。保存対象のキー一覧：

```
transactions / assetData / monthlyHistory / lifeEvents
userInfo / simulationSettings / darkMode / monthlyBudget
customCategories / recurringTransactions / creditCards
splitPayments / dismissedClosingAlerts / transactionTemplates
wallets / walletAdjustments
```

### Capacitor（ネイティブアプリ）への移行

`src/services/storage.js` にアダプター抽象化が実装済みです。`@capacitor/preferences` へ移行する場合：

1. `npm install @capacitor/preferences`
2. `storage.js` 内の `activeAdapter` を `capacitorAdapter` に変更（コメントアウト済みのコードを使用）
3. 永続化処理が `async/await` になるため `usePersistence.js` の各 `useEffect` を非同期対応に変更

---

## ディレクトリ構成

```
src/
├── components/
│   ├── tabs/          # 各タブの UI コンポーネント
│   ├── modals/        # モーダル（入力・編集・確認など）
│   ├── AppShell.jsx   # タブナビゲーション・全体レイアウト
│   ├── ErrorBoundary.jsx
│   └── SplashScreen.jsx
├── context/
│   └── AppContext.jsx  # テーマ定義・Context
├── hooks/
│   ├── useMoneyData.js    # 全 state・ビジネスロジック
│   ├── usePersistence.js  # localStorage への保存副作用
│   └── useGlobalStyles.js
├── services/
│   └── storage.js     # ストレージ抽象化レイヤー
├── utils/
│   ├── calc.js        # 純粋計算関数（シミュレーション・予算分析）
│   └── storage.js
└── constants/
    └── index.js
```

---

## PWA 対応

`public/manifest.json` にて PWA 設定済み。ホーム画面への追加でスタンドアロンアプリとして動作します。
