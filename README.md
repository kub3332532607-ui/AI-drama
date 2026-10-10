# AI 劇評室（PWA）

## 內容
- `index.html`：劇評室主程式
- `manifest.webmanifest`：PWA 安裝資訊
- `sw.js`：離線快取
- `icon.svg`：App 圖示

## 免費部署（GitHub Pages）
1. 登入 GitHub，建立一個新的 repository（例如 `ai-drama-review-room`）。
2. 將這個資料夾內的檔案全部上傳到 repository 根目錄：`index.html`、`manifest.webmanifest`、`sw.js`、`icon.svg`、`icon-192.png`、`icon-512.png`。README 可以一起上傳。
3. 進入 repository 的 **Settings → Pages**，在 Build and deployment 選擇 **Deploy from a branch**，分支選 `main`、資料夾選 `/ (root)`，按 Save。
4. 等待 GitHub Pages 顯示網站網址，開啟網址測試。
5. 手機使用該網址：Android Chrome 通常可從瀏覽器選單選「安裝應用程式」或「加到主畫面」；iPhone Safari 可用分享選單中的「加入主畫面」。實際選項會因瀏覽器版本而異。

## 重要資料安全提醒
- 劇評和圖片使用 IndexedDB 保存在目前裝置／瀏覽器，不會上傳至 GitHub，也不會自動跨裝置同步。
- 請先從舊版劇評室匯出 JSON 備份，再在新版的「我的口味 → 資料備份」匯入。舊版若是以本機檔案 `file://` 開啟，瀏覽器資料不一定能直接搬到新網址。
- 建議每 14 天下載一次完整 JSON 備份（含圖片），並把備份存到不會跟著手機一起損壞的位置。備份提醒會在你開啟 App 時顯示；不會在 App 關閉時傳送系統推播通知。
- 若清除網站資料、移除瀏覽器資料、換裝置或裝置故障，未備份的資料仍可能遺失。
- 本機儲存配額依瀏覽器與裝置而異。圖片會縮放至長邊約 2000px，並嘗試以 WebP（不支援時改用 JPEG）壓縮。
- 建議先在手機與電腦各自測試，再匯入正式資料。

## 更新紀錄
### v3.0.0
- 角色改為「左位 × 右位」兩格；新增「演員組合」與「第幾搭」（數字，可到五搭、六搭以上）。
- 標籤：輸入時顯示既有標籤、相近提醒；收藏頁可一鍵篩選（只比對標籤）；可管理／合併標籤。
- 圖片改為獨立儲存並自動產生縮圖；備份格式升級為 version 3（仍可還原舊版備份）。
- 刪除可復原（最近刪除保留 30 天）；還原備份可選合併或取代，並可復原上次還原。
- 支援手機返回鍵；輸出主題標籤可自訂；口味頁新增分布、月份與標籤統計。
- 更新方式：覆蓋上傳 `index.html`、`sw.js`、`manifest.webmanifest`、`README.md`，重新開啟 App 兩次即可。
