# 📐 設計系統生成器 (Design System Generator)

> **核心邏輯 (Core Logic)：** 我們採用 **「Master + Overrides (主檔 + 覆寫)」** 模式。一個全域的真理來源 (`MASTER.md`) 定義核心品牌，而頁面專屬檔案 (`pages/xxx.md`) 允許必要的偏差。

## 1. 檔案結構 (File Structure)

始終在 `design-system` 資料夾內建立此結構（若不存在則建立）：

```text
design-system/
├── MASTER.md            # 全域真理來源 (品牌 DNA)
└── pages/               # 頁面專屬覆寫
    ├── home.md
    └── dashboard.md
```

## 2. MASTER.md 格式

使用此模板生成 `design-system/MASTER.md`。使用 **推理引擎 (Reasoning Engine)** 的值填入 `[...]`。

```markdown
# Design System: [專案名稱]

## 🎨 Color Palette (配色盤)
| Token | Value | usage |
|-------|-------|-------|
| --primary | [Hex] | 主品牌色，主要行動 |
| --secondary | [Hex] | 強調色，次要資訊 |
| --bg-main | [Hex] | 主頁面背景 |
| --bg-card | [Hex] | 卡片/容器背景 |
| --text-main | [Hex] | 標題，主要文字 |
| --text-muted | [Hex] | 副標題，Meta 資訊 |
| --accent | [Hex] | 特別強調 |

## 🔤 Typography (字體排印)
- **Headings**: [Font Family] ([Weights])
- **Body**: [Font Family] ([Weights])
- **Scale**:
  - H1: [e.g., 3.5rem]
  - H2: [e.g., 2.5rem]
  - Body: 1rem

## 📐 Spacing & Layout (間距與佈局)
- **Container**: [e.g., 1200px / 1440px]
- **Radius**: [e.g., 8px / 24px / 0px]
- **Spacing Unit**: 4px (base)

## ✨ Effects (特效)
- **Shadows**: [e.g., Soft: 0 4px 20px rgba(0,0,0,0.05)]
- **Transitions**: [e.g., 200ms ease-out]

## 🚫 Anti-Patterns (反模式 - 禁止實作)
- [List from Reasoning Block]
```

## 3. 頁面覆寫模式 (Page Overrides Pattern)

當建立特定頁面（例如 "Dashboard"）時，檢查 `design-system/pages/dashboard.md` 是否存在。
- **若存在**：應用覆寫代碼（例如：「Dashboard 使用深色模式，儘管網站是淺色模式」）。
- **若不存在**：嚴格遵守 `MASTER.md`。

## 4. 程式碼中的使用 (Usage in Code)

當撰寫 CSS (Tailwind config 或 CSS 變數) 時，**必須** 先讀取 `MASTER.md` 並精確映射數值。
- 不要在開發時隨意發明新的 Hex 色碼。
- 不要隨意更改字體大小。
