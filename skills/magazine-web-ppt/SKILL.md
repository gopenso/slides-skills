---
name: slides-skills
description: 產生橫向翻頁網頁 PPT（單 HTML 檔案），含 WebGL 背景、章節幕封、資料大字報、圖片網格等範本。提供兩種風格：① "電子雜誌 × 電子墨水"（襯線 + 流體背景 + 暖色） ② "瑞士國際主義"（無襯線 + 網格點陣 + IKB/檸檬黃/檸檬綠/安全橙高亮）。當使用者需要製作分享 / 演講 / 釋出會風格的網頁 PPT，或提到"雜誌風 PPT"、"瑞士風 PPT"、"Swiss Style"、"horizontal swipe deck"時使用。
---

# Magazine Web Ppt

## 這個 Skill 做什麼

產生一份**單檔案 HTML**的橫向翻頁 PPT，提供兩種可選的視覺基調：

### 風格 A · 電子雜誌 × 電子墨水（預設）

- **WebGL 流體 / 等高線 / 色散背景**（hero 頁可見）
- **襯線標題（Noto Serif TC + Playfair Display）+ 非襯線正文 + 等寬後設資料**
- 適合：人文分享、行業觀察、商業釋出、需要"雜誌感"的演講
- 範本：`assets/template.html` · 主題色：`references/themes.md` · 佈局：`references/layouts.md`
- 美學錨點：像 *Monocle* 雜誌貼上了程式碼

### 風格 B · 瑞士國際主義（Swiss Style）

- **WebGL 極細網格 + 點陣背景**（資訊驅動程式設計）
- **全程無襯線（Inter + Helvetica + Noto Sans TC）+ 極致字號對比**
- **高反差功能色**：克萊因藍 IKB / 檸檬黃 / 檸檬綠 / 安全橙（四選一）
- 適合：科技產品、資料彙報、設計/工程領域分享、年度總結
- 範本：`assets/template-swiss.html` · 主題色：`references/themes-swiss.md` · 佈局：`references/layouts-swiss.md`
- 美學錨點：像 Massimo Vignelli + Helvetica Forever

**兩種風格共用**：橫向翻頁（鍵盤 ← →、滾輪、觸屏、ESC 索引）、Lucide 圖示、Motion One 入場動效（本地 + CDN 雙保險）。

## 何時使用

**合適的場景**：
- 線下分享 / 行業內部講話 / 私人分享會
- AI 新產品釋出 / demo day
- 帶有強烈個人風格的演講
- 需要"一次做完，不用翻頁工具"的網頁版 slides

**不合適的場景**：
- 大段表格資料、圖表疊加（用常規 PPT）
- 訓練課件（資訊密度不夠）
- 需要多人協作編輯（這是靜態 HTML）

## 工作流

### Step 1 · 需求澄清（**動手前必做**）

**如果使用者已經給了完整的大綱 + 圖片/截圖處理要求**，可以跳過直接進 Step 2。

**如果使用者只給了主題或一個模糊想法**，用這 7 個問題逐個對齊後再動手。不要基於猜測就開始寫 slide——一旦結構定錯，後期翻修代價很高：

#### 執行環境適配

- **在 Claude Code 中**：透過 Ask Question / `ask_question` 做逐項澄清，優先把風格、受眾、素材、截圖需求這些會影響版式的輸入問清楚。
- **在 Codex 中**：用普通對話直接詢問使用者，不要呼叫 Claude Code 的 Ask Question / `ask_question` 機制，也不要假設這些工具可用。一次最多問 1-3 個最關鍵問題；如果資訊缺口不影響開工，先做合理假設並在回覆裡說明。

#### 7 問澄清清單

| # | 問題 | 為什麼要問 |
|---|---|------|-----------|
| 1 | **風格 A 還是 B？**（電子雜誌風 / 瑞士國際主義風） | **必須先問**，決定用哪個 template + layouts + themes 檔案 |
| 2 | **受眾是誰？分享場景？**（行業內部 / 商業釋出 / demo day / 私人分享會） | 決定語言風格和深度 |
| 3 | **分享時長？** | 15 分鐘 ≈ 10 頁，30 分鐘 ≈ 20 頁，45 分鐘 ≈ 25-30 頁 |
| 4 | **有沒有原始素材？**（文件 / 資料 / 舊 PPT / 文章連結） | 有素材就基於素材，沒有就幫他搭 |
| 5 | **有沒有圖片或截圖？希望怎麼處理？** | 決定圖文版式、圖片槽位、截圖是否需要 CleanShot X 式適配或 GPT-M 2.0 重構 |
| 6 | **想要哪套主題色？** | 雜誌風 5 套（`themes.md`） / 瑞士風 4 套（`themes-swiss.md`），挑一 |
| 7 | **有沒有硬約束？**（必須包含 XX 資料 / 不能出現 YY） | 避免返工 |

#### 風格選取參考（問題 1）

| 如果使用者說... | 推薦風格 |
|---|---|---|
| "雜誌感" / "人文" / "Monocle 風" / 不指定 | **A · 電子雜誌風** |
| "瑞士風" / "Swiss Style" / "Helvetica" / "極簡" / "網格" / "資訊圖" / "資料驅動程式" | **B · 瑞士國際主義風** |
| 內容是 AI 產品 / 技術 / 工程 / 資料彙報 | B 更合適 |
| 內容是行業觀察 / 人文 / 故事 / 文化 | A 更合適 |
| 使用者給了大量 KPI 數字 / 路線圖 / 流程 | B 更合適（`Data Hero` 佈局是瑞士風專長） |
| 使用者給了大量紀實照片 / 人文圖片 | A 更合適（圖片網格、左文右圖是雜誌風專長） |
| 使用者需要 GPT-M 2.0 產生截圖再設計 / 資訊圖 / 證據牆 | B 也很合適（S22 主圖、S15/S16 圖片網格可以承載證據圖） |

#### 大綱協助（如果使用者沒有大綱）

用"敘事弧"範本搭骨架，再填內容：

```
鉤子（Hook）       → 1 頁   ： 拋一個反差 / 問題 / 硬資料讓人停下來
定調（Context）    → 1-2 頁 ： 說明背景 / 你是誰 / 為什麼講這個
主體（Core）       → 3-5 頁 ： 核心內容，用 Layout 4/5/6/9/10 穿插
轉折（Shift）      → 1 頁   ： 打破預期 / 提出新觀點
收束（Takeaway）   → 1-2 頁 ： 金句 / 懸念問題 / 行動建議
```

敘事弧 + 頁數規劃 + 主題節奏表（見 `layouts.md`），**三張表對齊後**再進 Step 2。

大綱建議儲存為 `專案記錄.md` 或 `大綱-v1.md`，便於後續迭代。

#### 圖片約定（告知使用者）

在動手前向使用者說清：

- **資料夾位置**：`專案/XXX/ppt/images/` 下（和 `index.html` 同級）
- **命名規範**：`{頁號}-{語義}.{ext}`，例如 `01-cover.jpg` / `03-figma.jpg` / `05-dashboard.png`
  - 頁號補零便於排序
  - 語義用英文，短、具體、和內容對應
- **規格建議**：
  - 單張 ≥ 1600px 寬（避免大屏模糊）
  - JPG 用於照片/截圖，PNG 用於透明 UI/圖表
  - 總大小控制在 10MB 內（影響翻頁流暢度）
- **如何取代**：保持**同名覆蓋**最穩（HTML 裡不用改路徑）；如果檔名變了，記得全域性搜 `images/舊名` 改成新名
- **沒圖怎麼辦**：和使用者對齊，可以先用佔位色塊產生結構，等圖片後期補；但要告知 layout 4/5/10 等圖文混排頁沒圖就沒法驗證視覺效果

#### 截圖需求約定（動手前必須問）

只要使用者提到產品截圖、網頁截圖、程式碼截圖、設計稿、dashboard、舊 PPT 截圖或"幫我美化截圖"，都要先確認：

- **截圖位置**：截圖檔案在哪個資料夾？是否已經命名好？
- **使用目的**：保真展示 / 截圖美化 / 截圖再設計 / UI 情景圖？
- **落位比例**：最終放進哪個版式槽位？常用 `21:9` / `16:10` / `16:9` / `4:3` / `1:1`
- **內容要求**：是否必須保留全部文字、品牌、資料？是否有敏感資訊要遮擋？
- **視覺處理**：是否需要主題背景、留邊、居中/角落對齊、拆成長截圖面板？

預設策略：先讓內容適配範本，再處理圖片比例。截圖需要保真時，先讀 `references/screenshot-framing.md`，優先使用 `assets/screenshot-backgrounds/` 的內建背景資產做程式化 CleanShot X 式背景畫布適配；只有原截圖太亂、太長、太窄或需要概念化表達時，才用 GPT-M 2.0 做截圖再設計。

#### Codex 配圖產生（可選）

如果當前執行環境是 **Codex**，完成 deck 初稿後，主動問使用者是否需要用 GPT-M 2.0 產生配圖並插入 PPT。不要預設產生。

推薦詢問方式：

> 要不要為這份 PPT 產生幾張配圖？可以做成人文紀實照片、雜誌風資訊圖、流程/對比/系統關係圖，或把截圖再設計成統一的雜誌風視覺。

如果使用者確認產生，再問他想要哪種圖片型別或風格；如果使用者沒有偏好，根據頁面內容自行推薦 1-3 張最值得產生的配圖。

如果使用者提供的是截圖，先判斷是**截圖美化**還是**截圖再設計**：

- 截圖美化：讀 `references/screenshot-framing.md`，用內建主題背景 + 程式化縮放/留邊/對齊處理，儘量不重畫截圖內容
- 截圖再設計：讀 `references/image-prompts.md`，按當前版式槽位產生目標比例圖片，並保持語言、主題色和邊距一致

產生配圖時遵守：

- 提示詞保持簡短，只框定主題、用途、風格和比例，不要寫長篇攝影指導
- 圖片風格必須貼合當前 deck 風格：風格 A 用「電子雜誌 × 電子墨水」；風格 B 用「瑞士國際主義 / Swiss Style」
- 資訊圖、圖表、截圖再設計裡的文字語言必須跟隨使用者正在使用的語言；中文 deck 用中文，英文 deck 用英文
- 先看 `references/image-prompts.md` 選取圖片型別和基礎提示詞
- 如果處理使用者原始截圖，先看 `references/screenshot-framing.md`：優先呼叫 `assets/screenshot-backgrounds/` 內建背景並程式化做 CleanShot X 式截圖適配，只有需要重構資訊時才用 GPT-M 2.0 重畫
- 配圖比例必須相符最終落位：主視覺 16:9，左文右圖 16:10 / 4:3，資訊圖 16:9 / 16:10，截圖再設計 16:10，圖文混排小圖 3:2 / 3:4，網格圖統一高度裁切
- 產生後的圖片放到 `images/` 下，命名遵守 `{頁號}-{語義}.{ext}`

### Step 2 · 複製範本

**根據 Step 1 選定的風格，複製對應的範本**到目標位置（通常是 `專案/XXX/ppt/index.html`），同時在同級建一個 `images/` 資料夾準備接圖片。

```bash
mkdir -p "專案/XXX/ppt/images"

# 風格 A · 電子雜誌風
cp "<SKILL_ROOT>/assets/template.html" "專案/XXX/ppt/index.html"

# 或 風格 B · 瑞士國際主義風
cp "<SKILL_ROOT>/assets/template-swiss.html" "專案/XXX/ppt/index.html"
```

兩個 `template*.html` 都是**完整可執行**的檔案——CSS、WebGL shader、翻頁 JS、字型/圖示 CDN 全已預設好，只有 `<!-- SLIDES_HERE -->` 佔位符等待你填充 slide 內容。

**注意**：風格 A 和 B **不能混用**。layouts.md 裡的類（如 `.h-hero` 襯線大標題、`.display-zh` 等）只在 template.html 有定義；layouts-swiss.md 裡的類（如 `.kpi-hero`、`.accent-block`、`.span-N`、`.dots` 等）只在 template-swiss.html 有定義。一份 deck 只能選一套。

#### 2.1 · 必改佔位符（**容易漏**）

複製後立刻改掉以下佔位符，否則瀏覽器 Tab 會顯示「[必填] 取代為 PPT 標題」這種尷尬文字：

| 位置 | 原始 | 需改為 |
|------|------|--------|
| `<title>` | `[必填] 取代為 PPT 標題 · Deck Title` | 實際 deck 標題（如 `一種新的工作方式 · Luke Wroblewski`） |

每次複製完 template.html 第一件事：grep 一下「[必填]」 確認全部取代完。

#### 2.2 · 選定主題色（5 套預設 · 不允許自定義）

本 skill **只允許從 5 套精心調配的預設裡選一套**，不接受使用者自定義 hex 值——色彩搭配錯了畫面瞬間變醜，保護美學比給自由更重要。

| # | 主題 | 適合 |
|---|---|------|
| 1 | 🖋 墨水經典 | 通用 / 商業釋出 / 不知道選啥的預設 |
| 2 | 🌊 靛藍瓷 | 科技 / 研究 / 資料 / 技術釋出會 |
| 3 | 🌿 森林墨 | 自然 / 可持續 / 文化 / 非虛構 |
| 4 | 🍂 牛皮紙 | 懷舊 / 人文 / 文學 / 獨立雜誌 |
| 5 | 🌙 沙丘 | 藝術 / 設計 / 創意 / 畫廊 |

**操作**：
1. 基於內容主題推薦一套，或直接問使用者選哪一套
2. 開啟 `references/themes.md`，找到對應主題的 `:root` 塊
3. **整體取代** `assets/template.html`（已複製版本）開頭 `:root{` 塊裡標有「主題色」註釋的那幾行（`--ink` / `--ink-rgb` / `--paper` / `--paper-rgb` / `--paper-tint` / `--ink-tint`）
4. 其他 CSS 都走 `var(--...)`，無需任何其他改動

**硬規則**：
- 一份 deck 只用一套主題，不要中途換色
- 不要接受使用者給的任意 hex 值——委婉拒絕並展示 5 套讓選
- 不要混搭（例如 ink 取墨水經典、paper 取沙丘）——會徹底違和

### Step 3 · 填充內容

#### 3.0 · 預檢：類名必須在範本的 `<style>` 裡有定義（**最重要**）

**這是所有產生問題的源頭**。layouts 骨架使用了很多類名，如果範本的 `<style>` 裡沒有對應定義，瀏覽器會 fallback 到預設樣式——大標題字型錯、卡片擠成一團、pipeline 糊成一行、圖片堆到頁面底部。

**兩種風格類名互不通用**（再次強調）：
- 風格 A 範本裡有 `h-hero`（襯線）、`stat-card`、`grid-2-7-5`、`frame` 等
- 風格 B 範本裡有 `h-hero`（無襯線）、`kpi-hero`、`accent-block`、`span-N`、`dots`、`grid-12` 等
- 同名 class 在兩個範本裡**視覺表現完全不同**（例：風格 A 的 `h-hero` 是 Noto Serif TC 襯線，風格 B 的 `h-hero` 是 Inter 無襯線）

**在寫任何 slide 程式碼之前：**

1. **先 Read 當前用的範本**（至少讀到 `<style>` 塊末尾）：
   - 風格 A → `assets/template.html`
   - 風格 B → `assets/template-swiss.html`
2. **對照對應 layouts 檔案的 Pre-flight 列表**，確認你要用的每個類都在 `<style>` 裡存在
3. 如果某個類缺失：**在範本的 `<style>` 裡補上**，不要在每個 slide 裡 inline 重寫
4. **範本是唯一的類名來源**——不要發明新類名，如需自定義用 `style="..."` inline

**風格 A 常見容易遺漏的類**：
`h-hero` / `h-xl` / `h-sub` / `h-md` / `lead` / `kicker` / `meta-row` / `stat-card` / `stat-label` / `stat-nb` / `stat-unit` / `stat-note` / `pipeline-section` / `pipeline-label` / `pipeline` / `step` / `step-nb` / `step-title` / `step-desc` / `grid-2-7-5` / `grid-2-6-6` / `grid-2-8-4` / `grid-3-3` / `grid-6` / `grid-3` / `grid-4` / `frame` / `frame-img` / `img-cap` / `callout` / `callout-src` / `chrome` / `foot`

**風格 B 常見容易遺漏的類**（2026-05 重構後）：
- 畫布：`canvas-card` / `chrome-min`
- 排版：`h-hero`（無襯線 7.4vw weight 200） / `h-statement`（9.6vw） / `h-xl` / `h-md` / `t-cat`（SemiBold 600 小標） / `t-meta`（mono uppercase） / `lead` / `num-mega` / `mono`
- 卡片（四類互斥）：`card-ink` / `card-accent` / `card-fill` / `card-outlined`
- 網格：`grid-12` / `grid-2-9` / `grid-2-9-5` / `span-N`
- 時間線：`timeline-v` + `tl-node` + `tl-axis` + `dot` / `timeline-h` + `tl-h-node` + `tl-h-axis`
- 圖表：`kpi-tower-row` + `bar-tower` / `h-bar-chart` + `bar-row` + `bar-fill` / `spec-bars` + `bar-vert`
- 裝飾：`dot-mat`（SVG mask 實心點）/ `ring-mat`（描邊圓）/ `cross-mat`（× 網格）/ `hr-hairline`
- 版式專屬：`cover-split` / `closing-split` / `duo-compare` + `vrule` / `manifesto-top` + `ink-banner-full` / `three-forces` / `loop-diagram` / `matrix-fill` + `matrix-cell` / `brief-grid` + `brief-card` / `system-diagram` / `why-now-grid` / `four-cards` / `stacked-ledger` + `ledger-row` / `tech-spec` / `image-hero` + `hero-img-wrap` + `hero-overlay-block` + `hero-stats`
- 圖片混排：`frame-img` / `fit-contain` / `r-21x9` / `r-16x9` / `r-16x10` / `h-22` / `h-26` / `swiss-img-split` / `swiss-img-grid` / `swiss-img-caption` / `swiss-keyline` / `swiss-lined`
- spacing token：`--sp-3`...`--sp-13`（8/12/16/24/32/40/48/64/80/96/160 px）

#### 3.0.5 · 規劃主題節奏（**和類預檢同等重要**）

**在挑佈局之前**，必須先列出每一頁的主題 class（`hero dark` / `hero light` / `light` / `dark`）並寫到文件或草稿裡對齊。詳細規則看 `references/layouts.md` 開頭的「主題節奏規劃」一節。

**強制規則**：

- 每頁 section 必須帶 `light` / `dark` / `hero light` / `hero dark` 之一，不要只寫 `hero`
- 連續 3 頁以上同主題 = 視覺疲勞，不允許
- 8 頁以上必須有 ≥1 個 `hero dark` + ≥1 個 `hero light`
- 整個 deck 不能只有 `light` 正文頁，必須有 `dark` 正文頁製造呼吸
- 每 3-4 頁插入 1 個 hero 頁（封面/幕封/問題/大引用）

**產生後自檢**：`grep 'class="slide' index.html` 列出所有主題，人工確認節奏合理再交付。

#### 3.1 · 挑佈局

**不要從零寫 slide**。開啟對應的 layouts 檔案，裡面有 10 種現成佈局骨架，每種都是完整可貼上的 `<section>` 程式碼塊。

**風格 A** → `references/layouts.md`：

| Layout | 用途 |
|---|---|
| 1. 開場封面 | 第 1 頁 |
| 2. 章節幕封 | 每幕開場 |
| 3. 資料大字報 | 拋硬資料 |
| 4. 左文右圖（Quote + Image） | 身份反差 / 故事 |
| 5. 圖片網格 | 多圖對比 / 截圖實證 |
| 6. 兩列流水線（Pipeline） | 工作流程 |
| 7. 懸念收束 / 問題頁 | 幕末 / 收尾 |
| 8. 大引用頁（Big Quote） | 襯線金句 / takeaway |
| 9. 並列對比（Before / After） | 舊模式 vs 新模式 |
| 10. 圖文混排（Lead Image + Side Text） | 資訊密集的圖文頁 |

**風格 B** → 先讀 `references/swiss-layout-lock.md`，再讀 `references/layouts-swiss.md`。

瑞士主題預設進入 **Swiss locked mode**：

- 正文頁只能使用原始參考 PPT 登記的 22 個版式 `S01-S22`；新增首頁/尾頁只能使用 Skill 明確提供的 `SWISS-COVER-ASCII` / `SWISS-CLOSING-ASCII`。
- 每個 `<section class="slide">` 必須寫 `data-layout="Sxx"`。沒有 `data-layout` 就視為未登記版式。
- 不允許臨時發明 `P23/P24`、`Swiss Image Split`、`Evidence Grid` 這類原始 22P 之外的正文結構，除非使用者明確要求實驗版式。
- 頂部中文標題預設靠左對齊、處在左上內容軸。不要把小標題放左列、大標題放右列，造成視覺居中；只有原始 statement/split 版式允許強中心敘事。
- SVG 只負責幾何圖形。不要在 SVG 裡寫文字標籤，所有標籤改用 HTML 網格/卡片/caption。
- 地理/歷史/城市路線/地點關係頁使用 `S08 + Swiss Map Component`：先讀 `references/swiss-map-component.md`，仍保留 `data-layout="S08"`。

原始 22 個正文版式如下：

| Layout | 用途 |
|---|---|
| S01 Index Cover | 原始索引封面 |
| S02 Vertical Timeline + KPI | 演化對比 / 年代變遷 |
| S03 Split Statement | 核心論點 / 左右分屏 |
| S04 Six Cells | 6 項概念定義 |
| S05 Three Layers | 三層架構 |
| S06 KPI Tower | 4 項資料視覺化高度差 |
| S07 H-Bar Chart | 5-10 項排名比較 |
| S08 Duo Compare | Before/After 對照 |
| S09 Dot Matrix Statement | 大引述 / statement |
| S10 Split Closing | 收束頁 |
| S11 Horizontal Timeline | 4-7 步流程 |
| S12 Manifesto + Ink Banner | 階段性結論 |
| S13 Three Forces | 3 個對等概念深化 |
| S14 Loop Form | 自學閉環 / 自動化 |
| S15 Matrix + Hero Stat | 8-12 項矩陣 + 總資料 |
| S16 Multi-card Brief | 6 項快訊小卡 |
| S17 System Diagram | 三層架構 / 生態地圖 |
| S18 Why Now | 三論點 + 資料支撐 |
| S19 Four Cards | 4 項等權特性 |
| S20 Stacked KPI Ledger | 縱向賬單資料 |
| S21 Tech Spec Sheet | 產品規格 / benchmark |
| S22 Image Hero | 21:9 頂圖 + 標題塊 + 三列 KPI |

**登記擴充**：`S08 + Swiss Map Component` 用於地點、人物住所、路線、城市關係。它不是新 layout，而是 S08 右側插槽的 MapLibre 地圖元件；必須按 `references/swiss-map-component.md` 的點位、連線、卡片和右上角縮放/拖動控制實作。

選對應 layout，粘過去，改文案和圖片路徑即可。**務必先完成 3.0 預檢**。

**風格 B 版式多樣性硬規則**：
- 7-8 頁 deck 至少使用 **6 個不同 S 編號版式**；10 頁以上至少使用 8 個不同版式。
- 如果使用者說"測試範本 / 看看效果 / 多一點版式"，必須覆蓋：一個封面、一個收尾、至少 1 個對比或時間線（S08/S11/S02）、至少 1 個結構圖（S14/S17/S15）、至少 1 個圖片版式（S22 或 S15/S16 圖片格改造）。
- 不允許連續 3 頁使用同一種主體結構，例如連續三頁 `head + grid + card`。
- 圖片頁不能偷懶發明新結構。2-3 張圖時，用 S15/S16 的原始網格骨架改造成圖片格；單張大圖用 S22。
- 開寫 HTML 前先列一張 `頁碼 → data-layout → 選用理由 → 圖片槽位` 草稿；交付前執行 `node <SKILL_ROOT>/scripts/validate-swiss-deck.mjs index.html`。

#### 3.2 · 圖片比例規範

永遠用**標準比例**，不要用原圖奇葩比例（如 `2592/1798`）：

| 場景 | 推薦比例 |
|------|---------|
| S22 頂部主圖 | **21:9**；照片關鍵主體放中央安全區 |
| S15/S16 多圖格 | 統一 21:9 或統一 16:10，不能混用 |
| 左文右圖 主圖（風格 A） | 16:10 或 4:3 + `max-height:56vh` |
| 圖片網格（風格 A） | **固定 `height:26vh`**，不用 aspect-ratio |
| 左小圖 + 右文字 | 1:1 或 3:2 |
| 全螢幕主視覺 | 16:9 + `max-height:64vh` |
| 圖文混排小插圖 | 3:2 或 3:4 |

**預設不要讓圖片 `align-self:end`**——會滑到頁面底部，很容易碰到分頁元件。用 grid 容器 + `align-items:start`（template 已預設）讓圖片貼頂即可；如果確實需要圖文靠下對齊，必須先控制圖片高度，再使用範本已有安全區類別 `.nav-safe-bottom` / `.nav-safe-bottom-tight`，不要讓最低處碰到分頁元件。

**風格 B 瑞士風額外規則**：
- 單張大圖用 S22；多圖測試用 S15/S16 的原始卡片網格改造，不要用未登記的 P23/P24
- 產生圖片前先寫 `data-image-slot`：例如 `s22-hero-21x9` / `s15-grid-21x9` / `s16-brief-21x9`
- S22 配圖預設產生 21:9，提示詞必須包含 `subject centered in the safe middle area`；照片容器用 `object-position:center 35%`，不要用 `top center`
- 圖片容器必須直角、無陰影、無圓角；預設背景用白色 `var(--paper)`，不要用灰底包白底資訊圖
- 白底 GPT 資訊圖/流程圖/UI 圖預設不要加外框描邊，不要隨手套 `.swiss-keyline`；需要強調時只用 `.swiss-lined` 的頂部 accent 線
- UI/資訊圖如果是使用者原始截圖或文字密集圖，才用 `.fit-contain`；如果已按 S15/S16 槽位重產生，必須用 `.frame-img.r-21x9` / `.frame-img.r-16x10` 鋪滿容器，不要固定 `height:18vh` 後把圖縮小
- 多圖同組必須統一圖片槽位、比例和高度，不能混用
- GPT-M 2.0 產生圖使用 `image-prompts.md` 的「風格 B：瑞士國際主義配圖規則」
- 任何圖片、caption、timeline label、footnote 的最低處都不能進入底部分頁區域；需要貼底時用 `.nav-safe-bottom` / `.nav-safe-bottom-tight`，不要手寫 `bottom:2vh`

#### 3.2.1 · 中文大標題字號分檔（風格 B 必做）

中文方塊字視覺面積大，不能直接套英文 hero 的 6.8-7vw。寫中文大標題前先分檔：

| 標題形態 | 推薦字號 |
|---|---|
| 1 行，≤ 8 箇中文字元 | `min(6.4vw,11.2vh)` |
| 2 行，每行≤ 8 箇中文字元 | `min(5.8vw,10.2vh)` |
| 2 行，任一行 9-12 箇中文字元 | `min(5.2vw,9.2vh)` |
| 3 行或更長 | 優先改寫標題；不得已用 `min(4.6vw,8.2vh)` |

如果標題擠佔了圖片或正文區域，先壓縮標題文案，再降字號；不要靠把下方內容推到底來硬塞。

#### 3.2.2 · 瑞士風簡報最小字號與字重階梯（風格 B 必做）

瑞士風用於投屏簡報時，小字不能按網頁註釋的 10-12px 寫。預設遵守以下下限：

| 文字型別 | 最小字號 |
|---|---|
| 正文段落 / 主要說明 | `18px` |
| 卡片描述 / 列表 / 時間線說明 / caption / 圖注 | `16px` |
| meta / kicker / mono label / 圖表標籤 | `14px` |

如果內容放不下，先刪減文案、拆成兩頁、換更適合的 Sxx 版式，不要把字號壓到 10/11/12/13px。尤其是中文 deck，不要為了塞三行解釋把 `body-sm`、caption、timeline label 改小。

**字號與字重階梯（瑞士風核心）** — "越大越細，越小越粗"不是感性描述，而是具體對映：

| 字號區間 | 推薦字重 | 典型場景 |
|---|---|---|
| ≥ 8vw | 200 (ExtraLight) | 封面大字、巨號 KPI、h-statement |
| 4-7.9vw | 200-300 | 章節標題（h-xl/h-xl-zh）、大編號 |
| 1.8-3.9vw | 300-400 | 中型標題、takeaway 標題（≈1.8vw）、中號數字 |
| 1-1.7vw / 16-20px | 400-500 | 正文段落、卡片描述、說明文字 |
| 13-15px（小字） | 500-600 | meta、kicker、角標、圖表標籤、caption 強調 |

**硬規則：**
- 同一頁內，字號越小的元素字重必須 ≥ 字號越大的元素（不允許 16px 正文用 300 而 1.8vw 標題用 500）
- 16px 左右的小字拒絕使用 weight 300（太細不可讀），最低 400，推薦 500
- 封面/IKB 反白大標題內強調字用 `italic + weight 300`，不要用 accent 色（藍壓藍看不見）

元件細節（字型、色彩、網格、圖示、callout、stat-card 等）在 `references/components.md`。

### Step 4 · 對照檢查清單自檢

產生完一定要開啟 `references/checklist.md`，逐項對照。裡面總結了**真實迭代過程中踩過的所有坑**，P0 級別的問題（emoji、圖片撐破、標題換行、字型分工）必須全部透過。

#### 4.0 · 不只看程式碼：必須開啟網頁做視覺核對

程式碼只能證明類名和結構存在，不能證明版式舒服。產生後必須開啟網頁逐頁看：

1. 同時開啟原始參考 PPT、當前範本或產生頁、測試 PPT；原始參考是 `assets/template-swiss.html`。
2. 截圖前等入場動效穩定（約 1-2 秒），不要把動畫中間態當成版式問題。
3. 先看視覺：大標題字重、標題與內容間距、圖片是否與正文對齊、圖片/說明是否碰到底部分頁元件。
4. 再看程式碼：確認該頁選用的版式與內容形狀相符，沒有把資料專用版式拿來講概念，也沒有把可選元件堆成裝飾。
5. 對照原始參考範本時，以實際頁面用法為準，不要只看 CSS helper 定義；原始頁面的大字實際多為 200/300，不要被 raw CSS 裡的 700/800/900 帶偏。
6. 如果頁面彆扭，先判斷是版式選錯、必選元件缺失、可選元件濫用，還是間距/安全區問題；不要直接靠加 margin 硬救。

#### 風格 A · 電子雜誌風必查

1. **大標題必須是襯線字型**——如果顯示成非襯線，99% 是 Step 3.0 預檢沒做，`h-hero` 類在 template.html 裡缺失
2. **圖片網格里只用 `height:Nvh`，不用 `aspect-ratio`**（會撐破）
3. **圖片不能堆到頁面底部**——不要用 `align-self:end`，用 grid + `align-items:start`（見 Step 3.2）
4. **圖片只能用標準比例**（16:10 / 4:3 / 3:2 / 1:1 / 16:9），不要複製原圖的奇葩比例
5. **中文大標題 ≤ 5 字且 `nowrap`**（避免 1 字 1 行）
6. **用 Lucide，不用 emoji**
7. **標題用襯線，正文用非襯線，後設資料用等寬**

#### 風格 B · 瑞士國際主義必查

1. **全程無襯線**——任何襯線字型出現都是錯的（檢查 `font-family` 沒用 `--serif` 類變數）
2. **只有一個 accent 色**——一份 deck 不能同時出現 IKB 藍 + 檸檬黃 + 安全橙等多個高亮色
3. **不允許漸層 / 陰影 / 圓角**——所有色塊直角純色，任何 `box-shadow` / `linear-gradient` / `border-radius` > 0 都要砍掉（rule 橫線除外）
4. **極致字號對比**——主標題與正文比例 ≥ 8:1
5. **大字號必須雙約束限高**——`font-size:min(Xvw, Yvh)`，只用 vw 在標準 16:9 屏會溢位（吸取 P15/P20/P22 教訓）
6. **大字字重 200**（ExtraLight）——字號越大越細，瑞士風靈魂；**禁止** 600/700/800 大字
7. **卡片填充型別互斥**——`card-ink` / `card-accent` / `card-fill` / `card-outlined` 四類**不能混用**（禁止"藍底+藍描邊"、"灰底+描邊"等）
8. **多卡並列時統一樣式**——3-12 張卡用同一類（優先 `card-fill` 灰底）；只突出一項時單獨換 `card-accent`，且**只允許一張**
9. **直角到底**——任何 `border-radius` 都不允許；裝飾用 8×8 直角小方塊，**不要** 9px 圓形點
10. **圖示用 lucide，不自己畫 SVG**——`<i data-lucide="name"></i>` + `lucide.createIcons()`，選稜角風格（避免圓胖）
11. **時間線對齊**——axis 列固定 12px + dot 絕對定位，**不要**用 grid `justify-self`（會與虛線錯位）
12. **章節級標題與內容間距 ≥ 9vh**——避免擁擠（吸取 P15/P16 教訓）
13. **每頁一個語義化動效 recipe**——不是統一 fade-up，數字 scale 彈入、bar scaleY 拉起、SVG stroke 描線、節點序列點亮等；**禁止**所有頁用同一個 generic 配方
14. **playSlide 入口 reveal 容器**——`[data-anim]` 容器先強制 opacity:1，recipe 內再用 motion `{opacity:[0,1]}` 覆蓋，否則有些頁會"看不見"
15. **ESC 索引頁可見性**——cloned slide 必須有 CSS override 讓 `[data-anim]` 在縮圖裡 opacity:1
16. **Helvetica/Inter 兜底中文字型**——Windows 使用者沒有"蘋方"，必須 fallback 到 `"Microsoft JhengHei UI", "Noto Sans TC"`
17. **字型粗細體例**：大字 200 / 正文 300 / `t-cat` SemiBold 600 / `t-meta` mono uppercase
18. **保留低功耗快速鍵**——右下角必須提示 `B 靜態`；按 `B` 轉場 `body.low-power`，停止 WebGL/ASCII canvas RAF 和 Motion 入場動畫
19. **裝飾元素嚴格在 grid 內**——bars 矩陣、點陣、ring-mat 不能貼邊或溢位頁面
20. **底部內容預留 nav 空間**——nav 在 ~97vh，內容收尾不要過 93vh（吸取 P22 KPI 大字溢底教訓）
21. **圖片容器直角無陰影**——`.frame-img` 不加 `border-radius` / `box-shadow`；邊界只用 hairline
22. **S15/S16/S22 圖片同組一致**——同一組圖片統一比例、高度、邊距、線條粗細；資訊圖/UI 圖加 `.fit-contain`
23. **元件角色要正確**——S15/S16 圖片格需要 caption 資訊錨點；S22 的 KPI/說明是必選；資料專用版式必須有真實資料，不能靠文案硬填
24. **通用/非通用版式要分清**——S03/S08/S11/S19 較通用；S06/S07/S20/S21/S22 是資料/案例專用；S14/S15/S17 是結構專用

### Step 5 · 本地預覽

直接在瀏覽器開啟 `index.html` 就行。macOS 下：

```bash
open "專案/XXX/ppt/index.html"
```

不需要本地伺服器。圖片走相對路徑 `images/xxx.png`。

### Step 6 · 迭代

根據使用者回饋修改——範本的 CSS 已經高度引數化，90% 的調整都是改 inline style（字號 `font-size:Xvw` / 高度 `height:Yvh` / 間距 `gap:Zvh`）。

---

## 資原始檔導覽

```
slides-skills/
├── SKILL.md                  ← 你正在讀
├── assets/
│   ├── template.html         ← 風格 A · 電子雜誌風範本（種子檔案）
│   ├── template-swiss.html   ← 風格 B · 瑞士國際主義風範本（種子檔案）
│   ├── screenshot-backgrounds/ ← 截圖美化內建背景（WebP）：style-a 5 套 / style-b 4 套
│   └── motion.min.js         ← Motion One 本地副本（離線兜底，約 64KB，共用）
├── scripts/
│   └── validate-swiss-deck.mjs ← 風格 B 靜態校驗：登記版式、圖片槽位、SVG 文字、標題對齊
└── references/
    ├── components.md         ← 元件手冊（字型、色、網格、圖示、callout、stat、pipeline、動效... 風格 A 適用）
    ├── layouts.md            ← 風格 A · 10 種頁面佈局骨架（可直接貼上，含動效標記）
    ├── swiss-layout-lock.md  ← 風格 B · 原始 22P 版式鎖，正文頁必須按這裡登記
    ├── layouts-swiss.md      ← 風格 B · 原始 22P 骨架說明 + 少量明確標註的實驗區
    ├── swiss-map-component.md ← 風格 B · S08 地圖擴充元件（MapLibre 點位/連線/卡片/控制）
    ├── themes.md             ← 風格 A · 5 套主題色預設（只能選不能自定義）
    ├── themes-swiss.md       ← 風格 B · 4 套瑞士風主題色預設（IKB / 檸檬黃 / 檸檬綠 / 安全橙）
    ├── image-prompts.md      ← GPT-M 2.0 配圖型別、比例和基礎提示詞
    ├── screenshot-framing.md ← CleanShot X 式截圖適配語義 + 內建背景資產對映
    └── checklist.md          ← 質量檢查清單（P0/P1/P2/P3 分級）
```

**載入順序建議**：
1. 先讀完 `SKILL.md`（這個檔案）瞭解整體
2. Step 1 需求澄清**第一問**先確定風格 A 還是 B，然後：
   - 風格 A：讀 `themes.md` 幫使用者選一套主題色
   - 風格 B：讀 `themes-swiss.md` 幫使用者選一套主題色
3. **動手前 Read 對應範本的 `<style>` 塊**——這是類名的唯一來源，缺類會導致整頁樣式崩
   - 風格 A → `assets/template.html`
   - 風格 B → `assets/template-swiss.html`
4. 讀對應的 layouts 檔案挑佈局：
   - 風格 A → `layouts.md`（頂部有 Pre-flight 類名清單、主題節奏規劃、動效 recipe 決策樹）
   - 風格 B → **先讀 `swiss-layout-lock.md`**，再讀 `layouts-swiss.md`；正文頁必須從 S01-S22 選取，每頁寫 `data-layout`
5. 如果風格 B 需要地點、路線、人物住所或城市關係地圖，讀 `swiss-map-component.md`
6. 如果在 Codex 中產生配圖，讀 `image-prompts.md` 挑圖片型別、比例和基礎提示詞；如果是使用者原始截圖，先讀 `screenshot-framing.md`，優先使用 `assets/screenshot-backgrounds/` 的內建背景資產
7. 細節調整時讀 `components.md` 查元件（含 Motion 動效系統章節，主要服務風格 A；風格 B 的元件細節在 `layouts-swiss.md` 附錄）
8. 產生後先執行 `node scripts/validate-swiss-deck.mjs path/to/index.html`，再讀 `checklist.md` 自檢

**動效相關**：範本已把 Motion One 的載入和 recipe 邏輯內嵌到底部 module script。你不需要改 JS，只需要按 `layouts.md` / `layouts-swiss.md` 的骨架在 HTML 里加 `data-anim` / `data-animate` 即可。離線簡報靠 `assets/motion.min.js`，斷網時自動降級為「無動畫但內容可讀」。風格 B 範本必須保留 `B` 鍵低功耗模式：轉場後停止 WebGL/ASCII canvas RAF，取消正在執行的 Web Animations，並把當前頁內容直接 reveal 到靜態最終態。

## 核心設計原則（哲學）

### 風格 A · 電子雜誌風（5 輪迭代總結）

> 違反其中任何一條，雜誌感都會垮。

1. **克制優於炫技** — WebGL 背景只在 hero 頁透出，普通頁幾乎看不見
2. **結構優於裝飾** — 不用陰影、不用浮動卡片、不用 padding box，一切資訊靠**大字號 + 字型對比 + 網格留白**
3. **內容層級由字號和字型共同定義** — 最大襯線 = 主標題，中襯線 = 副標，大非襯線 = lead，小非襯線 = body，等寬 = 後設資料
4. **圖片是第一公民** — 圖片只裁底部，保證頂部和左右完整；網格用 `height:Nvh` 固定，不要用 `aspect-ratio` 撐
5. **節奏靠 hero 頁** — hero 和 non-hero 交替，才不累眼睛
6. **術語統一** — Skills 就是 Skills，不要中英混合翻譯

### 風格 B · 瑞士國際主義風

> 違反其中任何一條，畫面瞬間從瑞士掉到 PowerPoint。

1. **單一錨點色** — 一份 deck 只用一個 accent，不允許多色高亮拼貼
2. **極致字號對比** — 主標題與正文比例 ≥ 8:1，KPI 必須是"Data Hero"（螢幕寬度的 18-22%）
3. **無襯線只此一家** — Inter / Helvetica / Noto Sans TC，任何襯線都是錯的
4. **直角純色** — 不允許漸層 / 陰影 / 圓角（rule 橫線除外）
5. **網格至上** — 所有元素吸附到 12-col grid，靠左對齊 + 大幅留白做非對稱美學
6. **Hairline 是手術刀** — 1px 的極細分割線就夠，不要加粗、不要加陰影
7. **點陣裝飾只在 hero 頁透出** — 正文頁保持純淨底色

## 參考作品

本 skill 的兩種風格分別參考了：

**風格 A · 電子雜誌風**：
- 歸藏 "一人公司：被 AI 摺疊的組織" 分享（2026-04-22，27 頁）
- *Monocle* 雜誌的版式
- YC 總裁 Garry Tan "Thin Harness, Fat Skills" 那篇部落格的 demo

**風格 B · 瑞士國際主義風**：
- Massimo Vignelli 的 NYC Subway / Unimark 系統
- *Helvetica Forever* 的字型設計語言
- Josef Müller-Brockmann 的網格系統經典著作
- 當代設計：Acne Studios / Off-White / IKEA / Beck Design

可以把它們當做風格錨點。
