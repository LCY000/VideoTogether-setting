# 給上游維護者的說明 / Notes for the upstream maintainer

> 這個檔只是 PR 的溝通說明，**合併時可以任意改寫或直接刪除**。
> This file is just PR context — **feel free to edit or delete it when you merge.**

**[中文](#中文) · [English](#english)**

---

## 中文

### 這份 PR 是什麼
- 來源 fork：`LCY000/VideoTogether-setting` → 目標上游：`VideoTogether/setting`。
- 內容是 **2026-06-14 ～ 06-18** 對設定頁 `v3` 的一次較大改版：外觀、功能、文案、四語在地化。
- 光看設定頁，改動幅度就不小（整體視覺重做 + 多個開關與文案調整）。

### ⚠️ 這次和前端 PR 是一組的
設定頁不直接呼叫前端，而是透過共用的擴充 storage 連動（見 `README.md`）。這次改版**和前端 PR 互相搭配**：

- 前端：`LCY000/VideoTogether` → `VideoTogether/VideoTogether`
- 設定頁新增／調整的開關（如 `EnableMiniBar`、`EnableMessageVoice`、`WaitForLoadding`、`MinimiseDefault` …）需要前端以 `getVideoTogetherStorage('<Key>', 預設值)` 讀取。
- **建議兩個 PR 一起合併**，否則新開關在設定頁會顯示，但前端沒有對應讀取就不會生效。

### 這 6–7 天改了什麼（重點整理）
- **外觀整體對齊浮動面板**：品牌藍頂列＋logo、玻璃圓角卡片、漸層背景、開關藍校準成面板 `#5b8def`、深色模式、列對齊。
- **ⓘ 就地展開說明**：每個開關點右側 ⓘ，在原地展開說明文字（不跳窗）。
- **版面重整**：左欄依重要度排序、欄位改寫、迷你小窗移到左欄、移除主畫面的「輕鬆分享」、「檢查新版本更新」改成醒目按鈕後再收進右上 ⋮ 選單、密碼＝控制權說明寫清楚。
- **官方網站連結**：頂列地球 icon，依語言切 `zh-cn` / `en-us`。
- **語言處理**：改為「Language」標籤、純「Auto」自動偵測、下拉與顯示語言以 `localStorage` 為準、修掉語言選單約 1 秒自動關閉的 bug。
- **開關與文案**：新增「文字訊息語音播報」、語音播報預設開、M3U8 固定使用內建播放器並加說明、多處文案準確化。
- **在地化**：`zh-cn / zh-tw / en-us / ja-jp` 四語同步。
- **建置來源完整性修復**（本 PR 最後一筆）：先前有 commit 直接手改成品 `v3.html`，會在下次建構被來源覆蓋；已把改動同步回 `v3.buildme.html` + `localization/*.json`，並在 README 補上疑難排解。

### 上游化檢查清單（你可能想調整的地方）
1. **fork 提及**：只有 `README.md` 兩處寫了「(fork: `LCY000/VideoTogether`)」當貢獻者指路用，其餘檔案沒有 fork 字樣——要保留或拿掉隨你。
2. **網址／服務（已是上游，無需更動）**：`videotogether.github.io`（站台與 logo）、`unpkg`（MDUI CDN）、`vt.panghair.com:5000`（語音角色錄製 / ReEcho，上游後端）。**沒有任何個人或 fork 的部署網址。**
3. **語音功能的後端依賴**：語音角色錄製會 `POST` 到 `vt.panghair.com:5000/reecho/new_voice`（上游後端）；請確認該端點在上游可用。
4. **設定頁網址**：前端用哪個網址開設定頁，是寫在**前端 repo**、由搭配的前端 PR 處理；本 repo 內沒有需要改的自我網址。
5. **部署**：GitHub Pages 來源維持 `main`；要重產 `v3.html` 的話，在前端主專案根目錄執行 `python3 script/build_extension.py`。

### 怎麼驗證沒被改壞
`v3.html` 是 `v3.buildme.html` + `localization/*.json` 建構出來的成品。把 v3.html 內嵌的各語言物件用大括號配對抽出來、模擬一次建構，應能原樣還原 v3.html（本 PR 已驗證一致）。

---

## English

### What this PR is
- Source fork: `LCY000/VideoTogether-setting` → upstream target: `VideoTogether/setting`.
- A sizable revamp of the `v3` settings page over **2026-06-14 to 06-18**: visuals, features, copy, and four-language localization.
- The settings page alone changed a lot (full visual redesign plus several toggle/copy changes).

### ⚠️ This pairs with a frontend PR
The settings page doesn't call the frontend directly; it links through shared extension storage (see `README.md`). This revamp **goes together with a frontend PR**:

- Frontend: `LCY000/VideoTogether` → `VideoTogether/VideoTogether`
- New/changed toggles here (`EnableMiniBar`, `EnableMessageVoice`, `WaitForLoadding`, `MinimiseDefault`, …) are read by the frontend via `getVideoTogetherStorage('<Key>', default)`.
- **Please merge both PRs together** — otherwise the new toggles render here but do nothing without the matching frontend readers.

### What changed in the last 6–7 days
- **Visuals aligned to the floating panel**: brand-blue top bar + logo, glassy rounded cards, gradient background, switches calibrated to the panel blue `#5b8def`, dark mode, row alignment.
- **Inline ⓘ help**: click the info icon on a row to expand its explanation in place (no popup).
- **Layout reorg**: left column ordered by importance, fields rewritten, mini-window moved to the left column, "Easy Share" removed from the main view, "Check for updates" turned into a prominent button then tucked into the top ⋮ menu, password-as-control explanation clarified.
- **Official-site link**: globe icon in the top bar, language-aware (`zh-cn` / `en-us`).
- **Language handling**: "Language" label, pure "Auto" auto-detect, dropdown and display language driven by `localStorage`, fixed the ~1s auto-close bug on the language menu.
- **Toggles & copy**: added "Read text messages aloud", voice readout default on, M3U8 forced to the built-in player with explanation, many copy accuracy fixes.
- **Localization**: `zh-cn / zh-tw / en-us / ja-jp` kept in sync.
- **Build-source integrity fix** (last commit in this PR): an earlier commit hand-edited the generated `v3.html`, which a rebuild reverts; the edits are now synced back into `v3.buildme.html` + `localization/*.json`, and the README gained a troubleshooting note.

### Upstreaming checklist (things you may want to adjust)
1. **Fork mention**: only `README.md` mentions "(fork: `LCY000/VideoTogether`)" as a pointer for contributors; nothing else references the fork — keep or drop as you prefer.
2. **URLs / services (already upstream, nothing to change)**: `videotogether.github.io` (site & logo), `unpkg` (MDUI CDN), `vt.panghair.com:5000` (voice-character recording / ReEcho, upstream backend). **No personal or fork deployment URLs anywhere.**
3. **Voice feature backend dependency**: voice-character recording `POST`s to `vt.panghair.com:5000/reecho/new_voice` (upstream backend); please confirm that endpoint is available upstream.
4. **Settings-page URL**: which URL the frontend uses to open the settings page lives in the **frontend repo** and is handled by the paired frontend PR; there's no self-URL to change in this repo.
5. **Deploy**: GitHub Pages source stays `main`; to regenerate `v3.html`, run `python3 script/build_extension.py` from the frontend project root.

### How to verify nothing is broken
`v3.html` is generated from `v3.buildme.html` + `localization/*.json`. Brace-match the inlined per-language objects out of `v3.html` and simulate one build — it should reproduce `v3.html` exactly (verified for this PR).
