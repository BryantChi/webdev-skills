---
id: installation_guide
name: installation_guide
description: 2026 最新版安裝指南，涵蓋全域安裝（CLI）與專案級安裝（IDE）
---

# 📦 安裝指南 (Installation Guide)

本 Skills 系統採用 **Agent Native** 架構，支援全域安裝（CLI 工具）和專案級安裝（IDE 工具）。

---

## 🚀 方式一：全域安裝（CLI 工具推薦）

安裝到 AI 工具的全域 skills 目錄，所有專案皆可自動使用。

```bash
# Claude Code CLI
git clone https://github.com/BryantChi/webdev-skills.git ~/.claude/skills/webdev_skills

# Codex CLI
git clone https://github.com/BryantChi/webdev-skills.git ~/.codex/skills/webdev_skills

# Gemini CLI
git clone https://github.com/BryantChi/webdev-skills.git ~/.gemini/skills/webdev_skills

# OpenCode CLI
git clone https://github.com/BryantChi/webdev-skills.git ~/.opencode/skills/webdev_skills
```

> **命名規範**：目錄名必須使用 `webdev_skills`（snake_case 英文），不可使用中文或空格。

---

## 🚀 方式二：專案級安裝（IDE 工具推薦）

### Git Submodule (推薦用於團隊)

```bash
mkdir -p .agent/skills
git submodule add https://github.com/BryantChi/webdev-skills.git .agent/skills/webdev
git submodule update --init --recursive
```

### Git Clone (適用於個人專案)

```bash
mkdir -p .agent/skills
git clone https://github.com/BryantChi/webdev-skills.git .agent/skills/webdev
```

### 手動下載

下載 ZIP 並解壓到專案根目錄的 `.agent/skills/webdev/`。

---

## 📂 安裝後目錄結構

### 全域安裝（CLI）

```text
~/.claude/skills/          # 或 ~/.codex/ ~/.gemini/ ~/.opencode/
├── webdev_skills/         # ← 本系統
│   ├── SKILL.md           # SOP 入口
│   ├── logic/             # 推理引擎
│   ├── ui/                # 設計系統
│   └── ...
├── coding_style_conventions/  # 其他 skill 範例
└── ...
```

### 專案級安裝（IDE）

```text
my-project/
├── .agent/
│   └── skills/
│       └── webdev/        # ← 本系統
│           ├── SKILL.md
│           ├── logic/
│           ├── ui/
│           └── ...
├── src/
└── package.json
```

---

## ⚙️ 環境設置 (Optional)

### Antigravity / Gemini IDE

啟用 `/build-website` 斜線指令：

```bash
mkdir -p .agent/workflows
cp .agent/skills/webdev/workflows/build-website.md .agent/workflows/
```

### Cursor 規則

建立 `.cursor/rules/webdev.mdc`：

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

### Windsurf 規則

建立 `.windsurf/rules/global.md`：

```markdown
當涉及網頁開發時，請優先參考 .agent/skills/webdev/SKILL.md 的標準作業程序。
```

---

## ✅ 驗證安裝

### CLI 工具

直接對話測試：

```text
幫我建立一個網站
```

AI 應自動觸發 SOP Phase 1（智慧推理）。

### IDE 工具

```text
讀取 .agent/skills/webdev/SKILL.md 並告訴我 SOP 的 Phase 1 是什麼？
```

---

## 🔄 更新

```bash
# 全域安裝
cd ~/.claude/skills/webdev_skills && git pull

# 專案 Submodule
git submodule update --remote .agent/skills/webdev

# 專案 Clone
cd .agent/skills/webdev && git pull
```

---

## 📚 安裝後第一步

| 用途 | 起點 |
|------|------|
| 完整建站 | 對 AI 說「幫我建立網站」→ 走 [`workflows/build-website.md`](workflows/build-website.md) 15 階段 |
| 改色 / 微調 | [`compact.md`](compact.md) + 對應 ui/styles |
| 加效能 / 測試 / SEO | [`_shared/load-paths.md`](_shared/load-paths.md) 對應任務的最小檔案集 |
| AI 工具設定 | [`ai-tools-guide.md`](ai-tools-guide.md) |
| 業界對照 | [README 業界對照表](README.md) |

> **建議**：第一次使用前，建議讀 [`SKILL.md`](SKILL.md) + [`compact.md`](compact.md) + [`_shared/load-paths.md`](_shared/load-paths.md) 三檔（合計 ~1500 tokens），就能掌握全庫導航。
