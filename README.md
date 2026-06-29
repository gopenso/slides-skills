# slides skills · 網頁 PPT / 配圖 / 封面

![GitHub stars](https://img.shields.io/github/stars/gopenso/slides-skills?style=flat-square)
![License](https://img.shields.io/github/license/gopenso/slides-skills?style=flat-square)
![Skill](https://img.shields.io/badge/Skill-Agent-111111?style=flat-square)
![HTML Deck](https://img.shields.io/badge/HTML-Deck-0A7CFF?style=flat-square)
![Claude Code](https://img.shields.io/badge/Claude%20Code-Supported-6B5B95?style=flat-square)
![Codex](https://img.shields.io/badge/Codex-Supported-222222?style=flat-square)
> 🌏 **English version: [README.en.md](./README.en.md)**

一個適配 Claude Code / Codex 等 Agent 環境的網頁 PPT 技能，用於產生**單檔案 HTML 橫向翻頁 PPT**、PPT 配圖和多平台封面。

內建兩套視覺系統：

- **Style A: 電子雜誌 × 電子墨水**。像 *Monocle* 貼上了程式碼，適合敘事、觀點、分享、個人風格表達。
- **Style B: 瑞士國際主義**。網格至上、單一高飽和錨點色、直角、髮絲線、極致字號對比，適合事實、產品、分析、方法論表達。

**舊主題 · Style A 電子雜誌風**

![Style A 電子雜誌風效果展示](https://github.com/user-attachments/assets/5dc316a2-401c-4e37-9123-ea081b6ae470)

**新主題 · Style B 瑞士國際主義**

![Style B 瑞士國際主義效果展示](https://github.com/user-attachments/assets/8960e78c-69bb-4b7e-aa95-6fad64b70314)

## 30 秒開始

```bash
npx skills add gopenso/slides-skills --skill slides-skills
```

也可以直接把這段話發給有 shell 許可權的 AI Agent：

```text
幫我安裝 slides-skills。請把 https://github.com/gopenso/slides-skills 複製到 ~/.claude/skills/slides-skills，安裝完成後檢查 SKILL.md、assets/、references/ 是否存在。
```

已經安裝過的話，用這段話更新：

```text
幫我更新 slides-skills。請進入 ~/.claude/skills/slides-skills 執行 git pull，然後告訴我當前最新 commit。
```

安裝後直接對 Agent 說：

```text
幫我基於這篇文章做一份瑞士風 PPT，控制在 7 頁左右，需要 2-3 張配圖。
```

也可以試這些請求：

```text
幫我把這份 Markdown 做成雜誌風演講 PPT。
基於這份 PPT 的核心觀點，產生一張 21:9 封面。
把這張產品截圖重新設計成適合 PPT 的 16:10 配圖。
```

## 效果

- 🖋 **雙視覺系統**：電子雜誌風負責敘事，瑞士風負責事實表達
- 📐 **橫向左右翻頁**：鍵盤 ← → / 滾輪 / 觸屏滑動 / 底部圓點 / ESC 索引
- 🧩 **Style A 10 種佈局**：封面、章節、資料大字報、圖文、圖片網格、Pipeline、對比等
- 🧱 **Style B 22 種鎖定版式**：Cover、Statement、KPI Tower、Loop Diagram、Duo Compare、Image Hero、Closing Manifesto 等
- 🎨 **主題色預設**：Style A 5 套電子墨水主題，Style B 4 套瑞士高飽和錨點色
- 🖼 **Codex 可選配圖流程**：可用 GPT-Image 2.0 / GPT-M 2.0 產生紀實照片、資訊圖、流程圖、系統關係圖、UI 情景圖，並按範本比例插入
- 📰 **多平台封面**：可用同一套視覺規則產生頻道橫幅 21:9、分享卡 1:1、IG 3:4、Youtube 橫版等封面
- 📴 **低效能靜態模式**：按 `B` 可關閉 WebGL / canvas 動畫，讓動態內容退回靜態背景
- 📄 **單檔案 HTML**：不需要建置、不需要伺服器，瀏覽器直接開啟

## 適合 / 不適合

**✅ 合適**：線下分享 / 行業內部講話 / 私人分享會 / AI 產品釋出 / demo day / 帶強烈個人風格的演講

**❌ 不合適**：大段表格資料 / 訓練課件（資訊密度不夠）/ 需要多人協作編輯（靜態 HTML）

## 常見使用場景

| 任務 | 推薦方式 |
|------|---------|
| 長文章變演講 PPT | 先抽核心觀點，再按 6-10 頁節奏產生 deck |
| 方法論 / 產品分析 | 用 Style B 瑞士風，優先使用鎖定版式和 21:9 主圖 |
| 個人分享 / 觀點表達 | 用 Style A 電子雜誌風，保留更強敘事感 |
| PPT 配圖 | 在 Codex 中用 GPT-Image 2.0 / GPT-M 2.0 產生照片、資訊圖、流程圖、UI 情景圖 |
| 多平台封面 | 從同一份內容產生頻道橫幅 21:9、1:1 分享卡、IG 3:4、Youtube 橫版封面 |
| 截圖統一風格 | 把原始截圖重新產生到範本需要的比例，再插入 PPT |

## 為什麼是 HTML PPT

- **更適合 Agent 產生和修改**：HTML / CSS 是文字，Agent 能直接讀、改、驗證。
- **表現力比 Markdown 更高**：可以做精細排版、空間定位、動畫、互動和回應式封面。
- **交付更輕**：單檔案 HTML 可以直接開啟、簡報、傳送、截圖。
- **更容易做質量控制**：瑞士風可以用指令碼校驗版式、圖片槽位、標題對齊和危險 SVG。
- **更適合視覺內容鏈路**：同一套主題能覆蓋 PPT、配圖、封面和截圖再設計。

## 平台支援

| 平台 | 狀態 | 說明 |
|------|------|------|
| Claude Code | 支援 | 原生 Skill 工作流，適合產生和迭代 HTML deck |
| Codex | 支援 | 適合產生 PPT、呼叫圖片產生能力、做瀏覽器視覺檢查 |
| Cursor / 其他本地 Agent | 可用 | 需要能讀寫檔案並執行 shell 命令 |
| WorkBuddy | 適配中 | 單獨整理上架版本，去掉平台不需要的渠道差異 |
| 普通 Chatbot | 不推薦 | 沒有檔案系統和瀏覽器預覽時，很難穩定產生完整 deck |

## 安裝

### 方式一：一行命令安裝（推薦）

```bash
npx skills add gopenso/slides-skills --skill slides-skills
```

### 方式二：把下面這段話直接發給 AI

> 幫我安裝 `slides-skills` 這個 Claude Code skill。請按下面步驟做：
>
> 1. 確保 `~/.claude/skills/` 目錄存在（不存在就建立）
> 2. 執行 `git clone https://github.com/gopenso/slides-skills.git ~/.claude/skills/slides-skills`
> 3. 驗證：`ls ~/.claude/skills/slides-skills/` 應該看到 `SKILL.md`、`assets/`、`references/` 三項
> 4. 告訴我安裝好了，之後我說「做一份雜誌風 PPT」之類的話就會觸發這個 skill

把這段話複製貼上給 Claude Code / Cursor / 任何有 shell 許可權的 AI Agent，它會自動完成安裝。

### 方式三：手動命令列

```bash
git clone https://github.com/gopenso/slides-skills.git ~/.claude/skills/slides-skills
```

### 觸發方式

裝好後，Claude Code 會在對話裡自動發現並呼叫這個 skill。觸發關鍵詞：

- "幫我做一份雜誌風 PPT"
- "幫我做一份瑞士風 PPT"
- "產生一個 horizontal swipe deck"
- "editorial magazine style presentation"
- "electronic ink 風格演講 slides"
- "基於這篇文章做一張 21:9 封面"
- "基於這份 PPT 產生一張 1:1 分享卡"

## 使用流程

Skill 本身是結構化工作流，Agent 會逐步引導：

1. **選取風格** — Style A 電子雜誌風，或 Style B 瑞士國際主義
2. **需求澄清** — 7 問清單：風格、受眾、時長、素材、圖片/截圖需求、主題色、硬約束
3. **複製範本** — Style A 用 `assets/template.html`，Style B 用 `assets/template-swiss.html`
4. **填充內容** — 先做主題節奏表，再從對應 layout 骨架裡挑、粘、改文案
5. **可選配圖** — 在 Codex 中詢問是否用 GPT-Image 2.0 / GPT-M 2.0 產生配圖，再按頁面比例插入
6. **自檢** — 對照 `references/checklist.md`，P0 級問題必須全過；瑞士風還要執行版式校驗器
7. **預覽** — 瀏覽器直接開啟
8. **迭代** — inline style 改字號/高度/間距

詳細說明見 [`SKILL.md`](./SKILL.md)。

## Style B 瑞士風

瑞士風是這次新增的結構化主題。它不是「換一套 CSS」，而是一套更嚴格的版式系統。

- **22 個具名版式**：正文頁只能從 `S01` 到 `S22` 中選取，不能臨時發明頁面結構
- **4 套錨點色**：克萊因藍 IKB、檸檬黃、檸檬綠、安全橙
- **網格鎖定**：16 列 grid、直角色塊、1px 髮絲線、無陰影、無漸層、無圓角
- **中文字號收斂**：全中文大標題需要降一檔，避免佔掉正文和圖片空間
- **圖文靠下對齊**：左文右圖 / 左圖右文場景優先讓正文塊與圖片底部對齊，同時避開頁尾翻頁元件
- **圖片槽位繫結**：圖片必須進入範本預留的 `data-image-slot`，常見主圖按 21:9 或 16:10 產生
- **強校驗**：用指令碼攔住居中標題、實驗版式、SVG 內寫字、圖片脫離槽位等問題

瑞士風校驗命令：

```bash
node scripts/validate-swiss-deck.mjs path/to/index.html
```

## Codex 配圖能力

在 Codex 環境中，完成 deck 初稿後可以主動詢問使用者是否需要產生配圖。使用者確認後，再詢問圖片型別或風格，常用型別包括：

- 人文紀實照片：富士 / 徠卡感的真實場景，增加人文表現力
- 資訊圖 / 流程圖 / 對比圖 / 系統關係圖：用於解釋無法用實拍照片說明的概念
- 截圖美化 / 截圖再設計：原始截圖優先用內建背景資產做 CleanShot X 式背景畫布適配；需要重構時再產生 UI 情景圖
- 資料大字報 / 資料圖表：把關鍵數字做成可直接插入 PPT 的視覺素材
- 多圖拼貼：用於極寬圖片槽位，避免把三張 16:9 圖片硬塞進三列

產生圖片時要遵守四個關鍵規則：

- 圖片是 PPT 中的嵌入素材，不要自帶頁尾、頁底、標題、角標、頁碼或裝飾邊框
- 圖片語言跟隨 deck 語言：中文 deck 的資訊圖用中文標籤，英文 deck 用英文標籤
- 圖片比例必須先相符落位：瑞士風主圖常用 21:9，通用主圖常用 16:9 / 16:10，截圖再設計常用 16:10，多圖網格統一高度
- 使用者截圖需要保真時，先讀 `references/screenshot-framing.md`，用 `assets/screenshot-backgrounds/` 內建背景 + 程式化縮放/留邊/對齊處理，不要預設重畫截圖內容

配圖提示詞見 [`references/image-prompts.md`](./references/image-prompts.md)，截圖適配見 [`references/screenshot-framing.md`](./references/screenshot-framing.md)。

## 封面產生

這個 Skill 也可以基於文章或 PPT 核心觀點產生平台封面。典型規格：

- **頻道橫幅**：21:9，主標題優先，右側或邊緣保留視覺錨點
- **分享卡**：1:1，與頻道橫幅共用主題色、關鍵詞和視覺元素
- **IG 封面 / 輪播**：3:4，大標題優先，多張時統一字號和視覺節奏
- **Youtube / 橫版封面**：16:9，適合標題 + 副標題 + 單一視覺焦點

封面原則和 PPT 一樣：只用少量關鍵詞，視覺重心落在大標題上，不要把正文堆滿。

## 範例請求

複製下面任意一條給 Agent，再附上你的文章、Markdown 或素材檔案：

```text
幫我基於這篇文章產生一份 8 頁左右的瑞士風 PPT，需要 3 張配圖，圖片比例跟範本槽位相符。
```

```text
幫我把這個產品分析文件做成電子雜誌風 PPT，重點突出觀點和敘事節奏。
```

```text
基於這份 PPT 的主題，做兩張封面：21:9 封面和 1:1 分享卡，視覺保持一致。
```

```text
把這些產品截圖重新設計成統一的 16:10 PPT 配圖，保留關鍵資訊，不要畫頁尾和標題。
```

## 目錄結構

```
slides-skills/
├── SKILL.md              ← Skill 主檔案：工作流、原則、常見錯誤
├── README.md             ← 本檔案
├── assets/
│   ├── template.html         ← Style A 電子雜誌風範本
│   ├── template-swiss.html   ← Style B 瑞士國際主義範本
│   └── screenshot-backgrounds/ ← 截圖美化內建背景（WebP）：style-a 5 套 / style-b 4 套
├── scripts/
│   └── validate-swiss-deck.mjs ← 瑞士風版式校驗器
└── references/
    ├── components.md     ← 元件手冊（字型、色、網格、圖示、callout、stat、pipeline）
    ├── layouts.md        ← 10 種頁面佈局骨架（可直接貼上）
    ├── layouts-swiss.md  ← 22 種瑞士風鎖定版式
    ├── swiss-layout-lock.md ← 瑞士風還原度和版式硬約束
    ├── themes.md         ← 5 套主題色預設（只能選不能自定義）
    ├── themes-swiss.md   ← 4 套瑞士風錨點色
    ├── image-prompts.md  ← GPT-Image 2.0 / GPT-M 2.0 配圖型別、比例和基礎提示詞
    ├── screenshot-framing.md ← CleanShot X 式截圖適配語義
    └── checklist.md      ← 質量檢查清單（P0 / P1 / P2 / P3 分級）
```

## 主題色預設

從 `references/themes.md` 裡選一套——**不允許自定義 hex 值**，保護美學比給自由更重要。

### Style A 電子雜誌主題

| 預覽 | 主題 | 核心色與適合場景 |
|------|------|------------------|
| <img src="https://github.com/user-attachments/assets/df21dbcb-5fe4-4852-a91a-a9cf00aceeb4" width="260" alt="墨水經典主題預覽"> | 🖋 **墨水經典** | `#0a0a0b` / `#f1efea`。通用預設、商業釋出、不知道選啥時最穩。 |
| <img src="https://github.com/user-attachments/assets/99ce0fd2-72a6-4368-a75a-a8e21657a537" width="260" alt="靛藍瓷主題預覽"> | 🌊 **靛藍瓷** | `#0a1f3d` / `#f1f3f5`。科技、研究、AI、技術釋出會。 |
| <img src="https://github.com/user-attachments/assets/bcc1cc4c-5e8e-4467-ae8d-f5801ae73657" width="260" alt="森林墨主題預覽"> | 🌿 **森林墨** | `#1a2e1f` / `#f5f1e8`。自然、可持續、文化、非虛構內容。 |
| <img src="https://github.com/user-attachments/assets/dfea080e-e916-417e-93cd-0a3627de84ca" width="260" alt="牛皮紙主題預覽"> | 🍂 **牛皮紙** | `#2a1e13` / `#eedfc7`。懷舊、人文、閱讀、歷史、文學分享。 |
| <img src="https://github.com/user-attachments/assets/f3705592-9a72-4dbc-9818-df3aea61bc75" width="260" alt="沙丘主題預覽"> | 🌙 **沙丘** | `#1f1a14` / `#f0e6d2`。藝術、設計、創意、時尚和畫廊感內容。 |

切換主題只需取代 `template.html` 開頭 `:root{}` 裡的 6 行變數，其他 CSS 全走 `var(--...)`。

### Style B 瑞士主題

瑞士風從 `references/themes-swiss.md` 裡選一套，同樣**不允許自定義 hex 值**。

| 預覽 | 主題 | 錨點色與適合場景 |
|------|------|------------------|
| <img src="https://github.com/user-attachments/assets/c02d02f7-ce6f-4e16-b8a6-778c96851f94" width="260" alt="克萊因藍瑞士主題預覽"> | 🔵 **克萊因藍 IKB** | `#002FA7`。通用預設、商業釋出、AI 產品、方法論。 |
| <img src="https://github.com/user-attachments/assets/c310a8c4-5d28-450e-b49a-6ac5b6ba4785" width="260" alt="檸檬黃瑞士主題預覽"> | 🟡 **檸檬黃** | `#FFD500`。年輕、運動、零售、消費品、Y2K 復古。 |
| <img src="https://github.com/user-attachments/assets/65f7b3f9-3358-419e-b513-f7f2cc24ec76" width="260" alt="檸檬綠瑞士主題預覽"> | 🟢 **檸檬綠** | `#C5E803`。生態、可持續、健康、Z 世代品牌。 |
| <img src="https://github.com/user-attachments/assets/9c3319c9-a134-4657-9a56-211c23411f7f" width="260" alt="安全橙瑞士主題預覽"> | 🟠 **安全橙** | `#FF6B35`。警示、新聞、工業、運動、活力主題。 |

如果使用者說「瑞士風 PPT」但沒有指定色彩，預設推薦克萊因藍 IKB。

## 核心設計原則

1. **克制優於炫技** — WebGL 背景只在 hero 頁透出
2. **結構優於裝飾** — 資訊靠字號 + 字型對比 + 網格留白，不用陰影和浮動卡片
3. **圖片是第一公民** — 圖片要對齊正文內容區，比例穩定，只裁底部，頂部和左右完整
4. **配圖只做素材** — 產生圖只保留核心照片 / 圖表 / UI，不要把 PPT 頁尾、標題和角標畫進圖片裡
5. **節奏靠 hero 頁** — hero / non-hero 交替，才不累眼睛
6. **低效能可退場** — 按 `B` 能轉場到靜態模式，動態效果不能成為閱讀負擔
7. **術語統一** — Skills 就是 Skills，不中英混譯
8. **瑞士風必須守版式** — Style B 優先還原原始 22P 版式，不要為了「多樣」發明不存在的頁面

## 視覺參考

- [*Monocle*](https://monocle.com) 雜誌的版式
- YC Garry Tan "Thin Harness, Fat Skills"
- Massimo Vignelli / Helvetica Forever / 瑞士國際主義網格系統
- 歸藏線下分享 PPT 系列

## Roadmap

- 補充更多真實案例和可開啟的 HTML deck 範例
- 擴充封面規格，覆蓋更多內容平台
- 增加更多瑞士風版式校驗規則
- 最佳化截圖再設計和資訊圖產生工作流
- 整理 WorkBuddy 等平台上架版本
- 增加更多主題包，但繼續限制自定義色彩

## FAQ

**可以匯出 PPTX 嗎？**
當前核心交付是 HTML。你可以用瀏覽器簡報、截圖或螢幕錄製。如果需要 PPTX，建議把 HTML 頁面作為視覺稿再轉換，但這不是當前主流程。

**為什麼不允許自定義色彩？**
這個 Skill 的重點是穩定產出。自由選色很容易破壞整體風格，所以只允許從預設主題裡選。

**我能加自己的版式嗎？**
可以。Style A 可以在 `references/layouts.md` 裡擴充；Style B 更嚴格，需要同步更新 `template-swiss.html`、`layouts-swiss.md`、`swiss-layout-lock.md` 和校驗器。

**Codex 配圖是必須的嗎？**
不是。沒有配圖也能產生 PPT。配圖流程只在需要照片、資訊圖、UI 情景圖或封面時使用。

**怎麼更新到最新版？**
重新執行安裝命令，或在本地 skill 目錄執行 `git pull`。

## 貢獻

Bug、排版問題、新佈局需求——歡迎開 Issue 或 PR。改動請優先：

- 在 `template.html` 裡補類，不要讓 layouts.md 使用未定義的類
- 在 `template-swiss.html` 裡補類時，同步更新 `layouts-swiss.md` 和 `swiss-layout-lock.md`
- 瑞士風新增規則後，同步更新 `scripts/validate-swiss-deck.mjs`
- 把踩過的坑寫到 `checklist.md` 對應的 P0 / P1 / P2 / P3 級別
- 新主題色進 `themes.md` 並給出適合的場景

## License

AGPL-3.0 © 2026 [gopenso](https://github.com/gopenso), [op7418](https://github.com/op7418)
