# 測試專案大綱 – 模組 F – SwapLoop 公開網站

## 競賽時間

選手完成本模組的時間為 **3 小時**。

## 簡介

**SwapLoop** 是一個虛構的上海區域專案，提供在**戶外**更安全的電動自行車電池充電方式。具相容可拆卸電池的外送騎士與私人騎士，可在 **Battery Swap Cabinets** 進行換電。內建（integrated）電池的電動自行車，則使用受監控的 **E-bike Charging Bays**。外送公司在尖峰時段可獲得有限的優先使用權。網站的核心訊息是：

> 充電或換電——在住家外安全完成。

在 **Module F** 中，你要為 SwapLoop 建置**公開網站**。本模組重點是**僅使用 HTML 與 CSS 的設計實作**。網站需說明服務內容、展示幾個範例站點，並呈現給**個別騎士**與**外送車隊**的方案資訊。

同一系統中的其他模組（僅供背景理解——**不要**重做這些模組）：

| 模組     | 角色                                                  |
| -------- | ----------------------------------------------------- |
| Module B | SwapLoop Admin（員工使用的網站）                      |
| Module C | Main Backend REST API                                 |
| Module D | Rider app（即時可用性、預約、收據）                   |

即時站點篩選、預約、付款與後台管理工具都**不在本模組範圍**。請告知訪客：即時可用性在 rider app 中查詢；本模組不要實作 app 功能。

## 專案與任務總述

建立一個由多個 HTML 頁面組成的靜態網站。摘要如下（完整細節見 [Requirements](#requirements)）：

- **五個頁面：** Home、How it works、Stations、For riders、For fleets
- 僅使用 **HTML + CSS** —— 不可使用 JavaScript（可用 Tailwind、Bootstrap 等 CSS 框架）
- 所有**必要內容**必須使用 [`assets/`](./assets/) 提供的文字、圖片與圖示 —— 不可更改或替換必要文案
- 在以下固定視窗尺寸下需具備**響應式**排版：手機（`768px` 以下）、平板（`768px`–`1023px`）、桌面（`1024px` 以上）
- 基本**無障礙**與**SEO**（搜尋引擎最佳化）

### 技術規範

| 允許                                                                                           | 不允許                                                         |
| ---------------------------------------------------------------------------------------------- | -------------------------------------------------------------- |
| HTML5、CSS3                                                                                    | JavaScript（包含 `onclick` 與 JS 框架）                        |
| CSS 框架（如 Tailwind、Bootstrap）僅作為 CSS 使用                                              | 呼叫 Module C、Station Service 或任何後端 API                 |
| SVG 圖示（inline 或圖片檔）                                                                    | 建置 rider app 或 admin website                               |
| 清晰的多頁 HTML 結構                                                                           | 虛構即時車位數或假造「目前可用」資料                           |
| 所有必要內容都取自 [`assets/texts/copy-deck.md`](./assets/texts/copy-deck.md)                | 變更或替換必要標題、內文文字或價格                             |

請使用以下**精確 breakpoint**（瀏覽器視窗寬度）：

| 名稱        | 範圍               |
| ----------- | ------------------ |
| **Mobile**  | `768px` 以下       |
| **Tablet**  | `768px`–`1023px`   |
| **Desktop** | `1024px` 以上      |

請使用 CSS media queries（或 CSS 框架中對應尺寸設定）。三個尺寸範圍的版面必須有明顯差異。

### 詞彙

請與 Modules B–D 使用一致詞彙（亦出現在提供的文字檔中）：

| 詞彙                     | 意義                                                         |
| ------------------------ | ------------------------------------------------------------ |
| **SwapLoop**             | 品牌／產品名稱                                               |
| **SwapLoop Station**     | 完整服務據點（`SWAP`、`CHARGING` 或 `HYBRID`）               |
| **Battery Swap Cabinet** | 儲存並充電可交換電池的設備                                   |
| **Battery Slot**         | 最多容納一顆電池的單一格位                                   |
| **E-bike Charging Bay**  | 為整台內建電池電動自行車充電的車位                           |

支援的電池／連接器類型（請勿發明其他代碼）：

| 模式       | 支援類型               | 電壓      |
| ---------- | ---------------------- | --------- |
| Swappable  | `SL-48`、`SL-60`       | 48V / 60V |
| Integrated | `GB-AC-48`、`GB-AC-60` | 48V / 60V |

站點類型：

| 類型       | 提供內容                  |
| ---------- | ------------------------- |
| `SWAP`     | 僅 Battery slots          |
| `CHARGING` | 僅 E-bike charging bays   |
| `HYBRID`   | 兩者皆有                  |

### 提供文字（重要）

[`assets/texts/copy-deck.md`](./assets/texts/copy-deck.md) 中的必要文案必須出現在網站上。

- 請從該檔複製必要標題、段落、價格、站點說明、title、meta description 與圖片 `alt` 文字。**不得**更改或替換其文字與數字。
- 可接受小幅版面調整（例如把一段分成兩行，或將表格儲存格改為不同版型）。
- 你可自行加入**額外**文字或視覺元素，但不得替換、隱藏或牴觸必要內容。
- 評分重點為 HTML/CSS 設計。提供必要文案是為了讓所有選手從相同內容起跑。

### 提供檔案

請使用以下提供素材（所有檔案皆位於 [`assets/`](./assets/)）：

| 路徑                                                                                     | 內容                                                                                 |
| ---------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------ |
| [`assets/texts/copy-deck.md`](./assets/texts/copy-deck.md)                               | 頁面文案、價格、站點描述、標題、meta 描述、圖片 `alt` 文字                         |
| [`assets/images/logo-swaploop.svg`](./assets/images/logo-swaploop.svg)                   | 頁首／頁尾用 SwapLoop logo                                                           |
| [`assets/images/favicon.svg`](./assets/images/favicon.svg)                               | Favicon                                                                              |
| [`assets/images/hero-home.jpg`](./assets/images/hero-home.jpg)                           | 首頁 Hero 圖                                                                         |
| [`assets/images/station-haitang.jpg`](./assets/images/station-haitang.jpg)               | Haitang Garden East Gate 照片                                                       |
| [`assets/images/station-canal-view.jpg`](./assets/images/station-canal-view.jpg)         | Canal View Delivery Hub 照片                                                        |
| [`assets/images/station-morning-bridge.jpg`](./assets/images/station-morning-bridge.jpg) | Morning Bridge Charging Court 照片                                                  |
| [`assets/images/og-default.jpg`](./assets/images/og-default.jpg)                         | 首頁 Open Graph 圖                                                                  |
| [`assets/images/map-vignette.svg`](./assets/images/map-vignette.svg)                     | 可選擇的風格化區域地圖                                                              |
| [`assets/images/priority-windows.svg`](./assets/images/priority-windows.svg)             | 可選擇的車隊優先時窗示意圖                                                          |
| [`assets/icons/`](./assets/icons/)                                                       | SVG 圖示：swap、charge、hybrid、rider、fleet、safety                                 |
| [`assets/videos/home-loop.mp4`](./assets/videos/home-loop.mp4)                           | 可選短循環首頁影片                                                                  |

### 建議時間規劃

| 區塊  | 重點                                                  | 預估時間     |
| ----- | ----------------------------------------------------- | ------------ |
| A     | 共用 header/footer/nav、Home、How it works            | 約 1 小時    |
| B     | Stations + 較高難度 CSS 技巧                          | 約 1 小時    |
| C     | For riders、For fleets、無障礙/SEO、動畫              | 約 1 小時    |

<a id="requirements"></a>
## 需求

以下各節為**最低限度**需求。你可加入額外文字或視覺元素，但不得覆蓋、隱藏或牴觸這些需求。必要頁面文案、標題、meta 描述與圖片 `alt` 文字仍必須符合 [`assets/texts/copy-deck.md`](./assets/texts/copy-deck.md)。

### 網站結構與共用版面

#### 頁面

必須交付**恰好五個** HTML 頁面。檔名可自行決定，但每個頁面都必須能從主導覽到達，且所有站內連結都必須可用。

| 頁面         | 目的                                                  |
| ------------ | ----------------------------------------------------- |
| Home         | 介紹 SwapLoop，並連到 riders 與 fleets 內容           |
| How it works | 以清楚步驟說明換電與充電差異                          |
| Stations     | 站點類型 + **恰好三個**範例站點                       |
| For riders   | 個別騎士方案（隨用隨付）                              |
| For fleets   | 外送夥伴方案與優先時段資訊                            |

#### Skip to main content（無障礙）

基於無障礙需求，每頁開頭都要有「Skip to main content」連結，直接跳到主要內容區。這能協助鍵盤與螢幕閱讀器使用者略過每頁重複的 header 與導覽。

#### Header 與 logo

每個頁面都要有網站 header。清楚顯示 **SwapLoop** 名稱或 logo（[`assets/images/logo-swaploop.svg`](./assets/images/logo-swaploop.svg)）。品牌應在頁面頂部易於辨識，而非只在頁尾小字出現。

#### 主導覽

每個頁面都要有前往五頁的主導覽。導覽標籤請使用文字檔中的用詞。需清楚標示當前頁面，讓訪客知道目前所在位置。

#### Footer

每頁都要有 footer。使用文字檔中的試點說明（pilot credit）與 rider-app 文案。另需加入 For riders 與 For fleets 連結，讓訪客可從任一頁進入方案內容。

#### 響應式共用版面

相同的 header、nav、footer 必須在三個 breakpoint 都能正常運作。手機版可使用簡單堆疊式或緊湊選單。僅能用 CSS，不可用 JavaScript。

### Home

#### Hero

在頁面**最上方**（未捲動前）介紹 SwapLoop。顯示品牌、主標題、輔助句與兩個主要連結或按鈕（**For riders** / **For fleets**），文案取自文字檔。使用 [`assets/images/hero-home.jpg`](./assets/images/hero-home.jpg) 作為主大型圖片，並套用提供的 `alt` 文字。首屏需保持清晰：品牌、單一核心訊息、簡短補充文案與兩個受眾連結。

#### Why outdoor charging

在頁面較下方說明為何戶外充電重要。使用文字檔中的「why outdoor charging」段落。語氣需平實、務實，不可改寫成災難式事故敘事。

#### 三步驟預覽

以三步驟短版內容預覽 SwapLoop 流程，使用文字檔中的三步驟 teaser（找站點 → 在 rider app 預約 → 換電或充電後取車）。並提供清楚連結到完整的 How it works 頁，供訪客深入閱讀。

#### 精選站點預告

預覽一個實際範例站點：Haitang Garden East Gate（`HYBRID`）。使用文字檔中的精選站點 teaser 文案，並連到 Stations 頁。可在此搭配 [`assets/images/station-haitang.jpg`](./assets/images/station-haitang.jpg) 作為輔助圖片。

#### 可選首頁影片

你可在 Home 頁放置短循環影片 [`assets/videos/home-loop.mp4`](./assets/videos/home-loop.mp4)。影片需放在 **hero 區塊下方**（頁面較下半部），例如放在「Why outdoor charging」或三步驟預覽之後。**不得**把它放在首屏，取代必要的 hero 圖、標題與按鈕。

### How it works

#### Intro

頁面開頭請使用文字檔中的 How it works 導言。內容要清楚說明 SwapLoop 支援兩種路徑：可相容可拆卸電池的換電，以及內建電池電動自行車的充電車位。

<a id="three-steps"></a>
#### Three steps

完整呈現文字檔中的三步驟流程：找站點 → 在 rider app 預約 → 換相容電池 **或** 在車位充電並取車。版面需在 **桌面版為水平時間軸**，在 **手機／平板為垂直時間軸**（僅 CSS 版面）。

#### 比較

使用文字檔中的比較內容，並排比較兩種方案：可拆卸電池（`SL-48` / `SL-60`）vs 內建電池（`GB-AC-48` / `GB-AC-60`）。請用表格或雙欄版面，讓訪客清楚看出各方案適用對象與站點流程。

#### 安全說明

顯示文字檔中的安全說明：電池回到可使用前會先檢查，充電車位受監控，夥伴優先權也不會跳過相容性或安全檢查。

#### Rider app 說明

結尾（或清楚位置）需包含文字檔中的 rider-app 文案：即時預約與可用性在 SwapLoop rider app。此頁僅說明服務，不實作預約。

### Stations

<a id="station-types"></a>
#### Station types

用文字檔文案與 [`assets/icons/`](./assets/icons/) 圖示（`icon-swap`、`icon-charge`、`icon-hybrid`）說明三種站點類型（`SWAP`、`CHARGING`、`HYBRID`）。

本區塊需以 **純 CSS tabs** 實作（不可 JavaScript）。不要做成全城市完整站點名錄。

#### 精選站點

以較完整內容展示 **恰好三個** 精選站點（不是超長列表）。每站都須使用文字檔提供的照片、類型標籤、地址、營業時間、適用對象文案與 `alt` 文字。不可虛構即時車位數或「現在可用」數字。

| Name                          | Type       | Hours       | Image                                                                      |
| ----------------------------- | ---------- | ----------- | -------------------------------------------------------------------------- |
| Haitang Garden East Gate      | `HYBRID`   | 00:00–24:00 | [`station-haitang.jpg`](./assets/images/station-haitang.jpg)               |
| Canal View Delivery Hub       | `SWAP`     | 05:00–23:30 | [`station-canal-view.jpg`](./assets/images/station-canal-view.jpg)         |
| Morning Bridge Charging Court | `CHARGING` | 06:00–22:00 | [`station-morning-bridge.jpg`](./assets/images/station-morning-bridge.jpg) |

#### 覆蓋範圍

請包含文字檔中的覆蓋範圍句：部分區域 **「尚未覆蓋」**、網路正在擴張，訪客應以 rider app 查詢即時站點。不要把未覆蓋區域描述為「不安全」或「不合規」。

#### 即將上線（可選）

可加入文字檔中的 Jade Lane「coming soon」文案，作為未來覆蓋範圍短註記。不可將其擴寫成第四個完整站點介紹並填入虛構細節。

#### 可選地圖

可加入風格化區域地圖 [`assets/images/map-vignette.svg`](./assets/images/map-vignette.svg) 輔助站點敘事。它是靜態圖片，不是即時地圖。

<a id="featured-stations--desktop-image-layout"></a>
#### 精選站點 — 桌面版圖片版型

在 **桌面版**（`1024px` 以上）時，每個精選站點區塊都必須採用圖片重疊版型：站點照片要與文字區塊重疊，或延伸到內容邊界（bleed / overlap 效果）。手機與平板則應使用較簡單的堆疊版型（圖片在文字上方或下方）。

### For riders

#### Intro

以文字檔中的 For riders 導言開場。本頁面服務對象是私人騎士與以個人計費（隨用隨付）的外送騎士，不是公司車隊訂閱方案。

<a id="pay-as-you-go-prices"></a>
#### Pay-as-you-go 價格

請以清楚表格（或相近版型）呈現隨用隨付價格，金額必須與文字檔完全一致（人民幣整數元，CNY）。不得更改數字。

在 **桌面版**，價格表（或價格比較區塊）必須有 **sticky 欄位或 sticky 表頭**，使訪客捲動 For riders 其他內容時仍可見。

| Service           | Compatibility | Price |
| ----------------- | ------------- | ----- |
| Battery swap      | `SL-48`       | ¥5    |
| Battery swap      | `SL-60`       | ¥7    |
| E-bike bay charge | `GB-AC-48`    | ¥3    |
| E-bike bay charge | `GB-AC-60`    | ¥4    |

#### 按次付費

請強調文字檔中的按次付費／無月租契約文案，讓訪客理解一般騎士不需強制訂閱。

#### 付款

請納入文字檔中的 Alipay 行銷文案，僅作產品資訊。不要實作真實付款表單，也不要串接支付 API。

#### 相容性檢查清單

顯示文字檔中的相容性清單，讓騎士在到站前可先確認電池包／連接器是否為 `SL-48`、`SL-60`、`GB-AC-48`、`GB-AC-60`。

#### 月租方案註記（可選）

可加入文字檔中的月租方案註記（未來可能推出；本站目前僅發布隨用隨付）。不可虛構新的消費者方案名稱或不同價格。

#### Rider app 連結

請加入 rider-app 按鈕或連結，連結文字必須與文字檔完全一致，讓訪客知道在哪裡註冊、預約與查看收據。

### For fleets

#### Intro

以文字檔中的 For fleets 導言開場。本頁面面向需要 Partner 方案、配額、超額費率與尖峰時段可選優先權的外送公司。

#### Partner 方案

請清楚呈現 **Partner starter** 與 **Partner fleet**（雙欄、雙區塊或比較表皆可）。方案細節需與文字檔完全一致：

| Plan                | Monthly base price | Included uses | Extra use (overage)        |
| ------------------- | ------------------ | ------------- | -------------------------- |
| **Partner starter** | ¥2,000             | 150 / month   | ¥6 per use above the limit |
| **Partner fleet**   | ¥5,000             | 400 / month   | ¥5 per use above the limit |

#### 用量折扣

顯示僅適用於**超額使用**的用量折扣（每月基本費保持不變）。請以小表格或簡單圖形呈現文字檔中的數字，不要做成大量重複卡片清單：

- Partner fleet: 0–499 → 0%; 500–999 → 10%; 1000+ → 20%
- Partner starter: 0–199 → 0%; 200–399 → 5%; 400+ → 15%

#### 優先時窗

依文字檔說明優先時窗，並包含範例時段 **11:00–14:00** 與 **17:00–20:00**。需清楚說明：部分容量會優先提供給該夥伴騎士，且優先權不會跳過相容性與安全檢查。可搭配 [`assets/images/priority-windows.svg`](./assets/images/priority-windows.svg)。

#### 資金來源

請包含文字檔中的資金來源句：區域安全補助協助站點啟動，外送夥伴也共同投入網路建置。這不是稅收，也不是對個人或企業的評分。

#### 範例夥伴（可選）

可列出文字檔中的虛構夥伴名稱作為範例。不要使用真實公司 logo 或真實品牌名稱。

#### 聯絡方式

提供文字檔中的靜態聯絡方式（例如連到 `partners@swaploop.example` 的 `mailto:`，或不送出資料的聯絡表單）。不可用 JavaScript submit，也不可連後端。

### 較高難度 CSS 與動畫

#### Sticky 網站 header

網站 header 在頁面捲動時需固定（sticky）在視窗頂端；五頁皆適用，且三個 breakpoint 都需成立。

#### 純 CSS 站點類型分頁

在 Stations 頁中，站點類型區塊需使用純 CSS tabs，規格如 [Station types](#station-types)。

#### How it works 時間軸

在 How it works 頁中，三步驟需使用 [Three steps](#three-steps) 所述的響應式時間軸版面。

#### 重疊站點圖片版型

在 Stations 頁中，精選站點需使用 [Featured stations — desktop image layout](#featured-stations--desktop-image-layout) 所述桌面重疊圖片版型。

#### For riders 的 sticky 價格區

在 For riders 頁中，隨用隨付價格需採用 [Pay-as-you-go prices](#pay-as-you-go-prices) 所述桌面 sticky 行為。

#### 轉場與動畫

- 主導覽連結與主要按鈕／連結需具可見的 hover transition 與 focus transition。
- Home hero 在頁面載入時需有柔和淡入（或向上淡入）進場動畫。
- 當 `prefers-reduced-motion: reduce` 生效時，需關閉或簡化 hero 進場動畫與其他非必要動態。hover/focus 回饋可保留。

### 無障礙

- 每頁標題層級需正確；且僅有一個符合頁面主題的 `h1`（使用文字檔中的頁面標題／headline）。
- 資訊型圖片需使用文字檔提供的 `alt`；裝飾型圖片應使用空 `alt`（或做成 CSS 背景）。
- 連結與控制項需有清楚 **focus** 樣式；除非替換成同等清楚效果，不可移除 focus 外框。
- 站點類型或方案差異不得只靠顏色呈現（亦需搭配文字與／或圖示）。
- 文字與控制項對比需足夠（建議達 WCAG AA）。

### SEO

- 每個頁面都需使用文字檔中的 `<title>` 與 meta description 值。
- 使用 `header`、`nav`、`main`、`footer`，且導覽／連結標籤需使用文字檔用詞。
- 至少在 Home 頁加入基本 Open Graph 標籤，使用文字檔中的 Home title 與 meta description，以及 [`assets/images/og-default.jpg`](./assets/images/og-default.jpg)（`og:title`、`og:description`、`og:image`）。
- favicon 使用 [`assets/images/favicon.svg`](./assets/images/favicon.svg)。

## 評分

專家將依評分表人工評分，使用目前版本桌面瀏覽器（Chrome 或 Firefox）與窄版行動視窗檢查。評審會確認：

- 必要頁面是否完整，且是否正確使用提供文案（術語、價格、三個站點）
- 響應式版面是否符合手機（`768px` 以下）、平板（`768px`–`1023px`）與桌面（`1024px` 以上）
- 上述無障礙與 SEO 項目
- 較高難度 CSS 技巧、動畫與 `prefers-reduced-motion` 支援
- 整體視覺品質與品牌清晰度（不需逐像素對齊固定設計）

HTML/CSS 驗證工具可作為輔助證據。判斷分依評分表執行。

## 配分

| WSOS 區塊 | 說明 | 分數 |
| ------------ | ----------- | ------ |
| 1 | 工作組織與自我管理 | 1.75 |
| 2 | 溝通與人際技能 | 1 |
| 3 | 設計實作 | 7.75 |
| 4 | 前端開發 | 6.5 |
| **合計** | | **17** |

第 3 區塊涵蓋版面、視覺設計與內容呈現。第 4 區塊涵蓋響應式行為、必要 CSS 技巧、動效與前端結構性需求。詳細評分面向見 [`marking/marking-scheme.json`](./marking/marking-scheme.json)。
