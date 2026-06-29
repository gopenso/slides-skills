# 截圖美化語義規則

用於把使用者提供的產品截圖、網頁截圖、程式碼截圖、設計稿截圖處理成符合範本比例的圖片資產。目標是類似 CleanShot X 的「截圖居中 + 背景填充 + 統一比例」，而不是預設讓 GPT-M 2.0 重畫截圖。

## 優先順序

1. **程式化適配優先**：截圖內容、文字、UI 細節需要保真時，不要重畫；建立目標比例畫布，把原截圖等比縮放後放入畫布。
2. **GPT-M 2.0 只做重構**：只有原圖過長、過窄、資訊太亂、需要 UI 情景化或概念化表達時，才使用「截圖再設計 / UI 情景圖」。
3. **範本槽位先行**：先確定 slide 版式和圖片槽位比例，再決定截圖適配引數。

## 開始前詢問

在主流程 Step 1 中，只要使用者可能提供截圖，就先問清楚：

- 截圖在哪個資料夾？是否包含網頁、App、程式碼、dashboard、設計稿或舊 PPT？
- 這批截圖要**保真展示**、**統一美化**、**重新設計成 UI 情景圖**，還是混合處理？
- 最終要放進哪些槽位：21:9 頂圖、16:10 主圖、4:3 側圖、1:1 方圖、還是多圖網格？
- 是否必須保留所有文字和資料？是否需要隱藏帳號、頭像、專案名等敏感資訊？
- 構圖希望居中、左上、右下，還是根據頁面內容自動判斷？

如果在 Claude Code 中，用 Ask Question / `ask_question` 做這些澄清；如果在 Codex 中，用普通對話詢問，不要呼叫 Ask Question。

## 處理鏈路

1. **先相符版式**：根據內容選取範本 layout，確定截圖槽位尺寸和比例。
2. **再選處理方式**：
   - 要保真：程式化適配，不重畫截圖。
   - 要統一視覺但不改內容：程式化適配 + 主題背景。
   - 原圖不可用或需要解釋概念：再走 GPT-M 2.0 截圖再設計。
3. **再選取背景**：優先使用內建背景資產，不應該每張截圖臨時產生一種風格。
4. **最後合成截圖**：建立目標比例畫布，背景 cover 鋪滿，截圖等比縮放後按 `padding` 和 `alignment` 放入。

預設不要裁掉截圖內容。只有截圖已經按目標槽位重新產生，或者使用者明確允許裁切時，才使用 cover 裁切。

## 語義引數

每次處理截圖前，先確定這 7 個引數：

| 引數 | 可選值 | 判斷方式 |
|---|---|---|
| `ratio` | `21:9` / `16:10` / `16:9` / `4:3` / `1:1` | 跟隨範本圖片槽位，不要跟隨原截圖比例 |
| `background` | `plain` / `gradient` / `wallpaper` / `blurred` / `grid` / `paper` | 跟隨當前 PPT 風格和主題 |
| `padding` | `compact` / `standard` / `spacious` | 普通截圖 standard；文字密集或高截圖 spacious；小圖組 compact |
| `inset` | `none` / `subtle` / `balanced` | 截圖需要從背景中浮出來時用 balanced；瑞士風多用 none/subtle |
| `shadow` | `none` / `soft` / `editorial` | Style A 可 soft/editorial；Style B 預設 none |
| `corners` | `square` / `small` / `medium` | Style B square；Style A small/medium |
| `alignment` | `center` / `top-left` / `top-right` / `bottom-left` / `bottom-right` | 跟隨頁面構圖，不是永遠居中 |

## 風格對映

### Style A · 電子雜誌風

- 背景：`paper` / `blurred` / 低飽和 `gradient`
- 質感：紙張、墨水、膠片顆粒、暖白、低對比
- 截圖：可用小圓角和輕微陰影，但不要像 SaaS 營銷卡片
- 背景資產：優先使用 `assets/screenshot-backgrounds/style-a/` 下對應主題的 16:9 crop-safe WebP，截圖合成時按槽位裁切
- 推薦語義：

```text
ratio:16:10, background:paper, padding:standard, inset:balanced, shadow:editorial, corners:small, alignment:center
```

### Style B · 瑞士國際主義

- 背景：`plain` / `grid` / `dot-matrix`
- 色彩：只允許當前錨點色作為極低佔比強調；不要大面積亮色塊
- 截圖：直角、無陰影、無圓角、少量 hairline 或頂部 accent 線
- 背景資產：優先使用 `assets/screenshot-backgrounds/style-b/` 下對應主題色的 16:9 crop-safe WebP，只用當前 accent，不要混色
- 推薦語義：

```text
ratio:21:9, background:grid, padding:standard, inset:subtle, shadow:none, corners:square, alignment:center
```

## 背景強度規則

截圖背景是「託底」，不是主視覺。

- 如果 `alignment` 不確定，背景中心和四角都必須安靜，不要放顯眼色塊。
- 如果截圖要放在右下角，右下角不能有強色塊；其他位置同理。
- 瑞士風錨點色只做 `5%-8%` 視覺佔比的淡線、點陣或極淺幾何場，不要產生高亮藍條、大色塊、霓虹漸層。
- 背景不能有文字、logo、圖示、人物、裝置、邊框、明顯主體或方向性構圖。
- 背景必須 crop-safe：裁成 `21:9`、`16:10`、`4:3`、`1:1` 都不能暴露「被裁掉」的痕跡。

## 內建主題背景資產

本 Skill 已經內建一組 GPT-M 2.0 預產生背景。處理截圖時**優先使用這些資產**，不要實時呼叫 GPT-M 2.0 重新產生背景。只有使用者明確要求新風格、現有主題缺失，或背景與內容明顯不相符時，才產生新的背景。

背景圖之後由程式複用到每張截圖中。不要把背景當作單張 slide 來畫，背景圖內部不能有標題、頁尾、邊框、logo、人物或明顯主體。

### Style A · 5 套主題背景

| 主題 | 內建資產 | 背景語義 |
|---|---|---|
| 墨水經典 | `assets/screenshot-backgrounds/style-a/monocle-classic.webp` | 黑白灰紙張紋理、柔和陰影、細顆粒 |
| 靛藍瓷 | `assets/screenshot-backgrounds/style-a/indigo-porcelain.webp` | 靛藍低飽和墨色、紙感漸層、輕微噪點 |
| 森林墨 | `assets/screenshot-backgrounds/style-a/forest-ink.webp` | 模糊植物陰影、低飽和綠色、紙張顆粒 |
| 牛皮紙 | `assets/screenshot-backgrounds/style-a/kraft-paper.webp` | 暖紙色、淡墨陰影、復古印刷顆粒 |
| 沙丘 | `assets/screenshot-backgrounds/style-a/dune.webp` | 沙色/灰調柔和漸層、低對比、留白安靜 |

### Style B · 4 套主題背景

| 主題色 | 內建資產 | 背景語義 |
|---|---|---|
| IKB 藍 | `assets/screenshot-backgrounds/style-b/ikb-dot-gradient.webp` | 點陣 + 低對比藍色漸層，避免亮藍大色塊 |
| 檸檬黃 | `assets/screenshot-backgrounds/style-b/lemon-grid.webp` | 純網格 + 稀疏點陣，黃色只做低透明細線/點 |
| 檸檬綠 | `assets/screenshot-backgrounds/style-b/lemon-green-dot-shadow.webp` | 點陣 + 陰影場，綠色只做輕微光感 |
| 安全橙 | `assets/screenshot-backgrounds/style-b/safety-orange-halftone.webp` | 模組化半調點陣 + 暗部陰影，橙色低佔比 |

內建背景都是 1920×1080 級別的 16:9 WebP。程式化合成時，先把背景 cover 到目標畫布，再裁成 `21:9` / `16:10` / `4:3` / `1:1` 等截圖槽位。背景必須四角安靜，因為截圖可能居中、左上、右下或被裁成不同尺寸。

## 截圖型別決策

| 原始素材 | 推薦處理 |
|---|---|
| 普通網頁 / App / 桌面截圖 | 程式化適配到目標比例 |
| 產品 UI 細節很重要 | 程式化適配，使用 `fit-contain`，不重畫 |
| 長網頁截圖 | 截關鍵區域或拆成 2-3 張同尺寸面板 |
| 極窄 / 極高截圖 | 先嚐試 `spacious + side alignment`；仍太小時再重構 |
| 程式碼截圖 | Style A 用紙感背景；Style B 用淺網格背景；文字必須可讀 |
| 概念解釋用的 UI 情景圖 | 可以 GPT-M 2.0 重新設計 |

## 產生背景圖提示詞

只有需要新增背景資產時才使用本節。常規截圖美化不要實時產生背景，直接使用上方內建資產。

### Style A 背景

```text
16:9 crop-safe screenshot background for an editorial magazine / e-ink PPT system. Warm off-white paper texture, subtle ink wash, fine film grain, low contrast, quiet center and quiet corners, no text, no logo, no objects, no border, no focal subject. Suitable for cropping to 21:9, 16:10, 4:3, or 1:1.
```

### Style B 背景

```text
16:9 crop-safe screenshot background for a Swiss International Style PPT system. Pure off-white base, ultra-subtle 16-column grid and sparse dot matrix, one accent color only: [theme color], used at very low opacity as thin lines or tiny dots, no large bright color blocks. Quiet center and quiet corners, no text, no logo, no objects, no border, no focal subject. Suitable for cropping to 21:9, 16:10, 4:3, or 1:1.
```
