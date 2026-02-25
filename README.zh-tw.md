# incremental-commit-workflow

一個紀律強制型的 AI 輔助開發工作流程，確保以方法/函式為粒度進行 commit，並在每個 commit message 中清楚說明「做了什麼」和「為什麼」。適用於 **Claude Code**、**Codex**、**Cursor**、**Gemini**、**GitHub Copilot**、**OpenCode** 及其他 AI 程式碼代理工具。

## 問題

當 AI 代理實作涉及多個方法的功能時，傾向於將所有變更打包成一個 commit。這使得 code review 困難，commit 歷史也變得難以閱讀。

## 解決方案

此工作流程強制執行：
- **一個方法，一個 commit** - 每個 commit 只修改一個方法/函式
- **先規劃再寫程式** - 實作前先產出 commit 計畫表並取得核准
- **Conventional Commits + why** - 每個 commit message 都包含描述和修改原因
- **僅 commit 到本地** - 未經人工明確同意，絕不推送到遠端

## 安裝

### Claude Code

將目錄複製到 Claude Code skills 資料夾：

```bash
git clone https://github.com/Chao-Shiun/incremental-commit-workflow.git
cp -r incremental-commit-workflow ~/.claude/skills/
```

或手動建立 `~/.claude/skills/incremental-commit-workflow/SKILL.md` 並放入 skill 內容。

### Cursor

將工作流程規則加入 Cursor 規則檔（`.cursor/rules/incremental-commit.mdc`），或將 `SKILL.md` 的內容貼入 **Cursor Settings > Rules for AI**。

### GitHub Copilot

將工作流程指引加入儲存庫的 `.github/copilot-instructions.md`，或在使用 Copilot Chat 時將其包含在提示中。

### Codex (OpenAI)

將工作流程指引加入 `AGENTS.md` 或 Codex 使用的系統提示設定檔。

### Gemini (Google)

將工作流程規則貼入 Gemini 的提示前綴，或加入專案的 AI 指引檔案（例如 `.gemini/style-guide.md`）。

### OpenCode

將工作流程指引加入 OpenCode 的 `AGENTS.md` 或系統提示設定中。

### 其他 AI 工具

核心工作流程定義在 `SKILL.md` 中。將其內容複製到你的 AI 工具所支援的系統提示、規則檔或指引機制中。

## 使用方式

當任務需要修改多個方法或函式時，此工作流程會啟動。

### 工作流程

1. **PLAN（規劃）** - AI 分析任務、列出要修改的方法，並呈現 commit 計畫表
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

## 相容性

| AI 工具 | 整合方式 |
|---------|---------|
| Claude Code | `~/.claude/skills/` 目錄（自動探索） |
| Cursor | `.cursor/rules/*.mdc` 或 Settings > Rules for AI |
| GitHub Copilot | `.github/copilot-instructions.md` 或 Copilot Chat 提示 |
| Codex (OpenAI) | `AGENTS.md` 或系統提示設定檔 |
| Gemini (Google) | 提示前綴或專案 AI 指引檔案 |
| OpenCode | `AGENTS.md` 或系統提示設定 |
| 其他 | 將 `SKILL.md` 內容複製到系統提示或規則檔 |

## 授權

MIT
