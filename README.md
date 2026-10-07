# 有約 (yǒuyuē)

單一 HTML 檔案、自架在 GitHub Pages 上的約會/聚會協調小工具。沒有後端伺服器、沒有建置流程 — 開啟 `meetup-planner-selfhost.html` 就是整個 app，狀態靠雲端 JSON 儲存服務同步。

## 部署方式

把 `meetup-planner-selfhost.html`（可改名為 `index.html`）上傳蓋掉 GitHub repo 裡的舊檔並 push 到 `main`（或你設定的 Pages 分支），GitHub Pages 會自動重新發布，網址不變：
https://afortunadofu-bot.github.io/meetup-planner/

**重要**：換檔案後如果瀏覽器還是顯示舊的行為，先試試強制重新整理（Ctrl/Cmd+Shift+R）或無痕視窗 — 靜態檔案常常被瀏覽器或 CDN 快取住。

## 目前使用的儲存服務

這個 app 會把每個新邀請「雙寫」到兩個獨立的免費 JSON 儲存服務，任何一個當掉都不影響 app 運作：

- **jsonbin.io**（主要）— 需要免費帳號 + API 金鑰，寫在檔案裡的 `JSONBIN_KEY`。
- **jsonstorage.net**（備援）— 同樣需要免費帳號 + API 金鑰，寫在檔案裡的 `ALT_KEY`。

兩把金鑰都設好時，新邀請會同時存在兩邊（`dual:` 開頭的連結編號）；只設一把時，app 會自動只用那一邊，仍然可以正常運作，只是少了備援。兩把都沒設時無法建立新邀請，會顯示明確的錯誤訊息。

金鑰申請方式與細節寫在 `meetup-planner-selfhost.html` 檔案開頭的註解裡。

## 版本紀錄

- **2026-09-03** — jsonstorage.net（備援服務）最近也改成需要 API 金鑰才能建立新邀請（先前完全免註冊）。新增 `ALT_KEY` / `ALT_KEY_PLACEHOLDER` 機制，跟 jsonbin.io 用同一套「沒金鑰就優雅跳過該服務、改用另一邊撐著」的邏輯，兩邊金鑰互相獨立。同步更新測試涵蓋「只有一邊有金鑰」「兩邊都沒金鑰」的情況。
- **稍早（同一批更新）** — jsonblob.com 被該服務自己的 Cloudflare 防火牆整個封鎖（對這個網站的所有請求回傳 403，且無法靠重試解決），確認為永久性阻擋後，主要儲存服務改為 jsonbin.io（需申請免費金鑰），jsonstorage.net 保留作備援。
- **雙寫備援架構** — 每個新邀請同時寫入兩個獨立服務；讀取/更新時只要有一邊還活著就能同步，兩邊都失敗才會顯示錯誤橫幅並提供重試按鈕。
- **拒絕按鈕動畫** — 單人邀請流程中的「不要」選項改成隨機的 6 種逗趣動畫（晃動/閃避/消失/旋轉/翻轉/彈跳），連續兩次不會重複，並搭配輪替提示文字。

> 這份版本紀錄從這一輪對話開始維護；更早期的功能與變更沒有留下紀錄。之後每次更新都會補上一筆。

## 已知限制

- 目前這個工作環境沒有連到你的 GitHub repo（沒有安裝 GitHub 連接器、也沒有存好的 git 憑證），所以沒辦法在這裡直接幫你 `git push`。每次更新後還是需要你自己把新的 `meetup-planner-selfhost.html` 上傳蓋掉 repo 裡的舊檔。如果你之後把電腦連進這個對話（並且那台電腦上已經 clone 好、設定好 git 憑證），我就可以直接在你的電腦上跑 `git add / commit / push` 幫你完成這一步。
