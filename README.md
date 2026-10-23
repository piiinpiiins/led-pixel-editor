# 96 × 16 跑馬燈點陣編輯器 · LED Pixel Editor

**🔗 線上使用 / Live demo：<https://led-pixel-editor.vercel.app/>**

[正體中文](#正體中文) | [English](#english)

---

## 正體中文

在瀏覽器上就能使用的 LED 跑馬燈點陣編輯器，專為 **96 × 16（共 1,536 點）** 的 LED 燈板設計。可以逐格繪製、每一格分別上色、將圖片轉成點陣、儲存常用圖案，並匯出 PNG 或 JSON 給燈板韌體使用。

整個工具只有一個 HTML 檔，不需安裝、不需伺服器，也不依賴任何外部函式庫。

### 快速開始

直接在線上使用：**<https://led-pixel-editor.vercel.app/>**，不需要下載。

要在本機使用，用瀏覽器開啟 `index.html` 即可，電腦、手機、平板都能使用。

```bash
open index.html
```

電腦版建議使用 Chrome 或 Edge；「畫面吸色」功能需要瀏覽器支援 EyeDropper API。

### 部署到 Vercel

線上網址是 <https://led-pixel-editor.vercel.app/>。這個 repository 已連接 Vercel，每次 push 到 `main` 都會自動重新部署。

若要部署你自己的版本：這是純靜態網站，不需要建置步驟，也不需要 `vercel.json`。

1. 到 [vercel.com/new](https://vercel.com/new) 匯入 GitHub 上的 `piiinpiiins/led-pixel-editor` repository
2. Framework Preset 維持 **Other**，Build Command 與 Output Directory 留白
3. 按 **Deploy**；之後每次 push 到 `main` 都會自動重新部署

也可以在 repository 資料夾用 Vercel CLI 部署：

```bash
npx vercel --prod
```

### 功能

#### 繪圖

| 操作 | 說明 |
| --- | --- |
| 左鍵拖曳 | 以目前的畫筆顏色連續繪製 |
| 右鍵拖曳 | 擦除 |
| Shift + 拖曳 | 圈選矩形範圍 |
| Ctrl / ⌘ + Z | 復原 |
| B / E / I / S | 畫筆／橡皮擦／滴管／圈選 |
| 單指 | 繪製（手機、平板） |
| 雙指拖曳 | 左右移動燈板 |
| 雙指開合 | 放大縮小 |

- **工具**：畫筆、橡皮擦、滴管（吸取燈板上某一格的顏色）、圈選（觸控螢幕沒有 Shift 鍵時使用）
- **平移**：整張圖上下左右循環平移一格，超出邊界的點會從另一側出現
- **反相**：亮暗互換，新點亮的格子使用目前的畫筆顏色
- **復原**：最多保留 40 步
- **全部清除**
- **格子大小**：顯示比例可在 3–24 px 之間調整；「符合寬度」會讓整片燈板剛好塞進螢幕（開啟時的預設）
- **預設作品**：第一次打開時燈板會載入 `default-artwork.json`，按「載入預設作品」也能隨時回到預設作品；替換這個檔案就能改變新訪客看到的畫面
- **自動暫存**：作品會暫存在這個瀏覽器的 localStorage，重新整理也會還原；要換裝置請用「儲存 JSON」
- 快速拖曳時會自動補上中間的格子，不會漏點

#### 顏色

- 每一格都會記錄自己的色碼，同一張圖可以有多種顏色
- **畫面吸色**：用系統滴管吸取螢幕上任何位置的顏色（僅部分瀏覽器支援）
- **把亮的點全部改成畫筆顏色**：一鍵統一顏色
- **色票區**：自動列出這張圖用到的所有顏色與格數，點一下即可設為畫筆顏色

#### 圖案

1. 按住 Shift 在燈板上拖曳圈選範圍
2. 輸入名稱後按「儲存為圖案」
3. 在圖案清單中按「貼上」，再點選燈板決定圖案左上角的位置

圖案中未點亮的格子是透明的，貼上時不會覆蓋底下原有的圖。圖案可以重新命名或刪除，也會一併存進 JSON 檔。

#### 匯入圖片轉點陣

1. 按「匯入圖片」選擇圖檔
2. 在圖片上拖曳框選範圍（框內拖曳＝移動，拖曳角落＝縮放，預設鎖定 6：1 比例）
3. 調整轉換參數，下方會即時預覽結果
4. 按「套用到燈板」

| 參數 | 說明 |
| --- | --- |
| 轉換方式 | 單色（全部使用畫筆顏色）或彩色（保留原圖每一格的顏色） |
| 亮度門檻 | 0–255，亮度高於門檻的格子會被點亮 |
| 明暗反轉 | 讓暗部變成亮點 |
| 抖色 | 使用 Floyd–Steinberg 演算法保留灰階層次，照片建議開啟 |

> 96 × 16 只有 1,536 個點，線條清楚、對比強烈的圖案效果最好。

#### 匯出與讀取

- **下載圖片 1 倍**：96 × 16 px 的 PNG，一格對應一個像素
- **下載圖片 10 倍**：960 × 160 px 的 PNG，方便預覽
- **儲存 JSON**：完整保存點陣、顏色與圖案
- **讀取 JSON**：載入先前儲存的檔案（僅接受 96 × 16 尺寸）
- **載入預設作品**：把燈板和圖案清單換成 `default-artwork.json`（燈板可以復原；目前有作品時會先詢問）

檔名會自動加上時間戳記，例如 `marquee-96x16-20260904-223058.json`、`marquee-96x16-10x-20260904-223058.png`。

### JSON 格式

範例（陣列內容已省略）：

```json
{
  "width": 96,
  "height": 16,
  "savedAt": "2026-09-04T14:30:58.522Z",
  "penColor": "#fd5db3",
  "palette": [{ "hex": "#fd5db3", "count": 255 }],
  "pixels": ["000000000100000010000000..."],
  "colors": [[null, null, "#fd5db3", "..."]],
  "patterns": [
    { "name": "heart", "w": 7, "h": 6, "cells": [[null, "#ff4242", "..."]] }
  ]
}
```

各欄位說明如下：

| 欄位 | 說明 |
| --- | --- |
| `width`、`height` | 固定為 96、16 |
| `savedAt` | 儲存時間（ISO 8601） |
| `penColor` | 儲存當下的畫筆顏色 |
| `palette` | 用到的顏色與格數，依格數由多到少排序 |
| `pixels` | 16 個字串，每個 96 字元，`'1'` 為亮、`'0'` 為暗 |
| `colors` | 16 × 96 的二維陣列，亮的格子是色碼，暗的格子是 `null` |
| `patterns` | 儲存的圖案，`cells` 為 h × w 的二維陣列，內容是色碼或 `null` |

座標以左上角為 (0, 0)，第 `(x, y)` 格對應 `pixels[y][x]` 與 `colors[y][x]`。

讀取沒有 `colors` 欄位的舊版檔案時，亮起的格子會改用檔案中的 `penColor`（或 `color`）作為顏色。

### 專案檔案

| 檔案 | 說明 |
| --- | --- |
| `index.html` | 編輯器本體，支援手機與平板（HTML + CSS + JavaScript 單一檔案） |
| `led-pixel-editor-v3_1.html` | 先前的電腦版，保留作為參考 |
| `default-artwork.json` | 第一次打開時載入的預設作品（內含 11 個圖案），也可以用「讀取 JSON」載入 |

---

## English

A browser-based pixel editor for **96 × 16 (1,536-dot)** LED marquee boards. Paint cell by cell with per-cell colors, convert images to dot matrices, save reusable patterns, and export PNG or JSON for your LED board firmware.

The whole tool is a single HTML file — no install, no server, no external libraries.

### Quick Start

Use it online at **<https://led-pixel-editor.vercel.app/>** — no download needed.

To run it locally, open `index.html` in a browser — it works on desktop, phones and tablets.

```bash
open index.html
```

Chrome or Edge is recommended on desktop; the **Screen Pick** button requires the EyeDropper API.

### Deploy to Vercel

The live site is <https://led-pixel-editor.vercel.app/>. This repository is connected to Vercel, so every push to `main` redeploys it automatically.

To deploy your own copy: the site is fully static, so no build step or `vercel.json` is needed.

1. On [vercel.com/new](https://vercel.com/new), import the `piiinpiiins/led-pixel-editor` GitHub repository
2. Keep Framework Preset as **Other**, leave Build Command and Output Directory empty
3. Click **Deploy** — every later push to `main` redeploys automatically

Or with the Vercel CLI from the repository folder:

```bash
npx vercel --prod
```

### Features

#### Drawing

| Action | Effect |
| --- | --- |
| Left-drag | Paint with the current pen color |
| Right-drag | Erase |
| Shift + drag | Select a rectangular area |
| Ctrl / ⌘ + Z | Undo |
| B / E / I / S | Pen / Eraser / Picker / Select |
| One finger | Paint (phones and tablets) |
| Two-finger drag | Pan the board |
| Pinch | Zoom in and out |

- **Tools**: Pen, Eraser, Picker (pick a color from a lit cell), Select (for touch screens without Shift)
- **Shift**: move the whole image one cell up/down/left/right, wrapping around the edges
- **Invert**: swap lit and unlit cells; newly lit cells use the pen color
- **Undo**: up to 40 steps
- **Clear all**
- **Cell size**: zoom the display from 3 to 24 px; **Fit width** fits the whole board on screen (the default on load)
- **Default artwork**: on first visit the board loads `default-artwork.json`, and **Load default artwork** brings it back at any time; replace that file to change what new visitors see
- **Autosave**: your work is kept in this browser's local storage and restored on reload; use **Save JSON** to move it to another device
- Fast strokes are interpolated, so no cells are skipped

#### Colors

- Every cell stores its own hex color, so one image can contain many colors
- **Screen Pick**: sample any color on screen with the system eyedropper (supported browsers only)
- **Recolor**: set every lit cell to the current pen color in one click
- **Swatches**: lists every color used and its cell count; click a swatch to make it the pen color

#### Patterns

1. Hold Shift and drag on the board to select an area
2. Enter a name and click **Save as pattern**
3. Click **Paste** in the pattern list, then click the board to place the pattern's top-left corner

Unlit cells in a pattern are transparent, so pasting won't overwrite what's underneath. Patterns can be renamed or deleted and are saved into the JSON file.

#### Image to Dot Matrix

1. Click **Import image** and choose a file
2. Drag on the image to set the crop box (drag inside to move, drag corners to resize; locked to 6:1 by default)
3. Adjust the options — the preview updates live
4. Click **Apply to board**

| Option | Description |
| --- | --- |
| Mode | Mono (all cells use the pen color) or Color (keep each cell's original color) |
| Threshold | 0–255; cells brighter than the threshold light up |
| Invert | Dark areas become lit |
| Dither | Floyd–Steinberg dithering to preserve gray levels; recommended for photos |

> With only 1,536 dots, high-contrast artwork with clean lines works best.

#### Export & Load

- **PNG 1×**: 96 × 16 px, one pixel per cell
- **PNG 10×**: 960 × 160 px, handy for previews
- **Save JSON**: dots, colors and patterns
- **Load JSON**: load a previously saved file (96 × 16 only)
- **Load default artwork**: replace the board and pattern list with `default-artwork.json` (the board can be undone; asks first if you have work)

Filenames include a timestamp, e.g. `marquee-96x16-20260904-223058.json` or `marquee-96x16-10x-20260904-223058.png`.

### JSON Format

```jsonc
{
  "width": 96,
  "height": 16,
  "savedAt": "2026-09-04T14:30:58.522Z",  // ISO 8601 timestamp
  "penColor": "#fd5db3",                   // pen color at save time
  "palette": [                             // colors used, sorted by count (desc)
    { "hex": "#fd5db3", "count": 255 }
  ],
  "pixels": [                              // 16 strings of 96 chars: '1' lit, '0' off
    "000000000100000010000000..."
  ],
  "colors": [                              // 16 × 96 array: hex for lit cells, null for off
    [null, null, "#fd5db3", ...]
  ],
  "patterns": [                            // saved patterns
    {
      "name": "heart",
      "w": 7,
      "h": 6,
      "cells": [[null, "#ff4242", ...], ...]   // h × w, hex or null
    }
  ]
}
```

The origin (0, 0) is the top-left corner; cell `(x, y)` maps to `pixels[y][x]` and `colors[y][x]`.

When loading a legacy file without a `colors` field, lit cells fall back to the file's `penColor` (or `color`).

### Files

| File | Description |
| --- | --- |
| `index.html` | The editor, with phone and tablet support (single-file HTML + CSS + JavaScript) |
| `led-pixel-editor-v3_1.html` | Previous desktop-only version, kept for reference |
| `default-artwork.json` | Default artwork loaded on first visit (11 patterns included); can also be opened with **Load JSON** |
