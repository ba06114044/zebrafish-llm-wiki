# ZEBRA LAB｜第二批 Ingest + 13 頁 HTML 教學簡報 完成報告

**作業：** 用 LLM Wiki 做一份教學 HTML 簡報  
**專案主題：** 小小斑馬魚，大大的科學發現——蛋白質、纖維素與腸道菌相  
**日期：** 2026-11-23（以當前本機完成日期標示）

## 1. 交付成果
- ✅ 官方介紹＋學術論文：共 **6 份 S01–S06**（其中學術研究 5 篇）。
- ✅ 逐項來源卡：**27 張 C01–C27**；每張有來源、原文位置、重要限制。
- ✅ AI 可讀 Markdown Wiki：`wiki/index.md`、`wiki/papers/`、`wiki/concepts/`、`wiki/synthesis/`。
- ✅ 可公開瀏覽的 Wiki 閱覽頁：`wiki.html`（搜尋與 Sxx 篩選）。
- ✅ HTML 簡報：`index.html`（13 頁、3 題互動測驗、第 8 頁雙模式配方資料表、來源抽屜、鍵盤／觸控導覽）。
- ✅ Agent 規範／流程圖：`AGENTS.md`、`WORKFLOW.md`、`SCHEMA.md`。
- ✅ 執行驗證及可追溯修改：`records/`。

## 2. 來源一覽
1. **S01** NIH News in Health (2023). *Fishing for Clues to Human Health.* https://newsinhealth.nih.gov/2023/04/fishing-clues-human-health
2. **S02** Leigh SC, Catabay C, German DP (2022). *Sustained changes in digestive physiology and microbiome across sequential generations of zebrafish fed different diets.* https://doi.org/10.1016/j.cbpa.2022.111285
3. **S03** Leigh SC, Nguyen-Phuc B-Q, German DP (2018). *The effects of protein and fiber content on gut structure and function in zebrafish (Danio rerio).* https://doi.org/10.1007/s00360-017-1122-5
4. **S04** Patula S et al. (2021). *Nutritional Programming with Dietary Soybean Meal and Its Effect on Gut Microbiota in Zebrafish (Danio rerio).* https://doi.org/10.1089/zeb.2020.1952
5. **S05** Childers et al. (2025). *Protein absorption in the zebrafish gut is regulated by interactions between lysosome rich enterocytes and the microbiome.* https://doi.org/10.7554/eLife.100611
6. **S06** Sieler MJ et al. (2023). *Disentangling the link between zebrafish diet, gut microbiome succession, and Mycobacterium chelonae infection.* https://doi.org/10.1186/s42523-023-00254-8

來源範圍說明：S01 為官網科普；S02 為使用者提供的完整 10 頁 PDF；S03 研究者公開 PDF 可檢索文字及 PubMed 摘要；S04 目前只核對公開 PubMed 摘要；S05 eLife 公開論文／摘要；S06 期刊公開 Methods、Results、摘要。**S02 與 S03 是延續性研究，不代表兩次獨立複製。**

## 3. 驗證結果
| 檢查 | 真實結果 | 位置 |
|---|---|---|
| Wiki 結構、來源、引文、13 頁 DOM | **361 PASS／0 FAIL／2 WARN** | `records/lint-run-02-final.txt` |
| 核心 S02 PDF 8 個證據錨點 | **8 PASS／0 FAIL** | `records/pdf-anchor-recheck-final.txt` |
| 互動 HTML JS 語法 | **PASS** | `records/js-syntax.txt` |
| 桌面 Chromium 1440×900 | **PASS** | `records/browser-smoke.txt` |
| iPad 尺寸 Chromium 1024×768 | **PASS** | 同上；`records/visual-ipad-*.png` |
| 手機尺寸 Chromium 390×844 | **PASS** | 同上；`records/visual-mobile-*.png` |
| 可搜尋 Wiki 網頁 | **PASS** | `records/wiki-viewer-smoke.txt` |
| GitHub Pages 真實公開部署 | **未執行** | 由使用者上傳後補 |
| iPad 真正 Safari 實機驗收 | **未執行** | 由使用者上傳後補 |

> 結構／程式測試與科學忠實度不完全相同。上列 361 次成功指檔案格式及可追溯性等機械判斷；不是 361 次獨立研究結論審查。

## 4. 反思（真實案例）
**最常見可見問題的節點：HTML 渲染／響應式檢查。** 第一版桌面封面文字出現孤字換行，iPad 第 8 頁較長數字表導致頁尾與文字距離過近。使用桌面與 iPad 截圖發現後，把「渲染後立即檢查不同 viewport」列為 HTML 節點嘅固定檢查，不再等最終部署。來源方面，核查 2018/2022 研究關係時發現兩篇並非完全獨立複製，立即在建立證據卡時加上研究系譜限制，防止將證據強度放大。相關執行過程見 `records/validation-iterations.md`。

## 5. 下個必要動作
在 GitHub 新建 `zebrafish-llm-wiki` Public repository，將檔案結構不變上傳、Settings → Pages → `main` + `/(root)`；到公開網址測試 `index.html` 及 `wiki.html`，在 COOL 繳交 Wiki link、HTML link、Loop／Graph、實際驗證與反思。**公開網址不可在部署前冒稱存在。**
