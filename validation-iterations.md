# 2026-11-23｜真實測試與修正紀錄（不可改寫為預先成功）

這份紀錄說明已實際執行的檢查、發現的問題，以及對 Graph 的調整。所有截圖由本地 Headless Chromium 渲染，**不是 iPad Safari 實機截圖**。公開網站部署亦尚未發生。

## A. 研究來源與 Wiki Ingest
| 項目 | 起始狀態 | 實際動作 | 結果 |
|---|---|---|---|
| 第 1 批 | S01/S02，2 篇、C01–C13 | 核對 NIH 文章、使用者 PDF 2022；`check_pdf_anchors.py` | 原有 8/8 論文定位測試通過；初版 `lint_wiki` 116/116 |
| 最終門檻（第 1 批） | 2 < 5 且未有 HTML | 實際跑 `--scope final` | 原本 **2 項 FAIL**，已保留 `lint-run-final-gate.txt` |
| 第 2 批 | 新增 S03–S06 四份資料 | 研究公開摘要／期刊文字，存 `raw/Sxx_source_extract.md`、Wiki 論文筆記及 C14–C27 | **6 份來源、27 張卡**；結構 lint 361 PASS, 0 FAIL |
| 證據風險前移 | S02 2022 / S03 2018 關係易混淆 | 在建立證據卡時確認兩篇屬前後研究，加入 C17 與比較矩陣 | 不將兩篇當作完全獨立研究複製；C13 限制不可單獨歸因纖維 |
| 來源可讀性 | S04 的開放全文未取得 | 只採 PubMed 摘要的群組、結果；在 raw、note、sources.json 標示 | 有界限地納入，不虛構未查明細節 |

## B. HTML 渲染與互動：實際問題與修正

### 測試階段 1：第一次 Chromium 嘗試
- 對 `file:///.../index.html` 執行 `page.goto` 時，執行環境回報 `net::ERR_BLOCKED_BY_ADMINISTRATOR`。
- 此問題來自測試環境對 `file://` 的限制，**不能當作 HTML 網站功能故障**。
- **修正**：改以 `page.set_content()` 載入同一份原始 HTML；測試內嵌 JavaScript、DOM、圖表與互動，不依賴網路。

### 測試階段 2：畫面溢出誤報
- 初版測試取 `section.scrollWidth`，所有頁面被判為水平溢出；實際截圖顯示是被父容器裁切的**裝飾性 CSS `:before` 背景圓形**，不是文字溢出。
- **修正**：改以 `document.documentElement.scrollWidth/clientWidth` 檢測實際視窗橫向溢出；保留真實截圖人工觀察。
- **原因**：測試指標必須測真實使用者可見的問題，否則誤報會使 Loop 無效反覆修改。

### 測試階段 3：平板顯示及封面
- 初版封面標題在 1440px 截圖出現孤字換行（「現。」落到第三行）。
- iPad 尺寸（1024×768）第 8 頁表格較長，絕對定位的證據頁尾與內容距離過近，局部視覺上接近重疊。
- **修正**：封面手動拆成自然兩行；在 ≤1100px 寬度將頁尾改成正常內容流（`position: static`），避免壓住長表格／說明。
- 重新截圖、重新測試：`records/visual-desktop-cover.png`、`visual-ipad-slide08.png`、`visual-mobile-cover.png`。

### 測試階段 4：可操作驗收
- `scripts/test_browser.py` 在 **1440×900、1024×768、390×844** 三種 viewport 分別通過：13 頁數量、上一頁／下一頁、目錄跳頁、第 8 頁原料／總蛋白切換、證據抽屜 27 條、三題測驗正確／錯誤回饋／重設、無 JS pageerror、無真實橫向溢出。
- `scripts/test_wiki_view.py` 通過 Wiki 6 份來源、27 張證據卡、文字搜尋、S04 篩選與重設。
- `node --check` 通過主頁內嵌 JS；`check_pdf_anchors.py` 8/8。
- `scripts/lint_wiki.py --scope final` **361 PASS／0 FAIL／2 WARN**（警告是科學獨立審核及 Safari 實機尚未做）。

## C. 對 Graph 的具體修改
**最常出現可見問題的階段：HTML 渲染／版面測試。** 本次已觀察到封面斷行與 iPad 表格頁尾問題，因此把裝置截圖檢查從「部署前最後檢查」前移到「HTML 草稿每次渲染後」。

在來源階段，另將**來源系譜、可讀程度、共同變動之混雜因素**移到「建立證據卡之前」查核。若來源不足或讀不到全文，就限制主張範圍或停下；不能為了達到引用數量而拼湊。

實際 Graph 因此採用局部退回：
- **科學來源問題**：Sxx → Cxx 證據卡 → Wiki，修正來源及限度。
- **逐頁稿問題**：只改問題頁與引文，無需重做 6 份 Wiki。
- **CSS/JS 問題**：只返回 HTML 渲染→再跑 3 viewport 測試，無需重跑文獻 Ingest。

## D. 仍未通過／尚未執行
- `S04` 僅直接查閱 PubMed 摘要，尚未完成原始全文逐項檢查。
- 沒有第二位獨立研究審查者完整審核所有科學卡。
- **沒有完成 GitHub Pages 公開部署**，不能提供已確認的公開網址。
- **沒有完成使用者本人在 iPad Safari 真實裝置的操作檢查**；本地 Chromium 1024px 只是模擬 viewport。
