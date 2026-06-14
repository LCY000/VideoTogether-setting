# VideoTogether 設定頁 / Settings Page

**[中文](#中文) · [English](#english)**

> VideoTogether 設定頁的原始碼。獨立的 git 儲存庫，透過 GitHub Pages 部署，與前端 userscript／擴充功能連動。
>
> Source for the VideoTogether settings page. A standalone git repository, deployed via GitHub Pages, that works together with the frontend userscript / extension.

---

## 中文

### 這是什麼
使用者點面板右上角的齒輪會開啟的設定頁。它**不直接呼叫**前端，而是透過共用的擴充 storage 連動：
設定頁送出 `postMessage({type:15, key, value})` → 內容腳本寫入擴充 storage → 同步回所有分頁 → 前端用 `getVideoTogetherStorage(key, 預設值)` 讀取。

### 檔案結構
| 檔案 | 說明 |
|------|------|
| `v3.buildme.html` | **唯一要手改的原始檔**。含版面、CSS、JS。 |
| `v3.html` | 由 `v3.buildme.html` **建構產生**，請勿手改（會被蓋掉）。這是線上實際載入的頁面。 |
| `v2.html` | 舊版網址，已改為轉址到 `v3.html`（讓仍指向 v2 的舊安裝不會 404）。 |
| `localization/*.json` | 四語字串（`zh-cn / zh-tw / en-us / ja-jp`），建構時用 `{{{ }}}` 注入進 `v3.buildme.html`。 |
| `vt_source.json` | 建構設定（`releaseTarget: "./"`，讓產物輸出在本資料夾）。 |
| `vercel.json` | 舊的 Vercel 設定（目前用 GitHub Pages，留著無害）。 |

### 建構
在前端主專案根目錄執行（設定頁是主專案的內嵌 repo）：
```bash
python3 script/build_extension.py disable_network   # disable_network 可避免覆蓋未 commit 的本地修改
```
建構會把 `*.buildme.html` + `localization/*.json` 編成 `v3.html`。

### 部署（GitHub Pages）
本儲存庫的 GitHub Pages 來源為 `main`：
```bash
git add -A && git commit -m "..."
git push origin main      # push 後 Pages 會自動重新部署 v3.html
```

### 怎麼新增一個設定開關
1. `v3.buildme.html` 加一段開關 DOM：label 的 `id` 必須是 `<Key>Label`、checkbox 的 `id` 必須是 `<Key>`。
2. 要說明就加 `<span class="vt-help-trigger" onclick="toggleHelp('<Key>LabelHelp')">…<i class="vt-help-icon">info_outline</i></span>`，並在該列後面加 `<div id="<Key>LabelHelp" class="vt-help-text"></div>`（會自動被填入翻譯、點 ⓘ 就地展開）。
3. `localization/*.json` 四語各加 `<Key>Label`（與選用的 `<Key>LabelHelp`）。
4. 前端用 `getVideoTogetherStorage('<Key>', 預設值)` 讀取。**預設值務必讓「未設定＝維持原本行為」**。

---

## English

### What this is
The settings page opened from the gear icon in the panel. It does **not** call the frontend directly; it talks through shared extension storage:
the page posts `postMessage({type:15, key, value})` → the content script writes extension storage → it syncs back to all tabs → the frontend reads it via `getVideoTogetherStorage(key, default)`.

### Files
| File | Purpose |
|------|---------|
| `v3.buildme.html` | **The only file you edit by hand.** Layout, CSS, JS. |
| `v3.html` | **Generated** from `v3.buildme.html` — do not edit (it gets overwritten). This is what loads in production. |
| `v2.html` | Old URL, now redirects to `v3.html` so older installs pointing at v2 don't 404. |
| `localization/*.json` | Strings for the four languages (`zh-cn / zh-tw / en-us / ja-jp`), injected into `v3.buildme.html` via `{{{ }}}` at build time. |
| `vt_source.json` | Build config (`releaseTarget: "./"` so output lands in this folder). |
| `vercel.json` | Legacy Vercel config (we use GitHub Pages; harmless to keep). |

### Build
Run from the frontend main project's root (this settings page is an embedded repo of it):
```bash
python3 script/build_extension.py disable_network   # disable_network avoids clobbering uncommitted local edits
```
The build compiles `*.buildme.html` + `localization/*.json` into `v3.html`.

### Deploy (GitHub Pages)
This repository's GitHub Pages source is `main`:
```bash
git add -A && git commit -m "..."
git push origin main      # Pages redeploys v3.html automatically after push
```

### Adding a toggle
1. Add the toggle DOM in `v3.buildme.html`: the label's `id` must be `<Key>Label`, the checkbox's `id` must be `<Key>`.
2. For a help note, add `<span class="vt-help-trigger" onclick="toggleHelp('<Key>LabelHelp')">…<i class="vt-help-icon">info_outline</i></span>` and, right after that row, `<div id="<Key>LabelHelp" class="vt-help-text"></div>` (auto-filled with the translation; click ⓘ to expand inline).
3. Add `<Key>Label` (and optional `<Key>LabelHelp`) to all four `localization/*.json` files.
4. In the frontend, read it with `getVideoTogetherStorage('<Key>', default)`. **Choose the default so "unset = current behavior".**
