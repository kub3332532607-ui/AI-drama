# AI 劇評室 開發筆記（給 Claude 讀的）

單檔離線 PWA。主程式只有 `index.html`（約 190 KB，內含 CSS、JS、i18n 字典），另有 `sw.js`、`manifest.webmanifest`、三份 README。
網站：https://kub3332532607-ui.github.io/AI-drama/　Repo：kub3332532607-ui/AI-drama
目前版本：v6.1.0。**原始碼模組已遺失，只能直接修改編譯後的 index.html**（用 Grep 定位後局部修改，不要整份讀）。

## 每次改版必做
1. `APP_VERSION` 加一。
2. `sw.js` 的 `CACHE` 名稱加一（現為 shell-v8）。
3. 三份 README 的更新紀錄各加一行（繁中、簡中、英文）。
4. 改完用 `node --check` 檢查 `<script>` 語法，並用 Playwright 跑一次載入與主要流程。

## 資料與結構
- IndexedDB：資料庫 `dramaRoomPWA`，store `kv`。鍵：reviews、groups、series、libs、trash、settings、draft、draftEdit、snapshot、schema（現為 6）、lastBackup、demoRefresh、`img:<id>`。
- 劇評欄位：id、libId、no、title、left/right、groupId、nth、seriesId、version、plotScore/productionScore/modelScore、plot/production/model/quotes、tags[]、images[]、cover、status（done／ongoing／dropped）、date、createdAt、updatedAt、links[{name,url}]。
- 收藏庫 `libs`：{id,name,hashtag,fields:{pair,group,series},nextNo,demo?,showLinks}。示範資料放在獨立的 demo 收藏庫，不進統計、備份與「全部」模式。
- 編號是永久的，刪除後不補號（已決定維持，見下）。
- 新增欄位時要同步：form()、fillForm、formEmpty、儲存驗證、搜尋、詳情頁、文字／圖片輸出、CSV、遷移（migration）、sanitizeReview、備份匯入、示範資料。

## 程式慣例
- 翻譯函式是 `_t('繁中字串',[參數])`（不是 `t`）。`DICT={en:{},'zh-Hans':{}}` 以繁中字串為鍵。新字串三種語言都要補，靜態 HTML 靠 `i18nStatic()`。
- 單行輸入用 contenteditable `.ce`（避免瀏覽器自動填入），選擇器用 bottom sheet（`makePicker`）。
- 浮層用 `openOv/closeTop/closeAllNow` 管理返回鍵。篩選共用 `matchF`（LF／SF）。圖片輸出是 canvas 的 `makeCard`。

## 使用者偏好（不要違反）
- 用詞中性：不用「攻／受」，程式碼不放個人用語。
- 完整句子結尾用「。」；標籤、按鈕、狀態詞不用。
- 圖片保持原比例。「我的口味」名稱與「今天又嗑到什麼了？」保留。
- 簡短回覆，先討論再動作。不要為了小事讀整個對話或整份程式。
- **已被拒絕，不要再提**：底部分頁列、啟動時備份提示、社群分享預覽圖、儲存後停留在寫劇評頁、自訂評分項目名稱、0.0–5.0 微調改成格子、「本命與地雷」清單、口味頁時間範圍切換、第幾搭數字輸入改成純按鈕。
- 介面不要變冗長：新功能預設收起，能不加欄位就不加。

## 下一版（v6.2）已決定的項目
1. **編號**：維持永久編號，收藏頁摘要小字加「下一部是 No.{nextNo}」。
2. **口味頁**：最上方加一行摘要「N 部劇評｜M 部完整評分（圖表依此統計）｜K 部未看完」。各圖小標註明統計基準（完整評分 M 部或全部 N 部）。
3. **每月追劇**：改成三層堆疊，只依觀看狀態分深、中、淺三色，標出未看完比例。評分完整度只放在摘要那一行，不另畫圖。
4. **回看（輕量版）**：寫劇評表單不動。詳情頁加「今天回看了」按鈕記錄日期。改分數時自動把舊分數存成歷史，詳情頁顯示「日期 舊分 → 新分」。口味頁一律用最新分數。
5. **標籤快速選單**：所有大標籤都放進，依底下標籤的總使用次數排序。沒歸類的小標籤收成「未分類（n）」按鈕。最前面放最近用過的 3 個標籤。
6. **又名**：劇名下方加收起來的「＋ 又名」，可填多個。搜尋會比對又名，詳情頁小字顯示，輸出可選要不要顯示，重複劇名檢查也要比對又名。
7. **輸出圖片重新設計**：目前的版本太像名片，要換掉。拿掉中間的分數長條（各段心得旁已有分數）。先做 B、C 兩個方案的樣圖給使用者選：
   - B 社群貼文風：有圖就用第一張滿版當封面、標題壓在圖上、下接心得。沒有圖用純色封面。
   - C 雜誌評分風：左側細色線，大大的總分數字當重點，其餘素淨。
