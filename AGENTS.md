# AGENTS.md — 韓國女團介紹網站專案開發規範與 Wiki 指南

本檔案為 AI Agent 在協助開發與維護「韓國女團介紹網站」時的核心規範、架構指南與任務清單。本專案採用 **Karpathy LLM Wiki 模式**，透過持久化、互聯的 Markdown 結構來管理所有女團與成員資料，並結合純前端（HTML/CSS/JS）與外部 API（如 YouTube API）部署於 GitHub Pages。

---

## 1. 專案願景與功能範疇

* **專案名稱**：K-Pop Girl Groups Wiki
* **技術棧**：HTML5, CSS3 (Tailwind CSS 或簡潔 CSS 模組), Vanilla JavaScript, GitHub Pages 靜態部署。
* **資料驅動**：利用本地 Markdown/JSON 作為 Wiki 知識庫，並動態串接外部 API（例如 YouTube Iframe API / Data API）實現 MV 隨選播放與影音欣賞。
* **核心互動層級**：
  1. **首頁 / 團體總覽 (`index.html`)**：展示所有收錄的女團清單與簡介卡片。
  2. **團體主頁 (`group.html?id=...`)**：該團體的介紹、歷代專輯、得獎紀錄。
  3. **成員個人頁 (`member.html?id=...`)**：成員個人檔案、官方定位/擔當、代表歌曲、MV 欣賞與個人歷程。

---

## 2. 專案目錄結構 (LLM Wiki Pattern)

```text
/
├── AGENTS.md                # 本專案的規範與指南
├── index.html               # 網站首頁
├── group.html               # 團體動態頁面
├── member.html              # 成員動態頁面
├── assets/
│   ├── css/                 # 樣式表
│   ├── js/                  # 核心邏輯與 API 串接腳本
│   └── images/              # 本地圖片與圖示
├── wiki/                    # 核心知識庫 (LLM 維護的 Markdown 檔案)
│   ├── index.md             # Wiki 內容總目錄與導航
│   ├── log.md               # 變更記錄與維護日誌 (Append-only)
│   ├── groups/              # 各團體詳細 Wiki 頁面
│   │   ├── illit.md
│   │   ├── le_sserafim.md
│   │   ├── ive.md
│   │   ├── twice.md
│   │   ├── aespa.md
│   │   ├── g_idle.md
│   │   ├── blackpink.md
│   │   ├── nmixx.md
│   │   ├── fromis_9.md
│   │   ├── babymonster.md
│   │   └── hearts2hearts.md
└── data/                    # 給前端靜態載入的 JSON 結構化資料
    └── groups.json          # 包含所有團體與成員的結構化對應表
```

---

## 3. 首波收錄名單與初始維護清單

目前首波納入以下 11 組韓國女子音樂團體（未來可透過 Ingest 持續新增）：
1. ILLIT
2. LE SSERAFIM
3. IVE
4. TWICE
5. aespa
6. (G)I-DLE
7. BLACKPINK
8. NMIXX
9. fromis_9
10. BABYMONSTER
11. Hearts2Hearts

---

## 4. AI Agent 核心運作流程 (Operations)

### A. Ingest (新增或更新資料)
* 當使用者提供新團體、新成員、專輯或得獎紀錄時：
  1. 更新 `wiki/index.md` 與對應的 `wiki/groups/{group_name}.md`。
  2. 同步更新 `data/groups.json`，確保前端網頁能夠正確渲染。
  3. 在 `wiki/log.md` 追蹤並記錄更新日誌（格式：`## [YYYY-MM-DD] ingest | 說明`）。

### B. Query & Build (開發與生成網頁功能)
* 當使用者要求實作特定功能（例如：串接 YouTube API 播放 MV、優化響應式排版、建立成員分頁切換）：
  1. 優先檢查現有的 HTML/JS 架構。
  2. 編寫乾淨、模組化的原生 JavaScript 程式碼，確保無須複雜的建置工具（Bundler）即可直接在 GitHub Pages 運行。

### C. Lint (健康檢查)
* 定期檢查 Wiki 檔案與 `groups.json` 是否有資料遺漏、連結失效、成員定位描述不符或 YouTube 嵌入連結錯誤。

---

## 5. 開發約定 (Conventions)
* **語系**：介面與說明文件以**繁體中文**為主，韓團專有名詞（如官方定位、專輯名稱）保留英文或標準譯名。
* **靜態優先**：所有功能必須能在 GitHub Pages 上以純靜態網頁完美運行，API Key（如 YouTube API Key）建議留空讓使用者自行填入或使用無 Key 的嵌入式播放方案。