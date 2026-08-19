# 測試專案大綱 – 模組 D – SwapLoop 騎士 SPA

## 競賽時間

選手完成本模組的時間為 **3 小時**。

## 簡介

SwapLoop 是一個虛構的上海社區試點服務，提供比在室內為電動自行車電池充電更安全的替代方案。相容的外送與私人電動自行車可在換電站交換可拆卸電池；內建電池的電動自行車則使用受監控的 **E-bike Charging Bay（電動自行車充電車位）**。

本模組要打造取代原生行動客戶端的 **行動優先騎士 SPA**。此 SPA 會呼叫所提供的 **模組 C Main Backend** REST API。業務規則、資格、預約完整性、末次充電隔離、充電模擬與價格快照仍以模組 C 為準。模組 D 負責呈現、用戶端狀態、驗證、載入與錯誤處理、導覽，以及 QR 模擬器整合。

## 專案與任務總述

實作一個可獨立執行的 **單頁應用程式（SPA）**，讓騎士可以註冊、登入、尋找站點、預約服務、完成換電或充電流程，並查看隨用隨付收據。

高層能力（細節見 [需求](#需求)）：

- **驗證與個人資料：** 註冊（兩個步驟：車輛資料 + 模擬支付寶綁定）、以 bearer token 登入、檢視與編輯個人資料
- **站點：** 列表、篩選（類型／附近／相容可用性）、站點詳情中樞、預約換電或充電預留
- **站點 QR：** 整合所提供的 QR 模擬器 web component；解析站點深層連結；開啟站點中樞
- **活動：** 進行中服務生命週期（換電的倒數、開始／確認／取消；充電的開始／即時輪詢／取車／取消），以及含收據的近期服務
- **安全 UX：** 處理模組 C 末次充電的 `409`，不要在用戶端自行發明可用性或隔離規則
- **錯誤與無障礙：** 針對常見 HTTP 狀態做不同處理；可用鍵盤操作的流程；不單靠顏色傳達狀態

### 環境與技術堆疊

- 以現代框架（例如 React、Vue 或 Angular）打造 **JavaScript SPA**，並使用 **用戶端路由**。重新載入深層連結 URL 時，必須在從儲存空間讀取驗證狀態後還原同一個畫面（未儲存的表單輸入除外）。
- 透過 `/api/v1` 呼叫 **模組 C Main Backend**。路徑、查詢參數與結構描述見 [`assets/api/main-backend.openapi.yaml`](./assets/api/main-backend.openapi.yaml)（離線 Swagger UI：[`assets/api/main-backend-docs/index.html`](./assets/api/main-backend-docs/index.html)）。評分與本機主機見 [主機、種子帳號與測試夾具](#主機種子帳號與測試夾具)。
- 將不透明的 bearer token 持久化到 `localStorage`（或同等機制），並在受保護的呼叫送出 `Authorization: Bearer <token>`。
- 嵌入競賽 **QR 模擬器**，來源為 [`assets/qr-code-emulator/`](./assets/qr-code-emulator/)。整合步驟見 [站點 QR 掃描](#站點-qr-掃描) 與 [`assets/handouts/handout-qr-emulator-integration.md`](./assets/handouts/handout-qr-emulator-integration.md)。
- 優先使用原生 `fetch`（或框架的 HTTP 用戶端）。只顯示 Main Backend 回傳的價格、隔離結果與可用性。

### 主機、種子帳號與測試夾具

將 `cXX` / `YYYY` 替換為競賽使用者名稱與 PIN。本機 URL 供開發使用。

| 服務                                          | 評分                                                                 | 本機                                     |
| --------------------------------------------- | -------------------------------------------------------------------- | ---------------------------------------- |
| Main Backend (`/api/v1`)                      | `https://cXX-YYYY-module-c.sitc.skillsit.eu/api/v1`                  | 通常為 `http://localhost:5000/api/v1`    |
| Station Service（僅供 QR 模擬器 `service-url`） | `https://cXX-YYYY-station-service.sitc.skillsit.eu`                  | `http://localhost:4020`                  |
| QR 測試器（選擇作用中的海報）                 | `https://cXX-YYYY-station-service.sitc.skillsit.eu/qr-code-emulator` | `http://localhost:4020/qr-code-emulator` |

SPA 除了透過 `<swaploop-qr-emulator>` 之外，不得呼叫 Station Service。末次充電遙測與即時充電工作階段由 Main Backend 代理。

所有種子使用者的明文密碼：`password123`。當 10 秒預留已過期時，請在評分項目之間重設 Main Backend 種子資料。

| 電子郵件                   | 狀態      | 資料                  | 用途                                      |
| -------------------------- | --------- | --------------------- | ----------------------------------------- |
| `lin.xiaoyu@swaploop.test` | ACTIVE    | SWAPPABLE / SL-48     | 登入、換電流程、station-005 的 409        |
| `chen.wei@swaploop.test`   | ACTIVE    | INTEGRATED / GB-AC-48 | 充電流程                                  |
| `zhao.min@swaploop.test`   | ACTIVE    | SWAPPABLE / SL-48     | 備用可換電騎士                            |
| `sun.hao@swaploop.test`    | SUSPENDED | INTEGRATED / GB-AC-60 | 登入 `403`                                |

**末次充電 409（換電）：** 以 `lin.xiaoyu@swaploop.test` 在 **station-005** 預約 SWAP 服務。該站唯一 READY 的 SL-48 電池包是 `battery-007`（持續過熱測試夾具）。Main Backend 應以 `409 CONFLICT` 拒絕。請勿用 station-002 做此測試：該站還有另一顆 READY 的 SL-48 電池包，尖峰電池被略過後仍可能預約成功。

### 技術限制

- 依照 API 回傳的時間戳顯示（帶偏移的 ISO 8601）。請勿在用戶端發明或重新計算時間。
- 對於 **附近** 站點篩選，使用瀏覽器 Geolocation API，並依 OpenAPI 在 `GET /stations` 傳入 `lat`、`lng` 與 `radiusMeters`（預設 **1500**）。開發與評分期間，請在 DevTools（Sensors / Location）將瀏覽器位置設為 **Shanghai（上海）**，因為種子站點位於上海。
- 真實支付寶開放平台整合、真實付款、真實硬體、OAuth／token 刷新／電子郵件驗證／密碼重設皆 **不在範圍內**。
- 範圍內僅有 **隨用隨付** 付款。其他付款方式（例如月費方案）將於日後提供。

### 實體詞彙

請在 UI 中一致使用此詞彙：

| 詞彙                               | 意義                                                                                         |
| ---------------------------------- | -------------------------------------------------------------------------------------------- |
| **SwapLoop Station**               | 完整服務據點（`SWAP`、`CHARGING` 或 `HYBRID`）。**只有站點有海報 QR。**                      |
| **Battery Swap Cabinet**           | 儲存並為可交換電池充電的設備（本競賽沒有 QR）                                                |
| **Battery Slot**（Swap Bay）       | 最多容納一顆電池的單一艙位（API 單元常為 `SWAP_BAY`）                                        |
| **E-bike Charging Bay**（Bike Bay） | 為整輛內建電池之電動自行車充電的車位（`BIKE_BAY`）                                           |

相容性目錄（請勿發明平行代碼）：

- 可交換：`batteryMode` `SWAPPABLE` + `batteryType` `SL-48` \| `SL-60`（衍生的 `voltageClass` `48V` / `60V`）
- 內建：`batteryMode` `INTEGRATED` + `connectorType` `GB-AC-48` \| `GB-AC-60`

### 範圍內角色

| 角色                    | 模組 D 行為                                                                               |
| ----------------------- | ----------------------------------------------------------------------------------------- |
| 已註冊騎士（可換電）    | 註冊／登入、預約換電、開始 + 確認、收據                                                   |
| 已註冊騎士（內建電池）  | 註冊／登入、預約充電車位、開始 + 輪詢 + 取車、收據                                        |
| 未驗證訪客              | 僅登入與註冊；登入前不得使用站點列表、站點詳情、掃描或活動                                |

### 素材

```text
assets/
  api/
    main-backend.openapi.yaml          # Main Backend 契約 (/api/v1)
    main-backend-docs/                 # 離線 Swagger UI
  handouts/
    handout-qr-emulator-integration.md # 嵌入 <swaploop-qr-emulator>
  qr-code-emulator/
    swaploop-qr-emulator.js            # IIFE bundle（註冊 custom element）
    README.md
  wireframes/                          # 頁面結構指引（非像素級還原）
```

[`assets/wireframes/`](./assets/wireframes/) 中的線框稿僅為 **頁面結構指引**。請勿逐像素實作。線框稿上的斜體或說明文字是給選手的作者指引——**不得**出現在騎士 UI 中。可見標籤、範例名稱與占位文字可以替換，只要必要行為仍在即可。

## 需求

SwapLoop 騎士 SPA 必須實作下列行為。

### 應用程式外殼與導覽

SPA 應有原生騎士 App 的感覺，而不是只有頂部選單的桌面網站。

騎士登入後，在站點、掃描、活動與個人資料畫面保持 **貼齊視窗底部的固定導覽列**。主內容捲動時它仍保持可見。至少包含下列目的地，並附標籤與／或圖示：

- **站點** — 站點列表
- **掃描** — 站點 QR 模擬器
- **活動** — 進行中與近期服務
- **個人資料** — 騎士帳號與車輛詳情

目前目的地必須在視覺上可區分。點選目的地必須切換該畫面，且不得整頁重新載入。登入與兩步驟註冊畫面不需要底部列。

請預留足夠的底部內距，避免卡片、預約按鈕與活動操作被導覽列遮住。

### 響應式設計與無障礙

先為拿在騎士手中的手機設計：單欄、夠大的點按目標，以及評審把視窗拉到桌面寬度時仍可用的版面。預約或活動操作不得只依賴 hover 控制項。

鍵盤使用者必須能在沒有指標裝置的情況下完成服務流程：預約、開始、確認、取車與取消必須可用 Tab 與 Enter／Space 到達並操作，且有可見的焦點狀態。

狀態不得只靠顏色。請將顏色搭配文字與／或圖示，讓騎士能分辨站點已關閉、預留即將到期，或請求失敗。倒數到期、重大服務狀態變更與 API 錯誤需要清楚的畫面上訊息，而不能只靠顏色變化。

### 驗證與個人資料

未登入者只能看到 **登入** 與 **註冊**。站點列表、站點詳情、掃描與活動必須在驗證之後。Main Backend 的 `GET /stations` 是公開的，但本 SPA 在騎士尚未擁有已儲存 token 前，不得顯示站點、掃描或活動畫面。

註冊是兩步驟流程。第一步收集電子郵件、密碼、顯示名稱，以及依電池模式而定的車輛資料。對 `SWAPPABLE` 騎士要求 `batteryType` 為 `SL-48` 或 `SL-60`，並省略 `connectorType`。對 `INTEGRATED` 騎士要求 `connectorType` 為 `GB-AC-48` 或 `GB-AC-60`，並省略 `batteryType`。註冊時請勿送出 `voltageClass`；模組 C 會推導它，並在騎士個人資料中回傳。

第二步是用於隨用隨付的 **模擬支付寶綁定**。顯示模擬的支付寶 QR／綁定 UI，等待短暫延遲，然後顯示成功並啟用 **建立帳號**。請勿呼叫即時付款 API。

註冊成功後，儲存回傳的 token 並進入已驗證應用程式。回訪騎士以電子郵件與密碼登入。若登入因 `403`（已停權帳號）被拒絕，留在登入畫面並顯示該訊息——請勿儲存 token。

個人資料畫面載入目前騎士，並顯示顯示名稱、電子郵件、角色、狀態、電池模式、電池類型 **或** 連接器類型（視何者適用）、衍生的電壓等級，以及可換電騎士目前持有電池包時的 `currentBatteryId`。騎士可以編輯顯示名稱與車輛資料，欄位規則與註冊相同、依模式而定。登出必須清除已儲存的 token 並回到登入。

### 站點列表與篩選

顯示可捲動的 SwapLoop 站點列表。載入符合已登入騎士車輛資料的可用性，讓每張卡片能說明就緒電池（換電）或充電車位（內建）是否真的可用。此列表僅供已驗證騎士使用。

騎士必須能縮小列表。依站點類型篩選：全部站點，或僅 `SWAP`、`CHARGING` 或 `HYBRID`（`GET /stations` 的 `type`）。提供 **相容可用性** 篩選，只保留對已登入資料有就緒電池包或車位的站點：`service=SWAP` 搭配 `batteryType`，或 `service=BIKE_BAY` 搭配 `connectorType`。提供 **附近** 篩選，使用瀏覽器 Geolocation API 與 `lat`、`lng`、`radiusMeters`（預設 **1500**，約 1.5 公里）。評分期間瀏覽器位置會設為上海。

每張站點卡片顯示名稱、類型、生命週期狀態，以及 API 有提供時的地址或營業時間。開啟附近篩選時，顯示距離（`distanceMeters`）。必須清楚顯示此騎士是否有相容電池或充電車位就緒——而不只是站點存在。仍出現在列表中的已關閉或已停權站點，必須能從生命週期狀態辨識（文字與／或圖示，不能只靠顏色）。

空的 **已篩選** 列表不得全部長得一樣。請區分「目前沒有相容項目就緒」與「附近沒有站點」。開啟卡片會進入該站點的詳情頁。

### 站點詳情中樞

此畫面是單一站點的現場中樞。納入已登入騎士的可用性，讓頁面能說明是否有相容電池或充電車位就緒。顯示識別資訊、地址、狀態、營業時間、相容性（服務、電池類型、連接器類型、電壓等級），以及給此騎士的簡短指引。

若騎士符合資格且站點為 `ACTIVE`，為可換電騎士提供 **預約電池**，或為內建電池騎士提供 **預約充電車位**。建立預留會向 Main Backend 請求此站點的 `SWAP` 或 `CHARGING` 服務。預留時長是 API 的 `expiresAt` 值——競賽 API 為 **10 秒**，方便評分到期（正式環境的預留通常為 15–30 分鐘）。成功後，顯示進行中的預留，以及通往 **活動** 的清楚路徑。在倒數結束前於活動畫面開始或取消；若需要新的預留，請重設 Main Backend 種子資料。

若騎士已有 **此站點的進行中服務**，顯示該預留或進行中狀態，以及開啟活動的主要控制項。在該預留仍屬於此騎士時，請勿顯示誤導性的「沒有就緒電池／車位」標記。若他們已有 **另一站點的進行中服務**，在此阻擋新的預約，改為連結到活動。

**末次充電安全（換電）：** Main Backend 可能在隔離或缺少遙測後，以 `409 CONFLICT` 拒絕換電預留。顯示面向騎士的文案，例如「所選電池已不可用」。重新載入此站點使可用性為最新，然後在沒有相容項目時隱藏或停用預約。請勿在用戶端重新實作尖峰或持續過熱規則，也請勿靜默把騎士送到另一站點。使用 [主機、種子帳號與測試夾具](#主機種子帳號與測試夾具) 中的 **station-005** 測試夾具。

未知的站點 id——包括仍開啟此中樞的錯誤 QR 解析——必須以面向騎士的語言呈現 `404`。

### 站點 QR 掃描

為騎士提供 **掃描** 畫面，嵌入所提供的 `<swaploop-qr-emulator>` web component。請勿使用裝置相機。模擬器提供會印在 SwapLoop Station 的海報 QR。SPA **不得** 為了末次充電遙測或即時自行車充電車位工作階段而呼叫 Station Service——那些由 Main Backend 代理。騎士 UI 對 Station Service 的唯一流量，是透過此元件的 `service-url`。

如何載入 IIFE bundle、設定 `service-url`、藉由變更 `scan-request-id` 開始掃描，以及從 `qr-scan` 事件讀取 `event.detail.payload`，記載於 [`assets/handouts/handout-qr-emulator-integration.md`](./assets/handouts/handout-qr-emulator-integration.md)，並含 React、Vue 與 Angular 範例。腳本為 [`assets/qr-code-emulator/swaploop-qr-emulator.js`](./assets/qr-code-emulator/swaploop-qr-emulator.js)。

另外提供 **QR 測試器**，讓你可以在沒有相機的情況下選擇並檢查作用中的海報代碼。請開啟 `https://cXX-YYYY-station-service.sitc.skillsit.eu/qr-code-emulator`（將 `cXX` / `YYYY` 替換為你的工作站主機；本機 Station Service：`http://localhost:4020/qr-code-emulator`）。用它設定模擬器將回傳哪個站點 QR，然後在 SPA 中觸發掃描。

每個站點海報編碼同一種深層連結：

```text
https://app.swaploop.test/stations/{stationId}
```

當元件發出 `qr-scan` 時，解析該字串。接受完整 URL，也接受單純的 `station-…` id。任何其他形狀都是不符——顯示清楚的面向騎士訊息，且不要發明站點。

若解析得到站點 id，開啟該站點的詳情中樞。在 Main Backend 成功回傳該站點之前，將掃描值視為不可信。偽造或未知的 id 不得看起來像真實預約或有效中樞。

### 活動中樞

活動是已登入騎士管理目前持有服務、並回顧近期服務的地方。從 Main Backend 載入騎士的 **進行中** 項目與 **近期** 列表。

對於進行中的 **換電**（`SWAP`），在預留為 `RESERVED` 時，依 `expiresAt` 顯示倒數（與站點中樞相同的競賽 **10 秒** 預留），並提供 **開始換電** 與 **取消**。開始（`STARTED`）後，提供 **確認換電** 與 **取消**。實體交接不需要再掃一次 QR。

對於進行中的 **充電** 工作階段（`CHARGING`），在 `RESERVED` 時顯示同樣的倒數、**開始充電** 與 **取消**（僅能在 `RESERVED` 取消）。充電開始後，約每秒輪詢一次即時狀態，並顯示 SOC、功率、溫度，以及任何 `endsAt` 倒數。競賽充電模擬約持續 **15 秒**。當 Main Backend 回報工作階段完成／`READY_FOR_COLLECTION` 時停止輪詢，然後提供 **取車**。請勿在正常路徑上放 **標記就緒** 控制項——後端會透過狀態輪詢推進到就緒。

確認或取車後，依 `priceYuan`／`priceCode` 以人民幣（CNY）顯示 **收據**。請勿在 SPA 中計算或猜測價格。列出近期已完成的服務，並讓騎士重新開啟那些收據欄位。若沒有進行中也沒有近期項目，請清楚說明，並連結回站點。

Main Backend 的安全停止與錯誤狀態（例如 `SAFETY_CUTOFF`）必須以淺白的面向騎士語言呈現。

### 用戶端狀態與 API 使用

依 OpenAPI 使用精確路徑。預期的能力分組：

```text
POST /auth/login, POST /auth/register
GET  /me, PATCH /me
GET  /me/activity
GET  /stations, GET /stations/{id}
POST /services, GET /services/{id}
POST /services/{id}/start
GET  /services/{id}/charging-status
POST /services/{id}/confirm
POST /services/{id}/collect
POST /services/{id}/cancel
```

以可操作、無障礙的訊息處理 `401`、`403`、`404`、`409`、`422` 與 `5xx`，且不得顯示內部堆疊追蹤。受保護騎士動作上的 `401`（缺少或未知 token）必須清除已儲存工作階段並回到登入。之後受保護呼叫上的 `403`（例如帳號已被停權）也必須清除工作階段並回到登入。已停權帳號的登入 `403` 留在登入畫面並顯示訊息，如驗證一節所述。

在變更請求進行中時防止重複送出。將伺服器狀態與暫時 UI 狀態分開；顯示載入或過期指示，而不是把快取的可用性當成當前狀態。

## 評分

評分由專家依評分表在 **Google Chrome** 中進行，使用模組 C 參考 API（重設種子）以及 QR 模擬器所用的 Station Service。若有提供自動化測試可以使用；必須進行註冊 → 尋找 → 預約 → 活動完成 → 收據的手動操作流程。

競賽預留為 **10 秒**：預約後立即開啟活動並開始，或在評分項目之間重設 Main Backend 種子資料。充電即時遙測約執行 **15 秒** 後進入 `READY_FOR_COLLECTION`。

## 配分

本專案配分如下：

| WSOS 區塊 | 說明               | 分數 |
| --------- | ------------------ | ---- |
| 1         | 工作組織與自我管理 | 1    |
| 2         | 溝通與人際技能     | 1.5  |
| 3         | 設計實作           | 2.5  |
| 4         | 前端開發           | 12   |
| **合計**  |                    | 17   |
