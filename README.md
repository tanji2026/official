# Tanji／碳吉官網

白底、紫色點綴的四頁靜態網站，支援繁體中文與英文。

## 預覽

在本資料夾執行 `python3 -m http.server 8000`，開啟 http://localhost:8000 。也可以直接開啟 index.html。

## GitHub Pages

將本資料夾「內部檔案」上傳至 official 儲存庫根目錄，不要多包一層資料夾。
Pages 選 main / (root)，Custom domain 保持 tanji.tw。CNAME 已包含此網域。
以真正 HTML 分頁處理網址，重新整理分頁不依賴伺服器路由。

## 修改內容

- assets/data.js：中英文文案、探索方向與六件提案題材。
- assets/style.css：色彩、字級與響應式版面。
- assets/app.js：導覽、頁面結構、語系與作品篩選。
- assets/tanjibaby.png：使用使用者提供的原始圖，不採用重製角色。

目前作品以「提案題材」呈現，提問式文字為編輯草稿，不代表已確認的功能。未填作品完成度、商用成績、實際展示網址或獎項。競賽頁保留簡潔的更新說明，確認紀錄後再新增時間軸。未建立虛構成員或客戶。

無 npm、CDN 或外部字型依賴；使用原生 JavaScript 與 CSS，方便維護及直接託管。圖片為有白底 PNG，在首頁以混色方式融入白底，未宣稱為透明背景。語言偏好儲存在本機，不收集使用者資料。
