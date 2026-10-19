# TanJi／碳吉官網與 Firebase 管理後台

專案：`tanji-official-1219f`  
官網：`https://tanji.tw/`  
管理員：`tanji2026@gmail.com`

## 先預覽

直接開啟 `index.html`，即可先看深色官網、完整中英文、作品分頁與角色介紹。首頁右側為實際作品網頁預覽，需要網路；若作品不允許嵌入，可點擊直接開啟，或日後在後台填入圖片網址。Firebase 登入與雲端資料需要經由 HTTPS 或 localhost 使用，直接開啟本機檔案不啟用 Firebase。

## 第一次上線：依序完成四步

### 1. 上傳到 GitHub

解壓縮後，將 `index.html`、`admin.html`、`404.html` 放到 `tanji2026/official` 儲存庫最外層。保留現有 Pages 的 main / root 及自訂網域 `tanji.tw`。隨包提供 CNAME，內容也是 `tanji.tw`。

不需要 npm、編譯或搬動網域；Gandi DNS 維持目前的 GitHub Pages 設定。免費 GitHub Pages 的儲存庫維持公開。

圖片以後可以放在 GitHub 的 `images` 資料夾，再於後台填寫 `./images/檔名.jpg`。碳吉寶寶的原始 PNG 已完整內嵌，無須另上傳，也沒有重新生成或拉伸。

### 2. 發布 Firestore 權限規則

1. 開啟 https://console.firebase.google.com/project/tanji-official-1219f/firestore 。
2. 進入「規則」。
3. 以此包 `firestore.rules` 的完整內容取代規則，按「發布」。

正式環境預設拒絕連線是正常的；不要改成所有人可讀寫的測試模式。**把規則檔上傳 GitHub，不等於已發布到 Firebase。**

有使用 Firebase CLI 時，也能在資料包資料夾執行：

```bash
npx --yes --package=firebase-tools@latest firebase login
npx --yes --package=firebase-tools@latest firebase deploy --only firestore:rules,firestore:indexes --project tanji-official-1219f
```

CLI 部署會同時設定內容欄位的索引豁免；使用 Console 貼上規則也能先正常運作。

### 3. 加入 Google 登入的授權網域

到 Authentication → Settings → Authorized domains，加入以下主機名稱（不加 https:// 或路徑）：

- `tanji.tw`
- `www.tanji.tw`
- `tanji2026.github.io`

若要本機以 HTTP 預覽後台，另加 `localhost`。Google 登入方式須已啟用。登入時使用 `tanji2026@gmail.com`，不需要提供密碼給任何人。

### 4. 帶入內容並發布

1. 開啟 `https://tanji.tw/admin.html`。
2. 使用指定 Google 帳號登入。
3. 在「總覽」按「帶入初始作品與文案」。包含 9 件作品、8 筆競賽成果、動態及關注議題。同 ID 的既有內容不會被覆蓋。
4. 可以先「儲存草稿」，確認文案與成果後再按「發布官網」。
5. 重新整理官網，會讀取 Firestore 的發布版本。後台顯示實際版本號與成功狀態；若權限或網路有錯，不會假裝已完成。

首次尚未發布時，官網使用檔案內的初始公開內容，因此仍可預覽。雲端發布文件存在後，以其內容為準；即使作品被清空，也不會自動合併回初始資料。若完全無法連線 Firebase，則回退到隨檔提供的既有公開內容，讓官網仍能瀏覽；已經公開過的初始內容仍存在 HTML 原始碼中。

## 已完成的前台

- 深色紫色介面；品牌固定為 TanJi／碳吉。
- 首頁基本介紹：「碳吉，探索永續解方的新創團隊。」沒有另加廣告主標語。
- 首頁：作品即時預覽、三大方向、精選作品、競賽成果、近期動態、團隊與聯絡入口。
- 分頁使用 `#/projects` 等路由，適用 GitHub Pages，自動重新整理仍能回到該頁。
- 完整中文與英文切換；語言偏好保存於瀏覽器，儲存功能被限制時仍可使用。
- 作品分類、每頁 6 件作品的頁碼、每頁 4 筆競賽成果的頁碼、作品詳細介紹。
- 獨立關注議題、聯絡頁與碳吉寶寶頁；角色也出現在關於頁，首頁不出現角色。
- 不呈現舊賽程進度，不虛構尚未提供的最終名次。保留附件中的獲獎及入圍結果。
- YouTube 尚未有網址，先顯示預設說明；後台填入後才出現可點擊連結。
- 不匯入個人履歷、個人證照或其他個人專案。

## 已完成的管理後台

- Google 管理員登入及伺服器端規則限制。
- 作品、競賽成果、動態、關注議題及網站基本設定的中英文編輯。
- 排序、首頁精選、公開／草稿、封存與還原。
- 草稿儲存與公開發布分開；公開快照只包含公開且未封存的內容。
- 每次發布保存版本記錄；可載入最近 15 個版本至草稿，確認後重新發布。
- JSON 匯出與匯入備份。匯入不會立即改變公開官網。
- 同時開多個視窗時，以版本號避免無聲覆蓋較新的草稿或發布版本。

目前未啟用後台檔案直接上傳；圖片採網址或 GitHub 檔案路徑。之後若需要 Cloud Storage，先確認 Blaze 計費方案再擴充。也未啟用 Analytics、Functions、會員註冊或寄信表單。

## Firestore 資料結構

| 路徑 | 內容與權限 |
|---|---|
| `site/public` | 公開網站快照；訪客只可讀這份文件，不能寫入或列出 site 集合 |
| `site/draft` | 後台完整草稿；僅指定 Google 管理員可讀寫 |
| `history/{版本ID}` | 每次發布的快照；僅指定管理員可讀與新增，禁止修改與刪除 |

預設使用 Firestore 的 `(default)` 資料庫。設定物件為 Firebase 公開網頁設定，並非管理員金鑰；寫入權限由 Authentication 與 Security Rules 保護。規則同時核對指定信箱、已驗證信箱及 Google 登入來源。未授權使用者即使修改 HTML，也不應取得資料寫入權限。

## 響應式與無障礙

- 640、900、1100px 等斷點：手機單欄、平板雙欄、桌機三欄。
- 支援窄至 320px 的排版設計；文字、卡片與信箱可換行，選單在平板與手機收合。
- 主要控制項至少 44px 觸控高度；鍵盤焦點、Escape 關閉選單與原生編輯對話框。
- 支援系統減少動態設定；角色圖片維持原始比例。
- 首頁作品使用外部即時網頁預覽，實際載入依作品服務狀況而定。

已完成程式語法與 48 組資料、頁面與事件檢查，包含雙語、分頁、分類、草稿隔離、內容跳脫、初始管理員載入與非管理員畫面。尚未登入實際 Firebase 專案進行寫入測試，未部署規則；本環境也未完成瀏覽器實機排版與 Firestore Emulator 測試。上傳後應在 Chrome、Safari 及手機確認登入、內容發布和作品預覽。這份資料包本身不代表已變更雲端設定。

## 檔案

- `index.html`：完整官網，樣式、互動、角色與初始內容都在單一檔案內。
- `admin.html`：完整管理後台。
- `firestore.rules`：必須在 Firebase 發布的資料存取規則。
- `firebase.json`、`firestore.indexes.json`：Firebase CLI 設定。
- `seed.json`：初始內容備份，與附件資料整理一致。
- `404.html`：網址不存在時的回首頁入口。

未取得的競賽照片與作品截圖不以其他圖片冒充。可在後台各作品或競賽紀錄加入實際圖片網址。
