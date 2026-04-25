---
id: verification_commands
name: verification_commands
description: 跨模組共用的驗證指令庫（lint/test/build/audit），各 SKILL.md 的 Verification 章節請引用此檔
---

# 驗證指令速查

## Lint / Format

| 任務 | 指令 |
|------|------|
| ESLint | `npx eslint . --max-warnings=0` |
| Stylelint | `npx stylelint "**/*.{css,scss,vue}"` |
| Prettier | `npx prettier --check .` |
| TypeScript | `npx tsc --noEmit` |
| PHP CS Fixer | `vendor/bin/php-cs-fixer fix --dry-run` |
| Pint (Laravel) | `vendor/bin/pint --test` |

## Build / Bundle

| 任務 | 指令 |
|------|------|
| Next 建置 | `npx next build` |
| Vite 建置 | `npx vite build` |
| Bundle 分析 | `ANALYZE=true npx next build` 或 `npx vite-bundle-visualizer` |

## Test

| 任務 | 指令 |
|------|------|
| Vitest | `npx vitest run` |
| Vitest watch | `npx vitest` |
| Playwright | `npx playwright test` |
| Playwright UI | `npx playwright test --ui` |
| 視覺回歸 | `npx playwright test --update-snapshots`（首次） |

## Audit

| 任務 | 指令 |
|------|------|
| Lighthouse CI | `npx @lhci/cli@latest autorun` |
| axe-core (cli) | `npx @axe-core/cli http://localhost:3000` |
| Pa11y | `npx pa11y http://localhost:3000` |
| 依賴漏洞 | `npm audit --audit-level=high` |

## Git Hook 建議

```json
// package.json
{
  "scripts": {
    "verify": "tsc --noEmit && eslint . && vitest run && playwright test"
  }
}
```

## 引用方式

各 SKILL.md 的 Verification 章節以「見 `_shared/verification-commands.md` 中 *xxx* 段落」引用，避免重複列指令。
