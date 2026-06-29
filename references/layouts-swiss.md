# Layouts · 風格 B 瑞士國際主義

22 個原始登記版式 · 嚴格模組化網格 · 每個版式說明用途、骨架、關鍵類名、專屬動效。

> ⚠️ 這套版式與風格 A（電子雜誌/電子墨水）**不通用**。類名同名但語義不同（例如 `h-hero` 在風格 A 是襯線，在風格 B 是無襯線極細 200）。一份 deck 只能選一套。

---

## Swiss locked mode（必須先讀）

本主題的 golden source 是：

`/Users/guohao/Documents/op7418的儲存庫/專案/Thin-Harness-Fat-Skills/ppt/index.html`

產生正文頁時不要把 Swiss 當成「自由組成的風格包」。預設只能使用 `references/swiss-layout-lock.md` 登記的 `S01-S22`。每個 slide 都必須在 `<section>` 上寫 `data-layout="Sxx"`。

**關鍵約束**：

- 頂部中文標題預設靠左對齊並處在左上內容軸；不要把標題放到頁面中間。
- 不允許臨時發明原始 22P 之外的正文結構。本文件末尾的 P23/P24 屬於歷史實驗區，預設禁用。
- 需要單張大圖時使用 `S22 Image Hero`；需要多圖時用 `S15/S16` 的原始矩陣/小報骨架改造成圖片格。
- 地點、路線、人物住所、城市關係頁使用 `S08 + Swiss Map Component`；這仍然是 S08 的右側插槽擴充，不是新正文頁。先讀 `swiss-map-component.md`。
- SVG 只畫幾何，不寫可見文字。標籤放 HTML 裡。
- 產生完成後執行 `node scripts/validate-swiss-deck.mjs index.html`。

---

## 設計語言基線

**配色**（`--accent` 由主題決定，見 `themes-swiss.md`）
- `--paper` 紙白底 #ffffff（主背景）
- `--ink` 黑墨字 #0a0a0a（主文字 / Ink 反轉塊）
- `--accent` 單色錨點（IKB 藍預設 / 黃 / 綠 / 橙 四套）
- `--text-primary / secondary / helper` 三級文字灰階
- `--border-subtle` 1px 髮絲細線 #e0e0e0

**排版**
- 字型：`var(--sans)` Inter / Helvetica Neue + `var(--mono)` JetBrains Mono
- 字重：**200（ExtraLight）大字** / **300（Light）正文** / **600（SemiBold）t-cat 小標**
- 大標題遵循原始 PPT 的實際頁面用法：主標題 `font-weight:200`，重點詞/數字 `font-weight:300`；不要因為舊 CSS helper 裡殘留過 800/900 就把 Swiss 大標題加粗
- 大字號收緊：`letter-spacing:-.04em` / `line-height:.9`
- mono 數字：`font-feature-settings:"tnum","ss01"`

**中文大標題字號分檔**
中文方塊字的視覺面積比英文更重，不能直接套英文頁的 `6.8vw-7vw`。產生前先按中文標題長度降級：

| 中文標題形態 | 推薦字號 |
|---|---|
| 1 行，≤ 8 箇中文字元 | `min(6.4vw,11.2vh)` |
| 2 行，每行≤ 8 箇中文字元 | `min(5.8vw,10.2vh)` |
| 2 行，任一行 9-12 箇中文字元 | `min(5.2vw,9.2vh)` |
| 3 行或更長標題 | 改寫標題；實在不能改時用 `min(4.6vw,8.2vh)` |

規則：中文標題優先改短，其次降字號；不要讓標題擠佔下方圖文區域。英文、數字型 hero 可以更大，中文方法論頁必須更克制。

**簡報最小字號與字重階梯**
瑞士風不是網頁說明頁，投屏時不能出現 10-12px 的註釋字。預設下限：

| 文字型別 | 最小字號 |
|---|---|
| 正文段落 / 主要說明 | `18px` |
| 卡片描述 / 列表 / 時間線說明 / caption / 圖注 | `16px` |
| meta / kicker / mono label / 圖表標籤 | `14px` |

內容過多時，先壓縮文案、拆頁或更換 Sxx 版式；禁止靠降低小字字號解決擁擠。圖注、時間線說明、KPI 註釋、底部 note 尤其要守住這個下限。

**字號與字重階梯（瑞士風核心）** — "越大越細，越小越粗"不是感性描述：

| 字號區間 | 推薦字重 | 典型場景 |
|---|---|---|
| ≥ 8vw | 200（ExtraLight） | 封面大字、巨號 KPI、h-statement |
| 4-7.9vw | 200-300 | 章節標題（h-xl/h-xl-zh）、大編號 |
| 1.8-3.9vw | 300-400 | 中型標題、takeaway 標題（≈1.8vw）、中號數字 |
| 1-1.7vw / 16-20px | 400-500 | 正文段落、卡片描述、說明文字 |
| 13-15px（小字） | 500-600 | meta、kicker、角標、圖表標籤、caption 強調 |

**硬規則：**
- 同一頁內，字號越小的元素字重必須 ≥ 字號越大的元素（不允許 16px 正文用 300 而 1.8vw 標題用 500）
- 16px 左右的小字拒絕使用 weight 300（太細不可讀），最低 400，推薦 500
- 封面/IKB 反白大標題內強調字用 `italic + weight 300`，不要用 accent 色（藍壓藍看不見）

**網格**（IBM Carbon 2x Grid 改造）
- 16 列 grid：`grid-template-columns:repeat(16,1fr)` + `gap:16px`
- spacing token：`--sp-3` 8 / `--sp-4` 12 / `--sp-5` 16 / `--sp-6` 24 / `--sp-7` 32 / `--sp-8` 40 / `--sp-9` 48 / `--sp-10` 64 / `--sp-11` 80 / `--sp-12` 96 / `--sp-13` 160

**畫布**
- `.canvas-card`：`100vw × 100vh`，直角無圓角，padding `5.6vh 5vw 4.4vh`
- `body{background:var(--paper)}` — 不用 WebGL 背景
- 必須保留右下角 `B 靜態` 快速鍵。低功耗模式使用 `body.low-power`，停止 WebGL/ASCII canvas RAF 與 Motion 入場動畫，重新整理後透過 `localStorage` 保持使用者選取。

---

### P0 對齊法則（每產生一頁都先過這 4 條，違反 = 整頁報廢）

**1. 不要二次疊加水平 padding** ⚠️ 最常踩
`.canvas-card` 已自帶 `padding:5.6vh 5vw 4.4vh`。
chrome-min（頁首）、主體內容、底部 footnote 都是 canvas-card 的子元素，**共用同一條 5vw 邊線**。
如果在主體那層再寫 `padding:5vh 5vw 4vh`，水平方向就變成 `5vw + 5vw = 10vw`，主體比 chrome-min 多內縮一圈，左右對不齊。

```html
<!-- ❌ 錯：主體多縮了 5vw -->
<div class="canvas-card">
  <div class="chrome-min">...</div>
  <div style="flex:1;padding:5vh 5vw 4vh;...">主體內容</div>
</div>

<!-- ✅ 對：主體 padding 為 0，只用 grid gap 控垂直間距 -->
<div class="canvas-card">
  <div class="chrome-min">...</div>
  <div style="flex:1;padding:0;display:grid;grid-template-rows:auto 1fr auto;gap:3vh">主體內容</div>
</div>
```

例外：`.slide.split .canvas-card{padding:0}` 已被 CSS 覆蓋，split 模式下兩個 `.half` 自己控制 padding（常用 `5.6vh 3.6vw 4.4vh`），與本法則不衝突。

**2. kicker 必須在大標題"上方"，不要壓成左右**
小標題（`.t-meta` / `.t-cat`）與大標題之間是從屬關係，版式上必須**上下結構**。

```html
<!-- ❌ 錯：auto 1fr 把 kicker 和大標題擠成左右兩列 -->
<div data-anim="head" style="display:grid;grid-template-columns:auto 1fr;gap:3vw;align-items:end">
  <div class="t-meta">METHODOLOGY · 03</div>
  <h2 class="h-xl-zh">為什麼是 N+1</h2>
</div>

<!-- ✅ 對：flex column 上下疊 -->
<div data-anim="head" style="display:flex;flex-direction:column;gap:1.4vh">
  <div class="t-meta">METHODOLOGY · 03</div>
  <h2 class="h-xl-zh">為什麼是 N+1</h2>
</div>
```

**3. 雙約束限高 `min(Xvw, Yvh)` 中 Y ≥ X × 1.6**
標準 16:9 屏 1vw : 1vh ≈ 1.78，如果 Y 太嚴（例如 `min(7vw, 10vh)`），大字號會被高度上限截斷到 10vh，不再受 7vw 主導，顯得整體縮小。
經驗數值：

| 用途 | 推薦 |
|---|---|
| h-hero 巨字宣言 | `min(11.6vw, 19vh)` |
| h-xl 章節標題 | `min(7vw, 12vh)` ~ `min(7.4vw, 13vh)` |
| 大數字 KPI | `min(8.4vw, 14vh)` |
| 中數字 / 編號 | `min(4.6vw, 8.5vh)` ~ `min(5.6vw, 10vh)` |

**4. canvas-card 子元素之間用 grid `gap`，不要靠 margin/padding 堆**
`.canvas-card` 預設 `display:flex;flex-direction:column`，chrome-min 自帶 `margin-bottom:48px`（`--sp-9`）。
主體區往下排幾行（head / 內容 / footnote），**首選** `display:grid;grid-template-rows:...;gap:Nvh`，**次選** flex column + gap，**禁用** 在每個子塊里加 `margin-top` / `padding-top` 調間距（會和 chrome-min 的 margin-bottom 重疊或撕裂）。

**5. 底部分頁安全區：主內容最低處不要觸及 nav**
底部分頁 dot 固定在 `bottom:2vh`，視覺上佔據約 `93vh` 之後的區域。主內容、圖片 caption、圖表說明、timeline label 的最低處必須停在安全區上方。

- 範本提供 `--nav-safe-bottom:8vh`，可用 `.nav-safe-bottom` / `.nav-safe-bottom-tight`
- P23 使用 `.swiss-img-split.align-image-bottom` 時，範本會自動給底部加安全區，避免圖片 caption 被分頁元件擋住
- 如果為某頁手寫 `align-items:end` / `margin-top:auto` / `position:absolute;bottom:...`，必須肉眼檢查最低處是否越過 nav
- 視覺自檢：開啟頁面到該頁，確認內容最低邊緣與分頁 dot 之間至少有 `3vh` 呼吸空間

---

**卡片填充規則（必須遵守）**
| 型別 | 類名 | 角色 | 用法 |
|---|---|---|---|
| Ink 黑底 | `.card-ink` | 反轉 / 宣言 | hero 塊、收束頁一半 |
| Accent 藍填充 | `.card-accent` | 唯一焦點 | 一組中突出一項 |
| Grey 灰底 | `.card-fill` | 預設中性 | 多卡並列、統計卡 |
| Outlined 描邊 | `.card-outlined` | 錨點（非卡片） | hairline 分割框 |

❌ 禁止混用（藍色背景+藍色描邊、灰底+描邊等）

**裝飾極簡原則**
- 1px hairline 分隔（`hr-hairline` / `border-bottom`）
- 8×8 / 12×12 直角小方塊替代圓點
- 點陣 `dot-mat` / 描邊圓 `ring-mat` / 叉 `cross-mat`（SVG mask）

**圖片使用原則（Swiss + GPT-M 2.0）**
- 圖片是網格中的"證據塊"，不是裝飾背景；必須有明確功能：案例、實拍證據、UI 截圖、系統圖、概念資訊圖
- 所有圖片容器保持直角、無陰影、無圓角；預設**不加圖片外框**，讓 caption 或頁面網格承擔層級
- 白底資訊圖 / 流程圖 / UI 圖：容器背景必須是 `var(--paper)`，不要用灰底包白圖，也不要加 `.swiss-keyline` 描邊
- 只有當圖片本身邊緣無法和頁面區分時，才用 `.swiss-lined` 加一條頂部 accent 線；不要給每張圖都套邊框
- 紀實照片用 `object-fit:cover` 只裁底部/邊緣；原始截圖或文字密集圖用 `.fit-contain`，避免文字被裁
- 如果資訊圖、流程圖、UI 情景圖是按 S15/S16 槽位重新產生的，必須用 `.frame-img.r-21x9` / `.frame-img.r-16x10` 鋪滿槽位；不要再加 `.fit-contain`，否則會變成小圖漂在白框裡
- 瑞士風圖片優先比例：S22 頂部橫幅 `21:9`；S15/S16 多圖格統一 `21:9` 或統一 `16:10`
- 產生 2-3 張配圖時，必須先繫結原始版式槽位：單張大圖 = S22；多圖 = S15/S16 網格改造；不要使用未登記的 P23/P24
- S22 的照片主體必須位於中央安全區，HTML 用 `object-position:center 35%` 或 `center center`，不要用 `top center` 截人臉
- GPT-M 2.0 產生圖必須遵守單一 accent 色、Helvetica/Inter 氣質、12/16 列網格、直角純色、無漸層/陰影/圓角
- 產生圖只保留核心影象本身，不要把頁首、頁尾、標題、頁碼、角標、邊框、署名畫進圖片裡

**版式多樣性硬規則**
Swiss 主題有 22 個登記版式，產生時要主動展示版式系統，不要把所有內容都做成 `head + grid-reveal + card`：

- 7-8 頁 deck 至少使用 **6 個不同 S 編號版式**
- 不允許連續 3 頁使用同一種主體結構（如三頁連續 S19 / 普通卡片）
- 如果是"測試範本"或"我想看看效果"，必須覆蓋：封面、收尾、至少 1 個對比/時間線（S08/S11/S02）、至少 1 個結構圖（S14/S17/S15）、至少 1 個圖片版式（S22 或 S15/S16 圖片格）
- 圖片頁不等於新發明一頁。單圖用 S22，多圖用 S15/S16 的原始網格骨架改造
- 每頁寫程式碼前先列 `頁碼 → data-layout → 為什麼選它 → 圖片槽位`；產生後用 validator 檢查

**動效原則（每頁一個語義化 recipe）**
- 不是統一 fade-up，而是**與圖形語義耦合**：數字 scale 彈入、bar scaleY 拉起、SVG 圓環 stroke-dashoffset 描線、時間線節點序列點亮
- 緩動：`EASE_PROD` `cubic-bezier(.2,0,.38,.9)` 用於 productive（120-240ms）、`EASE_ENTRY` `cubic-bezier(0,0,.3,1)` 用於 expressive（400-700ms）
- playSlide 入口要 reveal 所有 `[data-anim]` 容器到 opacity:1，recipe 內再用 motion `{opacity:[0,1]}` 覆蓋

---

## 視覺 + 程式碼雙維稽核（產生後必須做）

不要只看 HTML/CSS。Swiss 範本的還原度要同時從**瀏覽器視覺**和**程式碼結構**判斷：

1. 同時開啟三份頁面：原始參考 PPT、當前 `template-swiss.html` 或產生頁、正在修改的測試 PPT。原始參考路徑是 `/Users/guohao/Documents/op7418的儲存庫/專案/Thin-Harness-Fat-Skills/ppt/index.html`。
2. 截圖前先等入場動效穩定（約 1-2 秒）。不要把動畫中間態誤判成"內容缺失"或"版式空白"。
3. 先看視覺：標題重量、頭部距離、圖片落位、底部安全區、caption 是否被 nav 擋住。
4. 對照原始參考 PPT 的同類版式，不要只對照 CSS helper；以實際頁面結構和視覺結果為準。
5. 再回到程式碼，檢查該頁是否誤用了不屬於該版式的元件，例如把 P24 的三圖證據牆塞進 P23，或把 P7 圖表用於沒有真實數值的概念列表。
6. 若視覺不一致，優先判斷是**版式選取錯**、**必選元件缺失**、**可選元件濫用**還是**間距/安全區問題**，不要直接靠調 `margin` 硬救。
7. 修改範本時，新增能力必須用新類隔離；不要因為一頁出問題去改全域性基座類。

### 原始 PPT 視覺錨點（對照時優先看這些）

| 視覺錨點 | 原始 PPT 的實際做法 | 產生時的規則 |
|---|---|---|
| 大標題重量 | 實際頁面大量使用 `font-weight:200/300`；即使 raw CSS helper 裡有 700/800/900，也不能直接當視覺標準 | 大標題保持輕字重，字號越大越細 |
| 留白 | 頁面經常只佔上半屏或中部，底部留給 nav 和少量 footnote | 不要為了"填滿"而把內容推到底 |
| 分割線 | 只在章節邊界、證據牆、卡片層級處使用 1px hairline | 不要給每個內容塊都加線 |
| 標題與內容 | 標題區和正文/圖表之間有明顯空氣感 | 複雜頁用 grid `gap`，不要讓內容貼著標題 |
| Timeline | 軸線在中下部，但 label 不碰底部 nav | 橫向 timeline 必須同時檢查上下 label 和 nav 安全區 |
| 圖片頁 | 圖片是證據塊，要麼做 S22 主視覺，要麼放進 S15/S16 原始網格 | 不要使用未登記圖文結構 |

### 元件必選 / 可選 / 可省略

| 元件 | 規則 |
|---|---|
| `.canvas-card` / `.chrome-min` | 基礎頁必選；split 頁左右 half 各自有 chrome-min |
| `t-meta` / `t-cat` kicker | head 區必選，但正文卡片內可省略；必須在大標題上方 |
| 大標題 | 章節/論點頁必選；列表型小卡頁可以用較小標題，但不能缺頁級資訊錨點 |
| `lead` 說明 | 可選；如果標題已經解釋清楚，可以省略，但不能用長段正文貼著標題 |
| 圖片 caption | S15/S16 多圖格必選；S22 大圖可選，因為圖已經是主視覺且下方有 KPI/說明 |
| 髮絲線 / border-bottom | 可選；只能用於建立層級，不能為了裝飾堆線 |
| KPI / 數字 | 只在有真實資料時使用；不要為概念解釋編造數值 |
| `footnote` / 底部說明 | 可選；如果使用，必須避開 nav 安全區 |
| `S08 + Swiss Map Component` | 地點/路線/人物住所關係專用；右側地圖必須有點、連線、卡片和 `+` / `-` / `DRAG` 控制，詳見 `swiss-map-component.md` |

### 通用版式 / 非通用版式

| 型別 | 版式 | 使用邊界 |
|---|---|---|
| 通用 | S01, S03, S08, S09, S10, S11, S19 | 大多數敘事 deck 都能用，但仍要滿足內容形狀 |
| 條件通用 | S04, S05, S13, S16 | 取決於數量是否剛好相符：3/4/6 項 |
| 資料專用 | S02, S06, S07, S18, S20, S21, S22 | 必須有真實時間、數值、指標或案例資料 |
| 結構專用 | S14, S15, S17 | 必須有閉環、矩陣、層級/生態關係；不適合普通段落 |

---

## 22 個登記版式

### P1 · Cover · 封面頁

**用途**：整套 deck 起手 / 主題宣言。
**適用內容型別**：封面 / 章節首頁 / 主題宣言。**純文字結構**（主標題 + 副標 + 元資訊），不承載資料。

**預設推薦：IKB 滿屏 + ASCII 呼吸場** ⭐
- `<section class="slide accent">` 滿屏 IKB，**不是** light 白底
- `.canvas-card` 內首位插入 `<canvas class="ascii-bg" aria-hidden="true">`，範本底部 IIFE 自動驅動程式 sin/cos 二維噪聲呼吸場
- 主標題反白 weight 200，微強調字用斜體（`font-style:italic;font-weight:300`）而非 IKB 藍（底已是藍、藍壓藍看不見）
- **不要**再放編號大字"01"——chrome-min 已經標 01/NN
- 與 P9 Closing 的 IKB 半屏配合形成"開場全 IKB ↔ 收尾半 IKB"色彩閉環

**關鍵類**：`.slide.accent` `.ascii-bg` + `min(11.6vw,19vh)` 雙約束大字
**動效 recipe**：`hero` — ASCII 字元場持續呼吸，文字 fade-up 序列入場

**範例程式碼（IKB 預設變體）**：
```html
<section class="slide accent" data-animate="hero">
  <div class="canvas-card">
    <canvas class="ascii-bg" aria-hidden="true"></canvas>
    <div class="chrome-min">
      <div class="l">[必填] Deck 標題 · Issue/Field Note 編號</div>
      <div class="r">SS · 26.05.10 · 01 / NN</div>
    </div>
    <div style="flex:1;padding:0;display:grid;grid-template-rows:auto 1fr auto;gap:2.6vh">
      <div data-anim="kicker" class="t-meta" style="color:rgba(255,255,255,.78);letter-spacing:.22em">[必填] 章節英文 / Section En</div>
      <h1 data-anim="title" style="align-self:center;font-family:var(--sans),var(--sans-zh);font-weight:200;font-size:min(11.6vw,19vh);line-height:.94;letter-spacing:-.025em;color:#fff">[必填] 中文主標題<br/>（可在某字加 <span style="font-style:italic;font-weight:300">italic</span> 微強調）</h1>
      <div data-anim="bottom" style="display:grid;grid-template-rows:auto auto;gap:1.6vh;border-top:1px solid rgba(255,255,255,.22);padding-top:2vh">
        <div data-anim="lead" class="lead" style="max-width:52ch;color:rgba(255,255,255,.86);font-weight:300">[必填] 一段 1-2 行的副標 / 引子，定調全場.</div>
        <div style="display:flex;justify-content:space-between;align-items:end">
          <div class="t-meta" style="color:rgba(255,255,255,.6)">[選填] 作者 · 日期 · 出處</div>
          <div class="t-meta" style="color:rgba(255,255,255,.6)">→ swipe / arrow keys</div>
        </div>
      </div>
    </div>
  </div>
</section>
```

**經典變體（左 ink + 右 paper 對開）** — 僅當全 IKB 不合內容調性時使用：
```html
<section class="slide" data-animate="cover-reveal">
  <div class="canvas-card cover-split">
    <div class="cover-ink">
      <span class="t-cat">Volume 18 · 2026</span>
      <h1 class="h-hero">Thin Harness,<br>Fat Skills.</h1>
      <span class="t-meta">— Kevin · 2026-05</span>
    </div>
    <div class="cover-paper">
      <p class="lead">薄型承載層，厚重技能。</p>
      <ul class="meta-list">
        <li>22 PAGES</li><li>SWISS · IKB</li><li>MP-75</li>
      </ul>
    </div>
  </div>
</section>
```

---

### P2 · Vertical Timeline · 縱向時間軸

**用途**：演化對比、年代變遷、版本迭代（2-5 個時間節點）。
**適用內容型別**：**帶量化資料的時間演化**。每節點必須有「年份 + 量化數值（如 1× / 4× 倍數 / 單位數字）+ 描述」三件套。如果只有節點名沒有資料，改用 P11 橫向時間線。
**骨架**：左側 axis 列 12px 圓點 + 1px 虛線軸 / 右側節點資訊（年份 + 大字資料 + 小標 + 描述）。
**關鍵類**：`.timeline-v` `.tl-node` `.tl-axis`（12px 固定列寬，絕對定位 dot 防錯位） `.kpi-row-4`
**動效 recipe**：`timeline-vertical` — 節點按時間順序由上到下點亮（dot 先 pop 再擴 → 文字橫向滑入）
**網格規則**：axis 列 = 12px 固定；dot 用 `position:absolute;left:50%;transform:translateX(-50%)` 與虛線對齊
**範例程式碼**：
```html
<section class="slide" data-animate="timeline-vertical">
  <div class="canvas-card">
    <header class="chrome-min">...</header>
    <div class="timeline-v">
      <div class="tl-node">
        <div class="tl-axis"><span class="dot"></span></div>
        <div class="tl-body">
          <span class="yr">2023</span>
          <span class="multi">1<small>×</small></span>
          <p class="desc">Prompt Engineering Era</p>
        </div>
      </div>
      <!-- 重複 N 個 tl-node，axis 列貫穿 -->
    </div>
  </div>
</section>
```

---

### P3 · Statement · 極簡陳述

**用途**：中心論點、章節起始、口號。一頁只放一句話 + 簡單裝飾。
**適用內容型別**：**純定性論斷 / 口號 / 章節轉場**。一句話壓縮到 8-12 詞，**不承載任何資料或列表**。如果需要資料支撐，改用 P18 Why Now；如果是封面，用 P1。
**骨架**：左 1/3 空白 + 中段巨字陳述（8-10vw， weight 200） + 右下小字註腳 + 底部 hairline。
**關鍵類**：`.h-statement`（9.6vw，letter-spacing:-.05em） `.stmt-anchor`
**動效 recipe**：`statement-rise` — 大字按詞序錯峰升起（每詞延遲 180ms）+ 註腳 fade in
**範例程式碼**：
```html
<section class="slide" data-animate="statement-rise">
  <div class="canvas-card">
    <header class="chrome-min">...</header>
    <h1 class="h-statement">
      <span>Build it</span> <span>once.</span><br>
      <span>It runs</span> <span>forever.</span>
    </h1>
    <span class="stmt-anchor">— Statement 03</span>
  </div>
</section>
```

---

### P4 · Six Cells · 六格定義

**用途**：6 個並列概念定義、6 項功能並列。
**適用內容型別**：**6 個對等概念 / 功能列舉**（數量必須 = 6，過少用 P5，過多用 P15/P16）。每格僅承載「圖示 + 編號 + 短標題 + 一行描述」，**不承載需要展開的資料 / 段落**。
**骨架**：2×3 網格 / 每格上方 lucide 圖示 + 編號 + 短標題 + 一行描述 / 單元間用 hairline 分隔。
**關鍵類**：`.cell-6` `.cell-icon-row` `.cell-num`
**動效 recipe**：`six-cells` — 6 格按 z 形順序點亮（L→R, T→B，每格延遲 90ms）
**注意**：**不要自己畫 SVG 圖示**，用 `<i data-lucide="bookmark"></i>` 引線上 lucide。
**範例程式碼**：
```html
<div class="cell-6">
  <div class="cell">
    <i data-lucide="square-stack"></i>
    <span class="cell-num">01</span>
    <h4>Skill File</h4>
    <p>純 markdown，可手寫、可重寫</p>
  </div>
  <!-- 5 more -->
</div>
```

---

### P5 · Three Sub-cards · 三子卡

**用途**：三步流程、三類對比（輕度差異）。
**適用內容型別**：**3 個對等概念 / 步驟**（數量必須 = 3）。結構同質、**無強烈資料差異**（若資料可比，改用 P6 KPI Tower）。每卡內容比 P4 略多（編號 + 標題 + 1-2 行描述）。
**骨架**：左側大標題 + 描述 + 頂部 hairline / 右側 3 張水平堆疊 sub-card。
**關鍵類**：`.sub-card-stack` `.sub-card`（`.card-fill` 灰底，直角）
**動效 recipe**：`sub-stack` — 主標題先入 → 3 卡階梯式從右滑入（每卡延遲 140ms）
**範例程式碼**：
```html
<div class="grid-2-9">
  <div class="lead-col">
    <span class="t-cat">Three Forces</span>
    <h2 class="h-xl">壓成三個事實</h2>
  </div>
  <div class="sub-card-stack">
    <article class="card-fill sub-card">
      <span class="big-num">01</span>
      <h4>Skill File</h4>
      <p>...</p>
    </article>
    <!-- 2 more -->
  </div>
</div>
```

---

### P6 · KPI Tower · 不等高柱狀 KPI

**用途**：4 項資料用視覺高度表達層級差異。
**適用內容型別**：**4 項可比量化資料**（必須有真實數值，bar 高度由資料決定）。典型如：成本、容量、計數、效率指標。**禁止**用於無資料的概念列舉（那是 P4/P5 的事）。
**骨架**：4 列均分，每列底部一根不同高度的 IKB 藍矩形（資料決定高度）+ 頂部圖示 + 中段巨數 + 底部標籤。
**關鍵類**：`.kpi-tower-row` `.bar-tower`（min-height:6vh， max:36vh） `.tower-cap`
**動效 recipe**：`tower-grow` — 標籤先入 → 數字 scale 彈入 → tower scaleY 從 0 拉起（transform-origin:bottom）
**範例程式碼**：
```html
<div class="kpi-tower-row">
  <div class="tower-col">
    <i data-lucide="layers"></i>
    <span class="num-mega">90K</span>
    <span class="lbl">Skills</span>
    <div class="bar-tower" style="--h:36vh"></div>
  </div>
  <!-- 3 more，h 不同 -->
</div>
```

---

### P7 · H-Bar Chart · 橫向條形圖

**用途**：多項排名比較 / 佔比對比（5-10 項）。
**適用內容型別**：**5-10 項可比量化資料**（必須有真實百分比 / 評分 / 數值，bar 寬度由資料決定）。典型如：benchmark 排名、市場份額、問卷佔比。⚠️ **嚴禁用於無量化資料的概念列舉**（那是 P4/P5/P15）— 編造數字會被識破。
**骨架**：頂部大標題 / 中段空 / 下半部條形列表（每行：文字標籤 + 1px 藍條 0→target width + 末端數字）。
**關鍵類**：`.h-bar-chart` `.bar-row` `.bar-fill`（scaleX 動畫）
**動效 recipe**：`hbar-grow` — 大標題先入 → 每行依序 width 0→target（transform-origin:left）+ 末端數字 count-up
**範例程式碼**：
```html
<div class="h-bar-chart">
  <div class="bar-row">
    <span class="bar-lbl">Anthropic Advisor</span>
    <span class="bar-fill" style="--w:84%"></span>
    <span class="bar-num">84</span>
  </div>
  <!-- N more -->
</div>
```

---

### P8 · Duo Compare · 雙軌對照

**用途**：Before/After、A vs B、舊/新對比。
**適用內容型別**：**二元對照**（必須正好 2 項）。兩側結構同質（t-cat 標籤 + 大字標題 + 段落 / 列表說明）。典型如：舊/新工作流、傳統/AI、客戶視角/團隊視角。
**骨架**：左右兩半屏中間一根縱向 1px 長線分隔 / 各自頂部 t-cat + 大字標題 + 下方說明。
**關鍵類**：`.duo-compare` `.duo-half` `.vrule`（scaleY 拉開）
**動效 recipe**：`duo-mirror` — 中線 vrule 先 scaleY 0→1 → 左右各自標題、文字映象入場
**範例程式碼**：
```html
<div class="duo-compare">
  <div class="duo-half">
    <span class="t-cat">Before</span>
    <h2>交給模型</h2>
  </div>
  <span class="vrule"></span>
  <div class="duo-half">
    <span class="t-cat">After</span>
    <h2>交給程式碼</h2>
  </div>
</div>
```

---

### P9 · Closing Manifesto · 收束宣言

**用途**：整套 deck 收尾頁。
**適用內容型別**：**deck 收尾**（每個 deck 只有一頁）。固定結構：左側宣言短句 + 右側 3 條 takeaway（編號 + 標題 + 一行說明）。**不能在中間頁使用**（那會與 P1 封面重複）。

**預設推薦：左 IKB+ASCII / 右 paper takeaway** ⭐
- 用 `<section class="slide split">` + 左半 `.half.b-accent` + ASCII canvas + 右半白底 takeaway
- 與 P1 封面的全 IKB 形成"開場全 IKB ↔ 收尾半 IKB"色彩閉環
- 右側第 03 條 takeaway 用 `var(--accent)` 強調，把 IKB 藍從左半穿到右半，完成色彩縫合
- 大標題反白 weight 200，強調字用斜體（底已是藍、不要再用 `var(--accent)` 標藍）

**關鍵類**：`.slide.split` `.half.b-accent` `.ascii-bg`（IIFE 自動啟動）
**動效 recipe**：`split-statement` — 左 ink/IKB 標題字元序列升起 → 右白半 takeaway 三條尾隨

**範例程式碼（IKB 預設變體）**：
```html
<section class="slide split" data-animate="split-statement">
  <div class="canvas-card">
    <div class="split-half">
      <!-- 左半 · IKB + ASCII 呼吸場 -->
      <div class="half b-accent" style="padding:5.6vh 3.6vw 4.4vh;justify-content:space-between;position:relative;overflow:hidden">
        <canvas class="ascii-bg" aria-hidden="true"></canvas>
        <div class="chrome-min" style="margin-bottom:0;position:relative;z-index:1">
          <div class="l">NN / NN</div>
          <div class="r">CLOSING</div>
        </div>
        <div data-anim="manifesto" style="display:flex;flex-direction:column;gap:2vh;position:relative;z-index:1">
          <div class="t-meta" style="color:rgba(255,255,255,.78);letter-spacing:.22em;margin-bottom:1.6vh">MANIFESTO</div>
          <h2 style="font-family:var(--sans),var(--sans-zh);font-size:min(8vw,14vh);line-height:.94;letter-spacing:-.025em;font-weight:200;color:#fff">[必填] Build a model.<br/>Run <span style="font-style:italic;font-weight:300">forever</span>.</h2>
          <div style="font-family:var(--sans),var(--sans-zh);font-size:max(13px,1vw);line-height:1.6;color:rgba(255,255,255,.82);font-weight:300;max-width:36ch;margin-top:1.4vh">[必填] 一句中英文落地註腳.</div>
        </div>
        <div data-anim="signature" style="display:flex;justify-content:space-between;align-items:end;border-top:1px solid rgba(255,255,255,.22);padding-top:2vh;position:relative;z-index:1">
          <div class="t-meta" style="color:rgba(255,255,255,.62)">[選填] 作者 · 頭銜</div>
          <div class="t-meta" style="color:rgba(255,255,255,.62)">YY.MM.DD</div>
        </div>
      </div>
      <!-- 右半 · 白底 takeaway，第 03 條用 IKB 藍強調，首尾色彩閉環 -->
      <div class="half" style="padding:5.6vh 3.6vw 4.4vh;justify-content:space-between">
        <div class="chrome-min"><div class="l">TAKEAWAYS</div><div class="r">03 RULES</div></div>
        <div data-anim="rules">...</div>
        <div class="t-meta" style="color:var(--text-helper);text-align:right">→ 完 · END OF FIELD NOTE</div>
      </div>
    </div>
  </div>
</section>
```

**經典變體（`.closing-split` ink 雙半屏）** — 當封面沒有用 IKB 滿屏時，改用經典 ink 收束：
```html
<div class="closing-split">
  <div class="cl-ink">
    <p class="line-mega">Build it<br>once.</p>
    <p class="line-mega">It runs<br>forever.</p>
  </div>
  <div class="cl-paper">
    <ul class="takeaway-list">
      <li><span class="num">01</span><h4>Skill</h4><p>...</p></li>
      <!-- 2 more -->
    </ul>
  </div>
</div>
```

---

### P10 · Dot Matrix Statement · 點陣宣言

**用途**：第二張陳述頁 / 章節轉場 / 視覺透氣頁。
**適用內容型別**：**口號 / 隱喻 / 章節轉場**（同 P3，但加幾何點陣裝飾）。用於一個 deck 內**避免連續兩頁都是 P3**；通常用作"概念定義"前的視覺調味頁。
**骨架**：中段 7vw 巨字三行宣言 / 右上角 36vw 圓點矩陣 + 左下角描邊圓環矩陣。
**關鍵類**：`.dot-mat`（SVG mask 實心點）`.ring-mat`（描邊圓）`.cross-mat`（× 網格）
**動效 recipe**：`matrix-statement` — 文字逐行入 → 點陣 mask-position 從左推到右
**範例程式碼**：
```html
<div class="canvas-card">
  <span class="ring-mat" style="left:5vw;bottom:5vh;width:18vw;height:18vw"></span>
  <h1 class="h-statement">Build a thin harness.<br>Write fat skills.<br>Codify everything.</h1>
  <span class="dot-mat" style="right:0;top:0;width:36vw;height:36vw"></span>
</div>
```

---

### P11 · Horizontal Timeline · 橫向時間線

**用途**：多步驟流程（4-7 步）、時間演進。
**適用內容型別**：**4-7 步線性流程**（每步只有一個名稱，不需要展開資料 / 描述）。如果每步要展開，改用 P5；如果有量化資料，改用 P2。**禁止**用於迴圈結構（那是 P14）。
**骨架**：頂部大標題 / 中段一根 1px hairline 橫線 + N 個均布節點（8×8 直角方塊 + 上方 mono 編號 + 下方步驟名）。
**關鍵類**：`.timeline-h` `.tl-h-node` `.tl-h-axis`
**動效 recipe**：`timeline-walk` — 節點沿軸左→右依次點亮（每節點 220ms）
**對齊注意**：橫向時間線 label 的 CSS 相依性 `translateX(-50%)` 居中。動效裡如果要做上下位移，必須寫完整 `transform: translate(-50%, y)` 序列，不能只寫 `y`，否則動畫結束後 label 會偏離 dot。
**範例程式碼**：
```html
<div class="timeline-h">
  <span class="tl-h-axis"></span>
  <div class="tl-h-node">
    <span class="num">01</span>
    <span class="dot"></span>
    <span class="lbl">Investigate</span>
  </div>
  <!-- 4-6 more -->
</div>
```

---

### P12 · Manifesto + Ink Banner · 宣言 + 通欄 ink 條

**用途**：階段性結論、章節封底、口號 + 視覺強收束。
**適用內容型別**：**章節性收束 / 階段性宣言**（用於 deck 中段而非結尾，P9 是 deck 終結）。承載「主張 + 簡短說明 + ink 通欄宣言」三段結構，無資料。
**骨架**：上半屏左側 t-cat + 大字 4 行宣言 + 右側短段說明 / 下半屏 ink 通欄（無左右下邊距）+ 反白短句 + lucide 圖示矩陣。
**關鍵類**：`.manifesto-top` `.ink-banner-full`（`margin:0 -5vw -4.4vh` 取消父級 padding）
**動效 recipe**：`manifesto` — 大字三段錯峰升起 → 底 ink 條橫向 scaleX 0→1 鋪開 → 反白文字 fade in
**注意**：Skill File 那段小字 **靠上對齊於右側大字基線**（`align-items:flex-start;padding-top:1.2vw`）

---

### P13 · Three Forces Cards · 三力卡片小報

**用途**：3 個對等概念展示（每個 = 巨數 + 標題 + 雙列描述）。
**適用內容型別**：**3 個對等概念深化**（數量 = 3，比 P5 承載更多文字）。每卡內容比較豐富（巨編號 + 標題 + 雙列段落描述）。01/02/03 為編號錨點而非真實資料。典型如：三大反駁、三種力量、三大主張。
**骨架**：左 5/16 ink hero 塊（t-cat + 4 行標題 + 點陣裝飾）/ 右 11/16 三張水平卡堆疊。
**關鍵類**：`.three-forces` `.hero-ink-col` `.force-card`（`.card-fill`）`.force-num`（9.2vw IKB 藍）
**動效 recipe**：`three-forces` — 左 hero 橫移入 → 右 3 卡階梯式從右滑入 → 巨藍數字單獨 pop
**注意**：**3 張卡片必須統一樣式**（都用 `.card-fill` 灰底，不要混用描邊/藍底）；若需突出一張，改用 `.card-accent`，**禁止**藍底+描邊。

---

### P14 · Loop Diagram · 閉環流程圖

**用途**：自學閉環、自動化流程（3-5 步迴圈）。
**適用內容型別**：**迴圈 / 閉環流程**（終點回到起點，3-5 步）。如自學迴圈、CI/CD、回饋閉環、agent loop。**線性流程禁用**（那是 P11）。
**骨架**：左 4 行編號步驟（靠上對齊） / 右側 SVG 同心圓環 / 中央巨字 LOOP / 節點統一灰底直角方塊（不用圓點交替色）。
**關鍵類**：`.loop-diagram` `.loop-steps` `.loop-svg`
**動效 recipe**：`loop-form` — 左側步驟縱向序列 → 右 SVG 圓環 stroke-dashoffset 描線 → 節點序列點亮
**注意**：左右**整體居中對齊**（頂部對齊 + 高度等同）

---

### P15 · Image Matrix + Hero Stat · 矩陣 + 大字底注

**用途**：大量同類項展示（8-12 項 skill / 團隊成員 / 案例圖示），底部一個總資料收束。
**適用內容型別**：**8-12 項同型別小項 + 一個彙總指標**。每項只承載短標題（無展開），底部巨數為「彙總值」（專案總數 / 總流量 / 總使用者）。**項數過少改用 P4（6 項）**。
**骨架**：頂部標題（留 9vh 間距）/ 中段 4×3 矩陣卡（每卡 12vh 固定高度）/ 底部巨數 + 標籤（margin-top:auto 推到底）。
**關鍵類**：`.matrix-fill`（grid-template-columns:repeat(4,1fr)）`.matrix-cell`（`.card-fill` 灰底，**禁止描邊**）`.hero-stat-bottom`
**動效 recipe**：`matrix-fill` — 12 格隨機棋盤漸顯（每格 random delay）→ 底部巨數 count-up
**注意**：卡片高度限定（避免大數字溢位）；**所有卡用 `.card-fill` 灰底**，只突出強調項時單獨換 `.card-accent`

---

### P16 · Multi-card Brief · 微卡小報

**用途**：6 項小卡並列（快訊、tip 集合、特性概覽）。
**適用內容型別**：**6 項輕量短訊 / tip / 註腳**（數量 = 6，每項主文短 + 小字註腳）。比 P4 內容更碎，適合快訊類。**只允許一張 accent 藍突出**（單焦點法則）。
**骨架**：頂部大標題（留 9vh）/ 下方 3×2 微卡（每卡：左上主文 + 右下小字 + 中間留空）。
**關鍵類**：`.brief-grid` `.brief-card`（`.card-fill` 灰底）`.brief-card.is-accent`（單一藍底強調）
**動效 recipe**：`field-notes` — 6 卡按 z 形順序點亮（L→R, T→B，90ms 錯開）
**注意**：卡內排版**左上主文 + 右下小字**，中間空出（避免內容散）；**只允許一張 accent 藍**

---

### P17 · System Diagram · 同心圓系統圖

**用途**：層級架構（core→middle→outer）、生態地圖。
**適用內容型別**：**嚴格三層巢狀關係**（core 核心 / middle 中間層 / outer 外圈）。典型如：技術棧層級、生態分層、影響力輻射。**非三層結構禁用**（扁平用 P4，層級不清用 P5）。
**骨架**：左半屏標題 + 三段說明 / 右半屏 SVG 三層同心圓 + 標籤外引線。
**關鍵類**：`.system-diagram` `.sys-svg` `.sys-label`
**動效 recipe**：`system-diagram` — 同心圓從外向內 scale 入 → 標籤序列出現

---

### P18 · Why Now · 三列遞進 + 巨數

**用途**：三論點 + 各自支撐資料（為什麼是現在）。
**適用內容型別**：**3 個論點 + 每個論點對應一個量化資料**。每論點結構 = t-cat 標籤 + 一句標題 + 段落 + 一個底部巨數（可以是百分比/年份/倍數）。最後一列 IKB 藍強調錶示「重點支撐論據」。
**骨架**：頂部大標題 / 中段 3 列（每列：t-cat + 標題 + 描述）/ 列底各一個 8.4vw 巨數（01 / 02 / 03，最後一列 IKB 藍強調）。
**關鍵類**：`.why-now-grid` `.why-col` `.why-num-bottom`（8.4vw， weight 200）
**動效 recipe**：`why-now` — 三列垂直遞進 → 底部巨數 count-up
**注意**：巨數字號統一，只用色彩（IKB 藍）突出最後一列，**不要**用粗體

---

### P19 · Four Cards · 四列均分卡

**用途**：4 項功能/特性並列（等權重）。
**適用內容型別**：**4 項等權特性 / 模組**（數量 = 4，結構完全同質）。每項 = t-meta 編號 + 大字標題 + 一段描述。無資料維度，純定性。比 P5（三步）更平均，比 P6（資料高度）更純文字。
**骨架**：頂部 80px IKB 藍短髮絲頂線 + 大字雙行標題 / 下方 4 列均分卡（每卡：t-meta 頂部 "— 01 / SLASH" + 大字標題 + 段落描述）。
**關鍵類**：`.four-cards` `.fc-col`
**動效 recipe**：`four-cards` — 頂部藍線 width 0→100% → 4 列從下向上推入（每列 110ms 錯開）
**注意**：**不要**用 9px 圓形裝飾點（不符合直角語言），用 `.t-meta` 文字代替

---

### P20 · Stacked KPI Ledger · 縱向賬單 KPI

**用途**：4-6 行核心資料賬單式展示（每行=數字+標籤+圖示）。
**適用內容型別**：**4-6 項核心資料賬單**（每行必須有真實數值 + 標籤 + 圖示）。垂直 ledger 形式適合財務資料、KPI 儀表板、關鍵指標列表。比 P6 KPI Tower 容納資料更多但視覺化弱（無 bar 高度對比）。
**骨架**：每行一道 hairline 分隔 / 左側巨數（限高 `min(13vw,16vh)` 防溢位） / 中部標籤 / 右側 lucide 圖示。
**關鍵類**：`.stacked-ledger` `.ledger-row`（border-bottom:1px solid var(--border-subtle)）`.ledger-num`
**動效 recipe**：`stacked-ledger` — 每行數字升起 → 標籤左滑 → 圖示 pop（每行 180ms 錯開）
**注意**：**字號必須限高**（`font-size:min(13vw, 16vh)`），否則在標準 16:9 屏底部行會被擠出

---

### P21 · Tech Spec Sheet · 規格說明書

**用途**：產品規格、benchmark 資料、效能基線展示（多 KPI + 視覺化豎線裝飾）。
**適用內容型別**：**產品規格 / benchmark / 效能基線**（必須有真實多維資料，3 KPI + 9 根豎線 = 12+ 資料點）。典型如：模型評分、API 效能、壓測結果。是 deck 中資料密度最高的版式。
**骨架**：左 4 行大標題 / 中部 3 KPI（頂部 hairline + 數字 + 單位）/ 右下 9 根高低不一的垂直豎線 / 底部巨數 + Yearly goal + 三 tag + 右下角 MP-XX + 頁碼。
**關鍵類**：`.tech-spec` `.spec-title-col` `.spec-kpi-grid` `.spec-bars`（`.bar-vert`，scaleY 彈起，transform-origin:bottom）
**動效 recipe**：`tech-spec` — hero 區淡入 → 標題入 → KPI 頂線一根根畫出 → 底巨數 pop → 豎線從底部 scaleY 彈起（50ms 錯開）
**注意**：右下 bars 矩陣必須**靠下對齊**且**不超出右邊距**

---

### P22 · Image Hero · 圖文混排封面

**用途**：案例展示、產品圖 + 資料落地、章節封面帶圖。
**適用內容型別**：**案例展示 / 產品釋出 / 章節帶圖封面**（必須有真實圖片資源 + 3 個核心資料）。典型如：產品截圖 + 關鍵指標、案例圖 + ROI、使用者回饋圖 + 復購率。**沒有真實圖源時禁用**（佔位灰圖破壞視覺）。
**骨架**：上半屏 60% 全幅圖片 + 左上白底標題塊疊加（top:11vh，留出充分緩衝）/ 下半屏 40% 長說明 + 三列 KPI（$ / 127× / 100%）。
**關鍵類**：`.image-hero` `.hero-img-wrap`（60vh）`.hero-overlay-block` `.hero-stats`
**動效 recipe**：`image-hero` — 圖緩慢 zoom-out（scale 1.05→1）→ 白塊 scaleX 0→1 推開 → 三 KPI 頂線依序畫出
**注意**：
- 圖片優先用 `images/{頁號}-{語義}.png` 本地檔案（GPT-M 2.0 或使用者提供素材），不要預設外鏈 unsplash
- 圖片下方內容不要貼著圖下沿，使用 `.image-hero-body` 統一給下半屏增加頂部緩衝
- 三列 KPI 大字號要限高（`min(4.6vw, 7.6vh)`），小字用 `margin-top:auto` 錨定列底，防止溢到 nav 圓點
- 列高度統一（grid 不要 `align-items:start`，讓列拉伸到同一高度）

**範例程式碼**：
```html
<section class="slide light" data-animate="image-hero">
  <div class="canvas-card" style="padding:0;display:flex;flex-direction:column;overflow:hidden">
    <div data-anim="img" style="position:relative;flex:0 0 60%;overflow:hidden;background:var(--grey-1)">
      <img src="images/22-product-scene.png" alt="[必填] 圖片說明" loading="eager"
           style="position:absolute;inset:0;width:100%;height:100%;object-fit:cover;object-position:center 30%">
      <div class="chrome-min" style="position:absolute;top:0;left:0;right:0;color:rgba(255,255,255,.9);padding:5.6vh 5vw 0">
        <div class="l">Section · Case / Visual Evidence</div>
        <div class="r">22 / NN</div>
      </div>
      <div data-anim="title-block" style="position:absolute;left:5vw;top:11vh;background:var(--paper);padding:3.2vh 3.2vw;max-width:40vw">
        <div style="font-family:var(--sans),var(--sans-zh);font-weight:200;font-size:min(5.2vw,9vh);line-height:1;letter-spacing:-.035em;color:var(--text-primary)">
          [必填] Image<br>Evidence
        </div>
      </div>
    </div>
    <div data-anim="kpi" class="image-hero-body">
      <div style="max-width:48ch;font-family:var(--sans),var(--sans-zh);font-size:max(15px,1.3vw);line-height:1.55;font-weight:300;color:var(--text-primary);letter-spacing:-.005em">
        [必填] 1-2 行解釋這張圖為什麼重要，不要重複標題.
      </div>
      <div class="image-hero-stats" style="gap:4vw">
        <div style="display:flex;flex-direction:column;gap:.6vh"><div style="height:1px;background:var(--ink)"></div><div class="t-meta">Metric 01</div><div style="font-family:var(--sans);font-weight:200;font-size:min(4.6vw,7.6vh);line-height:.95;letter-spacing:-.04em">12×</div><div style="height:1px;background:var(--border-subtle);margin-top:auto"></div><p class="body-sm">[必填] 指標解釋</p></div>
        <div style="display:flex;flex-direction:column;gap:.6vh"><div style="height:1px;background:var(--ink)"></div><div class="t-meta">Metric 02</div><div style="font-family:var(--sans);font-weight:200;font-size:min(4.6vw,7.6vh);line-height:.95;letter-spacing:-.04em">3.4h</div><div style="height:1px;background:var(--border-subtle);margin-top:auto"></div><p class="body-sm">[必填] 指標解釋</p></div>
        <div style="display:flex;flex-direction:column;gap:.6vh"><div style="height:1px;background:var(--ink)"></div><div class="t-meta">Metric 03</div><div style="font-family:var(--sans);font-weight:200;font-size:min(4.6vw,7.6vh);line-height:.95;letter-spacing:-.04em;color:var(--accent)">100%</div><div style="height:1px;background:var(--border-subtle);margin-top:auto"></div><p class="body-sm">[必填] 指標解釋</p></div>
      </div>
    </div>
  </div>
</section>
```

---

## 歷史實驗區（預設禁用）

下面的 P23/P24 是早期為了探索圖文混排加入的實驗版式。它們不屬於原始 22P，預設不要用於正式產生。除非使用者明確說「我要實驗新圖文版式」，否則請使用 S22 或 S15/S16 的圖片槽位。

### P23 · Swiss Image Split · 左文右圖 / 右文左圖（實驗，預設禁用）

**用途**：解釋一個觀點時配一張紀實照片、資訊圖、UI 情景圖或系統關係圖。
**適用內容型別**：**一個核心論點 + 一張核心圖片**。適合"左側大標題 + 右側圖片證據"或"左圖右說明"。如果圖片是整頁主角且需要 KPI，用 P22；如果是多張圖片，用 P24。
**骨架**：`.canvas-card` 內 head 上下疊 / 主體 `.swiss-img-split` 兩列（5:7 或 reverse 7:5） / 圖片下方 `.swiss-img-caption`。
**關鍵類**：`.swiss-img-split` `.swiss-img-copy` `.frame-img.r-16x10.fit-contain|cover` `.swiss-img-caption`
**動效 recipe**：`grid-reveal` — head 先入，圖片和文字塊錯峰出現
**注意**：
- 圖片通常與正文首行對齊，不要與大標題頂端齊平；可在圖片列加 `padding-top:1vh` 到 `3vh`
- 如果希望左側內容塊與右側圖片底部對齊，使用 `.swiss-img-split.align-image-bottom`，不要靠額外空行硬推
- `.align-image-bottom` 已內建底部 nav safe zone；不要再額外把圖片或 caption 往頁面底部推
- 左側內容塊避免無意義分割線；除非需要章節感，不要額外插入 `.rule`
- 資訊圖/UI 圖必須 `.fit-contain`；紀實照片預設 cover
- 右圖寬度大，標題不要超過 3 行，正文控制在 2-3 個短段或 3 條 bullet

```html
<section class="slide light" data-animate="grid-reveal">
  <div class="canvas-card">
    <div class="chrome-min">
      <div class="l">Section · Visual Argument</div>
      <div class="r">23 / NN</div>
    </div>
    <div style="flex:1;padding:0;display:grid;grid-template-rows:auto 1fr;gap:5vh">
      <div data-anim="head" style="display:flex;flex-direction:column;gap:1.4vh">
        <div class="t-meta">Evidence · GPT-M 2.0</div>
        <h2 style="font-family:var(--sans),var(--sans-zh);font-weight:200;font-size:min(7vw,12vh);line-height:.96;letter-spacing:-.035em">[必填] 一句核心論點</h2>
      </div>
      <div class="swiss-img-split align-image-bottom" data-anim="up">
        <div class="swiss-img-copy">
          <div class="t-cat" style="color:var(--accent)">Why it matters</div>
          <p class="lead" style="font-weight:300;max-width:36ch">[必填] 2-3 行解釋圖片與論點的關係.</p>
          <div class="body" style="font-weight:300;color:var(--text-secondary)">[必填] 可以放 2-3 條短 bullet 或一段說明，保持靠左對齊和充足留白.</div>
        </div>
        <figure class="tile">
          <div class="frame-img r-16x10 fit-contain">
            <img src="images/23-visual-evidence.png" alt="[必填] 圖片說明">
          </div>
          <figcaption class="swiss-img-caption"><strong>[必填] 圖片標題</strong><span>16:10 · fit-contain</span></figcaption>
        </figure>
      </div>
    </div>
  </div>
</section>
```

---

### P24 · Swiss Evidence Grid · 多圖證據牆（實驗，預設禁用）

**用途**：三張同型別圖片/截圖/圖表並列，展示證據鏈或多案例對比。
**適用內容型別**：**2-3 張同類圖片**。適合 UI 截圖重繪、流程圖三段、三個案例實拍、三張資料小圖。不同型別混放會破壞瑞士風秩序。
**骨架**：head 上下疊 / `.swiss-img-grid` 三列 / 每張 tile 用同一個 `.h-22` 或 `.h-26`。
**關鍵類**：`.swiss-img-grid` `.frame-img.h-22|h-26` `.fit-contain` `.swiss-img-caption`
**動效 recipe**：`grid-reveal`
**注意**：
- 同組圖片必須同一比例、同一高度、同一邊距密度；不要一張 16:9、一張 4:3、一張長條截圖混排
- 標題區和圖片區之間必須有明顯緩衝；範本裡的 `.swiss-img-grid` 預設帶頂部間距，只有在外層 grid 已經給足 gap 時才加 `.tight`
- UI/資訊圖統一 `.fit-contain`；照片統一 cover
- 如果使用者原始截圖比例混亂，先按 `screenshot-framing.md` 做 CleanShot X 式程式化適配；只有太長、太窄或需要重構資訊時，才用 GPT-M 2.0 重產生同一比例的"截圖再設計"

```html
<section class="slide light" data-animate="grid-reveal">
  <div class="canvas-card">
    <div class="chrome-min">
      <div class="l">Section · Evidence Grid</div>
      <div class="r">24 / NN</div>
    </div>
    <div style="flex:1;padding:0;display:grid;grid-template-rows:auto 1fr;gap:6vh">
      <div data-anim="head" style="display:flex;flex-direction:column;gap:1.4vh">
        <div class="t-meta">Three visual proofs</div>
        <h2 style="font-family:var(--sans),var(--sans-zh);font-weight:200;font-size:min(6.6vw,11.6vh);line-height:.96;letter-spacing:-.035em">[必填] 三個證據，一個結論</h2>
      </div>
      <div class="swiss-img-grid" data-anim="up">
        <figure class="tile"><div class="frame-img h-26 fit-contain"><img src="images/24-proof-a.png" alt="[必填]"></div><figcaption class="swiss-img-caption"><strong>01</strong><span>[必填] 證據 A</span></figcaption></figure>
        <figure class="tile"><div class="frame-img h-26 fit-contain"><img src="images/24-proof-b.png" alt="[必填]"></div><figcaption class="swiss-img-caption"><strong>02</strong><span>[必填] 證據 B</span></figcaption></figure>
        <figure class="tile"><div class="frame-img h-26 fit-contain swiss-lined"><img src="images/24-proof-c.png" alt="[必填]"></div><figcaption class="swiss-img-caption"><strong>03</strong><span>[必填] 關鍵證據</span></figcaption></figure>
      </div>
    </div>
  </div>
</section>
```

---

## 選版式索引（給 LLM 的決策表）

| 內容意圖 | 推薦版式 |
|---|---|
| Deck 起手封面 | P1 Cover |
| 演化對比 / 時間軸（縱） | P2 Vertical Timeline |
| 一句口號 / 章節起 | P3 Statement / P10 Dot Matrix |
| 6 項概念定義 | P4 Six Cells |
| 三步流程（輕） | P5 Three Sub-cards |
| 4 項資料視覺化高度對比 | P6 KPI Tower |
| 5-10 項排名比較 | P7 H-Bar Chart |
| Before/After / 雙軌對照 | P8 Duo Compare |
| 整 deck 收尾 | P9 Closing Manifesto |
| 多步流程（橫，4-7 步） | P11 Horizontal Timeline |
| 階段性結論 + ink 通欄 | P12 Manifesto + Banner |
| 3 個對等概念深化 | P13 Three Forces Cards |
| 閉環流程 / 自學迴圈 | P14 Loop Diagram |
| 8-12 項矩陣 + 總資料 | P15 Image Matrix |
| 6 項快訊小卡 | P16 Multi-card Brief |
| 層級架構 / 同心圓系統 | P17 System Diagram |
| 三論點 + 資料支撐 | P18 Why Now |
| 4 項等權特性 | P19 Four Cards |
| 4-6 行賬單式 KPI | P20 Stacked Ledger |
| 產品規格 / benchmark | P21 Tech Spec |
| 案例圖 + 資料落地 | P22 Image Hero |
| 地點 / 路線 / 人物住所關係 | S08 + Swiss Map Component |
| 單圖解釋論點 / 圖文混排 | P23 Swiss Image Split |
| 2-3 張圖片/截圖/圖表證據鏈 | P24 Swiss Evidence Grid |

---

## 選版式 P0 原則：內容資料型別必須相符版式

> 這是寫 deck 時**最容易踩雷**的地方。版式承載內容的「形狀」是固定的——你必須先看內容，再選版式，**絕不能先選版式再編內容硬塞**。

| 內容型別 | 必須用 | 嚴禁用 |
|---|---|---|
| 有真實量化資料（百分比/數值） | P6 KPI Tower / P7 H-Bar / P20 Ledger / P21 Tech Spec | P3 / P4 / P10 / P13（無資料版式） |
| 無資料，純定性論斷 | P3 / P10 Statement / P12 / P13 / P19 | ⚠️ **P7 H-Bar / P6 KPI Tower**（編造資料會被識破） |
| 4 項對等 | P19 Four Cards / P6（若有資料） | 不能強湊成 6 用 P4 |
| 6 項對等 | P4 Six Cells / P16 Brief | 不能強湊成 4 用 P19 |
| 3 項對等 | P5 Sub-cards / P13 Three Forces | |
| Before/After | P8 Duo Compare（必須正好 2 項） | |
| 地點/路線/城市關係 | S08 + Swiss Map Component | 普通 S04/S16 卡片羅列 |
| 閉環結構 | P14 Loop Diagram | P11 橫向流程（線性 ≠ 閉環） |
| 三層巢狀 | P17 System Diagram | |
| 時間演化（有資料） | P2 Vertical Timeline | |
| 多步驟流程（無資料） | P11 Horizontal Timeline | |
| 8-12 項同類別 | P15 Image Matrix | |
| deck 收尾 | P9 Closing（每 deck 僅 1 次） | |
| 1 張核心圖片 + 一段解釋 | P23 Swiss Image Split | P22（除非圖片是主角且有 KPI） |
| 2-3 張同類圖片 | P24 Evidence Grid | P4/P16（文字卡片，不是圖片證據） |

**雷區案例**：用 P7 H-Bar Chart 展示「智慧補全 / 實時協作 / 自主代理」這種**無可比百分比的概念列舉**，編造 96/88/78 之類數字 → **資料不可信，版式濫用**。這種內容應該用 P2（若有時間維度）或 P3 Statement（若是論斷）。

---

## 常犯錯誤（P0 檢查項）

1. ❌ 給卡片加 `border-radius` → ✅ 必須直角
2. ❌ 在 `.card-accent` 上又加描邊 → ✅ 卡片填充型別互斥
3. ❌ 自己畫 SVG 圖示 → ✅ 用 `lucide` 線上庫，稜角風格
4. ❌ 時間線 dot 用 grid `justify-self` 對齊虛線 → ✅ axis 列固定 12px + dot 絕對定位
5. ❌ 大字號不限高（`13vw`）→ ✅ 永遠 `min(Xvw, Yvh)` 雙約束
6. ❌ ESC 索引頁縮圖看不到帶動效內容 → ✅ 給 cloned slide 加可見性 override CSS
7. ❌ 所有頁用同一個 fade-up recipe → ✅ 每頁一個語義化 recipe，與圖形耦合
8. ❌ 標題 + 卡片間距 < 5vh → ✅ 章節級標題至少 9vh
9. ❌ 9px 圓形裝飾點 → ✅ 8×8 直角小方塊 / mono `t-meta` 文字
10. ❌ 裝飾元素超出頁面邊距 → ✅ 嚴格在 grid 內，不貼邊
