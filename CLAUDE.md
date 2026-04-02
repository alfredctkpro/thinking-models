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

## 自動化

- SessionStart hook 會自動 `git pull --ff-only` 同步最新內容
