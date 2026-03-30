# 環境設定備忘錄

在新的 Mac 上設定此專案的 Claude Code + Things 3 整合。

## 前置條件

| 項目 | 需求 | 檢查指令 |
|------|------|----------|
| Things 3 | 已安裝，且開啟「Enable Things URLs」（Settings > General） | 開啟 Things 3 確認 |
| Python | 3.12+ | `python3 --version` |
| uv | 最新版 | `uv --version`（安裝：`curl -LsSf https://astral.sh/uv/install.sh \| sh`） |
| Claude Code | 已安裝 | `claude --version` |

## Things 3 MCP Server 安裝

使用 [hald/things-mcp](https://github.com/hald/things-mcp)（22 個 tools，功能最完整）。

不需要手動安裝套件，Claude Code 會透過 `uvx` 自動下載執行。只需設定 MCP 即可。

## Claude Code MCP 設定

在專案目錄下執行：

```bash
claude mcp add things3 -- uvx things-mcp
```

這會將設定寫入 `~/.claude.json`（以 project scope 綁定此專案）。

> **注意**：不要手動寫 `.claude/settings.local.json`，用 `claude mcp add` 指令才能確保設定被正確載入。

## 驗證

```bash
# 1. 確認 MCP server 已連線
claude mcp list
# 應顯示：things3: uvx things-mcp - ✓ Connected

# 2. 啟動 Claude Code，測試指令：
# 請 Claude 「列出 Things 3 Today 的任務」
# 確認能看到 Things 3 中的實際任務
```

## 串接應用場景

| 場景 | 說明 |
|------|------|
| inbox 捕捉 | 在 `inbox/` 寫下想法時，同步建立 Things 3 待辦 |
| 日記反思 | 從 Things 3 拉取今日完成事項，寫入 `journal/` 日記 |
| 健康追蹤 | 在 Things 3 設定每日量測提醒 |
| 思維模型學習 | 用 Things 3 管理待整理的模型清單 |

## 疑難排解

- **MCP 連線失敗**：確認 `uvx things-mcp` 可以在終端機獨立執行
- **Things 3 無回應**：確認已開啟「Enable Things URLs」
- **權限問題**：macOS 可能需要授權 Automation 權限給終端機或 Claude Code
