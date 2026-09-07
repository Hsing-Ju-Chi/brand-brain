# PPTX 燒圖管線（Claude Code 環境）

底稿的模糊、顆粒、手繪線條，python-pptx 做不出來。解法：**底稿層燒成圖鋪滿背景，文字、卡片框、icon 用原生物件放回去**。使用者在 PowerPoint 裡照樣能改字、移卡片；底稿是一張圖。用 QuickLook 實際渲染驗過，跟 HTML 版幾乎一樣。

需要：本機有 Chrome（headless 截圖）＋ python3（在 venv 裝 `python-pptx`）。沒有 Chrome 的環境只給 HTML，並說明可用瀏覽器印成 PDF。

## 做法（六步）

1. **HTML 先標記**：文字元素加 `data-txt`（多行標題用 `data-lines="第一行\n第二行"`），卡片、封面文字塊加 `data-box`，icon 的 `<svg>` 加 `data-icon="名稱"`。
2. **燒圖版**：同一頁 `<body class="bake native">`，CSS：
   ```css
   body.bake [data-txt]{visibility:hidden}
   body.bake.native [data-box]{visibility:hidden}
   body.bake.native [data-box] [data-icon]{visibility:hidden}
   ```
   手繪線稿風格不加 `native`（手繪框是風格本體，燒進圖）。
   Chrome：`--headless=new --force-device-scale-factor=2 --window-size=1280,720 --virtual-time-budget=8000 --screenshot=p.png`，再 `sips -s format jpeg -s formatOptions 85` 轉 JPG（不透明底稿用 JPG，一頁約 150 KB；同一底稿八頁會被 python-pptx 自動去重，只存一張）。
3. **量測版**：同一頁加一段 script，在 `load` 後把每個 `[data-txt]`／`[data-box]`／`[data-icon]` 的 `getBoundingClientRect()`（相對 `.slide`）和 `getComputedStyle`（字型、字級、字重、顏色、行距、字距、透明度；卡片的底色、邊框、圓角）寫進 `<pre id="rects">`，用 `chrome --headless=new --dump-dom` 讀出 JSON。
4. **icon 貼圖**：每顆 icon 用 160px 的 HTML 截透明 PNG（`--default-background-color=00000000`），顏色照量到的主色。
5. **python-pptx 組頁**（`scripts/pptx_from_spec.py` 可直接用，吃第 3 步的 JSON）：
   - `add_picture` 鋪滿 1280×720（1px = 9525 EMU）
   - 卡片 `add_shape`（圓角依量測值；填色用 `<a:alpha>` 做半透明；邊框寬度、顏色照量測；只有左邊線的用 connector 畫一條）
   - icon `add_picture`
   - 文字 `add_textbox`：margin 全 0、`word_wrap`、字級 px×0.75 pt、字重 ≥600 為粗體、顏色；**中文字型要另設 `<a:ea typeface>`**，否則 PowerPoint 會退成宋體；字距用 `rPr.spc`（pt×100）；**行距用固定值** `Pt(字級×行距×0.75)`，比例行距在 PowerPoint 會偏高、第二行會壓到下一段；透明度（章節頁淡化的大編號）用 `<a:alpha>`。
6. **交付說明**要講三件事：文字與卡片可改、底稿是圖層（換底稿要重出）、電腦沒裝該字型時 PowerPoint 會替換。

## 限制

- 卡片數量在 HTML 決定；使用者在 PowerPoint 加卡片要自己複製形狀。
- 手繪線稿風格的卡片框在圖層裡，不能單獨移動。
- 燒圖需要 Chrome；ChatGPT 免安裝版沒有這條，只有 HTML。
