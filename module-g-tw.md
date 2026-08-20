# 測試專案大綱 – 模組 G – SwapLoop 自動化測試

## 競賽時間

選手完成本模組的時間為 **3 小時**。

## 簡介

SwapLoop 是一個虛構的上海社區試點，提供更安全的電動自行車電池處理方式：騎士可在站點交換可拆卸電池，或使用受監控的充電車位；外送合作夥伴則透過訂閱方案付費，並依用量計算超量費用。

本模組為**純測試**。提供兩個可運作的應用程式：

1. **前端測試** — SwapLoop **多頁騎士 UI**（模組 D 行為），對接相容於模組 C 的 API，並附有 **Cypress** 測試骨架。
2. **後端測試** — **合作夥伴帳單服務**（模組 B 帳單概念），以 PHP 實作，並附有 **PHPUnit** 測試骨架。

選手**不得修改**應用程式原始碼。請填寫空白的測試內容，使測試套件能驗證下方所述行為。

工作坊工具與 Docker 配置依循 [ws26-cypress](https://github.com/Skill17-WebTechnologies/ws26-cypress) 與 [ws26-phpunit](https://github.com/Skill17-WebTechnologies/ws26-phpunit)。

## 專案與任務總述

### 你會收到什麼

| 子模組 | 路徑 | 你的工作 |
| --------- | ---- | --------- |
| Rider UI + Cypress | [`assets/frontend-testing/`](./assets/frontend-testing/) | 實作 `cypress/e2e/` 下的規格 |
| Billing + PHPUnit | [`assets/backend-testing/`](./assets/backend-testing/) | 實作 `tests/` 下的測試 |

### 規則

- **不要**編輯 `public/`、`server.js`，或 `src/` 下的 PHP（除非講義明確允許某個設定檔）。
- 優先使用頁面上已有的穩定選擇器（`data-testid`、文件中記載的 id）。若已有 test id，請勿僅依賴脆弱的 CSS 版面 class。
- 當情境需要與真實 I/O 或 HTTP 隔離時，請使用 mock、stub 與 intercept。
- 如何撰寫 assertion 由你決定；評分會檢查每個列出的期望是否被正確覆蓋。

### 建議時間分配

| 區塊 | 重點 | 預估時間 |
| ----- | ----- | ------------ |
| A | Cypress 騎士流程 | 約 1.5–2 小時 |
| B | PHPUnit 帳單 | 約 1–1.5 小時 |

### 種子帳號（前端）

| 電子郵件 | 密碼 | 說明 |
| ----- | -------- | ----- |
| `lin.xiaoyu@swaploop.test` | `password123` | 啟用中的可換電騎士（`SL-48`） |
| `chen.wei@swaploop.test` | `password123` | 啟用中的內建電池騎士（`GB-AC-48`） |
| `sun.hao@swaploop.test` | `password123` | 已停權 — 登入必須失敗，並顯示 API 的 `403` 訊息 |

### 實體詞彙（前端）

| 詞彙 | 意義 |
| ---- | ------- |
| **SwapLoop Station** | 據點（`SWAP`、`CHARGING` 或 `HYBRID`） |
| **Battery Slot** | 換電艙位（`SWAP_BAY`） |
| **E-bike Charging Bay** | 整車充電車位（`BIKE_BAY`） |

---

## 第 A 部分 — 前端測試（Cypress）

`assets/frontend-testing/` 下的多頁騎士 UI 是可運作的模組 D 風格用戶端（`login.html`、`stations.html`、`station.html`、`activity.html`、`scan.html` 等）。與應用程式一併提供相容於模組 C 的 mock API（評審也可能接上真實的模組 C Main Backend）。規格檔已含 `describe` / `it` 標題；請實作每個 `it` 主體。

### A1 · 登入（`01_login.cy.js`）

| 測試 | 期望（要驗證什麼） |
| ---- | ------------------ |
| loads the login page | 登入畫面可到達，並顯示電子郵件、密碼與送出控制項 |
| shows an error when email is missing | 未填電子郵件即送出時，會顯示清楚的驗證訊息；使用者留在登入頁 |
| shows an error for invalid credentials | 錯誤的電子郵件／密碼會顯示 API 回傳的無效憑證訊息 |
| blocks suspended accounts | 已停權種子帳號無法進入應用程式；停權訊息須與無效憑證訊息可區分 |
| signs in and reaches stations | 有效騎士可登入，進入已驗證的站點體驗，且工作階段身分可見 |

### A2 · 註冊（`02_register.cy.js`）

| 測試 | 期望（要驗證什麼） |
| ---- | ------------------ |
| requires vehicle profile fields for swappable mode | 可換電模式註冊若缺少電池類型，不得建立帳號 |
| completes two-step register for a swappable rider | 完成車輛資料 + 模擬支付寶綁定步驟後，新的可換電騎士可建立帳號並進入應用程式 |
| completes two-step register for an integrated rider | 同樣流程適用於內建電池模式：需有連接器類型、且不帶電池類型 |

### A3 · 站點列表（`03_stations.cy.js`）

| 測試 | 期望（要驗證什麼） |
| ---- | ------------------ |
| lists stations for a signed-in rider | 登入後，站點列表顯示種子站點的名稱與類型 |
| filters by station type | 選擇類型篩選後，列表僅保留該類型（例如僅 `SWAP`） |
| shows compatible availability indication | 依已登入騎士資料，相關卡片會指出是否有相容且就緒的電池或車位 |
| unauthenticated visitors can browse stations | 未登入訪客可開啟公開站點瀏覽路徑並看到列表 |

### A4 · 站點詳情與預約（`04_station_detail.cy.js`）

| 測試 | 期望（要驗證什麼） |
| ---- | ------------------ |
| opens station detail from a list card | 從卡片導覽後顯示該站點中樞（識別資訊、狀態、可用性指引） |
| reserves a swap hold when eligible | 符合資格的可換電騎士可預約電池；UI 顯示進行中預留，並可通往 Activity |
| surfaces last-charge conflict without inventing availability | 當預約回傳衝突時，騎士看到清楚的「已不可用」類訊息，且預約不得呈現為成功 |
| blocks a second reserve while another station hold is active | 若騎士已在其他站點有進行中服務，新預約會被阻擋，且可到達 Activity |

### A5 · 活動 — 換電（`05_activity_swap.cy.js`）

| 測試 | 期望（要驗證什麼） |
| ---- | ------------------ |
| shows countdown and actions while reserved | 進行中的換電處於 `RESERVED` 時，顯示與倒數相關的 UI，以及開始與取消控制項 |
| starts and confirms a swap | 騎士可從 reserved → started → confirmed，並看到來自 API（非自行發明）金額的收據 |
| cancels a reserved swap | 取消會結束進行中服務，且 Activity 反映該服務已不存在 |

### A6 · 活動 — 充電（`06_activity_charging.cy.js`）

| 測試 | 期望（要驗證什麼） |
| ---- | ------------------ |
| starts charging from a reserved hold | 符合資格的內建電池騎士可從 Activity 開始充電 |
| reflects live charging status until ready | 充電期間，UI 反映 API 的即時狀態，直到可取車（ready-for-collection） |
| collects the bike and shows a receipt | 取車完成流程，並顯示 API 收據欄位 |

### A7 · 站點 QR 掃描（`07_qr_scan.cy.js`）

| 測試 | 期望（要驗證什麼） |
| ---- | ------------------ |
| embeds the QR emulator | 掃描畫面露出 QR 模擬器介面 |
| navigates to station hub on a valid poster payload | 發出標準站點深層連結後，開啟該站點詳情中樞 |
| rejects a mismatched payload | 非站點 payload 顯示清楚的不符訊息，且不得開啟假站點 |

### A8 · HTTP intercepts 與錯誤（`08_http_errors.cy.js`）

| 測試 | 期望（要驗證什麼） |
| ---- | ------------------ |
| handles protected-route unauth by returning to login | 清除工作階段後開啟受保護畫面，會回到登入 |
| shows an actionable message on server error during reserve | 當預約被強制以伺服器錯誤失敗時，UI 顯示可操作的錯誤訊息（無堆疊追蹤） |
| prevents double-submit while reserve is pending | 預約請求進行中時，騎士無法從同一控制項再次觸發成功的重複預約 |

---

## 第 B 部分 — 後端測試（PHPUnit）

`assets/backend-testing/` 下的 PHP 套件計算合作夥伴訂閱帳單（模組 B 概念），**不使用資料庫**，也**不使用 Composer 或第三方函式庫**。`BillingService` 使用 PHP 的 `date()` 與 `file_put_contents()`，再透過具體的 `Notifier` 類別通知（無介面）。領域程式碼使用**繼承**與**工廠**。

請研讀 `src/`。實作扁平結構 `tests/*.php` 中每個未完成的測試。

### 受測帳單規則

1. 呼叫端提供 `SubscriptionPlan` 與使用次數（不是 SQL）。
2. 方案：每月**基本費用**、**包含額度**、超出額度的**超量**依方案費率計算。
3. **用量折扣級距**對應當月用量；折扣僅適用於**超量部分**。
4. 金額為整數**人民幣元（CNY yuan）**。
5. 匯出檔名包含期間與來自 `date('Y-m-d')` 的**今日日期**。
6. `Notifier` 僅在寫入成功後執行；寫入失敗／無效用量不得通知。

### B1 · 帳單計算器（`BillingCalculatorTest`）

| 測試 | 期望（要驗證什麼） |
| ---- | ------------------ |
| calculates base fee only when uses stay within quota | 用量在額度內 → 合計等於基本費用；超量與折扣為零 |
| calculates overage without discount when no tier matches | 用量超過額度且無符合級距 → 以全額費率計收超量 |
| applies volume discount to overage only | 符合級距時僅降低超量部分；基本費用不變 |
| rejects negative usage | 負數使用次數被拒絕 |

### B2 · 折扣級距（`DiscountTierTest`）

| 測試 | 期望（要驗證什麼） |
| ---- | ------------------ |
| selects the highest matching tier for a usage count | `SubscriptionPlan::matchTier` 選出正確級距 |
| returns no tier when usage is below all thresholds | 低用量不產生折扣級距 |

### B3 · 帳單文件／繼承（`BillingDocumentTest`）

| 測試 | 期望（要驗證什麼） |
| ---- | ------------------ |
| partner summary exposes expected breakdown fields | 合作夥伴、期間、方案、使用次數、額度、超量、費用、折扣、合計 |
| finance CSV renders a header and partner rows | 標題列 + 每個合作夥伴一列（id、名稱、期間、方案、使用次數、合計） |
| document types share a common abstraction | 具體類型繼承 `BillingDocument` |

### B4 · 文件工廠（`DocumentFactoryTest`）

| 測試 | 期望（要驗證什麼） |
| ---- | ------------------ |
| creates a partner summary document | 工廠對該種類回傳 `PartnerSummary` |
| creates a finance CSV document | 工廠對該種類回傳 `FinanceCsv` |
| rejects an unknown document kind | 未知種類被拒絕 |

### B5 · 帳單服務（`BillingServiceTest`）

| 測試 | 期望（要驗證什麼） |
| ---- | ------------------ |
| writes a partner summary file whose name includes the current date | 在輸出目錄寫入真實檔案；檔名包含 `date('Y-m-d')` 與合作夥伴／期間 |
| written file contains breakdown | 檔案內容反映計算後的帳單欄位 |
| notifies after a successful write | 真實寫入成功後，`Notifier` 被呼叫一次 |
| does not notify when write fails | 錯誤／不存在的輸出目錄 → 錯誤；`Notifier` 不得執行 |
| does not write or notify when usage is invalid | 負數使用次數 → 不寫檔也不通知 |

---

## 評分

評分會在不變更所提供應用程式的前提下執行，並依上述期望評估選手測試（盡可能自動化，並由專家審查覆蓋範圍與 assertion 品質）。空白或不完整的案例，該面向為零分。

## 配分

| WSOS 區塊 | 說明 | 分數 |
| ------------ | ----------- | ------ |
| 1 | 工作組織與自我管理 | 5 |
| 2 | 溝通與人際技能 | 5 |
| 4 | 前端開發（Cypress） | 45 |
| 5 | 後端開發（PHPUnit） | 45 |
| **合計** | | **100** |

