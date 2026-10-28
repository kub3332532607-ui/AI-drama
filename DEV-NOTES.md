# AI 劇評室 開發筆記（給 Claude 讀的）

單檔離線 PWA。主程式只有 `index.html`（約 190 KB，內含 CSS、JS、i18n 字典），另有 `sw.js`、`manifest.webmanifest`、三份 README。
網站：https://kub3332532607-ui.github.io/AI-drama/　Repo：kub3332532607-ui/AI-drama
目前版本：v6.6.0。**原始碼模組已遺失，只能直接修改編譯後的 index.html**（用 Grep 定位後局部修改，不要整份讀）。

## 每次改版必做
1. `APP_VERSION` 加一。
2. `sw.js` 的 `CACHE` 名稱加一（現為 shell-v13）。
3. 三份 README 的更新紀錄各加一行（繁中、簡中、英文）。
4. 改完用 `node --check` 檢查 `<script>` 語法，並用 Playwright 跑一次載入與主要流程。

## 資料與結構
- IndexedDB：資料庫 `dramaRoomPWA`，store `kv`。鍵：reviews、groups、series、libs、trash、settings、draft、draftEdit、snapshot、schema（現為 7）、lastBackup、demoRefresh、`img:<id>`。
- 劇評欄位：id、libId、no、title、left/right、groupId、nth、seriesId、version、plotScore/productionScore/modelScore、plot/production/model/quotes、tags[]、images[]、cover、status（done／ongoing／dropped／wish）、date、createdAt、updatedAt、links[{name,url}]、aliases[]（又名）、rewatch[]（回看日期）、hist[{d,o:[劇情,製作,建模],n:[…]}]（分數修改歷史，同日多次修改合併為一筆）。
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

## v6.2 已完成（7 項全做）
1. 編號維持永久；收藏頁摘要小字顯示「下一部是 No.n」（示範收藏庫不顯示）。
2. 口味頁最上方摘要行；各圖小標註明統計基準（完整評分 M 部／有該項分數 n 部／全部 N 部）。
3. 每月追劇：依觀看狀態三層堆疊（已完結深、未完結中、沒看完淺），標出未看完比例；平均只算完整評分。
4. 回看（輕量版）：詳情頁「今天回看了」，同日不重複；改分數自動寫入 hist；口味頁一律用最新分數。
5. 標籤快速選單（只用在寫劇評頁的 fStrip，由 `stripQuick` 產生）：最近 3 個＋有小標籤的大標籤（依底下標籤總使用次數）＋「未分類（n）」。收藏頁與口味頁的篩選列仍用舊的 `stripHTML`。
6. 又名：寫劇評劇名下方收起的「＋ 又名」（最多 10 個）；搜尋、列表命中顯示、詳情小字、輸出（預設不顯示，輸出頁勾選才加）、重複劇名檢查都會比對又名。
7. 輸出圖片改為 C 雜誌評分風（大字總分、左側色線、拿掉分數長條）。B 社群貼文風樣圖未採用。

## v6.3 已完成
1. 「今天回看了」可再按取消；回看日期可逐筆移除（×），不動 hist。
2. 新增狀態「想看」(wish)：不進口味頁統計（統計基準排除 wish、口味頁狀態篩選不列），詳情頁不顯示回看區塊。示範資料未加想看範例。

## v6.4 資料保護（因 2026-10-28 手機 IndexedDB 被重設而加）
- 事故：Android Chrome 上 `dramaRoomPWA` 變成全新空資料庫，localStorage 與同網域的國考刷題本都正常；原因未確定（疑為 Chrome 重設 IndexedDB）。
- 同網域（kub3332532607-ui.github.io）所有 repo 共用同一份 IndexedDB／localStorage／Cache Storage；exam-drill 的「更新」會刪全網域快取並取消全部 service worker。**不要把保護用的副本放在 Cache Storage。**
- localStorage 鍵（與資料庫分開，專門用來偵測與查原因）：`drama_guard`（上次的劇評筆數）、`drama_log`（黑盒子紀錄，最多 150 筆）、`drama_mirror`（本機副本狀態）、`drama_gist`（權杖、gist id、檔案雜湊，不進備份檔）。
- 啟動時若 guard.n>0 但資料庫非示範劇評＋垃圾桶為 0：設 `window.__lock=true`，kvSetMany／kvDel 變成空操作、persist／save 擋下並顯示提示列，不跑 `cleanupImages`，並跳出還原選單（重新載入／本機副本／雲端／備份檔／從空白開始）。解除用 `unlockWrites()`，匯入完成後 `relockIfEmpty()`。
- 本機副本：OPFS `drama-mirror/`，`main-a.json`／`main-b.json` 輪替（寫較舊的那個），圖片 `images/i-<id>`；格式同備份檔（含 trash）；沒資料時不更新副本。
- Gist：私人 gist，描述 `G_DESC`；`data.json`＋`images-001.json…`（每塊約 600 KB）＋`meta.json`；只上傳雜湊有變的檔；資料變動後 90 秒去抖自動上傳；本機為 0 篇時拒絕上傳；首次對既有雲端備份上傳時，若雲端筆數比本機多 3 篇以上會詢問。還原走原本「還原備份」流程（組成 File 後呼叫 `$('file').onchange`）。
- 啟動自動備份：`drama_gist` 內 `auto`（預設開，=== false 才關）、`d`（最近一次資料變動時間，`gistSchedule` 寫入）、`at`（最近一次備份「開始」時間）。啟動後若 `!h || !at || d>at` 就 3 秒後靜默 `gistPush(false)`；沒有變動就完全不動作。
- `cleanupImages` 現在在沒有任何劇評時不執行。
- `diag.html`：唯讀診斷頁，列出 IndexedDB／localStorage／OPFS／紀錄，可下載原始資料。

## v6.6 雲端同步（取代單向備份）
- `gistPush` 現在是同步：先 GET gist，若 `history[0].version` ≠ `drama_gist.rv` 就讀 `data.json` 呼叫 `syncMerge`，再上傳；成功後記 `rv` 與 IndexedDB 鍵 `syncBase`（{v:1,h}，上次同步後各項目的雜湊）。
- `syncMerge` 三方合併（本機／雲端／syncBase）：項目分 R（劇評，含狀態 l／t＝垃圾桶）、L、G、S、C（tagCats）、M（tagMap）、P（只同步 signature、sharedTags）。只有一邊變就採那邊（含刪除）；兩邊都變時劇評取時間較新（垃圾桶用刪除時間），其餘以本機為準；沒有 syncBase 時是聯集。示範資料不參與。雜湊用 `canon`（排序鍵），收藏庫忽略 nextNo，nextNo 取兩邊最大。
- 撞號：合併後跑 `ensureNos()`（較早建立的保留原號），被改號的劇評 updatedAt 更新並 toast 提示。
- 第一次同步時，本機沒有劇評且雲端沒有的空收藏庫會丟掉（避免新裝置多一個「我的收藏」）。
- 本機 0 篇時仍會下載合併，只是不上傳。觸發：開啟 3 秒後、回到前景（距上次 60 秒以上）、資料變動 90 秒去抖、切到背景。設定開關 `auto` 改名「自動同步」。
- 移除「雲端比本機多 3 篇以上就暫停」的檢查（合併後不需要）；「從雲端還原」維持原本匯入流程。
- 測試：Playwright 兩個 context 共用模擬的 api.github.com，測過新增、編輯、刪除、永久刪除、撞號、圖片、空裝置首次同步。

## 之後可考慮（尚未決定）
- 無。新想法先討論再動作。
