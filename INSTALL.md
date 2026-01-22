# 安裝指南

## 快速安裝

### 方式一：Git Clone

```bash
# 進入你的專案目錄
cd your-project

# 建立 .agent/skills 目錄
mkdir -p .agent/skills

# Clone 此 repo
git clone https://github.com/YOUR_USERNAME/webdev-skills.git .agent/skills/webdev
```

### 方式二：Git Submodule (推薦用於團隊)

```bash
# 新增為 submodule
git submodule add https://github.com/YOUR_USERNAME/webdev-skills.git .agent/skills/webdev

# 團隊成員需執行
git submodule update --init --recursive
```

### 方式三：直接複製

1. 下載此 repo 的 ZIP
2. 解壓縮
3. 複製整個目錄到 `.agent/skills/webdev/`

## 目錄結構

安裝後你的專案應該像這樣：

```text
your-project/
├── .agent/
│   └── skills/
│       └── webdev/          # ← 此 repo
│           ├── SKILL.md
│           ├── README.md
│           ├── workflows/   # ← 內含完整工作流程
│           │   └── build-website.md
│           └── ...
├── src/
├── package.json
└── ...
```

> **注意**：workflows 已包含在 webdev 目錄中，無需額外安裝。

## Antigravity 用戶專用

Antigravity 有自動偵測 `.agent/workflows/` 的機制，如果你想使用 `/build-website` 指令，需要額外設定：

```bash
# 方式一：建立 symlink（推薦）
mkdir -p .agent/workflows
# Windows
mklink /D .agent\workflows\build-website.md .agent\skills\webdev\workflows\build-website.md
# macOS/Linux
ln -s .agent/skills/webdev/workflows/build-website.md .agent/workflows/build-website.md

# 方式二：直接複製
cp .agent/skills/webdev/workflows/build-website.md .agent/workflows/
```

> 其他 AI 工具（Cursor、Windsurf、Claude 等）不需要此步驟，直接引用 `@.agent/skills/webdev/workflows/build-website.md` 即可。

## 驗證安裝

在你的 AI 工具中測試：

### Cursor / Windsurf

```text
@.agent/skills/webdev/SKILL.md 這個 skill 有什麼功能？
```

### VS Code + Copilot

```text
#file:.agent/skills/webdev/SKILL.md 這個 skill 有什麼功能？
```

如果 AI 能正確描述功能，表示安裝成功！

## 更新

### Git Clone

```bash
cd .agent/skills/webdev
git pull origin main
```

### Submodule

```bash
git submodule update --remote .agent/skills/webdev
```

## 問題排解

### AI 找不到檔案

- 確認路徑是否正確：`.agent/skills/webdev/`
- 確認檔案是否存在：`ls .agent/skills/webdev/SKILL.md`
- 不同 AI 工具的路徑引用格式不同，參考 [ai-tools-guide.md](ai-tools-guide.md)

### Token 超出限制

使用精簡模式：

```text
@.agent/skills/webdev/compact.md
```
