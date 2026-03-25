---
name: save-progress
description: 當使用者說「儲存目前進度」、「存檔」、「save progress」時，自動提交所有變更並推送到遠端。
user-invocable: true
allowed-tools: Bash(git *), Bash(cd:*)
argument-hint:
---

請執行以下步驟來儲存目前的工作進度：

1. 先執行 `git status` 查看目前所有變更（未追蹤檔案和已修改檔案）
2. 執行 `git diff` 和 `git diff --staged` 查看具體變更內容
3. 根據變更內容，撰寫一個有意義的繁體中文 commit message：
   - 簡短描述這次變更的主要內容（一行）
   - 如果變更較多，加上詳細說明
4. 使用 `git add -A` 加入所有變更
5. 建立 commit（使用你撰寫的 commit message）
6. 執行 `git push origin $(git branch --show-current)` 推送到遠端

注意事項：
- 如果沒有任何變更，告知使用者「目前沒有需要儲存的變更」
- 如果發現 .env 或含有敏感資訊的檔案，警告使用者並排除這些檔案
- commit message 結尾加上 `Co-Authored-By: Claude Opus 4.6 (1M context) <noreply@anthropic.com>`
- 如果 push 失敗，告知使用者原因並建議解決方式
