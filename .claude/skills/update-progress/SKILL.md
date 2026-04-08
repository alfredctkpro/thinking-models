---
name: update-progress
description: 從遠端 Repository 拉取最新資料。當使用者說「更新進度」、「update progress」、「拉最新的」、「pull」時觸發。
user-invocable: true
allowed-tools: Bash(git fetch:*), Bash(git pull:*), Bash(git log:*), Bash(git status)
argument-hint:
---

請執行以下步驟來更新本地資料：

1. 執行 `git fetch origin` 取得遠端最新狀態
2. 執行 `git status` 確認目前分支與遠端的差異
3. 如果有新的 commit，執行 `git pull --ff-only origin $(git branch --show-current)` 拉回最新資料
4. 執行 `git log --oneline -5` 顯示最近 5 筆 commit，讓使用者知道目前的進度

注意事項：
- 使用 `--ff-only` 避免產生不必要的 merge commit
- 如果本地有未提交的變更導致 pull 失敗，告知使用者並建議先使用 `/save-progress` 儲存變更
- 以繁體中文回報更新結果
