# 質量檢查清單（Checklist）

這個清單來自"一人公司"分享 PPT 的真實迭代過程。每一條都是踩過坑之後總結的，按重要性排序。

產生 PPT 前，先通讀一遍；產生後，逐項自檢。

---

## 🔴 P0 · 一定不能犯的錯

### 0-S. Swiss locked mode：正文頁必須來自原始 22P

**現象**：色彩、字型看起來像 Swiss，但標題跑到中間、圖片不在網格上、頁面結構和原始 22P 完全不是一套東西。

**根因**：產生時把 Swiss 當成風格包，自由組成了新的 P23/P24/自繪 SVG 頁面，沒有從原始參考 PPT 的 22 個登記版式裡選。

**做法**：
- 先讀 `references/swiss-layout-lock.md`
- 正文頁只能使用 `S01-S22`；新增首頁/尾頁只能使用 `SWISS-COVER-ASCII` / `SWISS-CLOSING-ASCII`
- 每個 `<section class="slide">` 必須寫 `data-layout="Sxx"`
- 產生後必須執行：

```bash
node <SKILL_ROOT>/scripts/validate-swiss-deck.mjs path/to/index.html
```

**校驗會攔截**：
- 未登記版式 / 缺少 `data-layout`
- P23/P24 實驗結構
- SVG 裡寫可見文字
- S22 圖片未繫結 `s22-hero-21x9`
- S22 照片使用 `object-position:top center`

### 0-S-2. Swiss 頂部標題預設左上，不是居中

**現象**：最頂上的中文標題在頁面中間，像一頁自制海報，不再像原始 PPT。

**做法**：
- 除 `S03/S09/S10` 這類別 statement/split 版式外，頂部標題必須貼原始範本的左上內容軸。
- 不要把小標題放左列、大標題放右側大列，這會導致標題視覺居中。
- 如果需要標題 + 說明兩列，必須複製原始 `S11` 或 `S17` 的骨架，不要自寫 `4fr 8fr`。

### 0-S-3. Swiss 地圖頁必須用 S08 Map Component

**現象**：地點/歷史內容只畫了簡易 SVG 地圖，沒有真實點位、關係卡片、縮放/拖動控制，或滾輪觸發了 PPT 翻頁。

**做法**：
- 使用 `data-layout="S08"`。
- 先讀 `references/swiss-map-component.md`。
- 右側地圖元件必須包含 marker 點、連線線、地點卡片、`+` / `-` / `DRAG` 控制。
- 預設禁用 scroll zoom 和 drag pan；使用者點選 `DRAG` 後才允許拖動。
- 必須保留靜態 fallback，地圖 CDN 或瓦片失敗時仍可讀。

**檢查**：
- `grep -n "data-map-ctrl" index.html`
- `grep -n "maplibregl.Map" index.html`
- 瀏覽器實測 `+` 可放大，`DRAG` 可轉場為 `DRAG ON`

### 0-S-4. Swiss 簡報字號不能小到看不清 + 字重階梯必須遵守

**現象**：瑞士風頁面整體結構沒問題，但圖注、說明、時間線、KPI note、卡片小字在投屏時看不清；或者 16px 小字用了 weight 300 導致又小又細。

**做法（字號下限）**：
- 正文段落 / 主要說明 ≥ `18px`
- 卡片描述 / 列表 / 時間線說明 / caption / 圖注 ≥ `16px`
- meta / kicker / mono label / 圖表標籤 ≥ `14px`
- 內容超出時，先刪減文案、拆頁或換 Sxx 版式，不要用 10/11/12/13px 小字硬塞。

**做法（字重階梯 ⭐）**：
瑞士風堅持"越大越細，越小越粗"，字號與字重必須成反比階梯：
- ≥ 8vw → weight **200**（ExtraLight）
- 4-7.9vw → weight **200-300**
- 1.8-3.9vw → weight **300-400**
- 1-1.7vw / 16-20px → weight **400-500**
- 13-15px → weight **500-600**
- 同一頁內，字號小的元素字重必須 ≥ 字號大的元素。
- **16px 左右小字禁止使用 weight 300**（太細不可讀），最低 400，推薦 500。
- 封面/IKB 反白大標題內強調字用 `italic + weight 300`，不要用 accent 色。

**檢查**：
- `rg -n "font-size:(10px|11px|12px|13px)|max\\((9|10|11|12|13)px" index.html`
- `rg -n "font-weight:(300)" index.html | rg -v "min\(|h-xl|h-hero|h-statement|num-mega|kpi-thin|name-mega|8vw|9vw|1[1-9]vw|cover-|\.multi"` —— 檢查 weight 300 是否落在了小字號上
- 瀏覽器以 100% 縮放檢視，底部 note、caption、timeline label、卡片描述仍能一眼讀清。

### 0-A. 瑞士風畫布對齊法則（每一頁必查 · 最常踩）

**現象**：頁首 chrome-min 和底部 footer 都靠在 5vw 的邊線上，但中間區域往內縮了一截，左右對不齊。

**根因**：`.canvas-card` 已經自帶 `padding:5.6vh 5vw 4.4vh`。如果在主體區再寫 `padding:5vh 5vw 4vh`，水平方向就變成 `5vw + 5vw = 10vw`，主體比 chrome-min 多內縮 5vw。

**做法**：
- 主體那層 `padding:0`，只用 grid `gap` 控垂直間距
- chrome-min 與主體之間的間距由 `.chrome-min{margin-bottom:48px}` 提供，**不要**在主體頂部疊 `margin-top` / `padding-top`
- split 模式例外：`.slide.split .canvas-card{padding:0}`，兩個 `.half` 自己定 `padding:5.6vh 3.6vw 4.4vh`

```html
<!-- ❌ 錯：主體多縮了 5vw，左右對不齊 -->
<div class="canvas-card">
  <div class="chrome-min">...</div>
  <div style="flex:1;padding:5vh 5vw 4vh;...">主體</div>
</div>
<!-- ✅ 對 -->
<div class="canvas-card">
  <div class="chrome-min">...</div>
  <div style="flex:1;padding:0;display:grid;grid-template-rows:auto 1fr auto;gap:3vh">主體</div>
</div>
```

**自檢命令**：`grep "padding:.*5vw" index.html`，如果命中 `padding:Xvh 5vw Yvh` 在 canvas-card 直系子元素裡，就是錯的（.half / 裝飾層除外）。

### 0-B. 瑞士風 head 區：kicker 必須在大標題"上方"（不要左右排）

**現象**：小標題（`.t-meta` / `.t-cat`）和大標題被擠在同一行，左側一坨小字、右側一坨大字，頭部失去層級。

**根因**：`grid-template-columns:auto 1fr` 把兩個本該上下疊的元素壓成左右兩列。

**做法**：
```html
<!-- ❌ 錯 -->
<div data-anim="head" style="display:grid;grid-template-columns:auto 1fr;gap:3vw;align-items:end">
  <div class="t-meta">METHODOLOGY · 03</div>
  <h2 class="h-xl-zh">為什麼是 N+1</h2>
</div>
<!-- ✅ 對 -->
<div data-anim="head" style="display:flex;flex-direction:column;gap:1.4vh">
  <div class="t-meta">METHODOLOGY · 03</div>
  <h2 class="h-xl-zh">為什麼是 N+1</h2>
</div>
```

例外：head 一行同時承載"左：kicker+大標題（自己上下疊）"和"右：小注腳"，外層可以用 `display:grid;grid-template-columns:1fr auto`，但**內層**仍要保持 flex column。

### 0-B-2. 瑞士風封面 / 封底預設：IKB 滿屏 + ASCII 呼吸場 + 白色 weight 200（強制）

**現象**：封面用 `slide light` 白底 + 黑字 + 一個大大的"01"——同時 chrome 角標已經寫了 `01 / 07`，螢幕上出現兩個"01"，視覺重複；白底太普通，完全沒有"開場打招呼"的儀式感。

**根因**：layouts-swiss.md 舊版預設推薦左 ink + 右 paper 對開，實操中容易寫成"白底 + 黑大字 + 編號大字"，失去 IKB 這個標誌色的開場衝擊。

**做法**（瑞士風必守）：
- **封面強制 `<section class="slide accent">`**（滿屏 IKB），不要 `slide.light`，也不要 `slide.dark`；在 `.canvas-card` 內**第一個子元素**插入 `<canvas class="ascii-bg">`（ASCII 字元呼吸場，範本自帶 IIFE 自動啟用）
- **不要再寫"01"等編號大字**：`.chrome-min` 已經顯示 `01 / N`，封面再放一個巨大的"01"=同義重複，直接刪掉
- **強調字必須用斜體**：`font-style:italic;font-weight:300`，**禁止**用 `color:var(--accent)`——IKB 藍壓 IKB 藍，人眼看不見任何強調
- **封底強制 `slide.split`** 雙半屏，左半 `.half.b-accent` + ASCII canvas（與封面色彩閉環），右半 paper 白底放 3 條 takeaway；**第 03 條**用 `var(--accent)` 上色，完成"開場全 IKB ↔ 收尾半 IKB"的色彩閉環
- ASCII canvas 在範本的 `<style>` 裡已經預設 `mix-blend-mode:screen;opacity:.92`，不要去動這個值
- 封面/封底主標題字號雙約束：`min(11.6vw,19vh)` ~ `min(8vw,14vh)`（遵守 Y ≥ X × 1.6 規則）

**自檢命令**：
- `grep -c "ascii-bg" index.html`——封面 + 封底應至少命中 ≥ 2（各一個 canvas）
- `grep -E '"slide accent"' index.html | head -1`——封面應是 `slide accent` 而非 `slide light`
- `grep "color:var(--accent)" index.html`——若命中行同時含 `font-style:italic` 即危險訊號（藍壓藍），改為只 italic 不 accent；只有封底"03 takeaway"那一處用 `var(--accent)` 是合法的（此時背景是白色）
- 目視：開啟頁面看封面有沒有"01"等大編號——有就刪

### 0-C. 瑞士風大字號雙約束：`min(Xvw, Yvh)` 中 Y ≥ X × 1.6

**現象**：在 16:9 標準屏（MacBook 13/14/16，常見顯示器）開啟，標題字號比預期小一截，整頁內容顯得空曠或縮水。

**根因**：1vw : 1vh ≈ 1.78，如果寫 `min(7vw, 10vh)`，在 16:9 屏 7vw = 12.46vh，會被 10vh 上限截斷到 10vh，字號縮水 20%。

**做法**：推薦數值速查
| 用途 | 推薦 |
|---|---|
| h-hero 巨字宣言 | `min(11.6vw, 19vh)` |
| h-xl 章節標題 | `min(7vw, 12vh)` ~ `min(7.4vw, 13vh)` |
| 大數字 KPI | `min(8.4vw, 14vh)` |
| 中數字 / 編號 | `min(4.6vw, 8.5vh)` ~ `min(5.6vw, 10vh)` |
| 副標 | `min(7.6vw, 13vh)` |

**自檢命令**：`grep -E "font-size:min\([0-9.]+vw,\s*[0-9.]+vh\)" index.html`，把所有命中的 X/Y 看一眼，任何 Y/X < 1.6 都改大。

### 0-D. 瑞士風圖片混排：直角、同高、只做證據

**現象**：圖片像普通 PPT 插圖，圓角、陰影、比例混亂；多張截圖高度不一，或 GPT-M 2.0 產生圖自帶標題/頁尾，和頁面 chrome 重複。

**根因**：瑞士風的圖片不是裝飾，而是 grid 裡的證據塊。沒有先選原始版式和圖片槽位，就會把任意圖片硬塞進頁面。

**做法**：
- 先選版式：單張大圖 + KPI 用 `S22`；多圖用 `S15/S16` 的原始網格骨架改造
- S22 產生圖比例固定 `21:9`，並在 `<img>` 上寫 `data-image-slot="s22-hero-21x9"`
- 照片預設 `object-position:center 35%` 或 `center center`，不要用 `top center` 截人臉
- 圖片容器只用 `.frame-img`；**不要** `border-radius` / `box-shadow`
- UI / 資訊圖 / 流程圖若是使用者原始截圖或文字密集圖，使用 `.fit-contain`；若已按槽位重產生，必須用對應比例類鋪滿容器，例如 `.frame-img.r-21x9`，不能再用固定短高度把圖片縮小
- 多圖同組必須統一槽位、比例、高度，不要混用
- 使用者原始截圖要先讀 `references/screenshot-framing.md`：優先用 `assets/screenshot-backgrounds/` 內建主題背景 + 程式化縮放/留邊/對齊，不要為了比例統一就重畫截圖內容
- 截圖背景必須跟隨當前主題色，且可裁成 `21:9` / `16:10` / `4:3` / `1:1`；背景裡不能有標題、頁尾、邊框、logo、人物或明顯主體
- GPT-M 2.0 提示詞必須寫明：Swiss Style、單一 accent、直角、無漸層/陰影/圓角、無頁首頁尾標題角標

**自檢命令**：
- `grep -E "frame-img.*border-radius|box-shadow" index.html`——命中就刪
- `grep -n "data-image-slot" index.html`——每張本地圖片都應有槽位宣告
- 目視：圖片內部如果自帶大標題、頁碼、頁尾、角標，優先重產生，不要在頁面裡再裁切硬救
- 目視：截圖外側背景應該是安靜託底，不能比截圖本身更搶眼；Swiss 風截圖不得出現圓角和投影

### 0-D-2. 瑞士風底部分頁安全區：最低處不要碰 nav

**現象**：圖片 caption、腳註、timeline 下方 label、底部 KPI 被分頁小方塊擋住，或者視覺上貼得太近。

**根因**：`#nav` 固定在 `bottom:2vh`，如果主體內容用 `align-self:end` / `align-items:end` / `margin-top:auto` 貼到底，最低處會進入分頁區域。

**做法**：
- 主內容最低邊緣與分頁元件之間至少留 `3vh` 呼吸空間
- 圖文頁需要底部對齊時，先控制圖片高度，再給主體容器加 `.nav-safe-bottom` / `.nav-safe-bottom-tight`
- 其他頁面需要貼底時，給主體容器加 `.nav-safe-bottom` 或 `.nav-safe-bottom-tight`
- 不要手寫 `bottom:2vh` / `bottom:0` 放說明文字；這會和 nav 搶位置

**自檢**：
- 視覺：翻到該頁，看最後一行 caption/label 是否明顯高於分頁元件
- 程式碼：`grep -E "align-items:end|align-self:end|bottom:0|bottom:2vh|margin-top:auto" index.html`，命中後逐個確認是否有 nav safe zone

---

### 0-E. Swiss 範本還原度守衛：原始 PPT 是 golden source

**現象**：產生頁看起來像瑞士風，但和原始參考 PPT 的實際字重、間距、時間線、卡片密度不一致；越迭代越偏離參考。

**根因**：把新增圖片版式或實驗結構寫成了全域性樣式修改，或無意改動了原始基座類，例如 `.h-hero` / `.h-xl` 字重、`.tl-node` 列寬、`.duo-compare` 間距。

**做法**：
- 原始參考檔案 `/Users/guohao/Documents/op7418的儲存庫/專案/Thin-Harness-Fat-Skills/ppt/index.html` 是 Swiss 主題的 golden source，但要以**實際頁面用法**為準，不要只看未使用的 CSS helper
- 原始頁面的大標題大量使用 `font-weight:200`，強調詞/數字用 `300`；`.h-hero` / `.h-xl` / `.h-hero-zh` / `.h-xl-zh` 在本範本裡必須保持輕字重，不要還原成 800/900
- 除新增封面/封底 ASCII 機制、S22 圖片槽位修復、橫向時間線 label 居中修復、以及把標題 helper 校正為實際輕字重外，不要改動原始基座 CSS/JS recipe
- 新增圖片能力必須繫結到 S22/S15/S16 原始槽位，不要發明新正文結構
- 如果要修改 `assets/template-swiss.html`，先做原始參考對比；可接受差異只應是 ASCII 類、S22 圖片定位類、輕字重標題 helper 和已知動效修復

**自檢命令**：
- 執行本次測試目錄裡的 `compare-swiss-base.mjs`，確認輸出裡 `missing in template: 0`
- 目視對比原始 PPT 的同類頁面：大標題字重、chrome-min 位置、timeline dot/label、卡片密度必須一致

### 0-F. 視覺 + 程式碼雙核對：不要只看 HTML

**現象**：程式碼看起來類名正確，但實際頁面擁擠、圖文關係不對、可選元件堆太多，或者用了不適合內容的版式。

**做法**：
- 同時開啟原始參考 PPT、當前範本或產生頁、測試 PPT，先做視覺並排判斷
- 等入場動效穩定後再截圖或下判斷，不要把動畫中間態誤判成"內容缺失"
- 先開啟網頁逐頁看視覺：標題字重、頭部間距、正文密度、圖片對齊、nav 安全區
- 再回程式碼看結構：該頁是否用了正確版式，必選元件是否齊，可選元件是否過度
- 對照原始 PPT 時以實際畫面為準；raw CSS helper 只能輔助，不能替代視覺判斷
- 判斷問題來源：版式選錯 / 必選元件缺失 / 可選元件濫用 / 間距和安全區問題
- 通用版式（S03/S08/S11/S19）可多用；資料專用（S06/S07/S20/S21/S22）必須有真實資料或案例；結構專用（S14/S15/S17）必須有閉環、矩陣或層級關係
---

### 0. 產生前必須透過的類名校驗（最重要）

**現象**：直接把 layouts.md 的骨架粘到新 HTML，結果樣式全部丟失——大標題變成非襯線、資料大字報字型小得像正文、pipeline 多頁糊成一坨、圖片堆到瀏覽器底部。

**根因**：如果當前範本的 `<style>` 裡沒有這些類的定義，瀏覽器就 fallback 到預設樣式。

**做法**：
- **產生 PPT 前，必須先 `Read` 當前風格對應範本**：風格 A 讀 `assets/template.html`，風格 B 讀 `assets/template-swiss.html`，確認 layouts 裡用到的類都已定義
- 最常見遺漏的類：`h-hero / h-xl / h-sub / h-md / lead / meta-row / stat-card / stat-label / stat-nb / stat-unit / stat-note / pipeline-section / pipeline-label / pipeline / step / step-nb / step-title / step-desc / grid-2-7-5 / grid-2-6-6 / grid-2-8-4 / grid-3-3 / frame / frame-img / img-cap / callout / callout-src`
- 如果某個類確實缺了，**在範本的 `<style>` 裡補上**，不要在每頁 inline 重寫
- 產生後開啟瀏覽器，如果看到"大標題是非襯線"或"pipeline 步驟擠在一行"，幾乎 100% 是這個問題

### 1. 不要用 emoji 作圖示

**現象**：在中式雜誌風格里用 emoji（🎯 💡 ✅）會立刻破壞格調。

**做法**：用 Lucide 圖示庫，CDN 方式引用：

```html
<script src="https://unpkg.com/lucide@latest/dist/umd/lucide.min.js"></script>
...
<i data-lucide="target" class="ico-md"></i>
...
<script>lucide.createIcons();</script>
```

常用圖示名：`target / palette / search-check / compass / share-2 / crown / check-circle / x-circle / plus / arrow-right / grid-2x2 / network`

### 2. 圖片只允許裁底部，左右和頂部絕對不能切

**現象**：用 `aspect-ratio` 撐圖，網格會在父容器不足時堆疊或切掉圖片關鍵資訊（比如截圖上部的標題列）。

**做法**：圖片容器用**固定 height + overflow hidden**，圖片走 `object-fit:cover + object-position:top`：

```html
<figure class="frame-img" style="height:26vh">
  <img src="screenshot.png">
</figure>
```

CSS 裡 `.frame-img img` 已經預設 `object-position:top`，只裁底。

**絕不用這種寫法**（會在網格中撐破容器）：

```html
<!-- 壞例 -->
<figure class="frame-img" style="aspect-ratio: 16/9">...</figure>
```

**例外**：單張主視覺（非網格內）可以用 `aspect-ratio + max-height`，因為父容器會兜底。

### 2b. 亮頁面配暗 WebGL = 灰濛濛（主題轉場沒生效）

**現象**：所有 light 頁面背景都像蒙了一層灰，甚至 hero light 也灰。

**根因**：JS 根據 slide 的主題轉場兩張 canvas 的 opacity。如果整個 deck 開場是 hero dark，而沒有任何機制能把 bg 切到 light，body 永遠不加 `light-bg` 類，`canvas#bg-dark` 一直在上面。

**做法**：
- 範本裡 `go()` 函式已改為從 `classList` 推斷主題（`light` / `dark`），所以 **slide 必須明確帶 `light` 或 `dark` 類**。不要漏寫，更不要用其他自定義主題名
- hero 頁用 `hero light` / `hero dark`，正文頁用 `light` / `dark`。只寫 `hero` 不帶主題色是壞的
- 一個 deck 裡必須至少有一個 **非 hero 的 light 頁**，確保 body 有機會加 `light-bg`

### 2b-2. 整個 deck 全是 light，沒有節奏

**現象**：除封面 `hero dark` 外，其餘所有頁面預設寫 `light`——視覺平淡，沒有呼吸感，白花花一片。

**根因**：layouts.md 的骨架預設全寫 `light`，如果只是貼上骨架不調整主題，就會全亮。

**做法**：
- **產生前畫「主題節奏表」**：每一頁寫清 `hero dark` / `hero light` / `light` / `dark` 中的哪一個，對齊後再寫程式碼
- **硬規則**：連續 3 頁以上同主題 = 不允許；8 頁以上的 deck 必須有 ≥1 `hero dark` + ≥1 `hero light`；不能全是 `light` 正文頁——必須有 `dark` 正文頁
- **按佈局選主題**（詳見 layouts.md 開頭「主題節奏規劃」）：
  - 左文右圖（Layout 4）、大引用（Layout 8）、圖文混排（Layout 10）→ **`light` / `dark` 交替**
  - 大字報、圖片網格、Pipeline、對比頁 → `light`（截圖/數字/流程需要亮底）
  - 封面、問題頁 → `hero dark`
  - 章節幕封 → `hero dark` 與 `hero light` 交替
- **產生後自檢**：`grep 'class="slide' index.html`，目視確認節奏有交錯

### 2c. chrome 和 kicker 不要寫同一句話

**現象**：左上角 `.chrome` 寫「Design First · 設計先行」，同一頁裡 `.kicker` 又寫「Phase 01 · 設計階段」——同義翻譯，AI 味濃。

**做法**：
- **chrome = 雜誌頁首 / 導航標籤**：跨多頁可相同（如 "Act II · Workflow"、"Data · Result"、"lukew.com · 2026.04"）
- **kicker = 本頁獨一份的引導句**：短、有鉤子、是大標題的"小字首"（如 "BUT"、"一個人，做了什麼。"、"The Question"）
- 一個描述欄目，一個描述這一頁——絕不互相翻譯

### 3. 大標題字號不能超過屏寬 / 單字數

**現象**：中文大標題字號設太大（比如 13vw），結果每行只容 1 個字，強制換行非常難看。

**做法**：
- `h-hero`（最大）：10vw，**且標題長度 ≤ 5 字**
- `h-xl`（次大）：6vw-7vw
- 長標題用 `<br>` 手工斷行，不要相依性自動換行
- 必要時加 `white-space:nowrap`

**範例**：`我不是程式設計師。`（6 字）用 `h-xl` 7.2vw + nowrap，一行排完。

### 4. 字型分工：標題襯線、正文非襯線

**做法**：
- 大標題、重點 quote、數字大字 → **襯線字型**（Noto Serif TC + Playfair Display + Source Serif）
- 正文、描述、pipeline 步驟名 → **非襯線字型**（Noto Sans TC + Inter）
- 後設資料、程式碼、標籤 → **等寬字型**（IBM Plex Mono + JetBrains Mono）

所有字型用 Google Fonts CDN 引入，範本裡已預設。

### 4b. 圖片不要用 `align-self:end` 貼底

**現象**：左文右圖佈局裡，為了讓右列圖片和左列 callout 底部對齊，在 `<figure>` 上加 `align-self:end`。結果：
- 如果父容器不是 grid（比如類名沒定義），`align-self` 完全失效，圖片掉到文件流最下面被瀏覽器底欄遮擋
- 即使是 grid，圖片會在 cell 裡貼底，低分屏上仍然被 `.foot` 和 `#nav` 圓點遮擋

**做法**：
- 圖文混排**必須用 `.frame.grid-2-7-5`**（或 `.grid-2-6-6`/`.grid-2-8-4`）
- 右列 `<figure class="frame-img r-16x10">` 或 `<figure class="frame-img r-4x3">` 自然貼頂即可
- 要讓左列 callout 看起來"貼底"，給**左列**加 flex column + `justify-content:space-between`，不要動右列
- 如果圖片與大標題頂端齊平但正文從標題下方開始，給圖片加 `margin-top:7vh` 到 `9vh`，讓圖片跟正文內容區對齊

### 4c. 圖片不要用原圖奇葩比例

**現象**：`aspect-ratio: 2592/1798` 這種從原圖複製的比例，在不同螢幕下撐出奇怪的空白或溢位。

**做法**：無論原圖什麼比例，佔位器固定用標準比例 **16/10 / 4/3 / 3/2 / 1/1 / 16/9**。圖片自動 `object-fit:cover + object-position:top`，頂部不裁，底部裁掉一點無傷大雅。

### 5. 不要給圖片加厚邊框 / 陰影

**現象**：為了"高階感"加了強陰影或黑框，瞬間變成商務 PPT。

**做法**：最多 1-4px 的微圓角 + **極淡的底噪**（已在範本裡）。不要加 `box-shadow`，不要加 `border`（除非 1px 極淡的灰）。

---

## 🟡 P1 · 排版節奏

### 6. Hero 頁和非 hero 頁要交替

**推薦節奏**（25-30 頁）：
```
Hero Cover → Act Divider (hero) → 3-4 pages non-hero → Act Divider (hero)
→ 4-5 pages non-hero → Hero Question → ... → Hero Close
```

連續 2 頁以上 hero 會讓人疲勞，連續 4 頁以上 non-hero 會讓節奏死。

### 7. 大字報頁和密集頁要交替

大字報（big numbers / hero question）和密集頁（pipeline / image grid）交替出現，聽眾眼睛才不累。

### 8. 同一概念的英文/中文用法要統一

**現象**：一會兒寫 "Skills"，一會兒寫 "技能"，一會兒寫 "薄承載厚技能"，全篇不一致。

**做法**：
- 術語優先用**英文單詞**（Skills / Harness / Pipeline / Workflow），這些都是圈內熟悉詞
- **別硬翻譯**，硬翻譯反而生硬
- 整個 deck 裡同一個詞 1 個寫法

### 9. 底部 chrome 的頁碼要一致

用 `XX / 總頁數` 的格式（比如 `05 / 27`）。**不要在右上角加動態頁碼**（會和 `.chrome` 重複）。

### 9b. 動效系統：每一頁都要有 data-anim 標記

**現象**：產生後開啟瀏覽器，翻頁時內容直接"啪"地出來，沒有任何節奏感——雜誌風完全靠排版硬撐，少了層級展開的儀式感。

**根因**：完全沒給任何元素加 `data-anim`，Motion One 指令碼找不到可播的元素，整頁靜態出現。

**做法**：
- 所有正文頁，**至少給 kicker / 主標題 / lead / callout / stat-card / figure 這些葉子元素加 `data-anim`**
- **Hero 頁**（開場/幕封/問題/結尾）：所有核心塊（kicker + 大標題 + lead + meta-row）都要加
- **不需要特殊 recipe 的頁**：什麼也不用寫，預設 cascade 就好看
- **需要特殊 recipe 的 4 類頁**：必須在 `<section>` 上加對應 `data-animate`
  - 大引用 → `data-animate="quote"` + 每行 `<span data-anim="line" style="display:block">`
  - Before/After 對比 → `data-animate="directional"` + 左列 `data-anim="left"` + 右列 `data-anim="right"`
  - Pipeline 流水線 → `data-animate="pipeline"` + 每 step 加 `data-anim="step"`
  - Hero 頁（自動用 hero recipe，但仍需給元素加 `data-anim`）

**自檢**：產生後 `grep -c 'data-anim' index.html`，應該數十條以上。如果只有個位數，一定漏標了。

### 9c. Pipeline 頁必須加 data-animate="pipeline"

**現象**：流水線頁直接全部淡入，失去"一步步講"的節奏，但切到下一頁時又只能往前翻，沒法回到上一個 step。

**做法**：Layout 6 的 `<section>` 必須加 `data-animate="pipeline"`。簡報時按 →/空格/滾輪下滑可以**逐個點亮 step**，全部點亮之後再按 → 才會翻到下一頁。這個節奏是刻意的，不是 bug。

---

## 🟢 P2 · 視覺打磨

### 10. WebGL 背景的遮罩透明度

**dark hero**：遮罩 12-15%（WebGL 明顯透出）
**light hero**：遮罩 16-20%（WebGL 隱約可見，不搶字）
**普通 light/dark 頁**：遮罩 92-95%（幾乎不透）

如果頁面文字非常少（hero question），遮罩可以再薄些；如果正文密集，必須加厚遮罩確保可讀。

### 11. Light hero 的 shader 不能有強中心點

**現象**：Spiral Vortex、徑向漣漪在 light 主題下太顯眼，像 Windows 98 屏保。

**做法**：light hero 用 FBM 域扭曲驅動程式的無中心流動，底色保持銀/紙色（接近 #F0F0F0 / #FBF8F3），彩虹偏色 subtle（0.05 以下）。

### 12. Dark hero 允許更多視覺衝擊

Dark hero 可以用 Holographic Dispersion（鈦金色散）等帶中心結構的 shader，因為黑底能容納更多視覺資訊。

### 13. 左文右圖的對齊

- 左列的文字組 `justify-content:space-between`：標題貼頂，引用框貼底
- 右列圖片保持自然靠上對齊，不要加 `align-self:end`
- 右列圖片通常要跟正文內容區對齊，不是跟大標題頂端對齊；必要時加 `margin-top:7vh` 到 `9vh`
- 網格整體 `align-items:start`（不是 `center` / `end`）

### 13b. 標題與正文間距

- 頂部標題 + 下方長文章/引用/圖表的兩段式佈局，中間必須有明顯間距，推薦 `margin-top:6vh` 到 `8vh`
- 居中大標題頁必須整體水平置中，不要只讓文字塊靠左對齊居中擺放
- 複雜內容頁用大標題定調，下方內容用 grid / rowline 兩端對齊，不要把大標題、小標題、正文擠成一坨

### 13c. UI 情景圖不要拉成巨長條

- 單張 UI 截圖如果放滿寬後變成長條，優先拆成 2-3 個區域性面板
- 多面板拼排時每個 `.frame-img` 用同一個固定高度類，如 `.h-16` / `.h-18` / `.h-22`，不要用同一個超寬容器硬塞
- 同一組圖片的視覺大小必須一致，不要混用不同高度、不同縮放和不同邊距密度
- 如果確實需要全寬，必須產生比例足夠長的橫向圖片，並在 prompt 裡明確"ultra-wide horizontal strip"

### 13d. 產生配圖不要自帶 slide 元素

- GPT-M 2.0 產生的配圖只是嵌入素材，不要讓圖片自帶頁首、頁尾、標題、頁碼、角標、署名或裝飾邊框
- 流程圖/資訊圖只保留核心圖形和必要短標籤，PPT 自己負責標題、頁尾和 chrome
- 如果產生圖已經帶了這些元素，優先重產生；不要在 PPT 裡再疊一層 chrome 造成干擾

### 13e. Swiss 圖文混排不能只用一種

- 7-8 頁 Swiss 測試 deck 至少使用 6 個不同 S 編號版式
- 有 2-3 張配圖時，至少使用兩種圖片承載方式：S22 主視覺 / S15 矩陣 / S16 小報 / S08 對照圖文 / S19 四卡證據
- 左文右圖或右文左圖需要靠下對齊時，先控制圖片高度和主體安全區，不要把整塊內容推到分頁元件附近
- 白底資訊圖容器必須白底、無描邊；不要用灰框包白圖

### 13f. Swiss 中文大標題要降級

- 中文 2 行標題預設從 `min(5.8vw,10.2vh)` 起步，不要直接用英文頁的 `6.8vw-7vw`
- 任一行 9-12 箇中文字元時降到 `min(5.2vw,9.2vh)`
- 3 行標題優先改寫，不能為了標題大而擠掉下方圖文內容

### 14. 圖片的微弱圓角

風格 A 可以有輕微圓角。風格 B Swiss 必須直角： `.frame-img` 和圖片本身都不要圓角、陰影或消費 app 式卡片感。
---

## 🔵 P3 · 操作細節

### 15. 圖片路徑用相對路徑

圖片放在 `images/` 資料夾下，HTML 裡用相對路徑 `images/xxx.png`，不要用絕對路徑。

### 16. 頁碼在 `.chrome` 裡寫死

JS 會動態算總頁數並擴充底部翻頁圓點，但 `.chrome` 裡的 `XX / N` 是寫死的。加頁/刪頁時要手工改 N。

### 17. 翻頁導航要保留

範本預設支援：← → / 滾輪 / 觸屏滑動 / 底部圓點 / Home·End。不要刪 JS 裡的導航邏輯。

### 18. 不要用 `height:100vh` 硬設，用 `min-height:80vh`

`100vh` 會讓內容剛好卡滿螢幕，但瀏覽器工具列、標籤欄會吃掉一部分高度，導致內容溢位。用 `min-height:80vh + align-content:center` 更穩。

---

## 🧪 最終自檢清單

產生完 PPT 後，逐項對照這個清單（勾一下）：

```
預檢（產生前）
  □ 已讀過 template.html 的 <style>，確認所需類都存在
  □ 已決定每頁用哪個 Layout（1-10）
  □ 已畫出「主題節奏表」：每頁明確 hero dark / hero light / light / dark
  □ 節奏表滿足硬規則：無連續 3 頁同主題 / 有 ≥1 hero dark + ≥1 hero light（8 頁以上） / 至少有 1 個 dark 正文頁
  □ `<title>` 已改為實際 deck 標題（grep "[必填]" 應無結果）
  □ 瑞士風：封面是 `slide accent` 滿屏 IKB + `<canvas class="ascii-bg">`（不是 `slide light` 白底）
  □ 瑞士風：封底是 `slide split` + 左 `b-accent` + ASCII canvas / 右 paper 3 條 takeaway，第 03 條用 var(--accent)
  □ 瑞士風：`grep -c "ascii-bg" index.html` ≥ 2（封面 + 封底各一）
  □ 瑞士風：封面沒有"01"等大編號（chrome 已顯示 01/N，不要重複）
  □ 瑞士風：IKB 背景上的強調字用 `font-style:italic`，禁止用 `color:var(--accent)`（藍壓藍）

內容
  □ 每一幕的頁數比例合理（不會頭重腳輕）
  □ 沒有使用 emoji 作圖示
  □ Skills / Harness 等術語用法統一
  □ 每頁的 kicker + 標題 + 正文 三級資訊清晰

排版
  □ 所有大標題沒有出現 1 字 1 行的換行
  □ 圖片網格用 height:Nvh 而非 aspect-ratio
  □ 圖片只裁底部，頂部和左右完整
  □ 襯線/非襯線字型分工符合範本
  □ Pipeline 多組之間有明顯分隔

視覺
  □ hero 頁和 non-hero 頁交替
  □ WebGL 背景在 hero 頁可見
  □ 圖片有微弱圓角
  □ 沒有沉重的陰影和邊框

互動
  □ ← → 翻頁正常
  □ 底部圓點數量與總頁數相符
  □ chrome 裡的頁碼和實際頁號一致
  □ ESC 鍵觸發索引檢視（如果保留）
  □ B 鍵觸發靜態/低功耗模式，右下角提示在 `B 靜態` / `B 動態` 之間轉場

動效
  □ `assets/motion.min.js` 存在（本地兜底）
  □ 低功耗模式下 WebGL/ASCII canvas 不再掛 RAF 迴圈，當前頁內容仍全部可見
  □ 翻頁時內容逐個淡入，不是「啪」一下全出
  □ 大引用頁 `<section>` 帶 `data-animate="quote"`，每行 `<span data-anim="line">`
  □ Before/After 對比頁 `<section>` 帶 `data-animate="directional"`，左右列標 left/right
  □ Pipeline 頁 `<section>` 帶 `data-animate="pipeline"`，每 step 標 data-anim="step"
  □ `grep -c 'data-anim' index.html` 數量 ≥ 頁數 × 3（平均每頁 3 個以上標記）
```

全勾完，才是合格的 PPT。
