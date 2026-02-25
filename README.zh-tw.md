# incremental-commit-workflow

一個紀律強制型的 Claude Code Skill，確保以方法/函式為粒度進行 commit，並在每個 commit message 中清楚說明「做了什麼」和「為什麼」。

## 問題

當 AI 代理實作涉及多個方法的功能時，傾向於將所有變更打包成一個 commit。這使得 code review 困難，commit 歷史也變得難以閱讀。

## 解決方案

此 skill 強制執行：
- **一個方法，一個 commit** - 每個 commit 只修改一個方法/函式
- **先規劃再寫程式** - 實作前先產出 commit 計畫表並取得核准
- **Conventional Commits + why** - 每個 commit message 都包含描述和修改原因
- **僅 commit 到本地** - 未經人工明確同意，絕不推送到遠端

## 安裝

將 `incremental-commit-workflow` 目錄複製到 Claude Code skills 資料夾：

```bash
# 複製儲存庫
git clone https://github.com/Chao-Shiun/incremental-commit-workflow.git

# 複製到 Claude Code skills 目錄
cp -r incremental-commit-workflow ~/.claude/skills/
```

或手動建立 `~/.claude/skills/incremental-commit-workflow/SKILL.md` 並放入 skill 內容。

## 使用方式

當 Claude Code 偵測到需要修改多個方法或函式的任務時，此 skill 會自動啟動。

### 工作流程

1. **PLAN（規劃）** - Claude 分析任務、列出要修改的方法，並呈現 commit 計畫表
2. **IMPLEMENT（實作）** - 一次修改一個方法，完成後立即 commit
3. **COMPLETE（完成）** - 顯示 commit 歷史摘要，等待是否推送的指示

### Commit 計畫範例

| # | 類型 | 範圍 | 方法/函式 | 描述 | 原因 |
|---|------|------|----------|------|------|
| 1 | feat | UserService | ValidateInput() | 新增電子郵件格式驗證 | 目前的實作接受格式錯誤的電子郵件，導致下游處理失敗 |
| 2 | feat | UserService | CreateUser() | 新增插入前的重複檢查 | 使用者可以用同一個電子郵件註冊兩次，違反商業規則 |
| 3 | test | UserService | ValidateInput_Email | 新增電子郵件驗證單元測試 | 驗證 RFC 5322 合規性和邊界條件 |

### Commit Message 格式

```
<type>(<scope>): <description>

<why - 說明為什麼需要這個修改>
```

**類型：** `feat`、`fix`、`refactor`、`test`、`docs`、`chore`、`perf`、`style`

**範例：**

```
feat(GameListService): add Redis cache for GetGameList method

Reduce database load during peak hours. Current implementation queries
DB on every request, causing latency spikes with concurrent users.
```

## 需求

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) CLI

## 授權

MIT
