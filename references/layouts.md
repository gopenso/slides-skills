# 頁面佈局庫（Layouts）

本文件收錄 10 種最常用的頁面佈局骨架。每種都是一個完整可貼上的 `<section class="slide ...">...</section>` 程式碼塊，直接取代文案/圖片即可使用。

---

## ⚠️ 產生前必讀（Pre-flight）

### A. 類名必須來自 template.html

layouts.md 使用的所有類（`h-hero` / `h-xl` / `h-sub` / `h-md` / `lead` / `meta-row` / `stat-card` / `stat-label` / `stat-nb` / `stat-unit` / `stat-note` / `pipeline-section` / `pipeline-label` / `pipeline` / `step` / `step-nb` / `step-title` / `step-desc` / `grid-2-7-5` / `grid-2-6-6` / `grid-2-8-4` / `grid-3-3` / `grid-6` / `grid-3` / `grid-4` / `frame` / `frame-img` / `img-cap` / `callout` / `callout-src` / `kicker`）都在 `assets/template.html` 的 `<style>` 塊裡預定義。

**不要發明新類名**。如果必須自定義，用 `style="..."` inline 寫。產生前若不確定某個類是否存在，grep template.html 確認。

### B. 圖片比例規範（非常重要）

**永遠用標準比例**，不要用原圖 `aspect-ratio: 2592/1798` 這種奇葩比例：

| 場景 | 推薦比例 | 寫法 |
|------|---------|------|
| 左文右圖 主圖 | 16:10 或 4:3 | `.frame-img.r-16x10` 或 `.frame-img.r-4x3` |
| 圖片網格（多圖對比） | 統一 | `.frame-img.h-22` / `.frame-img.h-26`，不用 aspect-ratio |
| 小型面板組 | 統一 | `.frame-img.h-16` / `.frame-img.h-18`，同組必須同高 |
| 左小圖 + 右文字 | 1:1 或 3:2 | `.frame-img.r-1x1` 或 `.frame-img.r-3x2` |
| 全螢幕主視覺 | 16:9 | `.frame-img.r-16x9` |
| 資訊圖 / 截圖再設計 | 16:9 或 16:10 | `.frame-img.r-16x9.fit-contain` 或 `.frame-img.r-16x10.fit-contain` |
| 圖文混排小插圖 | 3:2 或 3:4 | `.frame-img.r-3x2` 或 `.frame-img.r-3x4` |

圖片必須包在 `<figure class="frame-img">` 裡。預設照片會 `object-fit:cover + object-position:top center`，只裁底部，不裁頂/左/右。資訊圖和截圖再設計必須加 `.fit-contain`，避免文字或標註被裁切。

### B2. 圖片與內容的垂直對齊

圖片應該跟正文內容區對齊，不要預設貼到大標題頂端。特別是左文右圖和圖文混排頁：

- 如果左列是 kicker + 大標題 + 正文 + callout，右列圖片通常從正文高度開始，可給圖片加 `style="margin-top:7vh"` 到 `9vh`
- 如果圖片是資訊圖或 UI 情景圖，優先對齊正文首行或說明文字，不要和超大標題頂端齊平
- 如果一張截圖/UI 情景圖在橫向頁面裡變成很長的條，不要硬拉滿寬；改成極寬橫圖素材，或拆成 2-3 個區域性面板拼排
- 多圖面板必須使用同一個高度類，不要混用 `h-16` / `h-22` 或手寫不同 `height`

### B3. 標題與正文的間距

- 兩段式頁面（頂部標題 + 下方長正文/引用/圖表）必須在標題和內容之間留出明顯間距，推薦 `margin-top:6vh` 到 `8vh`
- 居中大標題頁必須讓主標題在頁面水平置中，使用 `.center` 或 `text-align:center; margin-inline:auto`
- 複雜內容頁（大標題 + 小標題 + 詳細內容）要讓大標題和下方內容分層，下方內容使用左右兩端對齊的 grid 或 rowline，不要全部堆在一條中軸線上

### C. 圖片定位準則（避免圖片堆到頁面最底部、被瀏覽器工具列遮擋）

**錯誤做法**（已踩坑，不要再犯）：
- 在非 grid 容器裡用 `align-self:end`：`align-self` 在 flex/grid 之外完全無效，圖片會掉到文件流末尾堆底
- 用 `position:absolute + bottom:0` 把圖"固定"到底：會被底部 `.foot` 和 `#nav` 圓點遮擋
- 單張圖片只寫 `height:N vh` 不限 `max-height`：在低分屏會撐出視口

**正確做法**：
- 圖文混排**必須用 `.frame.grid-2-7-5`**（或 `.grid-2-6-6` / `.grid-2-8-4`）的 grid 結構
- grid 容器預設 `align-items:start`（已在 template 中設定），圖片自然貼到 cell 頂端
- 如果需要"圖片靠下對齊左列 callout"：**左列用 flex column + `justify-content:space-between`**（讓 callout 自己貼左列底），**右列 figure 直接保持 align-items:start 即可**，不要加 `align-self:end`
- 所有 grid 父容器建議加 inline `style="padding-top:6vh"`，給標題區留呼吸空間

### D. 主題色與主題節奏

- 主題色從 `references/themes.md` 的 5 套預設裡選一套，不允許自定義 hex 值
- 主題節奏（每頁用 light / dark / hero light / hero dark 哪一個）在下文「主題節奏規劃」一節有硬規則，產生前必讀
- 兩件事都要在挑佈局之前決定，避免返工

### E. 動效系統（預設開啟 · Motion One 驅動程式）

**核心機制**：template.html 底部的 module script 會在翻頁時觸發入場動畫。所有帶 `data-anim` 的元素初始不可見，翻到當前頁時由 Motion One 逐個淡入。

**動效策略**：在 `<section>` 上加 `data-animate="<recipe>"` 選取動畫風格；每個需要入場動畫的元素加 `data-anim`（可選附值，如 `left` / `right` / `line` / `step`）。

| recipe | 用法 | 適合佈局 |
|---|---|---|
| 預設（cascade） | 什麼也不加，自動級聯淡入 | 大部分正文頁（Layout 3 / 4 / 5 / 10） |
| `hero` | `.hero` 頁自動啟用，節奏更慢更儀式感 | Layout 1 / 2 / 7（所有 hero 頁） |
| `quote` | 一句一句揭示，慢節奏（550ms stagger） | Layout 8 大引用 |
| `directional` | 左進 → 分割 → 右進，用於對比 | Layout 9 Before/After |
| `pipeline` | 手動推進，按 →/空格 一步步點亮 | Layout 6 流水線 |

**降級保底**：如果 motion.min.js 本地 + CDN 都載入失敗，指令碼會強制把所有 `data-anim` 元素設為 `opacity:1`，內容永遠可讀。

**不需要動效的頁面**：如果某頁想完全跳過動效，不加任何 `data-anim` 即可 —— Motion One 只對帶標記的元素生效。

---

## 0. 基礎結構（所有 slide 都一樣）

```html
<section class="slide [light|dark|hero light|hero dark]">
  <div class="chrome">
    <div>上下文標籤 · 子標籤</div>
    <div>ACT · 頁號 / 總頁數</div>
  </div>
  <!-- 主內容 -->
  <div class="foot">
    <div>頁碼說明 · Page Description</div>
    <div>— · —</div>
  </div>
</section>
```

- 非 hero 頁建議加 `light` 或 `dark` 主題；hero 頁加 `hero light` 或 `hero dark`（參與 WebGL 主題插值）
- `chrome` 和 `foot` 是可選但推薦保留的上下左右四角後設資料
- **hero 頁用於章節封面/開場/收束/轉場**，非 hero 頁用於正文

### ⚠️ chrome 和 kicker 不要寫同一句話

這是最常見的內容重複問題。兩者在語義上完全不同的維度：

| 位置 | 角色 | 內容性質 | 例子 |
|------|------|---------|------|
| `.chrome` 左上 | **雜誌頁首 / 導航後設資料** | 穩定的"欄目名"或"章節分類"，跨多頁可以相同 | "Act II · Workflow" / "Data · Result" / "lukew.com · 2026.04" |
| `.chrome` 右上 | **頁號 + 幕號** | 固定格式 | "Act II · 15 / 25" |
| `.kicker` | **這一頁獨一份的引導句** | 是大標題的"小字首"，像雜誌大標題上方的一行話，每頁都應不同 | "BUT" / "一個人，做了什麼。" / "The Question" |

**反例**（已踩坑）：chrome 寫「設計先行 · Design First」，kicker 又寫「Phase 01 · 設計階段」——意思重複，讀者一眼就覺得 AI 產生的。

**正確做法**：chrome 是**欄目標籤**（穩定、跨頁可複用），kicker 是**本頁鉤子**（短句、有戲劇性），兩者互為補充，不互相翻譯。

### ⚠️ 主題節奏規劃（必讀 · 產生前必做）

**核心機制**：每頁 `<section>` 必須帶 `light` / `dark` / `hero light` / `hero dark` 之一。JS 根據 class 推斷主題，決定 body 加不加 `light-bg`，從而轉場暗/亮兩張 WebGL canvas 哪張在前。不帶主題或寫自定義名 = fallback 出錯。

#### 按佈局的主題預設值

| Layout | 預設主題 | 原因 |
|---|---|---|
| 1. 開場封面 | `hero dark` | 開場儀式感，暗底強衝擊 |
| 2. 章節幕封 | `hero dark` 與 `hero light` **必須交替** | 呼吸節奏 |
| 3. 大字報（資料） | `light` | 數字需紙白底；多幕連發時可偶插 `dark` |
| 4. 左文右圖 | **`light` / `dark` 交替** | 正文節奏主力 |
| 5. 圖片網格 | `light` | 截圖需亮底 |
| 6. Pipeline | `light` | 流程圖需清晰 |
| 7. 問題頁 | `hero dark` | 強視覺衝擊預設 |
| 8. 大引用 | **`dark` 優先**，偶用 `light` | 金句儀式感靠暗底 |
| 9. 對比頁 | `light` | 雙列需清晰 |
| 10. 圖文混排 | **`light` / `dark` 交替** | 節奏 |

#### 節奏硬規則（產生後 grep 自檢）

- ❌ **禁止**連續 3 頁以上相同主題（包括 light 堆疊和 dark 堆疊）
- ❌ **禁止**8 頁以上的 deck 沒有至少 1 個 `hero dark` + 1 個 `hero light`
- ❌ **禁止**整個 deck 只有 `light` 正文頁沒有任何 `dark` 正文頁——會顯得平淡、沒呼吸
- ✅ **推薦**每 3-4 頁插入 1 個 hero（封面/幕封/問題/大引用）

#### 8 頁節奏範本（可直接套用）

| 頁 | 主題 | 佈局 | 備註 |
|---|---|---|---|
| 1 | `hero dark` | 封面 | 開場 |
| 2 | `light` | 大字報 | 資料丟擲 |
| 3 | `dark` | 左文右圖 | 對比/故事 |
| 4 | `light` | Pipeline | 流程 |
| 5 | `hero light` | 章節幕封 | 呼吸 |
| 6 | `dark` | 左文右圖 or 大引用 | |
| 7 | `hero dark` | 問題頁 | 懸念收束 |
| 8 | `light` | 大引用/結尾 | 收尾 |

**先畫這張表對齊，再動手寫 slide**。跳過規劃直接粘骨架 = 全是 light。

---

## Layout 1: 開場封面（Hero Cover）

```html
<section class="slide hero dark">
  <div class="chrome">
    <div>A Talk · 2026.04.22</div>
    <div>Vol.01</div>
  </div>
  <div class="frame" style="display:grid; gap:4vh; align-content:center; min-height:80vh">
    <div class="kicker" data-anim>私人分享會 · 李繼剛</div>
    <h1 class="h-hero" data-anim>一人公司</h1>
    <h2 class="h-sub" data-anim>被 AI 摺疊的組織</h2>
    <p class="lead" style="max-width:60vw" data-anim>
      一個 AI 創作者 —— 在 64 天裡做了 11 萬行程式碼、在 9 個平台上持續輸出，生活節奏幾乎沒有被改變。
    </p>
    <div class="meta-row" data-anim>
      <span>歸藏 Guizang</span><span>·</span><span>獨立創作者 / CodePilot 作者</span>
    </div>
  </div>
  <div class="foot">
    <div>一場關於 AI · 組織 · 個體的分享</div>
    <div>— 2026 —</div>
  </div>
</section>
```

**要點**：
- 用 `hero dark` 讓 WebGL 背景在大部分割槽域透出
- `h-hero` 是最大字號（10vw），這裡作標題主視覺
- 用 `min-height:80vh + align-content:center` 讓內容整體垂直置中
- 不需要 `.chrome` 裡寫頁碼，封面頁自成一體

---

## Layout 2: 章節幕封（Act Divider）

```html
<section class="slide hero light">
  <div class="chrome">
    <div>第一幕 · 硬資料</div>
    <div>Act I · 01 / 25</div>
  </div>
  <div class="frame" style="display:grid; gap:6vh; align-content:center; min-height:80vh">
    <div class="kicker" data-anim>Act I</div>
    <h1 class="h-hero" style="font-size:8.5vw" data-anim>硬資料</h1>
    <p class="lead" style="max-width:55vw" data-anim>
      先看數字，再談方法。
    </p>
  </div>
  <div class="foot">
    <div>第一幕引子</div>
    <div>— · —</div>
  </div>
</section>
```

**要點**：
- 極簡，只需要 kicker + 大標題 + 一行引語
- 兩個幕的封面可以交替 `hero light` / `hero dark`，製造節奏
- `h-hero` 字號可以從 10vw 調到 8.5vw 適配長短

---

## Layout 3: 資料大字報（Big Numbers Grid）

```html
<section class="slide light">
  <div class="chrome">
    <div>過去 64 天 · 開發篇</div>
    <div>Act I / Dev · 02 / 25</div>
  </div>
  <div class="frame" style="padding-top:6vh">
    <div class="kicker" data-anim>一個人，做了什麼。</div>
    <h2 class="h-xl" data-anim>過去 64 天</h2>
    <p class="lead" style="margin-bottom:5vh" data-anim>從 0 到開源 CodePilot。</p>

    <div class="grid-6" style="margin-top:6vh">
      <div class="stat-card" data-anim>
        <div class="stat-label">Duration</div>
        <div class="stat-nb">64 <span class="stat-unit">天</span></div>
        <div class="stat-note">從 0 到現在</div>
      </div>
      <div class="stat-card" data-anim>
        <div class="stat-label">Lines of Code</div>
        <div class="stat-nb">110K+</div>
        <div class="stat-note">一行行寫到 11 萬+</div>
      </div>
      <div class="stat-card" data-anim>
        <div class="stat-label">GitHub Stars</div>
        <div class="stat-nb">5,166</div>
        <div class="stat-note">一個開源儲存庫</div>
      </div>
      <div class="stat-card" data-anim>
        <div class="stat-label">Downloads</div>
        <div class="stat-nb">41K+</div>
        <div class="stat-note">裝到了幾萬臺電腦裡</div>
      </div>
      <div class="stat-card" data-anim>
        <div class="stat-label">AI Providers</div>
        <div class="stat-nb">19</div>
        <div class="stat-note">跨平台接入</div>
      </div>
      <div class="stat-card" data-anim>
        <div class="stat-label">Commits</div>
        <div class="stat-nb">608+</div>
        <div class="stat-note">沒有協作者</div>
      </div>
    </div>
  </div>
  <div class="foot">
    <div>專案 · CodePilot　|　github.com/codepilot</div>
    <div>Act I · Dev Numbers</div>
  </div>
</section>
```

**要點**：
- 3×2 或 4×2 網格最穩（見 `.grid-6`）
- 每個 `stat-card` 結構固定：label（英文小字）→ nb（大字數字）→ note（註釋）
- 數字建議 2-3 位字元（太長會溢位），用 K / M 簡寫
- 留 5vh 以上的上方緩衝，讓標題區先搶眼球

---

## Layout 4: 左文右圖（Quote + Image）

```html
<section class="slide light">
  <div class="chrome">
    <div>身份反差 · The Twist</div>
    <div>03 / 25</div>
  </div>
  <div class="frame grid-2-7-5" style="padding-top:6vh">
    <!-- 左列：標題 + 正文 + callout，flex column 讓 callout 貼列底 -->
    <div style="display:flex; flex-direction:column; justify-content:space-between; gap:3vh">
      <div>
        <div class="kicker" data-anim>BUT</div>
        <h2 class="h-xl" style="white-space:nowrap; font-size:7.2vw" data-anim>
          我不是程式設計師。
        </h2>
        <p class="lead" style="margin-top:3vh" data-anim>
          大學畢業之後再也沒寫過一行程式碼。過去十年做的是 UI 設計和 AI 特效。
        </p>
      </div>
      <div class="callout" data-anim>
        "這東西在三年前，<br>
        需要一個十人團隊做一年。"
        <div class="callout-src">— 一個觀察者的判斷</div>
      </div>
    </div>
    <!-- 右列：圖片用標準 16/10 比例 + max-height，不要 align-self:end -->
    <figure class="frame-img r-16x10" data-anim>
      <img src="images/codepilot.png" alt="CodePilot 產品截圖">
      <figcaption class="img-cap">CodePilot · 產品截圖</figcaption>
    </figure>
  </div>
  <div class="foot">
    <div>Page 03 · 我不是程式設計師</div>
    <div>— · —</div>
  </div>
</section>
```

**要點**：
- 用 `grid-2-7-5`（左 7 份、右 5 份），`align-items:start` 已在 template 預設
- **左列**用 flex column + `justify-content:space-between`：標題貼頂，callout 自然貼底
- **右列圖片** **不要加 `align-self:end`**。會讓圖片滑到 cell 底部，低分屏下被瀏覽器工具列遮擋
- 圖片必須用 **標準比例類別 `.r-16x10` 或 `.r-4x3`**，不要用原圖奇葩比例（`2592/1798` 這種）

---

## Layout 5: 圖片網格（多圖對比）

```html
<section class="slide light">
  <div class="chrome">
    <div>平台追蹤者實證</div>
    <div>Act I / Ops · 05 / 27</div>
  </div>
  <div class="frame" style="padding-top:5vh">
    <div class="kicker" data-anim>Proof · 追蹤者實證</div>
    <h2 class="h-xl" data-anim>10 個平台 · 6 張截圖</h2>

    <div class="grid-3-3" style="margin-top:4vh">
      <figure class="frame-img" style="height:26vh" data-anim>
        <img src="images/threads.png" alt="Threads 289K">
        <figcaption class="img-cap">Threads · 289K</figcaption>
      </figure>
      <figure class="frame-img" style="height:26vh" data-anim>
        <img src="images/x.png" alt="X 137K">
        <figcaption class="img-cap">X · 137K</figcaption>
      </figure>
      <figure class="frame-img" style="height:26vh" data-anim>
        <img src="images/line.png" alt="Line 96K">
        <figcaption class="img-cap">Line · 96K</figcaption>
      </figure>
      <figure class="frame-img" style="height:26vh" data-anim>
        <img src="images/dcard.png" alt="Dcard 26K">
        <figcaption class="img-cap">Dcard · 26K</figcaption>
      </figure>
      <figure class="frame-img" style="height:26vh" data-anim>
        <img src="images/ig.png" alt="IG 19K">
        <figcaption class="img-cap">IG · 19K</figcaption>
      </figure>
      <figure class="frame-img" style="height:26vh" data-anim>
        <img src="images/tiktok.png" alt="TikTok 10K">
        <figcaption class="img-cap">TikTok · 10K</figcaption>
      </figure>
    </div>
  </div>
  <div class="foot">
    <div>截圖時間 · 2026.04</div>
    <div>Page 05 · 追蹤者實證</div>
  </div>
</section>
```

**要點**：
- 關鍵：每個 `frame-img` 必須寫死 `height:NNvh`（不要用 `aspect-ratio`），否則網格會撐破
- 圖片會自動 `object-fit:cover + object-position:top`，只裁底部
- 用 `.grid-3-3`（3×2）或 `.grid-3`（3×1）承載

---

## Layout 6: 兩列流水線（Pipeline）

```html
<section class="slide light" data-animate="pipeline">
  <div class="chrome">
    <div>我的工作流 · Workflow</div>
    <div>Act II · 15 / 27</div>
  </div>
  <div class="frame">
    <div class="kicker">Pipeline · 流水線</div>
    <h2 class="h-xl">兩條流水線</h2>

    <!-- 第一組：文字側 -->
    <div class="pipeline-section">
      <div class="pipeline-label">文字側 · Text Pipeline</div>
      <div class="pipeline">
        <div class="step" data-anim="step">
          <div class="step-nb">01</div>
          <div class="step-title">Draft</div>
          <div class="step-desc">AI 幫我起草初稿</div>
        </div>
        <div class="step" data-anim="step">
          <div class="step-nb">02</div>
          <div class="step-title">Polish</div>
          <div class="step-desc">AI 潤色去 AI 味</div>
        </div>
        <div class="step" data-anim="step">
          <div class="step-nb">03</div>
          <div class="step-title">Morph</div>
          <div class="step-desc">AI 變形成 X / IG</div>
        </div>
        <div class="step" data-anim="step">
          <div class="step-nb">04</div>
          <div class="step-title">Illustrate</div>
          <div class="step-desc">AI 產生資訊圖</div>
        </div>
        <div class="step" data-anim="step">
          <div class="step-nb">05</div>
          <div class="step-title">Distribute</div>
          <div class="step-desc">一鍵分發 9 平台</div>
        </div>
      </div>
    </div>

    <!-- 第二組：影片側 -->
    <div class="pipeline-section">
      <div class="pipeline-label">視覺 · 影片側 · Video Pipeline</div>
      <div class="pipeline">
        <div class="step" data-anim="step">
          <div class="step-nb">06</div>
          <div class="step-title">Cut</div>
          <div class="step-desc">AI 幫我剪輯</div>
        </div>
        <div class="step" data-anim="step">
          <div class="step-nb">07</div>
          <div class="step-title">Wrap</div>
          <div class="step-desc">AI 幫我包裝</div>
        </div>
        <div class="step" data-anim="step">
          <div class="step-nb">08</div>
          <div class="step-title">Cover</div>
          <div class="step-desc">AI 產生封面</div>
        </div>
      </div>
    </div>
  </div>
  <div class="foot">
    <div>Page 15 · 我的內容工廠</div>
    <div>Workflow</div>
  </div>
</section>
```

**要點**：
- 用 `.pipeline-section` 分組 + `.pipeline-label` 作組標題
- 兩組之間用 3.6vh 的間距 + 頂部細分隔線（已在 CSS 中預設）
- 每個 step 是固定的 nb → title → desc 結構
- 步驟數不限但單行最好 ≤5 個，否則換到第二 pipeline
- **動效**：`<section>` 加 `data-animate="pipeline"`，每個 `.step` 加 `data-anim="step"`。翻到此頁時步驟預設 `opacity:.15`，按 →/空格/滾輪下滑時一次點亮一個 step；**所有 step 點亮完才會翻到下一頁**，可製造演講互動感

---

## Layout 7: 懸念收束 / 問題頁（Hero Question）

```html
<section class="slide hero dark">
  <div class="chrome">
    <div>留給你的問題</div>
    <div>24 / 27</div>
  </div>
  <div class="frame" style="display:grid; gap:8vh; align-content:center; min-height:80vh">
    <div class="kicker" data-anim>The Question</div>
    <h1 class="h-hero" style="font-size:7vw; line-height:1.15">
      <span data-anim style="display:block">你的公司裡，</span>
      <span data-anim style="display:block">哪些崗位本來就</span>
      <span data-anim style="display:block">不該由人來做？</span>
    </h1>
    <p class="lead" style="max-width:50vw" data-anim>
      這個問題，不是技術問題，是架構問題。
    </p>
  </div>
  <div class="foot">
    <div>Page 24 · The Question</div>
    <div>— · —</div>
  </div>
</section>
```

**要點**：
- Hero 頁留白越多越好，只放一個問題
- `h-hero` 字號視長度調整（7vw 適合 3 行，10vw 適合 1 行）
- 用 `<br>` 手工斷行，確保斷點在語義處
- 尾巴可以再給一行 `lead` 作為點破

---

## Layout 8: 大引用頁（Big Quote · 襯線金句）

```html
<section class="slide light" data-animate="quote">
  <div class="chrome">
    <div>The Takeaway · 核心金句</div>
    <div>18 / 25</div>
  </div>
  <div class="frame" style="display:grid; gap:5vh; align-content:center; min-height:80vh">
    <div class="kicker" data-anim>Quote · 金句</div>
    <blockquote style="font-family:var(--serif-zh); font-weight:700; font-size:5.8vw; line-height:1.2; letter-spacing:-.01em; max-width:72vw">
      <span data-anim="line" style="display:block">"沒有交接，</span>
      <span data-anim="line" style="display:block">所有人都在建置。"</span>
    </blockquote>
    <p class="lead" style="max-width:55vw; opacity:.65" data-anim>
      Without the handoff, everyone builds.<br>
      And that makes all the difference.
    </p>
    <div class="meta-row" data-anim>
      <span>— Luke Wroblewski</span><span>·</span><span>2026.04.16</span>
    </div>
  </div>
  <div class="foot">
    <div>Page 18 · 金句</div>
    <div>— · —</div>
  </div>
</section>
```

**要點**：
- 整頁留白，只放一個大引用 + 出處
- `<blockquote>` 用 inline style 單獨放大（5-6vw），不要用 `h-hero`（那是頁面主標題的命名）
- 下面跟隨英文原文（lead · opacity:.65）製造層級
- 配 `meta-row` 寫出處 · 日期

---

## Layout 9: 並列對比（A vs B · 舊 vs 新）

```html
<section class="slide light" data-animate="directional">
  <div class="chrome">
    <div>舊 vs 新 · The Shift</div>
    <div>12 / 25</div>
  </div>
  <div class="frame" style="padding-top:5vh">
    <div class="kicker" data-anim>Before / After · 正規化轉變</div>
    <h2 class="h-xl" style="margin-bottom:4vh" data-anim>從交接到共建</h2>

    <div class="grid-2-6-6" style="gap:5vw 4vh">
      <!-- 左列：舊 -->
      <div data-anim="left" style="padding:3vh 2vw; border-left:3px solid currentColor; opacity:.55">
        <div class="kicker" style="opacity:.9">Before · 舊模式</div>
        <h3 class="h-md" style="margin-top:2vh">設計 → 開發 → 交接</h3>
        <ul style="margin-top:3vh; padding-left:1.2em; display:flex; flex-direction:column; gap:1.4vh; font-family:var(--sans-zh); font-size:max(14px,1.1vw); line-height:1.55">
          <li>設計師在 Figma 做稿</li>
          <li>開發者盯著檔案翻譯畫素</li>
          <li>反覆 PR 溝通對齊</li>
          <li>非技術人員無法觸碰程式碼</li>
        </ul>
      </div>
      <!-- 右列：新 -->
      <div data-anim="right" style="padding:3vh 2vw; border-left:3px solid currentColor">
        <div class="kicker" style="opacity:.9">After · 新模式</div>
        <h3 class="h-md" style="margin-top:2vh">同工具 · 並行 · 共建</h3>
        <ul style="margin-top:3vh; padding-left:1.2em; display:flex; flex-direction:column; gap:1.4vh; font-family:var(--sans-zh); font-size:max(14px,1.1vw); line-height:1.55">
          <li>三個角色同時在 Intent 工作</li>
          <li>agents.md 作為共用上下文</li>
          <li>代理處理對齊 / 衝突 / 動畫</li>
          <li>任何人都能安全貢獻程式碼</li>
        </ul>
      </div>
    </div>
  </div>
  <div class="foot">
    <div>Page 12 · 正規化轉變</div>
    <div>Before / After</div>
  </div>
</section>
```

**要點**：
- 用 `.grid-2-6-6`（1:1）左右分半
- 左列 `opacity:.55` 做"舊"的視覺弱化，右列滿亮度做"新"的突出
- 兩列都用 `border-left:3px solid` + `padding-left` 做引用塊感
- 每列結構統一：`kicker` → `h-md` → `<ul>` 要點，節奏一致

---

## Layout 10: 圖文混排（Lead Image + Side Text）

```html
<section class="slide light">
  <div class="chrome">
    <div>Design First · 設計先行</div>
    <div>08 / 16</div>
  </div>
  <div class="frame grid-2-8-4" style="padding-top:6vh">
    <!-- 左列：大段正文 + 引用 -->
    <div>
      <div class="kicker" data-anim>Phase 01 · 設計階段</div>
      <h2 class="h-xl" style="margin-top:1vh; margin-bottom:3vh" data-anim>設計先行 · 2 周</h2>

      <p class="lead" style="margin-bottom:3vh" data-anim>
        在 Figma 中完成視覺探索與設計系統，網格 / 排版 / 色彩變數 / 可複用元件，桌面和移動端稿件幾輪回饋迭代。
      </p>

      <p data-anim style="font-family:var(--sans-zh); font-size:max(14px,1.15vw); line-height:1.75; opacity:.78; margin-bottom:2.4vh">
        兩週之內，視覺風格、粗略結構、方向性內容全部穩定。這是紮實的傳統設計流程——在這裡還沒什麼新鮮事。
      </p>

      <div class="callout" style="margin-top:3vh" data-anim>
        "This phase was pretty standard.<br>Just a solid Web design process."
        <div class="callout-src">— Luke Wroblewski</div>
      </div>
    </div>
    <!-- 右列：輔助圖 · 豎版或方形 -->
    <figure class="frame-img r-3x4" data-anim>
      <img src="images/figma.png" alt="Figma design system">
      <figcaption class="img-cap">Figma · Design System</figcaption>
    </figure>
  </div>
  <div class="foot">
    <div>Page 08 · Design First</div>
    <div>約 2 周</div>
  </div>
</section>
```

**要點**：
- `.grid-2-8-4`（8:4）讓正文佔主導，圖片作輔助
- 左列包含多種資訊層級：kicker → 大標題 → lead → 正文段落 → callout（引用）
- 右列圖片用 **豎版 3:4** 或方形 1:1，避免和左列文字競爭注意力
- 這種佈局適合**頁面資訊量偏大**的場景（不像 Layout 4 只有一句金句）

---

## 附錄：常用網格範本

| 類名 | 配比 | 用途 |
|---|---|---|
| `.grid-2-6-6` | 6:6（1:1） | 對半分 |
| `.grid-2-7-5` | 7:5 | 文字為主 + 輔助圖 |
| `.grid-2-8-4` | 8:4（2:1） | 大段文字 + 小圖/資料 |
| `.grid-3` | 1:1:1 | 3 項並列（案例/截圖） |
| `.grid-3-3` | 3×2 | 6 圖矩陣 |
| `.grid-6` | 3×2 | 6 個資料卡片 |

所有網格都預留 `gap: 3vw 4vh`（水平 3vw、豎直 4vh），可以單獨覆寫。

---

## 頁面節奏建議

一場 25-30 頁的分享，推薦以下節奏：

1. **Hero Cover**（第 1 頁）
2. **Act Divider**（第一幕開場，hero light 或 hero dark）
3. **Big Numbers**（拋硬資料製造衝擊）
4. **Quote + Image**（講身份反差/掛鉤）
5. **Image Grid**（證據支撐）
6. **Hero Question**（幕收束，留懸念）
7. ... 第二幕、第三幕同樣節奏 ...
8. **Hero Close**（最後一頁，問題或致謝）

hero 頁與 non-hero 頁應該 **2-3 : 1 比例交錯**，不要連續超過 3 頁 non-hero，也不要連續超過 2 頁 hero。
