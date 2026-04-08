# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 專案概述

個人思維模型知識庫，記錄日常思考以及學習到的思考模型。思考模型主要來自萬維鋼・得到 App《現代思維工具 100 講》，輔以每日日記反思。純 Markdown 知識管理系統，無建置或測試流程。

## 架構

- `models/` — 思維模型筆記，依主題分類（基本世界觀、成長戰略、決策判斷等）
- `models/_index.md` — 所有模型的總索引，新增模型後須同步更新
- `journal/YYYY/MM/YYYY-MM-DD.md` — 每日日記與反思
- `inbox/` — 快速捕捉的草稿，待整理至 models/ 或 journal/

## 新增內容的流程

### 新增思維模型
1. 複製 `models/_template.md` 到對應分類資料夾，以模型名稱命名
2. 填寫 frontmatter（title, source, tags, date）與各區段內容
3. 在 `models/_index.md` 對應分類下新增連結

### 新增日記
1. 在 `journal/YYYY/MM/` 下建立 `YYYY-MM-DD.md`
2. 依照 `journal/_template.md` 格式撰寫

### 快速捕捉
1. 在 `inbox/` 下建立 `.md` 檔，依照 `inbox/_template.md` 格式

## 寫作慣例

- 所有內容使用繁體中文（台灣）
- Markdown 檔案使用 YAML frontmatter（title, source, tags, date）
- 模型之間透過相對路徑互相連結（見 `相關模型` 區段）
- 主要來源標記為 `萬維鋼・得到`

## LLM Wiki 操作

參考 [Karpathy LLM Wiki](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) 模式，本專案支援三個核心操作：

### Ingest（`/ingest`）
將筆記或素材整理成結構化模型。使用者提供口述、草稿或 raw material，Claude 自動建立模型檔、更新索引、補上交叉連結、記錄到 log.md。

### Query（`/query`）
搜尋知識庫回答問題。搜尋範圍涵蓋 models/、journal/、inbox/、whiteboard/，回答時引用來源檔案。

### Lint（`/lint`）
知識庫健檢。掃描孤立頁面、空白區段、索引不一致、交叉連結建議、frontmatter 完整性，產出結構化報告。

### log.md
Append-only 時間序紀錄，記錄所有 ingest/query/lint 操作歷程。格式：`## [YYYY-MM-DD] 操作類型 | 標題`

## 自動化

- SessionStart hook 會自動 `git pull --ff-only` 同步最新內容
