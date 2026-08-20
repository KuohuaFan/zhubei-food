# 竹北食堂 · Zhubei Food Hall

> 九降風吹過的餐桌 — 一個以竹北在地飲食文化為主題的單頁式美食網站，
> 內建沈浸式（無氣泡）AI 對話介面「食通」。
> 收錄 2026 米其林一星「鮨安」等經查證之在地餐廳資訊。

純靜態、零建置（zero-build）、單一 HTML 檔案，可直接部署於 **GitHub Pages**。

---

## ✨ 功能特色

| 功能 | 說明 |
|---|---|
| 餐廳卡片 | 每間店含：料理類型、**獎項徽章**（★米其林一星／必比登推介／紅蝦評鑑／編輯・在地推薦，四級配色）、介紹、**推薦理由**、區域地址、**價位帶**（NT$／人約略區間）、可點擊的地圖連結 |
| 分類篩選 | 全部、米其林・必比登、中式、日本料理、義大利、西班牙・地中海、西式・餐酒、印度、蔬食 |
| 左側欄導覽 | 新對話、**記錄**（點擊載入歷史對話）、**已加星號**（卡片點 ★ 收藏並計數）、**專案**（三個主題專案，點擊直達詳情） |
| 專案（兩層結構） | 列表 → 詳情。三個專案：**米其林巡禮 2026**、**異國料理地圖**（依家庭聚餐／約會／多人分食場合排列）、**在地經典麵食**。每筆條目含介紹、價位、地圖連結；底部附**延伸閱讀**（米其林官方名單與食評文章）與「和食通聊這個主題」按鈕（一鍵開啟 AI 對話並帶入主題提問） |
| 沈浸式對話 | 右下角 chat icon 開啟；**無氣泡**文件流式排版，你的提問與 AI 回覆以標記與漸層細線分隔，如閱讀一篇美食筆記 |
| 三段式 AI 連線 | ① 先嘗試免金鑰直連（Claude.ai 預覽環境可用）→ ② 失敗改用使用者填入的 API Key（GitHub Pages 等外部部署）→ ③ 皆無則退回**內建示範回覆**（含米其林等查證資訊，離線可展示） |
| 視覺識別 | 客家藍衫靛 × 新埔柿餅橘；LXGW 文楷體標題；「九降風」動態風線簽名元素 |
| 無障礙與響應式 | 手機抽屜側欄、徽章自動換行、鍵盤焦點樣式、尊重 `prefers-reduced-motion` |

## 📁 專案結構

```
zhubei-food-site/
├── index.html    # 全站（HTML + CSS + JS 單檔）
├── README.md     # 本文件
├── LICENSE       # Apache License 2.0
├── NOTICE        # 著作權與第三方資源聲明
└── .nojekyll     # 停用 GitHub Pages 的 Jekyll 處理
```

## 🚀 部署到 GitHub Pages

### 方法一：網頁介面（最簡單）

1. 在 GitHub 建立新 repository（例如 `zhubei-food`）。
2. 上傳本專案全部檔案至 repository 根目錄。
3. 進入 **Settings → Pages**：
   - **Source** 選擇 `Deploy from a branch`
   - **Branch** 選擇 `main`，資料夾選 `/ (root)`，按 **Save**
4. 約 1–2 分鐘後，網站將發佈於：
   `https://<你的帳號>.github.io/zhubei-food/`

### 方法二：命令列

```bash
git init
git add .
git commit -m "feat: 竹北食堂初版"
git branch -M main
git remote add origin https://github.com/<你的帳號>/zhubei-food.git
git push -u origin main
# 再到 Settings → Pages 依方法一第 3 步啟用
```

### 更新已部署的網站

```bash
git add .
git commit -m "update: 內容更新"
git push
# GitHub Pages 會在數分鐘內自動重新部署
```

## 🤖 AI 對話設定

對話功能採**三段式自動偵測**，不需設定即可運作：

| 環境 | 行為 |
|---|---|
| Claude.ai 預覽／Artifact | 自動免金鑰連線 Claude，標題列顯示「AI 已連線」 |
| GitHub Pages（已填 API Key） | 以你的金鑰連線 Claude（模型：`claude-sonnet-4-6`） |
| GitHub Pages（未填金鑰）／離線 | 退回內建示範回覆（含米其林等查證資訊） |

要在 GitHub Pages 啟用即時 AI 回覆：

1. 至 [Anthropic Console](https://console.anthropic.com/) 取得 API Key。
2. 開啟對話視窗，點右上角 **API Key** 按鈕，貼上金鑰後按「套用」。

### ⚠️ API Key 安全須知（請務必閱讀）

- 金鑰**僅存於當前分頁的記憶體**，不寫入 localStorage、不上傳伺服器，重新整理即清除。
- 前端直連 Anthropic API 使用了 `anthropic-dangerous-direct-browser-access` 標頭。
  此作法**僅適合個人展示或內部 demo**。
- **正式對外服務請勿讓使用者持有你的金鑰**。建議改架設輕量代理（如 Cloudflare Workers、Vercel Edge Functions）：金鑰存於伺服器端環境變數，前端只呼叫你的代理端點，並可加上速率限制。
- 建議在 Anthropic Console 為此用途建立**獨立金鑰**並設定用量上限，便於隨時撤銷。

## 🛠️ 客製化指南

所有內容資料集中於 `index.html` 的 `<script>` 區段：

| 想改什麼 | 位置 |
|---|---|
| 餐廳資料（含徽章、價位、連結） | `const SHOPS = [...]`，每筆的 `links` 欄位可補上官網或訂位頁連結 |
| 專案內容（條目、延伸閱讀、AI 提問） | `const PROJECTS = [...]`，`chatQ` 為「和食通聊這個主題」按下後送出的問題 |
| 歷史對話示範 | `const HISTORY = [...]` |
| AI 人設與查證事實 | `const SYSTEM_PROMPT = ...`（更新店家資料時請同步更新此處） |
| 示範模式回覆 | `function localAnswer(q)` 的規則表 |
| 色彩系統 | `:root` CSS 變數（`--indigo`、`--persimmon` 等） |
| 分類 | `#chips` 區塊與 `SHOPS` 各筆的 `cats` 陣列 |
| 字型 | `<head>` 中的 Google Fonts 連結 |

## ⚠️ 資料來源與免責聲明

店家資訊整理自以下公開來源（2026 年 8 月查證）：

- **《臺灣米其林指南 2026》**（2026 年 7 月 21 日發布）：新竹縣首度摘星，竹北「鮨安」獲米其林一星與年輕主廚大獎；「季風 Monsoon」「善菓堂」獲必比登推介。
- 各食評媒體之公開報導（價位帶、地址、營業資訊）。

注意事項：

- 「編輯推薦／在地推薦」為本站整理之推薦標記，**非官方評鑑**，網站上已明文標註。
- 價位為約略區間，營業時間、菜單、訂位方式可能異動，一律以店家公告為準。
- 店家連結採 Google Maps 搜尋連結（不冒稱官方網站）；如需官網或訂位頁，請自地圖頁面查詢，或於 `SHOPS`／`PROJECTS` 資料的 `links` 欄位補上經確認的官方連結。
- AI 回覆為生成內容，僅供參考。

## 📌 已知限制

- 星號收藏與 API Key 僅存於記憶體，重新整理後清除（不使用 localStorage，如需持久化請自行擴充）。
- 在 Claude.ai 公開分享的 Artifact 沙盒中，外部連結（地圖、延伸閱讀）可能受平台安全限制無法開新視窗；GitHub Pages 部署版不受影響。
- 未登入 Claude 的訪客瀏覽公開 Artifact 時，對話自動退回示範模式，屬正常降級行為。

## 📄 授權

本專案採用 **Apache License 2.0** 授權 — 詳見 [LICENSE](LICENSE) 檔案。

```
Copyright 2026 Zhubei Food Hall Project Contributors

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```

第三方資源：LXGW WenKai TC 與 Noto Sans TC 字型由 Google Fonts 提供，
採 SIL Open Font License 1.1 授權（詳見 [NOTICE](NOTICE)）。
