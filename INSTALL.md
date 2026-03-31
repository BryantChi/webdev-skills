---
name: 安裝指南
description: 2026 最新版安裝指南 - 適用於任何專案與 AI 工具
---

# 📦 安裝指南 (Installation Guide)

本 Skills 系統採用 **Agent Native** 架構，只需將檔案放置於專案特定目錄，即可被大多數 AI 代理自動識別。

---

## 🚀 快速安裝

### 方式一：Git Submodule (推薦用於團隊) ✅

這是 2026 年最標準的 Skills 管理方式，讓 Skill 版本隨專案更新。

```bash
# 確保目錄存在
mkdir -p .agent/skills

# 新增 Submodule (請替換為實際 Repo URL)
git submodule add https://github.com/BryantChi/webdev-skills.git .agent/skills/webdev

# 初始化
git submodule update --init --recursive
```

### 方式二：Git Clone (適用於個人專案)

```bash
mkdir -p .agent/skills
git clone https://github.com/BryantChi/webdev-skills.git .agent/skills/webdev
```

### 方式三：手動下載

1. 下載 ZIP 檔案。
2. 解壓縮到專案根目錄的 `.agent/skills/webdev/`。

---

## 📂 標準目錄結構

安裝完成後，你的專案結構應如下所示（這是 Agent 自動發現的標準結構）：

```text
my-project/
├── .agent/
│   ├── skills/
│   │   └── webdev/          # [本系統核心]
│   │       ├── SKILL.md     # SOP 入口
│   │       ├── logic/       # 推理引擎
│   │       ├── ui/          # 設計系統
│   │       └── ...
│   └── workflows/           # (選擇性) 工作流程捷徑
│       └── build-website.md # 方便直接呼叫
├── src/
├── package.json
└── ...
```

---

## ⚙️ 環境設置 (Optional)

### 1. Antigravity / Gemini IDE 設定

為了啟用 `/build-website` 斜線指令，建議將 workflow 複製到頂層：

```bash
# Windows (PowerShell)
mkdir -p .agent/workflows
Copy-Item .agent/skills/webdev/workflows/build-website.md .agent/workflows/

# macOS / Linux
mkdir -p .agent/workflows
cp .agent/skills/webdev/workflows/build-website.md .agent/workflows/
```

### 2. Cursor / Windsurf 規則設定

為了讓 AI 自動遵守規範，建議建立全域規則 (.cursor/rules 或 .windsurf/rules)：

**建立 `.cursor/rules/webdev.mdc`：**

```markdown
---
description: Web Development Standards
globs: ["**/*.{ts,tsx,js,jsx,vue,html,css}"]
---
# Webdev Agent Skill
當涉及網頁開發時：
1. 必須參考 @.agent/skills/webdev/SKILL.md
2. 必須遵守 @.agent/skills/webdev/ui/no-ai-feel.md
```

---

## ✅ 驗證安裝

在你的 AI 工具中輸入以下指令測試：

**通用 Prompt:**
```text
讀取 .agent/skills/webdev/SKILL.md 並告訴我 SOP 的 Phase 1 是什麼？
```

**Antigravity:**
```text
/build-website (如果你有設定 workflow)
或
使用 webdev skill 建立網站
```

**OpenCode CLI:**
```bash
opencode map .agent/skills/webdev
```

若 AI 能正確回答「智慧推理 (Reasoning)」，則安裝成功！🎉

---

## 🔄 更新與維護

保持 Skill 在最新狀態：

```bash
# 若使用 Submodule
git submodule update --remote .agent/skills/webdev

# 若使用 Clone
cd .agent/skills/webdev && git pull
```
