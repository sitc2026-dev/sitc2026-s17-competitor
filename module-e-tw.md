# 測試專案大綱 – 模組 E – SwapLoop ChargeRun 互動式前端應用程式

## 競賽時間

選手完成本模組的時間為 **3 小時**。

## 簡介

SwapLoop 是一個虛構的上海社區試點計畫，旨在探索比室內為電動自行車電池充電更安全的替代方案。相容的外送與私人電動自行車可在換電站交換可拆卸電池；內建電池的電動自行車則使用受監控的充電車位；外送合作夥伴可取得受控的優先使用權；而營運人員與安全稽查人員負責管理站點、資產與事件。

每個 **SwapLoop Station** 在電池交換櫃旁都設有觸控式 kiosk。當騎士等待新電池包或受監控車位開放時，kiosk 會提供一個短時間的品牌小遊戲。

**ChargeRun** 就是這個 kiosk 體驗！玩家操控一台電動自行車，收集能量包、避開行人與坑洞，並在能量耗盡前抵達 Battery Swap Cabinet。成功的一趟遊戲，呼應了 SwapLoop 對真實騎士的期待：帶著電力抵達站點，而不是半路拋錨。

當一趟遊戲以 **win** 結束時，kiosk 會直接開啟 **Charge Card** 工作室。若是 **lose**，先顯示短暫 toast 或 overlay（見 [Win / lose](#win--lose)），再開啟工作室。工作室是個 canvas 編輯器，玩家可建立個人成績卡（背景照片、SwapLoop 邊框、分數文字與簽名）。卡片可下載、分享，或送到站點印表機。蒐集 Charge Card 是試點社群互動的一部分：騎士可保存為紀念，並可在參與站點兌換促銷換電點數。🔋

你建立的應用程式必須能 **獨立運作**。它 **不得** 呼叫後端 API、Station Service，或任何騎士端 REST 端點。所有行為都必須在用戶端完成，並依本簡報規則與所提供素材實作。

## 專案與任務總述

實作一個可獨立執行的單頁應用程式，呈現為 **固定尺寸的橫向 kiosk**。

畫面 **僅有兩個**：

1. **Game** - ChargeRun（DOM/CSS/JS），以鍵盤操控。
2. **Charge Card studio** - 具備上傳、簽名、下載、分享與列印的 canvas 合成器。

高層能力（細節見 [Requirements](#requirements)）：

- 固定 **1280×720** px kiosk 外殼（不需響應式）— kiosk 需置中於畫面
- ChargeRun 格狀遊戲：每趟隨機化、能量、能量包、坑洞、行人、櫃體目標與計分
- Charge Card 工作室：背景（檔案輸入 + 拖曳上傳）、動態文字、簽名、下載、分享、列印
- 以 `localStorage` 儲存顯示名稱
- Charge Card 上所有可見日期時間，需使用上海時區與中文日期格式
- 可在 Chrome 安裝／下載為 web app。只需提供最小可安裝設定。Logo 位於 `assets/module-e/logos` 資料夾
- 評分目標為最新版本 **Google Chrome**
- 盡可能使用提供的 `Inter` 字型

### 詞彙

| Term                     | Meaning                               |
| ------------------------ | ------------------------------------- |
| **SwapLoop**             | 平台品牌                              |
| **ChargeRun**            | 本 kiosk 小遊戲名稱                   |
| **Charge Card**          | 每趟完成後建立的個人成就圖片          |
| **Battery Swap Cabinet** | 目標格位 — 在能量仍有剩餘時抵達可獲勝 |
| **Energy pack**          | 可收集道具，用於恢復能量              |
| **Pothole**              | 一次性格位，會扣除能量與分數          |
| **Pedestrian**           | 移動障礙物 — 碰撞即失敗               |

### 建議時間分配

| Block | Focus                  | Approx. time |
| ----- | ---------------------- | ------------ |
| A     | Kiosk shell, ChargeRun | ~1.5 hours   |
| B     | Charge Card studio     | ~1.5 hours   |

<a id="design-and-wireframes"></a>

### Design and wireframes

選手應設計一個具 **kiosk 感** 且帶有現代遊戲風格的 ChargeRun 畫面。僅提供 **Inter** 字型。顏色、版面細節與視覺風格由選手自行決定。線框圖與範例影片僅為 **功能參考**（必備項目與行為）。它們展示的是結構，而非要照抄的外觀；不得重現線框圖本身的設計。

功能參考：

- Game screen wireframe: [`assets/module-e/wireframes/1-game-screen.png`](./assets/module-e/wireframes/1-game-screen.png)
- Charge Card studio wireframe: [`assets/module-e/wireframes/2-charge-card-studio-screen.png`](./assets/module-e/wireframes/2-charge-card-studio-screen.png)
- Example behaviour video: [`assets/module-e/wireframes/video-example.mov`](./assets/module-e/wireframes/video-example.mov)

![Game screen wireframe](./assets/module-e/wireframes/1-game-screen.png)

![Charge Card studio wireframe](./assets/module-e/wireframes/2-charge-card-studio-screen.png)

遊戲物件（電動自行車、能量包、加成包、坑洞、行人、櫃體與地形）應以 DOM/CSS 製作成 **有創意的 CSS 造型**（漸層、陰影、光暈、偽元素、細微動畫等）。僅用純色方塊加文字標籤不足。每個物件都必須能靠造型與樣式 **立即辨識**，而非只靠顏色。

HUD、Legend 與 Charge Card 工作室應共享一致的 SwapLoop 視覺風格。

<a id="requirements"></a>

## Requirements

### Kiosk shell and navigation

1. 提供固定橫向 kiosk 舞台，尺寸必須精確為 **1280×720** 像素，並置中於頁面。
2. 僅允許兩個畫面，且都在 kiosk 內：**Game** 與 **Charge Card studio**。
3. 兩畫面間需以 SPA 方式切換，不可整頁重載。
4. 應用程式載入時直接開啟 **Game**（無起始選單）。
5. 在 Charge Card 工作室提供 **Play again** 控制，回到 Game 並啟動 **新** 一趟配置（重新隨機；直到第一個移動鍵前保持凍結）。
6. 將該趟結果（`WIN` \| `LOSE`）與最終分數傳入卡片工作室。

### ChargeRun (game)

依以下規格建置遊戲。

#### Story and goal

玩家在小型城市格子中控制電動自行車。能量會隨時間流失。收集能量包以維持動力。避開行人與坑洞。帶著剩餘能量抵達 **Battery Swap Cabinet** 即可獲勝。

#### Run states

| State     | Behaviour                                                                                                                                                              |
| --------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `READY`   | 格局已建立並顯示。玩家位於出生點。能量流失與行人移動 **凍結**。顯示提示：`Press Arrow keys or WASD to start`。                                                         |
| `RUNNING` | 在 **第一個** 有效移動鍵（`ArrowUp` / `ArrowDown` / `ArrowLeft` / `ArrowRight` / `W` / `A` / `S` / `D`）按下時進入。該按鍵同時讓玩家移動一格。能量流失與行人移動啟用。 |

**沒有** Start 按鈕。開啟 Game 畫面後，直到第一個移動按鍵之前都維持 `READY`。

#### Grid (fixed structure)

1. 建立由 **DOM 格子** 組成的矩形網格。
2. 提供固定網格於 [`assets/module-e/layout.js`](./assets/module-e/layout.js)（`BASE_LAYOUT`）。
3. 各格位類型須有視覺區分（顏色、邊框、標籤或材質）。

#### Per-run randomization

每次建立新一趟遊戲（首次載入與每次 **Play again**）都要產生 **新** 配置：

1. **Energy packs (4):** 隨機選取 **4 個不同的 `ROAD` 格**，且不可是 `SPAWN` 或 `CABINET`。每格放一個能量包。四個中再隨機標記 **剛好 1** 個為 **boost pack**（`+40` 能量，非 `+25`）。
2. **Potholes (2):** 隨機選取 **2 個不同的 `ROAD` 格**，不可是 `SPAWN`、`CABINET`，且不可被能量包佔用。每格放一個坑洞。
3. **Pedestrian routes (2):** [`assets/module-e/layout.js`](./assets/module-e/layout.js) 提供路線目錄（`PEDESTRIAN_PATH_CATALOGUE`；每條路線是有序 `ROAD` 座標列表）。每趟遊戲隨機分配 **2 條不同** 路線給兩位行人。並隨機決定每位行人的起始索引與初始方向（正向或反向）。
4. 抽取後渲染網格，並將玩家放在 `SPAWN`，狀態為 `READY`。

#### Player movement

1. 在 `RUNNING` 中，玩家每次按鍵移動 **一格**（第一次按鍵同時觸發 `READY` → `RUNNING`）。
2. 必須支援以下移動按鍵（全部可用）：

| Key          | Move  |
| ------------ | ----- |
| `ArrowUp`    | Up    |
| `ArrowDown`  | Down  |
| `ArrowLeft`  | Left  |
| `ArrowRight` | Right |
| `W`          | Up    |
| `S`          | Down  |
| `A`          | Left  |
| `D`          | Right |

3. 嘗試移入 `OBSTACLE` 或超出網格範圍時，忽略該移動。

#### Energy

| Rule               | Value                                            |
| ------------------ | ------------------------------------------------ |
| Starting energy    | `100`                                            |
| Maximum energy     | `100`                                            |
| Drain              | `RUNNING` 時每 **1 秒**流失 `15` 點能量          |
| Normal energy pack | 進入該格時：`+25` 能量（上限 100）；移除該能量包 |
| Boost energy pack  | 進入該格時：`+40` 能量（上限 100）；移除該能量包 |
| Pothole            | 進入該格時：`−25` 能量（下限 0）；之後移除該坑洞 |
| Energy reaches `0` | 立即 `LOSE`                                      |

`READY` 狀態不會流失能量。

#### Pedestrians

1. 必須剛好 **2** 位行人，各自在隨機分配路線上。
2. 僅在 `RUNNING` 中每 **600 ms** 移動一步（`READY` 凍結）。
3. 行人到達路線任一端時反向（ping-pong）。
4. 玩家與任一行人落在 **同一格** 時 → 立即 `LOSE`。

<a id="win--lose"></a>

#### Win / lose

| Outcome | Condition                           |
| ------- | ----------------------------------- |
| `WIN`   | 玩家進入 `CABINET` 格，且能量大於 0 |
| `LOSE`  | 能量歸零，**或** 玩家與行人同格     |

**On `WIN`:** 立即導向 **Charge Card studio**。

**On `LOSE`:** 該趟遊戲需 **暫停**，並顯示數秒的 **toast 或 overlay**，說明 **失敗原因**。顯示期間，能量流失、行人移動與玩家輸入都必須凍結。訊息必須符合實際失敗原因。

toast / overlay 關閉（或倒數結束）後，導向 **Charge Card studio**。

#### HUD

在 Game 畫面中需持續顯示：

- 當前 **energy**
- 當前 **score**
- `READY` 狀態提示（`Press Arrow keys or WASD to start`）

#### Scoring

使用整數運算；最終分數下限為 `0`：

```text
score =
    (normalPacksCollected * 100)
  + (boostPackCollected * 150)
  + (remainingEnergy * 2)
  + timeBonus
  - collisionPenalty
  - potholePenalty
```

| Term                   | Definition                                                                                     |
| ---------------------- | ---------------------------------------------------------------------------------------------- |
| `normalPacksCollected` | 收集到的一般（`+25`）能量包數量                                                                |
| `boostPackCollected`   | 若有收集到 boost pack 則為 `1`，否則 `0`                                                       |
| `remainingEnergy`      | 遊戲結束時能量                                                                                 |
| `timeBonus`            | 僅 `WIN`：`max(0, 60 - elapsedWholeSeconds) * 5`。`LOSE` 時為 `0`。計時自進入 `RUNNING` 開始。 |
| `collisionPenalty`     | 若因行人碰撞結束，為 `200`；否則 `0`                                                           |
| `potholePenalty`       | 每觸發一個坑洞扣 `30`                                                                          |

當能量包或坑洞狀態改變時，更新即時分數；回合結束時定稿。

#### `localStorage`

1. 顯示名稱（字串）。
2. Charge Card 工作室中，若有已存名稱，需從 `localStorage` 預填顯示名稱輸入框。

### Charge Card studio

遊戲結束後開啟：`WIN` 時立即開啟；`LOSE` 時在失敗提示（toast/overlay）後開啟。Charge Card 是該趟遊戲的 **個人成績證明**：一張橫向圖片，整合背景照片、SwapLoop 品牌元素、玩家姓名與分數、手寫簽名。可供保存（下載）、傳送（分享）或於 kiosk 列印。

卡片需在 kiosk 內的 HTML `<canvas>` 上合成。

**Card size:** **960×540** 像素（橫向）。下載 PNG 必須為 960×540。

預覽需有 `16px` 邊角圓角。

#### What appears on the card

由下到上：

1. **Background photo** - 最底層照片背景。玩家可替換。若未上傳背景，改以藍綠漸層顯示。
2. **Brand overlay** - 使用提供的透明 PNG（`/assets/module-e/charge-cards/charge-card-overlay.png`）全尺寸覆蓋。此檔含 SwapLoop 外框 / logo 視覺；不得自行重繪。
3. **Dynamic text**（以 canvas 文字 API 繪製）：
   - **Name** - 玩家顯示名稱
     - Font: 28px, regular
     - 右對齊：文字右邊界位於 x=927, y=55
   - **Outcome** - 勝利時必須精確為 `SAFE ARRIVAL / 平安抵达`（色彩 #91FF89）；失敗時必須精確為 `RUN ENDED / 比赛结束`（色彩 #FF8989）
     - Font: 32px, semi bold
     - x: centered, y: 135
   - **Score** - 數字最終分數，需大且醒目
     - Font: 128px, bold
     - x: centered, y: 185
   - **Date** - 使用 `Asia/Shanghai (zh-CN)` 格式化目前日期時間
     - Font: 16px, regular
     - x: 50, y: 500
     - 日期範例：2026/8/7 10:46:42
4. **Signature** - 玩家在簽名框中畫出的筆跡。
   - x: 0, y: 358
   - width: 960, height: 120

文字填色用 `#FFFFFF`。簽名筆畫顏色 `#FFFFFF`，線寬 `3`。canvas 文字需載入並使用 [`assets/module-e/fonts/`](./assets/module-e/fonts/) 的 webfonts。

除非另有註明，`x`、`y` 為左上角座標。

可在 `assets/module-e/charge-cards` 資料夾找到 Charge Card 範例。

#### Studio UI (DOM controls beside or below the canvas, still inside the kiosk)

| Control           | Behaviour                                       |
| ----------------- | ----------------------------------------------- |
| Display name      | 文字輸入；Download/Share/Print 必填；儲存在本機 |
| Upload background | 檔案輸入                                        |
| Reset background  | 還原漸層背景                                    |
| Clear signature   | 清空簽名筆跡                                    |
| Download          | 下載合成 PNG，檔名 `chargerun-charge-card.png`  |
| Share             | 使用 Web Share API 分享 PNG 檔                  |
| Print             | 將 Charge Card 送到印表機                       |
| Play again        | 回到 Game，建立一趟新的隨機回合，狀態為 `READY` |

#### Display name

1. 需去除前後空白。
2. 若為空，阻擋 Download、Share、Print，並顯示可見欄位錯誤。
3. 若 `localStorage` 有值，需預填。
4. 當 Download、Share 或 Print 成功啟動時，保存 trim 後名稱。

#### Background upload (file input + drag and drop)

1. 提供可見 **file input**（或按鈕開啟）並接受 `image/jpeg`、`image/png`、`image/webp`。
2. 將檔案拖放到 **kiosk** 時，必須走與 file input **相同** 驗證與套用流程，因此拖曳上傳背景需可用。
3. 其他 MIME 類型需拒絕並顯示可見訊息。
4. **檔案大小超過 5 MB** 需拒絕並顯示可見訊息。
5. 只接受與卡片同尺寸（960×540）的背景圖。
6. 合格圖片需繪製到卡片上。

#### Signature pad

1. 以滑鼠在卡片 canvas 繪製（目前不要求觸控支援）
2. 所有筆跡需裁切在簽名框內，框外不得留痕。
3. **Clear signature** 需清除全部筆跡。
4. 若為空，阻擋 Download、Share、Print，並顯示：`Please sign inside the box`。

#### Download, Share, Print

1. **Download:**（PNG）→ 檔名 `chargerun-charge-card.png`。
2. **Share:** 使用 Web Share API，傳入 PNG `File`。Title/text：`SwapLoop ChargeRun`。
3. **Print:** 開啟瀏覽器列印對話框列印 Charge Card。只列印卡片影像，不列印整個 kiosk。
4. 不可上傳圖片到自建伺服器。

遊戲視覺（玩家、行人、能量包、坑洞、櫃體、障礙）需由選手以 **創意 DOM/CSS** 製作，參見 [Design and wireframes](#design-and-wireframes)。每個物件都需具視覺辨識度；純色方塊加文字標籤不符合要求。

## Assessment

使用最新版 Google Chrome，以人工測試與專家審查評分。可觀察行為比框架選型更重要。

## Mark distribution

| WSOS SECTION | Description                            | Points |
| ------------ | -------------------------------------- | ------ |
| 1            | Work organization and self-management  | 1.75   |
| 2            | Communication and interpersonal skills | 1.1    |
| 3            | Design Implementation                  | 4.05   |
| 4            | Front-End Development                  | 9.1    |
| **Total**    |                                        | **16** |

本模組為純前端，因此不使用 Section 5（Back-End Development）。由於 ChargeRun 規則與 Charge Card 工作室都屬前端行為，Section 4 配分較一般更高。詳細面向見 [`marking/marking-scheme.json`](./marking/marking-scheme.json)。
