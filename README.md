# LINE 貼圖製造機

免安裝、免後端、免註冊的 LINE 靜態貼圖產線。打開網頁就能用，圖片與金鑰只留在你自己的瀏覽器。

可以先輸入想參考的類型／風格，按「AI 幫我整理成 Prompt」；AI 會把風格、色彩、畫面感與既有描述整理成一段可直接生圖的 Prompt，自動填入下方描述欄位。Prompt 仍可手動修改，再開始生成。

**線上使用：** https://micl-legal.github.io/line-sticker-maker/

## 兩條路徑

| 路徑 | 適合誰 | 需要金鑰嗎 |
|---|---|---|
| [製造機 `index.html`](https://micl-legal.github.io/line-sticker-maker/) | 只有一個想法或一張角色照 | 要（自己的 Google AI Studio 金鑰；生圖模型依 Google 方案計費） |
| [切割器 `cutter.html`](https://micl-legal.github.io/line-sticker-maker/cutter.html) | 已在 ChatGPT / Gemini 生好一張總圖 | 不要，純本機 |

兩條路徑最後都輸出**同一份可直接上架的 ZIP**。

## 輸出規格（LINE Creators Market 靜態貼圖）

| 項目 | 規格 | 程式是否自動保證 |
|---|---|---|
| 張數 | 8 / 16 / 24 / 32 / 40 | 是，不合法不給打包 |
| 貼圖尺寸 | 370 x 320 以內、偶數、透明 PNG | 是，含 6% 安全邊距置中 |
| main.png | 240 x 240 | 是 |
| tab.png | 96 x 74 | 是 |
| 單檔大小 | 1 MB 以內 | 是，超過自動縮壓最多 6 次 |
| 上架申報 | AI 生成須據實申報 | 否，ZIP 內附檢查清單提醒 |

ZIP 內另附 `上架檢查清單.txt`，分「程式已保證」與「你要自己確認」兩區。

## 怎麼拿免費金鑰

1. 開 https://aistudio.google.com/apikey
2. 建立 API key，複製 `AIza...`
3. 貼進製造機的「設定金鑰」，按儲存（存在你瀏覽器的 localStorage，不會傳給任何人）

金鑰直接從瀏覽器送到 Google，不經過任何中間伺服器；本站是純靜態頁面，沒有後端。按下儲存或貼上 key 後，金鑰會保存於目前網站來源的瀏覽器 `localStorage`，重新整理同一網址不會消失；清除網站資料、私密瀏覽、換瀏覽器或換裝置時需要重新輸入。

## 生圖模型

| 模型 | 什麼時候用 |
|---|---|
| `gemini-3.1-flash-image` | 預設。2026-09-19 實測繁體中文可正確渲染 |
| `gemini-3-pro-image` | 複雜中文排版或多主體時 |
| `gemini-2.5-flash-image` | 最省，但中文字會糊，只適合純表情無字 |

## 隱私

純前端靜態網頁。圖片在瀏覽器裡處理，去背、裁切、打包都用 Canvas 完成，不上傳伺服器。JSZip 已內嵌，離線也能跑切割與打包。

## 授權

MIT。內嵌 JSZip v3.10.1（MIT / GPLv3 雙授權）。
