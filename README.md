# 簡報品牌大腦 brand-brain

AI 做的簡報總是不像你的品牌？因為它根本不認識你的品牌。

## 這個 skill 在幹嘛

你叫 AI 做簡報，它給你一份「AI 的審美」：字型隨機、顏色隨機、每次長得都不一樣。
然後你花半小時一頁頁調位置、改字級、換顏色。

問題不在 AI 不會設計，在你每次都讓它從零開始猜。

這個 skill 把你的品牌打包成一份「品牌大腦」檔案。建一次，之後每次做簡報，AI 先讀大腦再動手，做出來就是你品牌的樣子。

## 30 秒上手

裝好之後，第一次先說：

```
幫我建品牌大腦
```

它先問你手上有什麼：舊簡報、想參考的簡報、logo、品牌規範，有就丟給它，顏色、字型、版型習慣它直接抽出來，不再問。然後最多問你 3 題，每題都附好選項，你只要挑：

1. **品牌是做什麼的、簡報給誰看**：一句話。
2. **用色**：三種情況它都接得住。
   - 已經有品牌色：主色、輔色給它，其他它補齊。
   - 沒有色系：它給你8 款代表範本（簡潔提案、塗鴉手繪、玻璃科技、紙本雜誌、粉彩玩趣、靜物極簡、螢光撞色、便當格）挑一款，每款一種底稿風格。
   - 只說得出參考：「我想要像法拉利 F1 那種配色」，它幫你抽成一套用色系統，而且會講清楚這是靈感不是複製。
3. **字型**：有指定就說，它會先查你電腦有沒有裝、Google Fonts 有沒有；沒指定就給你兩個組合選。

排版不用你管，跟著範本走。任何一題說「你決定」，它就用預設補完，不再問。

**不想回答也可以**：說「用範本『粉彩玩趣』開始」，它直接拿 `templates/examples/` 裡填好的那份，只換品牌名。8 款長什麼樣，`examples/` 裡各有一份九頁式簡報（第 1 頁是 Design System，接封面、章節頁、三欄重點、左文右圖、一頁一數字、步驟流程、封底），瀏覽器打開就能看。

之後每次做簡報只要說：

```
用我的品牌大腦做簡報，內容是：（貼上你的大綱或文章）
```

它交出一份完整的 HTML 簡報（第 1 頁是你的 Design System）：顏色只用你的色票、字型照你的規定、版型照範本，icon 自動配好同一套，插圖告訴你去哪個免費庫拿、搜什麼、改成什麼色，最後附一張自我檢查清單證明它沒有偷加東西。

## 裝法（同一包，四個落點）

| 你用的工具 | 放哪裡 | 能做到 |
|---|---|---|
| Claude Code（桌機 app 或終端機） | `~/.claude/skills/` | 全功能：查你電腦有沒有裝指定字型、直接抓 icon 內嵌、把底稿燒成圖輸出可編輯的 PPTX |
| Codex（桌機 app 或終端機） | `~/.agents/skills/` | 讀得到整包規則與範本；PPTX 燒圖目前只在 Claude Code 驗過 |
| Gemini CLI | `~/.agents/skills/`（跟 Codex 同一個位置，放一次兩家都吃） | 同 Codex；第一次用會要你按一次確認 |
| Claude 網頁版或 App（免費版可用） | 設定 → 自訂 → Skills → 建立 → 上傳 ZIP；先在「功能」開啟「程式碼執行與檔案建立」 | 規則、範本、授權表全在；在雲端跑，所以查不到你電腦的字型、也不燒底稿 PPTX |
| ChatGPT | Skills 只開放 Business、Enterprise、Edu；Plus 與免費版把 `SKILL.md` 和 `references/` 的兩個檔案全文貼進自訂指令 | 規則生效，沒有本機功能 |
| Gemini 網頁版 | 做成 Gem，指令欄貼 `SKILL.md` 全文 | 同上 |

終端機使用者一行裝好（Claude Code）：

```bash
git clone https://github.com/Hsing-Ju-Chi/brand-brain.git ~/.claude/skills/brand-brain
```

Codex、Gemini CLI 把目標路徑換成 `~/.agents/skills/brand-brain`。不想碰指令的話：GitHub 頁面右上角綠色「Code」按鈕 → Download ZIP → 解壓縮，把資料夾改名成 `brand-brain` 放進上表你那一格的位置。之後正常講話就會自動生效，不用叫它的名字。

## 裡面有什麼

- **SKILL.md**：三個模式的完整規則（建大腦、做簡報、更新大腦）
- **references/color-presets.md**：8 款代表範本（配色、字型、版型預設、底稿風格、適合場景），對比度都算過；「底稿風格庫」八種全部純 CSS（角標細線、紙紋塗鴉、玻璃擬態、紙本編輯、粉彩色塊、純色極簡、撞色分割、便當格）；還有「從參考品牌抽色」和「版型怎麼配」
- **references/asset-licenses.md**：哪些免費圖庫、icon 庫可以商用、哪些能打包再送人、哪些只能連結。2026-09-07 逐一讀過官方授權頁，之後條款若改以原頁為準
- **references/pptx-bake.md**＋**scripts/pptx_from_spec.py**：PPTX 燒圖管線，底稿燒成圖、文字卡片 icon 原生可編輯（需要 Chrome）
- **templates/brand-brain-template.md**：品牌大腦的標準格式
- **templates/examples/**：八份填好的範例大腦，換掉品牌名就能用
- **examples/**：八份九頁式範例簡報（HTML＋PPTX），第 1 頁 Design System，各配一種底稿風格與封面構圖，也是做簡報時的頁型庫

## 免安裝版

只想先試試看？免安裝的簡報提示詞版在這裡，貼進任何 AI 就能用：

https://hsing-resource-center.vercel.app/resources/ai-brand-brain

## 出處

阿幸 hsing.daily。內容全部原創撰寫。

- Instagram: [@hsing.daily](https://www.instagram.com/hsing.daily/)
- 資源中心: https://hsing-resource-center.vercel.app

## 給維護者：檔數紅線

skill 安裝功能（貼 GitHub 網址或上傳 zip）實測有 **200 個檔案的硬上限**（官方文件沒寫）。這個 repo 目前 33 個檔案。加東西的規矩：範例簡報只放 HTML 不放圖片（截圖在資源中心網站），素材超過十幾個就打包成單一 zip 再 commit。發佈前跑一次 `git ls-files | wc -l`，超過 150 就停下來整理。

## 授權

MIT。自由使用、修改、散布。圖庫授權表裡列的第三方素材各依其原授權。
