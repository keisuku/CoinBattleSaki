# CoinBattleSaki UI画像生成プロンプト集

## 使い方

1. **Phase 1（本ドキュメント）**: 筐体デザイン — 枠・ボタン・画面比率を確定
2. Phase 2: カラースキーム確定後、各状態（S1〜S8）の液晶内演出
3. Phase 3: キャラクター立ち絵・神獣デザイン
4. Phase 4: 特殊演出画面（勝利・敗北・SNS共有カード）

**対象AI**: ChatGPT (DALL-E) → 最終版はMidjourney/PixAI等で精緻化

---

## Phase 1: 筐体デザイン（Cabinet Design）

### コンセプト

> スマートフォンの画面全体を「パチンコ/パチスロの筐体（きょうたい）」として扱う。
> 物理的なパチンコ台のように、装飾的なフレーム・光るボタン・メイン液晶エリアが
> デジタル空間上に再現される。ユーザーは「自分のスマホが筐体になる」体験をする。

### 設計制約

- 画面サイズ: iPhone 15 Pro想定（393 x 852px, 19.5:9）
- フルスクリーン（ステータスバー下まで使用）
- セーフエリア: 上部48px（Dynamic Island対応）、下部34px（Home Indicator対応）
- タッチ操作（物理ボタンなし。ただしボタンに3Dの立体感を持たせる）

---

### Prompt 1-A: 筐体フレーム全体図（カラー未定・構造重視）

```
Design a smartphone game UI mockup (iPhone 15 Pro, 393x852px portrait) that looks like a digital pachinko/pachislot machine cabinet.

The screen is divided into these zones from top to bottom:

1. TOP FRAME (approximately 8% of screen height):
   - Decorative header panel with metallic/chrome frame edges
   - Small data readouts embedded in the frame (like instrument gauges)
   - Subtle LED strip accent along the bottom edge of this frame

2. MAIN LCD AREA (approximately 65% of screen height):
   - The largest area, framed by a subtle beveled border
   - Currently showing a dark placeholder screen with a faint grid
   - The frame around the LCD has a premium, machined-metal look
   - Corner accents where the frame meets — small gem-like rivets or LED dots

3. LOWER INFO PANEL (approximately 12% of screen height):
   - A secondary display strip below the main LCD
   - Recessed into the cabinet frame, like a pachislot's credit display
   - Shows placeholder data bars and status indicators
   - Has its own mini-frame with subtle edge lighting

4. BUTTON ARRAY (approximately 15% of screen height):
   - 4 circular buttons arranged horizontally with even spacing
   - Each button has a 3D raised appearance with:
     - A glossy dome surface (like real pachinko buttons)
     - A metallic ring border
     - A subtle inner glow (currently dim/inactive)
     - A small label area below each button
   - One larger "MENU" button on the far right (rectangular, flat)
   - The button panel sits on a slightly textured surface

Overall aesthetic:
- Dark background (near-black) with metallic chrome/silver frame accents
- The cabinet frame should feel PHYSICAL — bevels, shadows, depth
- No color theme yet — keep it monochrome/silver/dark chrome
- Premium quality — think high-end pachislot machine like "CR Evangelion" or "Hokuto no Ken"
- The frame itself is decorative but not overwhelming — functional elegance
- All text placeholders in Japanese

Style: Hyper-realistic digital UI mockup, product design rendering, dark theme, metallic materials, 4K quality
```

---

### Prompt 1-B: 筐体フレーム — ボタン詳細図

```
Close-up detail view of a smartphone game UI's button panel, designed to look like physical pachinko/pachislot machine buttons rendered digitally.

Show 4 circular buttons + 1 rectangular menu button in a horizontal row:

Button 1 (leftmost):
- Circular, 52px diameter appearance
- Glossy dome surface with glass-like reflection
- Metallic chrome ring border (2px)
- Currently INACTIVE state: dim inner glow, muted color
- Label below: "ノーポジ" (No Position) in small text

Buttons 2-4 (character selection):
- Same circular design as Button 1
- Each has a tiny character icon/avatar silhouette in the center
- INACTIVE state: monochrome silhouette, dim glow
- One button shown in ACTIVE state:
  - Bright inner glow (color TBD, shown as green for example)
  - The chrome ring pulses with light
  - The character silhouette is illuminated
  - A subtle "selected" indicator (like a small LED dot above)
- Labels below: "SARAH", "EMMA", "JOHN"

Button 5 (rightmost, menu):
- Rectangular, smaller height than circular buttons
- Flat design with subtle bevel
- "MENU" text or hamburger icon
- Metallic finish, understated

The button panel surface:
- Slightly textured dark material (like soft-touch plastic or leather)
- Subtle edge lighting from above (spillover from the LCD)
- The buttons cast tiny shadows on the surface
- Overall feeling: you want to PRESS these buttons

Viewing angle: Slight 3/4 perspective to show the 3D depth of buttons
Style: Product design close-up, macro photography feel, premium materials, dark environment
```

---

### Prompt 1-C: 筐体 — カラーバリエーション比較（3案並列）

```
Three color scheme variations of the same smartphone pachinko-style game cabinet UI, shown side by side for comparison. Each is a full iPhone screen (portrait).

All three share the same layout:
- Top decorative frame (8%)
- Main LCD area (65%) showing a dark screen with faint chart lines
- Lower info panel (12%)
- Button array at bottom (15%) with 4 circular + 1 rectangular button

VARIATION A — "漆黒×ネオン" (Jet Black + Neon):
- Frame: Matte black with extremely subtle dark chrome edges
- Accent: Cyan/teal neon edge lighting along frame borders
- Buttons: Dark with cyan neon rings when active
- LCD border: Thin neon cyan line
- Feel: Cyberpunk, Bloomberg terminal meets arcade

VARIATION B — "黒金" (Black × Gold):
- Frame: Black with brushed gold accents and trim
- Accent: Warm gold LED strips, gold rivet details
- Buttons: Black with gold metallic rings, warm amber glow when active
- LCD border: Gold pinstripe frame
- Feel: Luxury, high-stakes gambling, VIP room

VARIATION C — "メタリック筐体" (Metallic Cabinet):
- Frame: Gunmetal/dark silver with visible machined texture
- Accent: White LED edge lighting, chrome highlights
- Buttons: Silver metallic with white-blue glow when active
- LCD border: Polished chrome frame with corner bolts
- Feel: Industrial premium, real pachislot machine translated to digital

Show all three at equal size with labels "A", "B", "C" below.
Background: Dark gradient.
Style: UI/UX design comparison mockup, product rendering quality
```

---

### Prompt 1-D: 筐体 — 状態変化による筐体エフェクト（S1 vs S5 vs S7）

```
Three states of the same smartphone pachinko game cabinet, showing how the CABINET FRAME ITSELF changes with game intensity. Same layout, same phone.

STATE S1 — "凪" (Dead Calm):
- Frame: Normal state. Subtle metallic finish, dim accents
- Edge lighting: OFF or barely visible
- Buttons: Dim, inactive glow
- LCD: Dark, calm content (placeholder)
- Overall: Quiet, elegant, waiting
- The cabinet is "asleep"

STATE S5 — "激アツ" (Burning Hot):
- Frame: The metallic frame is now radiating GOLD light
- Edge lighting: Bright gold LED strips pulsing along all frame edges
- Buttons: Active buttons glow intensely with gold halos
- LCD border: Gold energy crackling along the frame
- Corner accents: Bright, like they're overcharged
- Small particle effects (sparks) escaping from frame seams
- Overall: The cabinet itself is EXCITED — like a pachislot machine during a jackpot sequence
- The frame feels like it's barely containing the energy inside

STATE S7 — "臨界" (Critical Mass):
- Frame: RAINBOW/prismatic light flooding through every seam and edge
- Edge lighting: Rapidly cycling rainbow LED strips
- Buttons: All buttons blazing with white-hot glow
- LCD border: The frame is CRACKING — light pouring through fractures
- Energy tendrils/lightning escaping from the frame into the surrounding darkness
- The entire cabinet is vibrating (motion blur on edges)
- Overall: The cabinet is about to EXPLODE with energy
- Maximum visual overload — this is the rarest moment in the game

Show all three side by side with Japanese labels:
"S1: 凪", "S5: 激アツ", "S7: 臨界"
Style: Game UI concept art, special effects, energy/glow effects, dramatic lighting
```

---

### Prompt 1-E: メイン液晶エリア — 空の状態（コンテンツなし、枠のみ）

```
The main LCD area of a smartphone pachinko game cabinet, shown empty/idle to establish the screen space and frame design.

The LCD area occupies approximately 65% of the phone screen (the middle section).

Frame details:
- The LCD is recessed slightly into the cabinet frame (like a real screen bezel)
- A thin beveled border creates depth — the screen sits "inside" the frame
- Four corner pieces where the frame meets — decorative metallic accents
- The top edge of the LCD frame has a thin data strip showing:
  "BTC/USDT" (left) and "---" placeholder (right)
- The bottom edge has a thin gradient line separating LCD from lower panel

The empty LCD screen shows:
- Very dark background (#0a0a1a or similar)
- A faint grid overlay (3-5% opacity) suggesting a chart area
- A single horizontal dashed line at 40% height (future entry price line)
- Two faint horizontal zones:
  - Top 15%: barely visible green-tinted area (future TP zone)
  - Bottom 25%: barely visible red-tinted area (future SL zone)
- A small "waiting..." text in the center, very dim
- The screen has a subtle vignette effect at edges (darker corners)

The feeling should be:
- An empty stage waiting for action
- Like looking at a pachislot LCD between games
- The frame is the star — the content area is a dark canvas
- Premium, minimal, anticipatory

Aspect ratio of the LCD area itself: approximately 9:12 (portrait, slightly wider than it is tall)
Style: UI design mockup, dark theme, product rendering, subtle details
```

---

### Prompt 1-F: 下部インフォパネル — 詳細図

```
Close-up of the lower information panel of a smartphone pachinko game cabinet UI. This panel sits between the main LCD screen and the button array.

Panel dimensions: Full width of phone, approximately 12% of screen height.

The panel is a secondary display recessed into the cabinet frame:

LEFT SECTION (40% width):
- A small character portrait frame (48x48px circle)
  - Currently showing a placeholder silhouette
  - Surrounded by a thin metallic ring matching the character's color
  - Below: character name "SARAH" in small bold text
  - Below that: tiny text "15m / TREND" (timeframe and style)

CENTER SECTION (35% width):
- Three horizontal indicator bars stacked vertically:
  - "RSI" label + progress bar (currently at 50%, neutral gray)
  - "MACD" label + progress bar (currently at 30%, neutral gray)
  - "VOL" label + progress bar (currently at 45%, neutral gray)
  - Each bar has a thin metallic frame
  - When active, bars would glow with the character's color

RIGHT SECTION (25% width):
- A circular gauge/dial showing "Composite Score"
  - 0-100 scale
  - Currently at "---" (no position)
  - The gauge has a metallic bezel like a watch face
  - A small needle or arc indicator
  - Below: "STACK: 0%" text

The panel surface:
- Slightly different shade than the main frame (darker or subtly different material)
- Thin bright line separating it from the LCD above
- Thin line separating it from the button panel below
- Feels like the "instrument cluster" of the pachinko cabinet

Style: UI detail mockup, dashboard/instrument design, dark theme, metallic accents
```

---

### Prompt 1-G: 全体組み立て — 完成筐体（アイドル状態、SARAH選択中）

```
Complete assembled view of a smartphone pachinko-style game cabinet UI in idle state (S1: Dead Calm). Character SARAH is selected.

Full iPhone 15 Pro screen (393x852px portrait), showing all zones:

TOP FRAME (8%):
- Decorative metallic header
- Left: "BTC/USDT" in small metallic text
- Center: Small game logo placeholder
- Right: "$67,432.50" price in dim white text with "−0.02%" in gray
- Thin LED accent line at bottom edge (dim green, SARAH's color)

MAIN LCD (65%):
- Dark background with faint grid
- 15 small candlestick bars in the lower-center area (very calm, small movements)
- A dashed horizontal line (entry price)
- Faint green zone at top (TP), faint red zone at bottom (SL)
- In the lower-left of the LCD: A tiny chibi mascot (phoenix chick) sitting on a perch
  - Very small (the palm-sized S1 form)
  - Cute, round eyes, tiny green flame flickering
  - Occasionally blinking animation implied
- Small text bubble from the mascot: "...静かだな" (It's quiet...)
- The LCD feels calm, ambient, like watching a quiet aquarium

LOWER INFO PANEL (12%):
- SARAH's portrait circle (green border, glowing softly)
- Name: "SARAH 🟢" with "calm" emotion text below
- RSI/MACD/VOL bars at neutral levels, dim green
- Composite gauge: "32%" in dim display
- Everything muted and peaceful

BUTTON ARRAY (15%):
- Button 1: "ノーポジ" — inactive, dim
- Button 2: "SARAH" — ACTIVE, green glow ring, green inner light, character silhouette lit
- Button 3: "EMMA" — inactive, dim red tint barely visible
- Button 4: "JOHN" — inactive, dim blue tint barely visible
- MENU button: subtle, inactive

OVERALL ATMOSPHERE:
- The cabinet is in its resting state
- Beautiful but quiet — like a pachislot machine between players
- The only life is the tiny chibi phoenix and the slowly moving price ticker
- Frame accents have a very subtle green tint (SARAH's color influence)
- This is what the user sees 60-70% of the time — it must be gorgeous even when "nothing is happening"

Style: Complete game UI mockup, production quality, dark theme with subtle green accents, premium pachinko cabinet aesthetic, mobile game screenshot quality
```

---

## 補足: ChatGPTへの追加指示テンプレート

各プロンプトの前に以下を付けると精度が上がります:

```
You are a UI/UX designer specializing in Japanese mobile game interfaces,
particularly pachinko/pachislot-inspired digital designs.

Create a high-fidelity mockup image.
The design should feel like a premium Japanese pachislot machine (like CR Evangelion
or CR Hokuto no Ken series) translated into a smartphone app interface.

Important: This is a PORTRAIT mobile screen (9:19.5 aspect ratio).
All text should be in Japanese where applicable.
The overall quality should be production-ready concept art level.
```

---

## 次のステップ

1. **Prompt 1-A** を生成 → 筐体の基本構造を確認
2. **Prompt 1-C** を生成 → カラースキームを選択
3. 選択したカラーで **Prompt 1-G** を生成 → 完成形の確認
4. フィードバックを反映してイテレーション
5. Phase 2（液晶内コンテンツ）に進む
