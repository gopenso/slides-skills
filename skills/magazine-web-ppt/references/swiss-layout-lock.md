# Swiss Layout Lock

本檔案是瑞士主題的硬約束。它的目的不是增加靈感，而是防止產生時「看起來像 Swiss，但已經脫離原始範本」。

## Golden Source

原始參考檔案：

`../assets/template-swiss.html`

瑞士主題產生時，除使用者明確要求實驗版式外，只能從下面登記的 22 個版式中選取。新增首頁/尾頁可以使用 Skill 裡的 IKB ASCII 版本，但正文頁必須來自這 22 個版式。

## 產生前硬規則

1. 每個正文頁都必須先選一個登記版式，並在 `<section>` 上寫 `data-layout="Sxx"`。
2. 不允許臨時發明 `P23/P24` 這類未出現在原始 22P 的正文結構。需要圖片時，優先使用 `S22 Image Hero`；多圖時使用 `S15/S16` 的原始網格骨架做圖片格改造，不要發明新的證據牆。唯一登記的互動擴充是 `S08 + Swiss Map Component`，詳見 `references/swiss-map-component.md`。
3. 頂部中文標題預設靠左對齊並貼近左上內容軸。除原始 `S03/S09/S10` 這種 statement/split 版式外，不要把大標題放到頁面水平中心。
4. SVG 只能負責幾何線條、圓、箭頭、路徑。不要在 SVG 裡寫可見文字；所有文字標籤用 HTML 放在網格、卡片或 caption 裡。
5. 圖片槽位和圖片產生比例必須繫結。先確定版式和槽位，再產生圖片。

## 登記版式

| ID | 原始頁 | 名稱 | 必須保留的骨架 | 圖片規則 |
|---|---|---|---|---|
| S01 | 01 | Index Cover | 三行 `cover-row`，左大編號，右大標題 | 無 |
| S02 | 02 | Vertical Timeline + KPI | 頂部靠左對齊標題，中部 `.timeline-v`，底部 `.kpi-row-4` | 無 |
| S03 | 03 | Split Statement | `.slide.split` 雙半屏，左巨字，右灰底解釋 | 無 |
| S04 | 04 | Six Cells | 頂部靠左對齊標題，下方 `.sub-grid-3-2` 六卡 | 可把卡片內部換成小圖示，不放大圖 |
| S05 | 05 | Three Layers | 頂部靠左對齊標題，下方 `.stack-row` 三大塊 | 無 |
| S06 | 06 | KPI Tower | 左標題+右說明，下方不等高 KPI 塔 | 無 |
| S07 | 07 | Horizontal Bar | 靠左對齊標題，橫向條形圖 | 無 |
| S08 | 08 | Duo Compare | `.duo-compare` 兩列 + 中線 | 無；地點/路線內容可使用 `S08 + Swiss Map Component` 取代右側插槽 |
| S09 | 09 | Dot Matrix Statement | 大號 statement + 點陣裝飾 | 無 |
| S10 | 10 | Split Closing | `.slide.split` 左巨字右列表 | 無 |
| S11 | 11 | Horizontal Timeline | 原始 `grid-template-columns:auto 1fr` 頭部 + `.timeline-h` | 無 |
| S12 | 12 | Manifesto + Ink Banner | 大字 statement + 底部通欄 ink 條 | 無 |
| S13 | 13 | Three Forces | 左 ink hero 塊 + 右 3 張卡 | 無 |
| S14 | 14 | Loop Form | 左 4 步列表 + 右幾何 loop | SVG 禁止文字，標籤改 HTML |
| S15 | 15 | Matrix + Hero Stat | 頂部靠左對齊標題，中段 6×2 矩陣，底部巨數 | 多圖可改造矩陣格，同組統一 `21:9` |
| S16 | 16 | Multi-card Brief | 頂部靠左對齊標題，下方 3×2 微卡 | 多圖可改造卡片內容，同組統一 `21:9` |
| S17 | 17 | System Diagram | 頂部左小標題+右段落，中部幾何系統圖，底部三列解釋 | SVG 禁止文字，標籤改 HTML |
| S18 | 18 | Why Now | 三列遞進 + 底部巨數 | 無 |
| S19 | 19 | Four Cards | 頂部藍線 + 四列均分 | 無 |
| S20 | 20 | Stacked KPI Ledger | 縱向賬單式巨數 | 無 |
| S21 | 21 | Tech Spec Sheet | 大標題 + 三 KPI + 右下豎線矩陣 | 無 |
| S22 | 22 | Image Hero | 頂部全寬圖 + 左上白塊標題 + 下方三列 KPI | 主圖按 `21:9` 產生，關鍵主體放中央安全區 |

## 登記擴充元件

### S08 + Swiss Map Component

- 使用場景：地理、歷史、城市路線、門店/校區/事件點位、人物住所關係。
- 版式身份：仍是 `data-layout="S08"`，不是新正文頁。
- 頁面結構：頂部靠左對齊標題 + 左側關係/說明卡片 + 右側 MapLibre 地圖卡片。
- 標記結構：點 + 連線 + HTML 卡片；SVG 只畫 fallback 關係線，不寫文字。
- 互動控制：右上角必須有 `+` / `-` / `DRAG`；預設禁用滾輪縮放和拖動，避免觸發 PPT 翻頁。
- 詳細程式碼和資料契約見 `references/swiss-map-component.md`。

## 圖片槽位規則

### S22 · Hero Strip

- 產生比例：`21:9`
- 圖片用途：實拍場景、產品場景、UI 情景圖。
- 產生提示詞必須包含：`21:9 ultra-wide strip`, `subject centered in the safe middle area`, `no title, no footer, no page chrome, no logo, no border`.
- HTML 容器必須使用原始 S22 的頂部全寬圖骨架；不要改成普通居中大圖。
- 照片用 `object-fit:cover;object-position:center 35%`。如果是人像/會議場景，不要用 `top center`。
- 資訊圖/UI 截圖如果放 S22，必須重新產生接近 `21:9`，並用 `object-fit:contain` 或保證核心內容在中央 70% 安全區。

### S15/S16 · Multi Image Grid

- 產生比例：統一 `21:9` 或統一 `16:10`，不要混用。
- 同一組圖片必須同高、同寬、同一容器背景。
- 圖片格必須吸附原始卡片網格，不要讓圖片自己決定寬高。
- 如果圖片是按槽位重新產生的 `s15-grid-21x9` / `s16-brief-21x9`，容器必須用 `.frame-img.r-21x9` 鋪滿槽位，不要再加 `.fit-contain`，也不要用固定 `height:18vh` 這類短槽把長圖縮小。
- `.fit-contain` 只用於必須保留原始比例的使用者截圖或文字密集圖片；一旦決定重產生圖片，就應該按槽位比例重產生並鋪滿。
- 如果原始截圖比例不可控，先按 `references/screenshot-framing.md` 做程式化比例適配；只有長截圖、極窄截圖或資訊需要重構時，才用 GPT-M 2.0 重產生「截圖再設計」。

## 禁止清單

- 禁止 `text-align:center` 用在頂部中文大標題。
- 禁止將頂部標題寫進右側 7.8fr 欄，造成視覺居中。
- 禁止未登記正文頁：例如臨時 `Swiss Image Split`、`Evidence Grid`、三圓圖自繪頁。
- 禁止圖片容器灰底包白底資訊圖。
- 禁止 SVG 中出現 `<text>` 作為可見標籤。
- 禁止圖片預設 `object-position:top center` 用於照片。
