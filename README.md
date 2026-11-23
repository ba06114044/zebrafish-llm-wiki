# ZEBRA LAB｜斑馬魚 × 蛋白質 × 纖維素 LLM Wiki

**已完成可檢查的本地專案（2026-11-23）**：來源 6 份、逐項證據卡 27 張、13 頁互動教學 HTML 簡報、公開 Wiki 瀏覽器、Loop／Graph 設計、批次 Ingest 及真實測試紀錄。**注意：截至交付仍未公開 GitHub Pages，也未由使用者在 iPad Safari 實機驗收。**

## 兩個入口
- `index.html`：**13 頁教學 HTML 簡報**，有左右箭頭與手機觸控切頁、配方數據切換、每頁 Cxx 來源查核抽屜、3 題互動測驗。
- `wiki.html`：**可搜尋 LLM Wiki**，有 S01–S06 六份來源、C01–C27 二十七張證據卡、搜尋及來源篩選。
- `wiki/index.md`：給 AI 使用的 Markdown Wiki 入口。每項原始證據卡可依 ID 找到 `claims.json`。

## 檔案角色
```text
index.html                    GitHub Pages 首頁／13 頁互動教學簡報
wiki.html                     可搜尋的 Wiki 閱覽介面
slides/index.html             同一簡報副本（可獨立開啟）
slides/slide-map.json         13 頁 -> Cxx 對照表
AGENTS.md                    AI ingest、引用、製作及審查規則
SCHEMA.md                    sources / claims 資料契約
WORKFLOW.md                  Loop／Graph、退回路線和反思
sources.json                 S01-S06 的學術來源（含原址、可查閱層級）
claims.json                  C01-C27 逐項研究證據、定位及 caveat
raw/                         使用者 PDF、URL 與公開資料的摘要抽取快照
wiki/                        每份研究的筆記、跨研究概念與逐頁稿
records/                     真實 lint、瀏覽器 smoke test、查核及畫面截圖
scripts/                     不需額外服務的資料檢查；瀏覽器測試需要 Playwright
SUBMISSION.md                COOL 繳交說明與待填公開網址
```

## 教學方法／科學界線
- 前半部介紹斑馬魚與腸道菌群；後半部聚焦 Leigh et al. 2022 的三種**共同變動蛋白質／纖維素的配方**，並對照 2018、2021、2023、2025 年研究。
- S03 2018 與 S02 2022 是延續性的研究計畫，**不能當成兩次完全獨立實驗複製**。
- S04 2021 只直接核對到 PubMed 摘要，標示證據範圍；不得假稱全文審查。
- 「菌相有差異」不能直接解讀為「更健康」；動物研究不等於人體營養建議。
- 原始研究的完整非自有文章**沒有**在 `raw/` 重製；代之以來源連結與已核對的整理快照。S02 則保留使用者提供 PDF。

## 本機開啟／測試
直接用瀏覽器開啟 `index.html` 與 `wiki.html`，或者在專案資料夾執行：
```bash
python -m http.server 8000
```
進入 `http://localhost:8000/`。JS/CSS 已內嵌，離線也能翻頁、搜尋及互動；只有點開外部研究來源需要連線。

檢查：
```bash
python scripts/check_pdf_anchors.py
python scripts/lint_wiki.py --scope final
python scripts/test_browser.py       # 需 Playwright + Chromium；測試環境用 set_content 載入
```
註：`scripts/lint_wiki.py` 依賴 `beautifulsoup4`，`test_browser.py` 依賴 `playwright` 及可執行的 Chromium；專案在 GitHub Pages 發布**不需要**這些套件。

## GitHub Pages 部署（與先前 PAW ATLAS 分開）
1. 到 GitHub 建立一個**新的 Public Repository**，建議名稱：`zebrafish-llm-wiki`。不要修改已存在的 `paw-atlas`。
2. 將這個專案資料夾**內部的檔案與資料夾**上傳到新 repository 根目錄；根目錄要直接看見 `index.html`、`wiki.html`、`AGENTS.md`。
3. Repository → Settings → Pages → Build and deployment → Deploy from a branch → `main` + `/(root)` → Save。
4. 等網站建置完成後，從 Pages 的 **Visit site** 確認網址。預計形式 `https://<GitHub帳號>.github.io/zebrafish-llm-wiki/`；**這只是預計 URL，未經正式部署證實。**
5. 實際用 iPad Safari 開啟網站，檢查第 8 頁數字切換、第 12 頁互動題、證據抽屜、`wiki.html` 搜尋／來源。將成功畫面截圖。

## 作業繳交
請看 `SUBMISSION.md`。繳交前把 Wiki/HTML 連結換成**實際公開網址**，並附 `records/` 內的驗證與修改紀錄。
