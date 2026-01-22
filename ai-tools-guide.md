---
name: AI 工具使用指南
description: 如何在各種 AI 編程助手中加載和使用此 Skills 系統
---

# 🤖 AI 工具使用指南

> 本 Skills 系統可用於任何支援讀取本地檔案的 AI 編程助手

---

## 📋 相容性總覽

| AI 工具 | 相容性 | 載入方式 | 推薦指數 |
| ------- | ------ | -------- | -------- |
| **Antigravity (Gemini)** | ✅ 完全相容 | 自動讀取 Skills | ⭐⭐⭐⭐⭐ |
| **Cursor** | ✅ 完全相容 | Rules + @Files | ⭐⭐⭐⭐⭐ |
| **Windsurf (Codeium)** | ✅ 完全相容 | Rules + @Files | ⭐⭐⭐⭐⭐ |
| **VS Code + Copilot** | ✅ 相容 | #file 引用 | ⭐⭐⭐⭐ |
| **VS Code + Continue** | ✅ 相容 | @codebase | ⭐⭐⭐⭐ |
| **Claude (API/網頁)** | ✅ 相容 | Projects/貼上 | ⭐⭐⭐⭐ |
| **Aider CLI** | ✅ 相容 | /add 指令 | ⭐⭐⭐⭐ |
| **Claude CLI** | ✅ 相容 | 自動讀取 | ⭐⭐⭐⭐ |
| **ChatGPT** | ⚠️ 部分 | 手動貼上/GPTs | ⭐⭐⭐ |
| **Gemini 網頁版** | ⚠️ 部分 | Gems/貼上 | ⭐⭐⭐ |

---

## ⚡ Token 優化指南

不同 AI 工具有不同的 token 限制，本系統提供多種載入模式：

### Token 消耗對比

| 模式 | 估計 Tokens | 載入檔案 | 適用場景 |
| ---- | ----------- | -------- | -------- |
| **精簡模式** | ~2,000 | `compact.md` | Token 受限、快速開發 |
| **單一風格** | ~3,000 | `compact.md` + 風格檔 | 已知風格 |
| **完整精靈** | ~8,000 | SKILL + Wizard | 需要引導選擇 |
| **全部載入** | ~50,000+ | 所有檔案 | ❌ 不建議 |

### 精簡模式（省 Token）⚡

只需載入一個檔案，涵蓋核心規範：

```text
讀取 .agent/skills/webdev/compact.md
```

包含：
- 去 AI 感核心原則
- 9 種常用風格速查
- 技術快速選擇表
- CSS 設計 Token
- 核心元件模板
- 精簡上線檢查

### 風格色碼速查

需要快速查詢所有 55 種風格的配色：

```text
讀取 .agent/skills/webdev/ui/styles/colors.md
```

一張表涵蓋所有風格的 Primary/Secondary/Background 色碼

### 依 AI 工具選擇模式

| AI 工具 | 建議模式 | 原因 |
| ------- | -------- | ---- |
| ChatGPT 免費版 | 精簡模式 | Token 限制 4K |
| ChatGPT Plus | 單一風格模式 | Token 限制 32K |
| Claude Free | 精簡模式 | 對話長度限制 |
| Claude Pro | 完整流程 | Token 充裕 |
| Cursor/Windsurf | 完整流程 | 支援大量檔案 |
| Antigravity | 完整流程 | 自動管理 |
| Aider | 按需載入 | 手動控制 |

### 載入範例

#### ChatGPT / Token 受限場景：

```text
# 方式 1：貼上精簡版
[複製 compact.md 的內容貼上]

幫我用 Next.js + 玻璃擬態風格建立網站

# 方式 2：只查配色
我需要「北歐風」的配色
Primary: #2c2c2c, Secondary: #c4a77d, BG: #faf9f6
```

#### Cursor / Windsurf：

```text
# 精簡模式
@.agent/skills/webdev/compact.md 幫我建立網站

# 完整模式
@.agent/skills/webdev/SKILL.md 幫我建立網站
```

#### Claude Projects：

上傳檔案時選擇：
- **Token 受限**：只上傳 `compact.md` + `colors.md`
- **Token 充裕**：上傳整個 `.agent/skills/webdev/` 目錄

### Token 節省技巧

1. **先用精簡模式開始**，需要細節再追加
2. **使用 colors.md** 快速取色，不需載入完整風格檔
3. **分階段引用**，完成一個階段再載入下一個
4. **複用 CSS 變數**，compact.md 中的 Token 可直接複製使用

## 1️⃣ Antigravity (Gemini IDE 整合)

> 你目前正在使用的就是 Antigravity！

### 自動載入

Antigravity 會自動偵測 `.agent/skills/` 目錄中的 SKILL.md 檔案。

### 使用方式

直接說：

```text
幫我建立一個網站
```

或指定使用 skill：

```text
使用 webdev skill 建立一個電商網站
```

或使用 workflow：

```text
/build-website
```

### 引用特定檔案

Antigravity 可以直接讀取專案中的任何檔案：

```text
讀取 .agent/skills/webdev/ui/styles/04-glass.md 然後用玻璃擬態風格設計
```

---

## 2️⃣ VS Code

### 方法 A：GitHub Copilot Chat

使用 `#file` 引用檔案：

```text
#file:.agent/skills/webdev/SKILL.md 幫我建立網站
```

多檔案引用：

```text
#file:.agent/skills/webdev/SKILL.md 
#file:.agent/skills/webdev/ui/no-ai-feel.md
#file:.agent/skills/webdev/ui/styles/01-minimal.md

使用極簡風格建立一個作品集
```

### 方法 B：Continue.dev 擴充套件

安裝 Continue.dev 擴充套件後，使用 `@codebase` 引用：

```text
@codebase 中的 .agent/skills/webdev 目錄包含開發規範，請參考來建立網站
```

### 方法 C：Codeium 擴充套件

安裝 Codeium 擴充套件後，使用 `@` 引用檔案：

```text
@.agent/skills/webdev/SKILL.md 幫我建立網站
```

---

## 3️⃣ Cursor

### 設定方式

**方法 A：專案規則（推薦）**

在專案根目錄建立 `.cursor/rules/webdev.mdc`：

```markdown
---
description: 網站開發時自動載入相關 Skills
globs: ["**/*.tsx", "**/*.vue", "**/*.css", "**/*.html"]
---

當進行網站開發時，請參考以下 Skills：

1. 設計規範：@.agent/skills/webdev/ui/SKILL.md
2. 去 AI 感：@.agent/skills/webdev/ui/no-ai-feel.md
3. 選擇的風格：@.agent/skills/webdev/ui/styles/[風格].md

確保所有設計都遵守「去 AI 感設計指南」。
```

**方法 B：對話中引用**

```text
@.agent/skills/webdev/SKILL.md 幫我建立一個網站
```

### 使用範例

```text
# 啟動完整流程
讀取 @.agent/skills/webdev/SKILL.md 然後幫我建立網站

# 使用特定風格
參考 @.agent/skills/webdev/ui/styles/04-glass.md 設計一個登入頁面

# 選擇技術棧
根據 @.agent/skills/webdev/wizard/tech.md 幫我選擇技術
```

---

## 4️⃣ Windsurf (Codeium)

### 設定方式

**方法 A：全域規則**

在 `.windsurf/rules/webdev.md` 建立：

```markdown
# 網站開發規則

開發網站時，必須：

1. 先讀取 .agent/skills/webdev/SKILL.md
2. 必須遵守 .agent/skills/webdev/ui/no-ai-feel.md
3. 根據需求選擇對應的設計風格

所有設計都要「去 AI 感」。
```

**方法 B：對話引用**

```text
@.agent/skills/webdev/SKILL.md 幫我建立網站
```

### 使用範例

```text
# Cascade 中使用
讀取 .agent/skills/webdev/ 目錄，然後幫我建立一個電商網站

# 指定風格
使用 @.agent/skills/webdev/ui/styles/37-nordic.md 北歐風格建立首頁
```

---

## 🔄 Workflow 使用指南

> Workflow 是預定義的完整開發流程，`build-website.md` 包含 12 步驟的標準網站開發流程

### Workflow 機制對比

| AI 工具 | 原生支援 | 位置要求 | 觸發方式 |
| ------- | -------- | -------- | -------- |
| **Antigravity** | ✅ 原生 | `.agent/workflows/` | `/build-website` 指令 |
| **Cursor** | ❌ 無 | 任意 | `@` 引用檔案 |
| **Windsurf** | ❌ 無 | 任意 | `@` 引用檔案 |
| **Aider** | ❌ 無 | 任意 | `/add` 加入檔案 |
| **Claude CLI** | ❌ 無 | 任意 | `--file` 參數 |
| **VS Code** | ❌ 無 | 任意 | `#file:` 引用 |
| **Claude/ChatGPT** | ❌ 無 | N/A | 貼上內容 |

### Antigravity（原生 Workflow 支援）

Antigravity 是唯一有原生 workflow 機制的工具，會自動偵測 `.agent/workflows/` 目錄。

**設定步驟：**

```bash
# Windows
mkdir .agent\workflows
copy .agent\skills\webdev\workflows\build-website.md .agent\workflows\

# macOS/Linux
mkdir -p .agent/workflows
cp .agent/skills/webdev/workflows/build-website.md .agent/workflows/
```

**使用方式：**

```text
# 方式 1：斜線指令（推薦）
/build-website

# 方式 2：自然語言觸發
幫我建立網站
啟動網站開發流程

# 方式 3：直接讀取
讀取 .agent/workflows/build-website.md 並執行
```

### Cursor

Cursor 沒有原生 workflow 機制，但可以用 `@` 引用達到相同效果。

**使用方式：**

```text
# 引用並執行 workflow
@.agent/skills/webdev/workflows/build-website.md 請按照這個流程幫我建立網站

# 簡化版（讓 AI 解讀流程）
讀取 @.agent/skills/webdev/workflows/build-website.md 然後開始執行 Phase 1
```

**進階：建立自動觸發規則**

在 `.cursor/rules/webdev-workflow.mdc` 建立：

```markdown
---
description: 當用戶說「建立網站」時自動載入 workflow
globs: ["**/*"]
---

當用戶說「幫我建立網站」或類似的話時，請先讀取 @.agent/skills/webdev/workflows/build-website.md 然後按照 12 個 Phase 依序執行。

Phase 1 先詢問需求，Phase 2-4 選擇技術和風格，Phase 5-12 進行開發。
```

### Windsurf

與 Cursor 類似，使用 `@` 引用 workflow 檔案。

**使用方式：**

```text
# 在 Cascade 對話中
@.agent/skills/webdev/workflows/build-website.md 按這個流程建立網站

# 分步驟執行
讀取 @.agent/skills/webdev/workflows/build-website.md
先執行 Phase 1 的需求收集
```

**建立全域規則：**

在 `.windsurf/rules/webdev.md` 新增：

```markdown
當用戶要求建立網站時，必須讀取並遵循 .agent/skills/webdev/workflows/build-website.md 中的 12 步流程。
```

### Aider CLI

Aider 可以用 `/add` 指令將 workflow 加入上下文。

**使用方式：**

```bash
# 啟動 Aider
aider

# 加入 workflow
/add .agent/skills/webdev/workflows/build-website.md

# 執行流程
請根據剛才加入的 build-website.md 開始執行網站開發流程
先從 Phase 1 需求收集開始
```

**一次載入相關檔案：**

```bash
/add .agent/skills/webdev/workflows/build-website.md
/add .agent/skills/webdev/SKILL.md
/add .agent/skills/webdev/ui/no-ai-feel.md

開始執行完整網站開發流程
```

### Claude CLI

Claude CLI 可以用 `--file` 參數載入 workflow。

**使用方式：**

```bash
# 載入 workflow 並開始
claude --file .agent/skills/webdev/workflows/build-website.md "請按照這個流程幫我建立網站"

# 互動模式
claude
> 讀取 .agent/skills/webdev/workflows/build-website.md 然後開始執行
```

**搭配 CLAUDE.md：**

在專案根目錄的 `CLAUDE.md` 中加入：

```markdown
## 網站開發

建立網站時，請遵循 `.agent/skills/webdev/workflows/build-website.md` 中的 12 步流程。

必須參考的檔案：
- .agent/skills/webdev/ui/no-ai-feel.md (去 AI 感)
- .agent/skills/webdev/spec.md (需求規格)
```

### VS Code + Copilot

使用 `#file:` 語法引用 workflow。

**使用方式：**

```text
#file:.agent/skills/webdev/workflows/build-website.md

請按照這個 workflow 的 12 個步驟幫我建立網站
先執行 Phase 1 需求收集
```

**多檔案引用：**

```text
#file:.agent/skills/webdev/workflows/build-website.md
#file:.agent/skills/webdev/ui/no-ai-feel.md

按照 workflow 建立網站，確保遵守去 AI 感設計原則
```

### Claude (網頁版)

**方法 A：Projects（推薦）**

1. 建立新專案
2. 上傳 `build-website.md`
3. 設定專案說明：

```text
當用戶要求建立網站時，請遵循 build-website.md 中的 12 步流程：
Phase 1: 需求收集
Phase 2: 技術棧選擇
Phase 3: 設計風格確定
...
Phase 12: 部署
```

**方法 B：對話貼上**

```text
以下是網站開發流程：
[貼上 build-website.md 內容]

請按照這個流程幫我建立一個 [描述] 網站
```

### ChatGPT

**方法 A：建立 GPT**

1. 上傳 `build-website.md` 作為知識
2. 設定說明：「當用戶要建立網站時，遵循上傳的 workflow」

**方法 B：手動貼上**

```text
請遵循以下流程建立網站：

[貼上 build-website.md 的 Phase 1-12]

我想建立一個 [描述] 網站，請從 Phase 1 開始
```

### Gemini

**方法 A：Gems（付費版）**

上傳 `build-website.md` 並設定 Gem 說明

**方法 B：Google AI Studio**

在 System Instructions 中貼上 workflow 內容

**方法 C：對話貼上**

與 ChatGPT 類似

### Workflow 使用技巧

1. **分步驟執行**：不要一次執行全部，每完成一個 Phase 確認後再繼續
2. **跳過不需要的步驟**：如果已知技術棧，可跳過 Phase 2-4
3. **搭配精靈文件**：Phase 2-4 可引用對應的 wizard 檔案
4. **保存決策**：將每個 Phase 的決定記錄在 spec.md 中

---

## 5️⃣ CLI 工具

### Aider

```bash
# 安裝
pip install aider-chat

# 啟動並加入 Skills
aider

# 在 Aider 中加入檔案
/add .agent/skills/webdev/SKILL.md
/add .agent/skills/webdev/ui/no-ai-feel.md

# 開始開發
請根據加入的 Skills 規範幫我建立網站
```

批次加入：

```bash
/add .agent/skills/webdev/**/*.md
```

### Claude CLI (Anthropic)

```bash
# 安裝
npm install -g @anthropic-ai/claude-cli

# Claude CLI 會自動讀取 .agent 目錄中的 skills
claude "幫我建立一個網站"

# 或明確指定
claude --file .agent/skills/webdev/SKILL.md "幫我建立網站"
```

### GitHub Copilot CLI

```bash
# 安裝
gh extension install github/gh-copilot

# 使用
gh copilot explain "讀取 .agent/skills/webdev/SKILL.md 的內容"
```

### OpenAI CLI

```bash
# 需要先手動讀取內容
cat .agent/skills/webdev/SKILL.md | openai api chat.completions.create \
  -m gpt-4 \
  -g system "你是網站開發助手，請遵守以下規範" \
  -g user "幫我建立網站"
```

---

## 6️⃣ Claude (Anthropic)

### 方法 A：Claude Projects（推薦）

1. 前往 [claude.ai/projects](https://claude.ai/projects)
2. 建立新專案
3. 上傳 `.agent/skills/webdev/` 整個目錄
4. 設定專案說明：

```text
這是網站開發 Skills 系統。

當用戶要求建立網站時：
1. 參考 SKILL.md 的流程
2. 必須遵守 ui/no-ai-feel.md 去 AI 感指南
3. 根據需求選擇適合的設計風格
```

### 方法 B：對話中貼上

將需要的檔案內容複製貼上到對話中：

```text
以下是我的設計規範：
[貼上 no-ai-feel.md 內容]

請根據以上規範，幫我設計一個...
```

---

## 7️⃣ ChatGPT / GPT-4

### 方法 A：建立自訂 GPT

1. 前往 [chat.openai.com/gpts](https://chat.openai.com/gpts)
2. 建立新 GPT
3. 上傳 Skills 檔案作為知識庫
4. 設定系統提示詞：

```text
你是網站開發專家。請遵守上傳的 Skills 檔案中的規範。
特別注意 no-ai-feel.md 中的去 AI 感設計原則。
```

### 方法 B：手動貼上

```text
以下是我的網站開發規範：

# 去 AI 感設計指南
[貼上 no-ai-feel.md 內容]

# 設計風格：極簡風
[貼上 01-minimal.md 內容]

請根據以上規範幫我建立...
```

---

## 8️⃣ Gemini

### 方法 A：Gemini Gems（付費版）

1. 建立新 Gem
2. 上傳 Skills 檔案
3. 設定指示

### 方法 B：對話貼上

與 ChatGPT 類似，手動貼上需要的規範內容

---

## 🔧 通用提示詞模板

### 啟動完整流程

```text
請讀取以下網站開發規範：
[貼上 SKILL.md 或引用路徑]

我想建立一個 [描述] 網站，請按照流程：
1. 確認需求
2. 推薦技術棧
3. 推薦設計風格
4. 開始開發

所有設計必須遵守「去 AI 感」原則。
```

### 快速開發

```text
技術棧：Next.js + Tailwind
風格：玻璃擬態
頁面：首頁、關於、聯絡

請根據以下規範開發：
[貼上對應的 skill 檔案]

確保設計有個性、不像模板。
```

---

## 💡 最佳實踐

### 1. 總是引用 no-ai-feel.md

無論使用哪個 AI 工具，都要確保它讀取了「去 AI 感設計指南」

### 2. 先選擇風格再開發

```text
1. 引用 wizard/style.md 選擇風格
2. 引用對應的 ui/styles/XX-name.md
3. 開始開發
```

### 3. 分階段引用

大型專案不要一次引用所有檔案，按需引用：

```text
階段 1：引用 spec.md 填寫需求
階段 2：引用 wizard/tech.md 選擇技術
階段 3：引用 wizard/style.md 選擇風格
階段 4：引用對應框架的 skill 開發
階段 5：引用 check/launch.md 上線檢查
```

---

## ❓ 常見問題

### Q：AI 說找不到檔案？

確認路徑正確，不同工具的路徑格式可能不同：

- Cursor/Windsurf：`@.agent/skills/webdev/...`
- Copilot：`#file:.agent/skills/webdev/...`
- 其他：完整路徑或直接貼上內容

### Q：檔案太多 AI 處理不過來？

只引用當前需要的檔案，核心檔案：

1. `SKILL.md`（入口）
2. `ui/no-ai-feel.md`（必讀）
3. 選定的風格檔案
4. 選定的框架指南

### Q：可以自動載入嗎？

- Antigravity：自動偵測 skills 目錄
- Cursor/Windsurf：使用 Rules 功能
- Claude CLI：自動讀取 .agent 目錄

### Q：如何在團隊中共享？

將 `.agent/skills/webdev/` 目錄提交到版本控制，團隊成員就能使用相同的規範。
