---
name: journal-sync
description: 雙向同步 Heptabase Journal 與本地 journal 檔案。當使用者說「同步日誌」、「sync journal」、「拉 Heptabase journal」、「journal-sync」或任何涉及 Heptabase 與本地 journal 同步的請求時觸發。
user-invocable: true
allowed-tools: mcp__heptabase-mcp__get_journal_range, mcp__heptabase-mcp__append_to_journal, Read, Write, Edit, Glob, Bash(mkdir *)
argument-hint: "[startDate] [endDate] — 日期格式 YYYY-MM-DD，預設最近 7 天到今天"
---

# Journal 雙向同步

將 Heptabase Journal 與本地 `journal/` 目錄進行雙向同步：
- Heptabase → 本地：拉取 Heptabase 內容，寫入本地檔案的 `## Heptabase Journal` 區塊
- 本地 → Heptabase：將本地獨有的內容（非 `## Heptabase Journal` 區塊）推送到 Heptabase

## 參數解析

- 如果使用者提供兩個日期參數，使用它們作為 startDate 和 endDate
- 如果只提供一個日期，以該日期為 startDate，今天為 endDate
- 如果沒有提供參數，預設 startDate = 7 天前，endDate = 今天
- 日期格式：YYYY-MM-DD

## 執行步驟

### 1. 取得 Heptabase Journal

使用 `mcp__heptabase-mcp__get_journal_range` 取得指定日期範圍的所有 Journal。
注意：每次最多 92 天，超過需分批呼叫。

### 2. 掃描本地 Journal 檔案

使用 Glob 搜尋 `journal/YYYY/MM/YYYY-MM-DD.md` 格式的檔案，找出日期範圍內已存在的本地 Journal。

### 3. 比對並執行同步

對每個日期，判斷屬於以下哪種情況並執行對應動作：

#### 情況 A：Heptabase 有、本地沒有 → 建立新本地檔案

建立必要的目錄（如 `journal/2026/04/`），然後建立檔案：

```markdown
---
date: YYYY-MM-DD
---

# YYYY-MM-DD

## Heptabase Journal

（Heptabase 的內容）
```

#### 情況 B：兩邊都有、本地沒有 `## Heptabase Journal` 區塊 → 追加到本地

在本地檔案末尾追加：

```markdown

## Heptabase Journal

（Heptabase 的內容）
```

#### 情況 C：兩邊都有、本地已有 `## Heptabase Journal` 區塊 → 檢查本地獨有內容

讀取本地檔案，提取 `## Heptabase Journal` 之前的所有內容（排除 YAML front matter 和 `# 日期` 標題行）。如果有實質內容（不只是空的模板佔位符如 `<!-- 待補充 -->`），則使用 `mcp__heptabase-mcp__append_to_journal` 將這些本地獨有內容推送到 Heptabase。

判斷「有實質內容」的方式：去掉 front matter、標題、`## Heptabase Journal` 區塊及其以下內容後，剩餘文字（排除空行和 HTML 註解）超過 10 個字元。

#### 情況 D：本地有、Heptabase 沒有 → 推送到 Heptabase

讀取本地檔案的全部內容（排除 YAML front matter 和 `# 日期` 標題行），使用 `mcp__heptabase-mcp__append_to_journal` 推送到 Heptabase。

### 4. 報告結果

完成後列出摘要表格：

| 日期 | 動作 | 說明 |
|------|------|------|
| YYYY-MM-DD | 新建本地 / 追加 Heptabase 內容 / 推送到 Heptabase / 已同步（無需動作） | 簡短描述 |

## 注意事項

- 所有溝通使用繁體中文
- 同步是追加式的，不會覆蓋或刪除任何已存在的內容
- 如果某個日期兩邊的內容完全一致（已經同步過），跳過該日期
- 本地 journal 的根目錄是工作目錄下的 `journal/`
