# LINE 貼圖製造機

免安裝、免後端、免註冊的 LINE 靜態貼圖產線。打開網頁就能用；參考圖直接送 Google AI，不經本站伺服器，專案資料與生成結果保存於你自己的瀏覽器。

主流程是單頁完成：上傳參考圖後，AI 會分析角色外觀、色彩、線條、材質、構圖與整體氣氛，整理成可直接生圖的原創 Prompt；接著生成固定 16 張、組成 4×4 總圖、用內建 Cutter 切回 16 格，最後打包成 LINE 上架 ZIP。Prompt 仍可手動修改。

「補充貼圖主題／風格／文字語氣」也會影響台詞內容。AI 會產生 16 組「台詞｜動作」，你可以逐句修改；角色圖片由影像模型生成，確認過的繁體中文台詞再由瀏覽器後製到圖片上。

**線上使用：** https://micl-legal.github.io/line-sticker-maker/

## 單頁完整流程

| 路徑 | 適合誰 | 需要金鑰嗎 |
|---|---|---|
| [製造機 `index.html`](https://micl-legal.github.io/line-sticker-maker/) | 從參考圖到上架 ZIP 的完整流程 | 要（Google、Hugging Face 或 Cloudflare 的自己的憑證） |
| [舊版獨立切割器 `cutter.html`](https://micl-legal.github.io/line-sticker-maker/cutter.html) | 已經有外部 AI 產生的 4×4 總圖 | 不要，純本機 |

一般使用不需要離開主頁；獨立切割器保留給既有總圖的額外入口。

## 角色專案記憶

主頁的「角色專案記憶」會在目前瀏覽器的 IndexedDB 保存：

- 角色參考圖
- 參考類型／風格文字
- AI 產生及你修改後的 Prompt
- Prompt 歷史版本
- 16 個貼圖點子
- 已生成的貼圖結果與生成設定

重新整理或隔天回到同一個網址，可以從「已保存的角色專案」載入並繼續生成。這是瀏覽器本機資料，不會自動同步到其他裝置；清除網站資料、使用私密瀏覽或換瀏覽器時不會保留。

## 輸出規格（LINE Creators Market 靜態貼圖）

| 項目 | 規格 | 程式是否自動保證 |
|---|---|---|
| 張數 | 固定 16 張（4×4） | 是，不完整不給打包 |
| 貼圖尺寸 | 370 x 320 以內、偶數、透明 PNG | 是，含 6% 安全邊距置中 |
| main.png | 240 x 240 | 是 |
| tab.png | 96 x 74 | 是 |
| 單檔大小 | 1 MB 以內 | 是，超過自動縮壓最多 6 次 |
| 上架申報 | AI 生成須據實申報 | 否，ZIP 內附檢查清單提醒 |

ZIP 內另附 `上架檢查清單.txt`，分「程式已保證」與「你要自己確認」兩區。

## 多服務商金鑰與自動備援

主頁的「設定 AI 服務商金鑰」可以分別填入：

- Google Gemini：3 組 API Key；負責 Prompt、台詞與 Gemini 生圖。
- Hugging Face：3 組 Token；有參考圖時嘗試 image-to-image，沒有參考圖時使用 FLUX 文字生圖。
- Cloudflare Workers AI：3 組 Token 與對應的 Account ID；使用 FLUX.1 Schnell 文字生圖。

「生圖服務商優先順序」預設是 Google → Hugging Face → Cloudflare。某組憑證失敗後，會先嘗試同服務商的下一組，再切換下一個已設定的服務商。服務商憑證只保存於目前瀏覽器的 localStorage，不會寫入專案 IndexedDB。

Cloudflare 的 FLUX.1 Schnell 是文字生圖模型，因此不能直接接收參考圖；程式會繼續使用參考圖分析出的 Prompt。若需要參考圖轉圖，優先使用 Google 或 Hugging Face。

## 怎麼取得 Google AI Studio 金鑰

1. 開 https://aistudio.google.com/apikey
2. 建立 API key，複製 `AIza...`
3. 貼進製造機的「設定 AI 服務商金鑰」，按儲存（存在你瀏覽器的 localStorage，不會傳給本站）

金鑰直接從瀏覽器送到對應服務商，不經過本站伺服器；本站是純靜態頁面，沒有後端。按下儲存或貼上 key 後，憑證會保存於目前網站來源的瀏覽器 `localStorage`，重新整理同一網址不會消失；清除網站資料、私密瀏覽、換瀏覽器或換裝置時需要重新輸入。角色專案資料則使用瀏覽器 `IndexedDB` 保存，與 API 金鑰分開。請只填自己的憑證，不要把憑證寫入 GitHub 或貼給別人。

## 生圖模型

| 模型 | 什麼時候用 |
|---|---|
| `gemini-3.1-flash-image` | 預設。2026-09-19 實測繁體中文可正確渲染 |
| `gemini-3-pro-image` | 複雜中文排版或多主體時 |
| `gemini-2.5-flash-image` | 最省，但中文字會糊，只適合純表情無字 |

Prompt 分析與貼圖點子規劃優先使用 `gemini-3.6-flash`；若遇到暫時性高流量，程式會自動重試並依序切換 `gemini-3.7-flash`、`gemini-3.5-flash-lite`。參考圖會作為圖片輸入交給文字模型分析。

## 隱私

純前端靜態網頁，不經本站伺服器。若上傳參考圖，圖片會由瀏覽器直接送給 Google Gemini 做分析與生圖；生成後的結果、去背、4×4 組圖、切割與打包都在瀏覽器用 Canvas 完成。JSZip 已內嵌，沒有 AI API 的切割與打包可離線執行。

## 授權

MIT。內嵌 JSZip v3.10.1（MIT / GPLv3 雙授權）。
