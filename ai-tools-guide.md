---
name: AI Tools Guide
description: 2026 最新版：如何在各種 AI 編程助手與 CLI 工具中載入和使用此 Skills 系統
---

# 🤖 AI 工具使用指南 (2026 Edition)

> 本 Skills 系統設計為 **AI Native**，相容於所有支援 Context Loading 的 AI 編程助手與 CLI 代理。

---

## 📋 相容性總覽

| AI 工具 | 類型 | 相容性 | 最佳載入方式 | 推薦指數 |
| ------- | ---- | ------ | ------------ | -------- |
| **Claude Code** | CLI | ✅ Native | `~/.claude/skills/webdev_skills/` | ⭐⭐⭐⭐⭐ |
| **Codex CLI** | CLI | ✅ Native | `~/.codex/skills/webdev_skills/` | ⭐⭐⭐⭐⭐ |
| **Gemini CLI** | CLI | ✅ Native | `~/.gemini/skills/webdev_skills/` | ⭐⭐⭐⭐⭐ |
| **OpenCode CLI** | CLI | ✅ Native | `~/.opencode/skills/webdev_skills/` | ⭐⭐⭐⭐⭐ |
| **Antigravity** | Agent IDE | ✅ Native | 自動讀取 `.agent/skills/` | ⭐⭐⭐⭐⭐ |
| **Cursor** | IDE (Fork) | ✅ 優化 | `.cursor/rules` | ⭐⭐⭐⭐⭐ |
| **Windsurf** | IDE (Fork) | ✅ 優化 | `.windsurf/rules` | ⭐⭐⭐⭐⭐ |
| **Roo Code** | VS Code Ext | ✅ 優化 | MCP / Custom Modes | ⭐⭐⭐⭐⭐ |
| **Goose** | CLI Agent | ✅ 優化 | `--instruction-file` | ⭐⭐⭐⭐⭐ |
| **Aider** | CLI Chat | ✅ 相容 | `/read` 或 `/add` | ⭐⭐⭐⭐ |
| **Copilot** | VS Code Ext | ✅ 相容 | `#file:` 引用 | ⭐⭐⭐⭐ |

---

## ⚡ 核心載入策略

無論使用何種工具，核心邏輯都是：**「將標準作業程序 (SOP) 注入 AI 的 Context」**。

### 三種層級的載入模式：

| 模式 | 載入檔案 | 消耗 Tokens | 適用場景 |
| ---- | -------- | ----------- | -------- |
| **🟢 SOP 啟動** | `SKILL.md` | ~800 | **預設推薦**：啟動標準流程，AI 會按需讀取其他檔案 |
| **🟡 精簡模式** | `compact.md` | ~2,000 | **Token 受限時**：包含基本原則與樣式，無複雜邏輯 |
| **🔴 全量載入** | 整個目錄 | ~50,000+ | **高階 Agent**：如 Roo Code / Goose，具備長上下文能力 |

---

## 1️⃣ IDE 整合 (GUI)

### Antigravity (Gemini IDE)
> **原生支援**：無需設定，自動偵測 `.agent/skills`。

- **啟動**：直接說「幫我建立網站」或 `/build-website`。
- **優勢**：自動執行 Reasoning Engine，無需手動引導。

### Cursor
> **設定 Rules**：在 `.cursor/rules/webdev.mdc` 加入規則。

```markdown
---
description: Web Development SOP & Standards
globs: ["**/*.tsx", "**/*.ts", "**/*.css"]
---
# 網站開發標準
當用戶要求「建立網站」時：
1. 讀取 @.agent/skills/webdev/SKILL.md
2. 遵循 SOP Phase 1-4 流程
3. 必須遵守 @.agent/skills/webdev/ui/no-ai-feel.md
```

- **使用**：`Ctrl+L` 開啟 chat，輸入「幫我建立網站」。

### Windsurf (Cascade)
> **設定 Rules**：在 `.windsurf/rules/global.md` 或專案設定中。

```markdown
當涉及網頁開發時，請優先參考 .agent/skills/webdev/SKILL.md 的標準作業程序。
```

- **使用**：Cascade 會自動索引該目錄，直接對話即可。

### Roo Code (VS Code Extension)
> **強大的 Agent 擴充**，支援 MCP (Model Context Protocol)。

- **設定**：將 Skills 目錄加入 Context。
- **Custom Mode**：編輯 `.roo/modes/webdev.json` (若有) 或直接在 Prompt 預設指令中加入：
  ```text
  You are an expert web developer. Always follow the SOP strictly defined in .agent/skills/webdev/SKILL.md.
  Before writing code, execute the Reasoning Phase.
  ```

---

## 2️⃣ AI CLI 工具 (Command Line Interface)

### Claude Code CLI (Anthropic)
> 全域安裝後自動識別，無需額外載入。

```bash
# 安裝（一次性）
git clone https://github.com/BryantChi/webdev-skills.git ~/.claude/skills/webdev_skills

# 使用：直接對話
claude
> 幫我建立一個 Next.js 電商網站
```

Claude Code 會自動載入 `webdev_skills` skill 並執行 SOP。

### Codex CLI (OpenAI)
> 全域安裝後自動識別。

```bash
# 安裝（一次性）
git clone https://github.com/BryantChi/webdev-skills.git ~/.codex/skills/webdev_skills

# 使用
codex
> 幫我建立一個網站
```

### Gemini CLI (Google)
> 全域安裝後自動識別。

```bash
# 安裝（一次性）
git clone https://github.com/BryantChi/webdev-skills.git ~/.gemini/skills/webdev_skills

# 使用
gemini
> 幫我建立一個網站
```

### OpenCode CLI
> 全域安裝後自動識別。

```bash
# 安裝（一次性）
git clone https://github.com/BryantChi/webdev-skills.git ~/.opencode/skills/webdev_skills

# 使用
opencode start --plan ~/.opencode/skills/webdev_skills/SKILL.md
```

**指令技巧：**

- **自動執行 SOP**：OpenCode 會自動解析 markdown 中的步驟。
- **專案地圖**：`opencode map` 可生成專案結構，搭配 `logic/reasoning.md` 使用效果極佳。

---

## 3️⃣ Prompt Loading 技巧

若你使用的是網頁版 (ChatGPT / Claude.ai / Gemini Advanced) 或不支援本地檔案的工具，請使用 **Prompt Loading**。

### Universal Kick-off Prompt (通用啟動提示詞)

複製以下內容貼上：

```markdown
# Role
你是一個精通現代網頁開發的 AI 代理。

# Context
我將提供一套「標準作業程序 (SOP)」與「設計規範」。
請仔細閱讀並在接下來的開發中嚴格遵守，特別是「去 AI 感」的設計原則。

# SOP (Standard Operating Procedure)
[請在此處貼上 SKILL.md 的內容]

# Rule: No AI Feel
[請在此處貼上 ui/no-ai-feel.md 的內容]

# Action
現在，請執行 SOP Phase 1：智慧推理。
我想建立一個 [你的專案描述，例如：極簡風格的個人部落格]。
```

---

## 🔄 常見問題 (FAQ)

### Q: 為什麼 CLI 工具讀不到檔案？
- **路徑問題**：請確保你在專案的 **根目錄** 執行指令。
- **權限問題**：Goose 或 Aider 需要讀寫權限。

### Q: 哪個工具最適合這個 Skills 系統？
1. **Antigravity**：原生整合，體驗最流暢。
2. **Cursor / Windsurf**：適合喜歡 IDE介面 的開發者。
3. **Goose / Roo Code**：適合喜歡 **全自動 Agent** 模式的開發者。

### Q: Token 爆炸怎麼辦？
- 使用 **精簡模式** (`compact.md`)。
- 在 Aider 中使用 `/drop` 移除不需的檔案。
- 在 Roo Code 中使用 `.rooignore` 排除非核心目錄。
