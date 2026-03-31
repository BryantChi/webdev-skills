---
id: no_ai_feel_design_guide
name: no_ai_feel_design_guide
description: 確保設計呈現自然、有溫度、不商業化的指南
---

# 🎭 去 AI 感設計指南

## 核心理念

> **「設計應該像是有經驗的設計師手工打造，而非機器自動生成」**

AI 生成的設計常有以下問題：
- 過於完美、缺乏個性
- 商業模板感太重
- 元素排列過於規律
- 配色過於「安全」
- 缺乏人味和故事感

---

## ❌ 避免的特徵

### 1. 過於商業化的元素

| 避免 | 為什麼 |
|-----|--------|
| 「立即購買」「限時優惠」等促銷文案 | 商業感太重 |
| 過多的 CTA 按鈕 | 像廣告頁面 |
| 庫存照（handshake、微笑商務人士） | 假假的、沒有溫度 |
| 「專業」「領先」「最佳」等空洞詞彙 | AI 愛用的詞彙 |
| 完美對齊的網格 | 太機械化 |

### 2. AI 生成的典型特徵

| 避免 | 替代方案 |
|-----|---------|
| 完美對稱佈局 | 適度的非對稱 |
| 每個元素都居中 | 左對齊或錯落排版 |
| 過度一致的間距 | 有意識的變化 |
| 太「安全」的配色（藍+白） | 大膽的品牌色 |
| 常見的漸層組合 | 獨特的色彩選擇 |
| 過多的圓角 | 混合使用圓角和直角 |
| 每個區塊都有陰影 | 精選使用陰影 |

### 3. AI 愛用的通用字體

| ❌ 避免 | 原因 |
|--------|------|
| Inter | AI 預設首選，極度氾濫 |
| Roboto | Google 預設，缺乏個性 |
| Arial / Helvetica | 系統預設，沒有設計感 |
| Open Sans | 過於中性，到處都是 |
| Lato | 曾經流行，現在普遍 |
| Montserrat | 太常見於模板 |
| Space Grotesk | 近期 AI 過度使用 |

**✅ 推薦有個性的替代字體：**

| 用途 | 推薦字體 |
|-----|----------|
| 現代科技感 | Outfit, Plus Jakarta Sans, Satoshi |
| 優雅質感 | Bricolage Grotesque, General Sans |
| 人文溫度 | Source Serif Pro, Fraunces |
| 大膽標題 | Cabinet Grotesk, Instrument Sans |
| 手工感 | Caveat, Kalam (手寫), Gochi Hand |
| 中文 | Noto Sans TC (搭配特色英文字體) |

### 4. 模板化設計

| 避免 | 替代方案 |
|-----|---------|
| Hero + 3 Features + Testimonials | 根據內容決定結構 |
| 標準的 12 欄網格 | 自訂的網格系統 |
| 千篇一律的卡片 | 為內容設計的容器 |
| 固定的頁首頁尾 | 有創意的導航方式 |

---

## ✅ 增加人味的方法

### 1. 排版要有呼吸感

```css
/* ❌ 機械化 */
.section {
  margin-bottom: 80px;  /* 每個都一樣 */
}

/* ✅ 有變化 */
.section-hero { margin-bottom: 120px; }
.section-intro { margin-bottom: 60px; }
.section-gallery { margin-bottom: 100px; }
```

### 2. 適度的非對稱

```css
/* ❌ 完美對稱 */
.grid {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
}

/* ✅ 有變化 */
.grid {
  display: grid;
  grid-template-columns: 1.2fr 0.8fr 1fr;
}
```

### 3. 個性化的細節

- 使用手寫字型於重點處
- 加入手繪元素或圖標
- 使用真實照片而非庫存照
- 微妙的動畫有不規則感
- 適度的「不完美」
- 圖片處理有質感（微調 contrast/saturation）

### 4. 色彩要有性格

```css
/* ❌ 安全的商業藍 */
--primary: #2563eb;

/* ✅ 有性格的顏色 */
--primary: #6366f1;  /* 帶紫的藍，更現代 */
--primary: #0d9488;  /* 青綠色，清新 */
--primary: #dc2626;  /* 大膽的紅，有態度 */
```

**配色策略：主色 + 尖銳強調色**

```css
/* ❌ 膽小的均分配色 */
--color-1: #3b82f6;  /* 藍 33% */
--color-2: #10b981;  /* 綠 33% */
--color-3: #f59e0b;  /* 黃 33% */

/* ✅ 主導色 + 尖銳點綴 */
--dominant: #0f172a;   /* 深色佔 85% */
--accent: #f43f5e;     /* 強調色只佔 5% 但超搶眼 */
--neutral: #64748b;    /* 中性色 10% */
```

### 5. 真實的內容

| 避免 | 使用 |
|-----|------|
| Lorem ipsum | 真實文案 |
| 123-456-7890 | 真實格式 |
| user@example.com | 品牌化 email |
| 庫存照片 | 真實照片或獨特插畫 |
| 「我們的團隊」+ 完美微笑 | 真實的團隊照片 |

---

## 🧭 設計前三問

**在寫任何 code 前，必須先回答這三個問題：**

### 1. 這個設計的「美學極端」是什麼？

不要中庸，選一個方向並徹底執行：

| 極端方向 | 特徵 |
|---------|------|
| 極簡主義 | 大量留白、精煉元素、克制用色 |
| 華麗極大化 | 豐富層次、多重動效、視覺密度 |
| 復古懷舊 | 舊時代質感、暖色調、類比元素 |
| 未來科技 | 冷色調、幾何圖形、霓虹光效 |
| 有機自然 | 不規則形狀、大地色系、手繪感 |
| 奢華精緻 | 深色調、金屬質感、細膩細節 |
| 童趣玩味 | 飽和色彩、圓潤造型、動態互動 |
| 編輯雜誌 | 強烈字體對比、不對稱排版、留白 |
| 粗獷原始 | 厚重色塊、工業感、無裝飾 |

### 2. 用戶會記住什麼「一個」特點？

每個設計都需要一個「記憶點」：
- 獨特的滾動動畫？
- 意想不到的導航方式？
- 個性化的載入畫面？
- 有態度的文案風格？
- 吸睛的主視覺？

### 3. 這個設計有什麼「出乎意料」的地方？

設計應該有一處讓人意外的元素：
- 顏色：不是預期的產業常見色
- 佈局：打破傳統網格
- 互動：超出預期的回饋
- 細節：彩蛋或微互動

---

## 🎨 設計原則

### 1. 不對稱平衡
視覺重量平衡，但不需要鏡像對稱

### 2. 留白要大膽
比你覺得需要的更多留白

### 3. 空間構圖要有創意

突破傳統佈局的方法：

| 技巧 | 說明 |
|-----|------|
| **Overlap 重疊** | 元素之間有 z-index 交錯，不要各自獨立 |
| **Diagonal 對角** | 內容沿對角線流動，打破水平垂直 |
| **Grid-breaking** | 刻意讓某元素突破網格邊界 |
| **Negative space** | 大膽留白或極度密集，不要中庸 |

```css
/* ✅ 元素重疊 */
.card-overlap {
  position: relative;
}
.card-overlap::before {
  content: '';
  position: absolute;
  inset: -20px -20px auto auto;
  width: 80px;
  height: 80px;
  background: var(--accent);
  z-index: -1;
}
```

### 4. 背景要有層次（不只是純色）

**背景是 AI 生成最容易偷懶的地方。**

| 技巧 | CSS 範例 |
|-----|----------|
| Gradient Mesh | `background: radial-gradient(at 30% 20%, #a855f7, transparent), radial-gradient(at 70% 80%, #3b82f6, transparent);` |
| Noise 噪點 | `background-image: url("data:image/svg+xml,...");` + filter |
| Geometric | 使用 SVG 幾何圖形作背景 |
| Grain 顆粒 | `filter: contrast(1.05) brightness(1.02);` + noise overlay |

```css
/* ✅ 漸層網格背景 */
.bg-mesh {
  background-color: #0f172a;
  background-image:
    radial-gradient(at 40% 20%, rgba(120, 119, 198, 0.3) 0px, transparent 50%),
    radial-gradient(at 80% 0%, rgba(225, 29, 72, 0.15) 0px, transparent 50%),
    radial-gradient(at 0% 50%, rgba(14, 165, 233, 0.2) 0px, transparent 50%);
}

/* ✅ 顆粒效果 */
.bg-grain::before {
  content: '';
  position: absolute;
  inset: 0;
  background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noise'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.8' numOctaves='4' /%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noise)'/%3E%3C/svg%3E");
  opacity: 0.03;
  pointer-events: none;
}
```

### 5. 動畫的哲學

**重點不是動畫數量，而是時機和編排。**

> 一個精心編排的頁面載入動畫，比到處散落的微互動更令人驚艷。

**高影響力動畫時機：**
- 頁面初次載入（staggered reveal）
- 滾動觸發（scroll-triggered）
- 重要互動（hover/click on key elements）

```css
/* ❌ 過於完美的預設 */
transition: all 0.3s ease;

/* ✅ 自然的彈性曲線 */
transition: transform 0.4s cubic-bezier(0.34, 1.56, 0.64, 1);

/* ✅ 錯開的載入動畫 */
.item { animation: fadeUp 0.6s ease-out both; }
.item:nth-child(1) { animation-delay: 0ms; }
.item:nth-child(2) { animation-delay: 80ms; }
.item:nth-child(3) { animation-delay: 160ms; }

@keyframes fadeUp {
  from { opacity: 0; transform: translateY(20px); }
  to { opacity: 1; transform: translateY(0); }
}
```

### 6. 微互動有驚喜
- 滑鼠移過時的小變化
- 點擊時的輕微彈跳
- 滾動時的漸入效果
- 自訂游標（關鍵頁面）

---

## 🔍 檢查清單

### 瀏覽你的設計時，問自己：

- [ ] 這看起來是「模板」嗎？
- [ ] 換掉 Logo 和文字，還認得出是誰的網站嗎？
- [ ] 有沒有一個讓人記住的特色？
- [ ] 動畫是否太過「完美」？
- [ ] 配色是否太「安全」？
- [ ] 佈局是否太「對稱」？
- [ ] 內容是否太「商業」？
- [ ] 圖片是否像「庫存照」？

### 每個設計都應該有：

- [ ] 一個獨特的視覺記憶點
- [ ] 真實感的內容
- [ ] 品牌個性的體現
- [ ] 至少一處「意外」的設計
- [ ] 人性化的微互動

---

## 📐 實作範例

### ❌ AI 感設計（純 HTML/CSS）
```html
<section style="padding: 80px 0; background: #fff; text-align: center;">
  <h2 style="font-size: 2rem; font-weight: bold; margin-bottom: 16px;">我們的服務</h2>
  <p style="color: #666; margin-bottom: 32px;">提供最專業的解決方案</p>
  <div style="display: grid; grid-template-columns: repeat(3, 1fr); gap: 32px;">
    <!-- 三個一模一樣的卡片 -->
  </div>
</section>
```

### ✅ 有人味的設計（純 HTML/CSS）
```html
<section class="services">
  <div class="services-grid">
    <div class="services-intro">
      <span class="label">服務</span>
      <h2>
        我們不只是做網站，<br>
        <strong>我們說故事。</strong>
      </h2>
    </div>
    <div class="services-content">
      <!-- 不對稱的內容佈局 -->
    </div>
  </div>
</section>

<style>
.services {
  padding: 4rem 2rem;
}
@media (min-width: 768px) { .services { padding: 6rem 2rem; } }
@media (min-width: 1024px) { .services { padding: 8rem 2rem; } }

.services-grid {
  display: grid;
  gap: 2rem;
}
@media (min-width: 1024px) {
  .services-grid {
    grid-template-columns: 5fr 7fr;
    gap: 4rem;
  }
}

.label {
  font-size: 0.75rem;
  letter-spacing: 0.2em;
  text-transform: uppercase;
  color: #64748b;
}

.services-intro h2 {
  margin-top: 0.5rem;
  font-size: clamp(2rem, 5vw, 3rem);
  font-weight: 300;
  line-height: 1.2;
}

.services-intro h2 strong {
  font-weight: 500;
}
</style>
```

### ✅ Tailwind 版本（選用）
```html
<section class="py-16 md:py-24 lg:py-32 px-8">
  <div class="grid lg:grid-cols-12 gap-8">
    <div class="lg:col-span-5">
      <span class="text-xs tracking-[0.2em] uppercase text-slate-500">服務</span>
      <h2 class="mt-2 text-4xl lg:text-5xl font-light leading-tight">
        我們不只是做網站，<br>
        <span class="font-medium">我們說故事。</span>
      </h2>
    </div>
    <div class="lg:col-span-6 lg:col-start-7">
      <!-- 不對稱的內容佈局 -->
    </div>
  </div>
</section>
```

### ✅ Bootstrap 5 版本（選用）
```html
<section class="py-5 py-lg-6">
  <div class="container">
    <div class="row g-4 g-lg-5">
      <div class="col-lg-5">
        <span class="text-uppercase small letter-spacing-2 text-secondary">服務</span>
        <h2 class="mt-2 display-5 fw-light lh-sm">
          我們不只是做網站，<br>
          <span class="fw-medium">我們說故事。</span>
        </h2>
      </div>
      <div class="col-lg-6 offset-lg-1">
        <!-- 不對稱的內容佈局 -->
      </div>
    </div>
  </div>
</section>

<style>
.letter-spacing-2 { letter-spacing: 0.2em; }
.py-lg-6 { padding-top: 6rem; padding-bottom: 6rem; }
</style>
```

### ✅ Vue 3 元件版本（選用）
```vue
<template>
  <section class="services">
    <div class="services-grid">
      <div class="services-intro">
        <span class="label">{{ label }}</span>
        <h2>
          {{ titleLine1 }}<br>
          <strong>{{ titleLine2 }}</strong>
        </h2>
      </div>
      <div class="services-content">
        <slot />
      </div>
    </div>
  </section>
</template>

<script setup>
defineProps({
  label: { type: String, default: '服務' },
  titleLine1: { type: String, default: '我們不只是做網站，' },
  titleLine2: { type: String, default: '我們說故事。' }
})
</script>

<style scoped>
.services { padding: 4rem 2rem; }
@media (min-width: 1024px) { .services { padding: 8rem 2rem; } }
.services-grid { display: grid; gap: 2rem; }
@media (min-width: 1024px) {
  .services-grid { grid-template-columns: 5fr 7fr; gap: 4rem; }
}
.label {
  font-size: 0.75rem;
  letter-spacing: 0.2em;
  text-transform: uppercase;
  color: var(--color-muted, #64748b);
}
.services-intro h2 {
  margin-top: 0.5rem;
  font-size: clamp(2rem, 5vw, 3rem);
  font-weight: 300;
  line-height: 1.2;
}
.services-intro h2 strong { font-weight: 500; }
</style>
```

### ✅ React 元件版本（選用）
```jsx
import styles from './Services.module.css';

export function Services({ 
  label = '服務',
  titleLine1 = '我們不只是做網站，',
  titleLine2 = '我們說故事。',
  children 
}) {
  return (
    <section className={styles.services}>
      <div className={styles.grid}>
        <div className={styles.intro}>
          <span className={styles.label}>{label}</span>
          <h2>
            {titleLine1}<br />
            <strong>{titleLine2}</strong>
          </h2>
        </div>
        <div className={styles.content}>
          {children}
        </div>
      </div>
    </section>
  );
}
```

```css
/* Services.module.css */
.services { padding: 4rem 2rem; }
@media (min-width: 1024px) { .services { padding: 8rem 2rem; } }
.grid { display: grid; gap: 2rem; }
@media (min-width: 1024px) {
  .grid { grid-template-columns: 5fr 7fr; gap: 4rem; }
}
.label {
  font-size: 0.75rem;
  letter-spacing: 0.2em;
  text-transform: uppercase;
  color: var(--color-muted, #64748b);
}
.intro h2 {
  margin-top: 0.5rem;
  font-size: clamp(2rem, 5vw, 3rem);
  font-weight: 300;
  line-height: 1.2;
}
.intro h2 strong { font-weight: 500; }
```

### ✅ SCSS 版本（選用）
```scss
// _services.scss
.services {
  padding: 4rem 2rem;

  @media (min-width: 768px) { padding: 6rem 2rem; }
  @media (min-width: 1024px) { padding: 8rem 2rem; }

  &-grid {
    display: grid;
    gap: 2rem;

    @media (min-width: 1024px) {
      grid-template-columns: 5fr 7fr;
      gap: 4rem;
    }
  }

  &-intro {
    .label {
      font-size: 0.75rem;
      letter-spacing: 0.2em;
      text-transform: uppercase;
      color: $color-muted;
    }

    h2 {
      margin-top: 0.5rem;
      font-size: clamp(2rem, 5vw, 3rem);
      font-weight: 300;
      line-height: 1.2;

      strong {
        font-weight: 500;
      }
    }
  }
}
```

---

## ⚖️ 複雜度要匹配美學

**關鍵原則：極簡需克制，華麗需繁複。**

| 美學方向 | 程式碼複雜度 | 重點 |
|---------|-------------|------|
| 極簡主義 | ⬇️ 簡潔 | 精準的 spacing、typography、subtle details |
| 華麗極大化 | ⬆️ 繁複 | 多層動畫、豐富特效、dense layouts |
| 編輯雜誌 | ➡️ 中等 | Typography 為王、大膽留白 |
| 粗獷原始 | ⬇️ 簡潔 | 厚重元素、無裝飾、直接 |

**不要做的事：**
- ❌ 極簡設計卻塞滿特效
- ❌ 華麗風格卻只有基本元件
- ❌ 設計方向和程式碼複雜度不匹配

---

## 🚫 絕對禁止

1. **不使用** 「Lorem ipsum」作為最終內容
2. **不使用** 庫存照片的握手、微笑商務人士
3. **不使用** 「專業團隊」「領先業界」「最佳選擇」等 AI 愛用空洞詞彙
4. **不使用** 完全對稱的三欄設計
5. **不使用** 純白或純灰背景（要有層次）
6. **不使用** 每個按鈕都是藍色
7. **不使用** 完美規律的 8px 間距系統
8. **不使用** 分散無重點的微動畫
9. **不使用** AI 氾濫字體（Inter、Roboto、Open Sans 等）
