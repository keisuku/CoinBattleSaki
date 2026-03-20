# CoinBattleSaki — Unity構築順序

## 開発の4レイヤー構造

```
1. 素材    → AI生成 + Asset Store + フリー素材
2. 動き    → DOTween + Spine + Unity Animator
3. 制御    → C# + PlayMaker + CRIWARE
4. 組み合わせ → 演出シーケンス（楽譜）
```

アセットは「1と2を短縮」する役割。
組み合わせ（演出シーケンス）が先にないと、何を買うべきかも決まらない。

---

## Step 1: 演出シーケンス（楽譜）を書く

Unityを触る前に、各状態遷移の「何が・いつ・どう変わるか」を定義する。
ADR-0007に素材はあるので、それを時間軸に展開する。

### 記述する内容

各遷移（S1→S2, S2→S3, S3→S4, S4→S5, S5→S6, S6→S7, ANY→S8）ごとに：

| タイミング | Layer 1: 背景 | Layer 2: キャラ/神獣 | Layer 3: エフェクト | Layer 4: UI | Layer 5: 音 |
|-----------|-------------|-------------------|------------------|------------|------------|
| 0ms | — | — | — | — | — |
| 100ms | 色温度変化開始 | 目を開く | — | — | 低音ping |
| 300ms | — | 戦闘態勢 | 画面端シマー | 価格表示拡大 | — |
| 500ms | — | — | フラッシュ | テキスト表示 | インパクトSE |
| ... | ... | ... | ... | ... | ... |

### この楽譜から導出されるもの

- 必要なエフェクトの種類と数 → Asset Store で何を買うか
- 必要なSE/BGMの種類と数 → フリー素材 or AI生成で何を作るか
- キャラアニメの必要ポーズ数 → Spine でどこまで作るか
- UI変化のパターン → DOTween のシーケンスをどう組むか

---

## Step 2: 素材を揃える

### Unity Asset Store（購入）

| カテゴリ | 用途 | 導入タイミング |
|---------|------|-------------|
| **DOTween Pro** | UIアニメ全般。必須 | 最初 |
| **VFXパーティクルパック** | 光・スパーク・フラッシュ・画面シェイク | 最初 |
| **ダークUI素材パック** | パネル・ボタン・ゲージのベース | 最初 |
| **PlayMaker** | 8状態マシンを視覚的に構築 | 最初 |
| **Spine Runtime** | キャラアニメーション | キャラ実装時 |

### AI生成

| 素材 | 生成ツール | 用途 |
|------|----------|------|
| 画面イメージ（各状態） | ChatGPT / Midjourney | UIの方向性確認 → 実装の参考画像 |
| キャラ立ち絵 | Midjourney / PixAI | 仮素材 → 後にSpine用パーツ分解 |
| 神獣デザイン（ちび/最大） | Midjourney | コンセプト → パーツ化 |
| 背景 | Stable Diffusion | 状態ごとの環境ビジュアル |
| エフェクト用テクスチャ | Midjourney | パーティクル素材 |
| BGM | Suno / Udio | 状態ごとのBGM（8トラック） |
| SE | AI + Freesound | 確定音、デフレーションSE等 |
| C#スクリプト | Claude Code | Botエンジン、API、状態制御 |

### フリー素材

| ソース | 種類 |
|--------|------|
| Kenney | UIパーツ、アイコン |
| itch.io | エフェクト、BGM素材 |
| OpenGameArt | SE、環境音 |
| Freesound | 効果音ライブラリ |

---

## Step 3: Unityプロジェクト構築

### 3-1: プロジェクト作成 + ミドルウェア導入

```
Unity 2022 LTS（モバイル向け）
├── ターゲット: iOS + Android
├── 画面: Portrait固定, 393x852参照
├── URP（Universal Render Pipeline）
│
├── Plugins/
│   ├── DOTween Pro ← 最初に入れる
│   ├── PlayMaker ← 状態マシン用
│   └── TextMeshPro（Unity内蔵）
│
├── Scripts/
│   ├── API/           ← Binance REST APIクライアント
│   ├── BotEngine/     ← RSI/MACD/SMA/ATR計算、売買ロジック
│   ├── StateEngine/   ← σ計算、8状態マシン
│   ├── Presentation/  ← 演出制御、エフェクトトリガー
│   ├── UI/            ← 画面レイアウト、ボタン、チャート
│   └── Audio/         ← BGM管理、SE再生
│
├── Art/
│   ├── Characters/    ← Bot別スプライト
│   ├── Backgrounds/   ← 状態別背景
│   ├── Effects/       ← パーティクルプリファブ
│   └── UI/            ← フレーム、ボタン、インジケーター
│
├── Audio/
│   ├── BGM/           ← 状態別BGM
│   └── SFX/           ← SE、確定音
│
└── Animations/        ← DOTweenシーケンス、Animator
```

### 3-2: データパイプライン（Binance → Bot → 状態）

```
BinanceClient.cs
  └── REST API でOHLCV取得（5m / 15m / 1h）
  └── 現在価格ティッカー

TechnicalIndicators.cs
  └── RSI, MACD, SMA, ATR 計算（純粋な数学）

BotEngine.cs
  └── Bot定義（ScriptableObject）
  └── エントリー/エグジット判定
  └── ポジション管理、PnL計算

SigmaCalculator.cs
  └── ローリング48本のlog returnの標準偏差
  └── 3つの独立窓（5m, 15m, 1h）

MarketStateManager.cs (PlayMaker or C#)
  └── σ閾値 → 8状態判定
  └── 遷移ルール（段階的上昇、即時S8）
  └── S8トリガー（50%リトレース検知）
```

### 3-3: 演出レイヤー（楽譜の実装）

```
PresentationController.cs
  └── 状態変化を受け取る
  └── 演出シーケンス（Step 1の楽譜）をDOTweenで実行
  └── Layer別に制御:
      ├── BackgroundController  ← 背景の色・天候
      ├── CharacterController   ← キャラポーズ、神獣サイズ
      ├── EffectController      ← パーティクル、フラッシュ、シェイク
      ├── UIController          ← 価格表示、テキスト、ゲージ
      └── AudioController       ← BGM遷移、SE発火
```

### 3-4: UI画面

Step 1の演出シーケンスとAI生成画像を元に構築。
Canvas + UI要素で組む。具体的なレイアウトは画像イメージ確定後に決定。

---

## Step 4: 音を入れる

パチンコは「音が7割」。

### 必要な音素材（ADR-0007 Part 7より）

| カテゴリ | 数 | 調達方法 |
|---------|-----|---------|
| 状態別BGM | 8トラック（S1〜S8） | AI生成（Suno/Udio）+ 手調整 |
| 確定音 | 1（ゲーム最重要の音） | 専用制作 or AI + 手調整 |
| 状態遷移SE | 7（各遷移ごと） | Freesound + AI |
| Bot固有シグネチャ | 8（キャラごとの音テーマ） | AI生成 |
| 環境音 | 4（天候別アンビエント） | Freesound |
| UIのSE | 10前後（ボタン、通知等） | フリー素材 |
| S8デフレーションSE | 1（「しゅぅぅ...」） | AI or 手制作 |
| ちび神獣の鳴き声 | 8 | AI生成 |
| **合計** | **約45-50** | |

### 音の導入順

1. まずフリー素材の仮BGM/SEで全状態を通す
2. 体験を確認した上で、重要なものから差し替え
3. 確定音は早めに作り込む（条件付けの起点）
4. CRIWARE導入はフレーム精密同期が必要になった段階

---

## Step 5: キャラクターを作り込む

### Spine導入

パーツ分け式アニメーションで、以下を実現：
- ちびマスコット（S1）→ 巨大化（S7）のサイズ遷移
- 待機、警戒、戦闘、必殺技、勝利、敗北、S8リアクション
- 各キャラの個性を動きで表現

### 制作フロー

```
AI生成（コンセプト）
  → 手修正（統一感）
  → パーツ分解（頭/胴/腕/武器/神獣を別レイヤー）
  → Spine Editor でリグ作成
  → Unity に読み込み
  → PresentationController から制御
```

### キャラ1体あたりの必要アニメ

| アニメ | 用途 |
|--------|------|
| idle | S1待機 |
| alert | S2警戒 |
| ready | S3戦闘態勢 |
| battle | S4戦闘 |
| excited | S5興奮 |
| awakening | S6覚醒 |
| ultimate | S7必殺技 |
| fizzle | S8尻すぼみリアクション |
| victory | 勝利ポーズ |
| defeat | 敗北 |

× 8キャラ = 80アニメスロット

---

## Step 6: プラットフォーム統合 + 製品化

| 項目 | 内容 |
|------|------|
| ランキング | GameCenter (iOS) / Google Play Games (Android) |
| 課金 | 通知サブスク（¥1,000/月）、スキンIAP |
| 通知 | ローカル通知（Bot がポジション取得時） |
| 永続化 | PlayerPrefs / JSON でトレード履歴保存 |
| SNS共有 | OGP画像生成、共有カード |
| 最適化 | モバイルパフォーマンス、バッテリー |
| チュートリアル | 初回起動フロー |

---

## 全体の流れ

```
Step 1: 演出シーケンス（楽譜）を書く
  ↓ 何が必要か分かる
Step 2: 素材を揃える（AI + Asset Store + フリー）
  ↓ 部品が揃う
Step 3: Unityで組む（データ → 状態 → 演出 → UI）
  ↓ 動くものができる
Step 4: 音を入れる（パチンコは音が7割）
  ↓ 体験が完成する
Step 5: キャラを作り込む（Spine + AI生成）
  ↓ 個性が出る
Step 6: プラットフォーム統合 → リリース
```

各Stepの中で、常に「素材→動き→制御→組み合わせ」の順で進める。
