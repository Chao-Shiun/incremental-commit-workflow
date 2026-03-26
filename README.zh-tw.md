# incremental-commit-workflow

一個紀律強制型的 AI 輔助開發工作流程，確保以邏輯單元為粒度進行 commit，每次 commit 後驗證編譯，並在 commit message 中追溯需求來源。適用於 **Claude Code**、**Codex**、**Cursor**、**Gemini**、**GitHub Copilot**、**OpenCode** 及其他 AI 程式碼代理工具。

## 問題

當 AI 代理實作涉及多個 API 或 Job 的功能時，傾向於將所有變更打包成一個 commit。這使得 code review 困難，commit 歷史難以閱讀，且編譯失敗時更難定位問題。

## 解決方案

此工作流程強制執行：
- **以邏輯單元為 commit 粒度** — 新建項目（repository、service、API）獨立 commit；修改既有邏輯以 API endpoint 或 Job 為單位進行 commit
- **先規劃再寫程式** — 實作前先產出包含需求連結的 commit 計畫表並取得核准
- **每次 commit 後驗證編譯** — 每個 commit 必須編譯成功才能繼續下一個
- **Conventional Commits + 需求連結** — 每個 commit message 都包含描述和對應的需求說明
- **僅 commit 到本地** — 未經人工明確同意，絕不推送到遠端

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

當任務需要修改多個 API、Job 或建立新元件時，此工作流程會啟動。

### Commit 粒度

**情境一：新建項目** — 每個新元件（service、API endpoint、Job）獨立 commit。

**情境二：修改既有邏輯** — 同一個 API endpoint 或 Job 的所有相關變更放在同一個 commit 中（controller + service + repository + DTO 作為一個單元）。

### 工作流程

1. **IDENTIFY（確認）** — 確認 ticket 編號和需求細節
2. **PLAN（規劃）** — 識別受影響的 API/Job，呈現包含需求連結的 commit 計畫表
3. **IMPLEMENT（實作）** — 完成一個邏輯單元（API/Job/新元件）
4. **COMMIT（提交）** — 暫存並 commit，包含需求追溯
5. **BUILD（編譯）** — 執行編譯，必須通過才能繼續
6. **REPEAT（重複）** — 返回 IMPLEMENT 處理下一個單元
7. **COMPLETE（完成）** — 顯示 commit 歷史摘要，等待是否推送的指示

### Commit 計畫範例

| # | 類型 | 範圍 | 情境 | 單元 | 描述 | 需求連結 |
|---|------|------|------|------|------|----------|
| 1 | feat | PT-1234 | New | GameListService | 建立遊戲列表 API 的 service | 需求 2.1：提供遊戲列表端點 |
| 2 | feat | PT-1234 | New | GET /api/games | 建立遊戲列表 API endpoint | 需求 2.1：提供遊戲列表端點 |
| 3 | feat | PT-1234 | Modify | GET /api/users | 在使用者個人資料 API 中新增遊戲數量欄位 | 需求 2.3：在個人資料中顯示遊戲數量 |

### Commit Message 格式

```
<type>(<ticket-number>): <description>

<對應的需求說明和修改原因>
```

**類型：** `feat`、`fix`、`refactor`、`test`、`docs`、`chore`、`perf`、`style`

**範例：**

```
feat(PT-1234): add game count field to user profile API

Requirement 2.3 specifies that user profiles must show the number of
games owned. Modified controller, service, and DTO together as one
cohesive change to GET /api/users.
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

本專案採用 MIT 授權。詳細內容請參考 `LICENSE`。
