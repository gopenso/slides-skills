# 元件參考 · Components

這是 `slides-skills` skill 的元件手冊。template.html 已經定義好了所有樣式，這裡只寫「這個元件長什麼樣、怎麼用」。

## 目錄

- [元件參考 · Components](#元件參考--components)
  - [目錄](#目錄)
  - [基礎 Slide 外殼](#基礎-slide-外殼)
  - [字型 Typography](#字型-typography)
  - [Chrome \& Foot](#chrome--foot)
  - [Callout 引用框](#callout-引用框)
  - [Stat 數字矩陣](#stat-數字矩陣)
  - [Platform 平台卡](#platform-平台卡)
  - [Rowline 表格行](#rowline-表格行)
  - [Pillar 支柱卡](#pillar-支柱卡)
  - [Tag \& Kicker](#tag--kicker)
  - [Figure 圖片框](#figure-圖片框)
    - [基礎結構](#基礎結構)
    - [關鍵約束（血淚經驗，不要違反）](#關鍵約束血淚經驗不要違反)
    - [Frame Caption 變體](#frame-caption-變體)
    - [圖片佔位（設計階段佔位符）](#圖片佔位設計階段佔位符)
  - [Icons 圖示](#icons-圖示)
  - [Ghost 巨型背景字](#ghost-巨型背景字)
  - [Highlight 熒游標記](#highlight-熒游標記)
  - [Motion 動效系統](#motion-動效系統)
    - [載入方式](#載入方式)
    - [資料屬性驅動程式](#資料屬性驅動程式)
    - [5 種 recipe 一覽](#5-種-recipe-一覽)
    - [給 slide 選 recipe 的決策樹](#給-slide-選-recipe-的決策樹)
    - [什麼元素該加 `data-anim`?](#什麼元素該加-data-anim)
    - [常見問題](#常見問題)

---

## 基礎 Slide 外殼

每一頁都是一個 `<section class="slide ...">`。必須包含 `data-theme` 屬性（`light` 或 `dark`），JS 翻頁時會根據這個屬性轉場背景。

```html
<section class="slide light" data-theme="light">   <!-- 淺色頁 -->
<section class="slide dark" data-theme="dark">     <!-- 深色頁 -->
<section class="slide light hero" data-theme="light">  <!-- Hero 頁：淺色 + 薄遮罩透出 WebGL -->
<section class="slide dark hero" data-theme="dark">    <!-- Hero 頁：深色 + 薄遮罩 -->
```

**light vs dark 的使用：交替使用**，每 2-3 頁轉場一次主題，避免連續超過 3 頁同色。翻頁時 WebGL 背景會自動在兩個 shader 之間漸層過渡。

**hero 類的使用**：只給視覺主導的頁面加（封面、金句頁、章節過渡、結尾）。加 `hero` 後遮罩降到 12-16%，WebGL 背景會大幅透出，所以不要在 hero 頁上放太多文字。

---

## 字型 Typography

字型分工是本範本最重要的規則，嚴禁混用。

| Class | 用途 | 字型 |
|---|---|---|
| `.display` | 超大號英文（Hero 頁） | Playfair Display 700, 11vw |
| `.display-zh` | 超大號中文標題 | Noto Serif TC 700, 7.8vw |
| `.h1-zh` | 頁面主標題 | Noto Serif TC 700, 4.6vw |
| `.h2-zh` | 副標題 | Noto Serif TC 600, 3.2vw |
| `.h3-zh` | 流水線步驟標題 | Noto Serif TC 500, 1.9vw |
| `.lead` | 引導段（比 body 大） | Noto Serif TC 400, 1.9vw |
| `.body-zh` | **正文/描述（非襯線）** | Noto Sans TC 400, 1.22vw |
| `.body-serif` | 正文（襯線） | Noto Serif TC 400, 1.3vw |
| `.kicker` | 小節提示（標題上方） | IBM Plex Mono, 12px uppercase |
| `.meta` | 元資訊標籤 | IBM Plex Mono, 0.88vw uppercase |
| `.big-num` | 巨型數字 | Playfair Display 800, 10vw |
| `.mid-num` | 中號數字 | Playfair Display 700, 5.5vw |

**核心規則**：
- **襯線**（`serif-zh` / `serif-en`）：標題、重點金句、數字 —— 用於「視覺重音」
- **非襯線**（`sans-zh`）：正文描述、大段閱讀內容 —— 用於「資訊密度」
- **等寬**（`mono`）：kicker、meta、foot 的英文標籤 —— 用於「裝飾節奏」

**強調技巧**：
- `<em class="en">英文詞</em>` —— 把英文詞渲染成 Playfair Display 斜體（很好看）
- `<em style="opacity:.65">短語</em>` —— 讓標題後半段淡出，製造節奏

---

## Chrome & Foot

每一頁的頂部和底部的元資訊條。幾乎所有頁都應該有。

```html
<div class="chrome">
  <div class="left">
    <span>第一幕 · 硬資料</span>
    <span class="sep"></span>
    <span>Act I</span>
  </div>
  <div class="right"><span>02 / 27</span></div>
</div>

<!-- ... 頁面主體 ... -->

<div class="foot">
  <div class="title">專案名 · CodePilot　|　github.com/codepilot</div>
  <div>Act I · Dev Numbers</div>
</div>
```

**規則**：
- `chrome.right` 總是放頁碼 `NN / TOTAL` （TOTAL 為總頁數）
- `foot.title` 是中文說明，`foot.right` 是英文 act 標記
- chrome 和 foot 共同構成雜誌感的「頁首頁尾」

---

## Callout 引用框

展示金句 / 關鍵觀點 / 他人引言。

```html
<div class="callout" style="max-width:80vw">
  <div class="q-big">「這東西在三年前，<br>需要一個十人團隊做一年。」</div>
  <span class="cite">— 一個觀察者的判斷</span>
</div>
```

變體：
- 不帶 cite：去掉 `<span class="cite">` 即可
- 帶英文金句：`<em class="en">"Thin Harness, Fat Skills."</em>`
- 在 hero 頁使用：外層加 `style="position:relative;z-index:2"`（避免被背景遮罩蓋住）

---

## Stat 數字矩陣

展示資料指標，常與 `.grid-6` / `.grid-4` 配合。

```html
<div class="grid-6">
  <div class="stat">
    <span class="m">Duration</span>
    <span class="n">64<em style="font-size:.4em;opacity:.5;font-style:normal"> 天</em></span>
    <span class="l">從 0 到現在</span>
  </div>
  <!-- ... 更多 stat ... -->
</div>
```

三段式結構：`.m` 等寬小標籤 → `.n` 巨型數字 → `.l` 描述說明。數字後的單位用 `<em>` 縮小到 0.4em，opacity 0.5。

**常用佈局容器**：
- `.grid-6` — 3×2 網格（最常用，6 個 stat）
- `.grid-4` — 2×2 網格（4 個 stat）
- `.grid-3` — 3 等分單行（3 個 stat / pillar）

---

## Platform 平台卡

展示社交平台 / 渠道 + 追蹤者數。

```html
<div class="plat">
  <div class="sub">FB</div>
  <div class="name">臉書</div>
  <div class="nb">289K</div>
</div>
```

可選第四行（補充說明）：
```html
<div class="body-zh" style="font-size:max(11px,.8vw);opacity:.5;margin-top:.6vh">
  含 IG 同步
</div>
```

**"Also On" 變體**（補充平台）：
```html
<div class="plat" style="border-top-style:dashed;opacity:.72">
  <div class="sub">Also On</div>
  <div class="body-zh" style="font-weight:600;margin-top:.8vh">
    Youtube　·　Blog
  </div>
</div>
```

---

## Rowline 表格行

列表式內容，每行一個條目。

```html
<div class="rowline">
  <div class="k">CLAUDE.md</div>
  <div class="v">你該怎麼做事 —— 行為規則 + 工作偏好 + 禁止事項</div>
  <div class="m">EMPLOYEE · HANDBOOK</div>
</div>
```

三列結構：`.k` 襯線關鍵詞 · `.v` 正文描述 · `.m` 等寬標籤（靠右對齊）。第一個和最後一個 rowline 自動加上下邊框。

**變體：2 列**：`style="grid-template-columns:1fr 3fr"` 去掉 `.m` 列。

---

## Pillar 支柱卡

三支柱結構，常用於「概念並列」型別頁面。

```html
<div class="grid-3">
  <div class="pillar">
    <div class="ic">01</div>
    <div class="t">三層<br>文件體系</div>
    <div class="d">CLAUDE.md<br>+ 專案知識庫<br>+ 護欄檔案</div>
  </div>
  <!-- ... 更多 pillar ... -->
</div>
```

**帶圖示的 pillar（用於強調性頁面）**：
```html
<div class="pillar" style="padding:4vh 2vw;border:1px solid currentColor;border-color:rgba(10,10,11,.2)">
  <div class="ic"><i data-lucide="compass" class="ico-lg"></i></div>
  <div class="t">判斷力</div>
  <div class="d">決策和方向的權威。<br>取捨、品味、方向感。</div>
</div>
```

`.ic` 可以是序號（`01 / 02 / 03` 或 `A. / B. / C.`），也可以是 Lucide 圖示。

---

## Tag & Kicker

**Kicker** 是標題上方的小提示文字（等寬、全大寫、小字號）：
```html
<div class="kicker">過去 64 天 · 開發篇</div>
<div class="h1-zh">一個人，做了什麼。</div>
```

**Tag** 是獨立的標籤膠囊（帶邊框）：
```html
<div style="display:flex;gap:1.6vw;flex-wrap:wrap">
  <div class="tag">早上 10 點起床</div>
  <div class="tag">週二 / 四下午健身</div>
  <div class="tag">晚上照樣看劇 · 玩遊戲</div>
</div>
```

---

## Figure 圖片框

**這是本範本最容易踩坑的元件，務必遵守以下規則**。

### 基礎結構

```html
<figure class="tile">
  <div class="frame-img" style="height:26vh">
    <img src="圖片素材/xxx.png" alt="說明">
  </div>
  <figcaption class="frame-cap">
    <span class="pf">X · Twitter</span>
    <span class="nb">137K</span>
  </figcaption>
</figure>
```

### 關鍵約束（血淚經驗，不要違反）

1. **圖片網格必須用 `height:Nvh` 固定高度**，不要用 `aspect-ratio`。
   - 原因：網格里用 aspect-ratio 容易撐破父容器，導致圖片堆疊。
   - 推薦尺寸：`.h-16` (小型面板) / `.h-18` (緊湊條形) / `.h-22` (標準網格) / `.h-26` (突出展示) / `.h-28` (大圖)。
   - 單張主圖可以用範本提供的比例類：`.r-16x9` / `.r-16x10` / `.r-4x3` / `.r-3x2` / `.r-3x4` / `.r-1x1`。
   - 同一組圖片必須使用同一個高度類，不要一張 `25vh`、一張 `21vh` 混用。

2. **`object-position:top center`（已在 CSS 裡設好）**，只允許裁掉底部。
   - 嚴禁裁剪左右和頂部 —— 這是圖片的核心身份資訊區。

3. **網格里多張圖時，用內聯 grid 而不是 `grid-3`**：
   ```html
   <div style="display:grid;grid-template-columns:1fr 1fr 1fr;gap:1vh 1.2vw">
     <figure class="tile">...</figure>
     <figure class="tile">...</figure>
     <figure class="tile">...</figure>
   </div>
   ```

4. **圖片與佈局其他部分對齊**：使用 `.grid-2-7-5` / `.grid-2-6-6` / `.grid-2-8-4` 的 grid 結構自然靠上對齊。不要給圖片加 `align-self:end`。

5. **資訊圖 / 截圖再設計**：給 `.frame-img` 同時加 `.fit-contain`，避免圖內文字和標註被裁切。

6. **使用者原始截圖比例不合適時**：優先按 `screenshot-framing.md` 做 CleanShot X 式程式化適配；只有截圖太長、太窄或需要重構資訊時，才重新產生「截圖再設計 / UI 情景圖」。

### Frame Caption 變體

```html
<!-- 標準：左 figure 名，右數字 -->
<figcaption class="frame-cap">
  <span class="pf">X · Twitter</span>
  <span class="nb">137K</span>
</figcaption>

<!-- 帶編號 -->
<figcaption class="frame-cap">
  <span class="idx">01</span>
  <span class="pf">AI 潤色</span>
  <span>Polish</span>
</figcaption>
```

### 圖片佔位（設計階段佔位符）

圖片還沒有就位時，用虛線框佔位：
```html
<div class="img-slot r-4x3">  <!-- r-4x3 / r-16x9(default) / r-3x2 / r-1x1 -->
  <span class="plus">+</span>
  <span class="label">GitHub 截圖位置</span>
</div>
```

---

## Icons 圖示

**嚴禁使用 emoji**。用 Lucide via CDN（template.html 已引入）。

```html
<i data-lucide="compass" class="ico-lg"></i>     <!-- 大圖示（pillar 用） -->
<i data-lucide="target" class="ico-md"></i>      <!-- 中圖示（列表項用） -->
<i data-lucide="check-circle" class="ico-sm"></i>  <!-- 小圖示（inline 用） -->
```

**常用 Lucide 圖示名**（按含義分組）：

- 判斷類：`compass`, `target`, `crosshair`, `search-check`
- 關係類：`share-2`, `users`, `network`, `link`, `handshake`
- 品牌類：`crown`, `gem`, `award`, `star`, `badge-check`
- 流程類：`workflow`, `route`, `arrow-right-left`, `repeat`
- 資料類：`grid-2x2`, `bar-chart-3`, `trending-up`, `activity`
- 審美類：`palette`, `brush`, `eye`, `sparkles`
- 對錯類：`check-circle`, `x-circle`, `check`, `x`
- 方向類：`arrow-right`, `arrow-up-right`, `corner-down-right`

**圖示與文字 inline 組成**：
```html
<div class="h3-zh" style="display:flex;align-items:center;gap:.8em">
  <i data-lucide="target" class="ico-md"></i>
  判斷 — 什麼值得寫
</div>
```

---

## Ghost 巨型背景字

用作「裝飾性背景字」，極低透明度，營造雜誌感。

```html
<div class="ghost" style="right:-6vw;top:-8vh">BUT</div>
<div class="ghost" style="left:-8vw;bottom:-18vh;font-style:italic">Harness</div>
```

- 字號 34vw，opacity 0.06
- 常用定位：`right:-6vw;top:-8vh`（右上超出）/ `left:-8vw;bottom:-18vh`（左下超出）
- 內容：英文單詞或數字（章節序號 01/02/03、關鍵詞 BUT/NOW/HERE）

**注意**：使用 ghost 的頁面裡，其他內容要加 `position:relative;z-index:2` 避免被壓到下面。

---

## Highlight 熒游標記

行內短語的「螢光筆」效果：

```html
<span class="hi">不是</span>
<span class="hi">一次性爆發</span>
```

在文字底部產生一條半透明高亮條。深色主題用亮條，淺色主題用暗條（CSS 已處理）。

**適合場景**：只對關鍵 1-3 個詞使用，不要大面積用。

---

## Motion 動效系統

整套 deck 預設開啟翻頁入場動畫，由 Motion One(vanilla 版 Framer Motion，約 4KB)驅動程式。

### 載入方式

`assets/template.html` 底部的 module script 會先嚐試**本地** `assets/motion.min.js`，失敗則回落到 **jsdelivr CDN**，兩者都失敗則強制把所有帶 `data-anim` 的元素設為 `opacity:1`—— 內容永遠可讀，簡報不相依性網路。

```js
// template 裡的核心載入器(不用改)
let motion;
try { motion = await import('./assets/motion.min.js'); }
catch(e1) {
  try { motion = await import('https://cdn.jsdelivr.net/npm/motion@11.11.17/+esm'); }
  catch(e2) {
    document.querySelectorAll('[data-anim]').forEach(el=>{el.style.opacity='1';el.style.transform='none'});
  }
}
```

### 資料屬性驅動程式

你只需要在 HTML 里加兩種屬性：

```html
<!-- 1. 在 section 上選 recipe(可選，預設 cascade / hero 自動) -->
<section class="slide light" data-animate="quote">

<!-- 2. 在需要入場的元素上加 data-anim（可選值：left/right/line/step/divider） -->
<h1 class="h-xl" data-anim>大標題</h1>
<div class="stat-card" data-anim>...</div>
<div data-anim="left">左列內容</div>
<span data-anim="line" style="display:block">引用第一行</span>
```

### 5 種 recipe 一覽

| recipe | 觸發方式 | 行為 | 代表佈局 |
|---|---|---|---|
| `cascade`(預設) | 不加 `data-animate` 即為此值 | 所有 `data-anim` 逐個 stagger 淡入，75ms/step | Layout 3 / 4 / 5 / 10 |
| `hero` | `.hero` slide 自動用此值 | 慢節奏 stagger，儀式感更強，160ms/step | Layout 1 / 2 / 7 |
| `quote` | `data-animate="quote"` | 其他元素先出，`data-anim="line"` 的行 550ms 間隔逐句揭示 | Layout 8 |
| `directional` | `data-animate="directional"` | `data-anim="left"` 從左滑入 → divider → `data-anim="right"` 從右滑入 | Layout 9 |
| `pipeline` | `data-animate="pipeline"` | 翻到此頁 step 保持 15% 透明；按 →/空格/滾輪逐個點亮，最後一步才放行翻頁 | Layout 6 |

### 給 slide 選 recipe 的決策樹

1. **它是 `.hero` slide 嗎？** → 不用加 `data-animate`，自動用 `hero`
2. **它是大引用金句頁？** → `data-animate="quote"`，每句用 `<span data-anim="line" style="display:block">`
3. **它是左右對比 Before/After？** → `data-animate="directional"`，左列 `data-anim="left"`、右列 `data-anim="right"`
4. **它是流水線分步講解？** → `data-animate="pipeline"`，每步 `data-anim="step"`
5. **其他所有正文頁** → 什麼也不加，自動用 `cascade`

### 什麼元素該加 `data-anim`?

- ✅ 每一層有獨立語義的塊：kicker / h1 / h-xl / lead / callout / stat-card / figure / tag / rowline
- ✅ 多列結構裡每一列，讓它們逐列淡入而不是一起
- ❌ 不要在容器(`.grid-6` / `.frame`)上加，只加給葉子元素
- ❌ 不要在每個 `<li>` 上加，一般在 `<ul>` 層加就夠
- ❌ 如果某頁不想要任何動畫(比如過渡頁)，整頁不加 `data-anim` 即可 — Motion One 只對帶標記的元素生效

### 常見問題

- **圖片閃一下再出現？** 這是預期行為，翻頁中段(450ms 時)觸發動畫
- **Pipeline 頁卡住翻不下頁？** 正確的，按 → 一步一步點亮 step，全部點亮後再按 → 才翻頁
- **內容靜態時也不顯示？** 檢查 motion.min.js 是否在 `assets/` 下；或者瀏覽器控制檯看錯誤資訊
