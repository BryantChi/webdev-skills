---
id: web_deployment_ci_github
name: web_deployment_ci_github
description: GitHub Actions CI/CD 完整範本（lint/test/build/e2e/lighthouse/deploy）
---

# GitHub Actions CI/CD

## 基本結構

```
.github/
└── workflows/
    ├── ci.yml          # PR / push 跑驗證
    ├── deploy.yml      # main 推送觸發部署
    └── nightly.yml     # 每晚跑長測試
```

## ci.yml — PR 驗證

```yaml
name: CI

on:
  pull_request:
  push:
    branches: [main]

jobs:
  verify:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: 'npm'
      - run: npm ci
      - run: npm run lint
      - run: npm run typecheck
      - run: npm test -- --coverage
      - run: npm run build
      - uses: codecov/codecov-action@v4
        if: always()

  e2e:
    runs-on: ubuntu-latest
    needs: verify
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with: { node-version: 20, cache: 'npm' }
      - run: npm ci
      - run: npx playwright install --with-deps chromium
      - run: npm run build
      - run: npx playwright test
      - uses: actions/upload-artifact@v4
        if: failure()
        with:
          name: playwright-report
          path: playwright-report/

  lighthouse:
    runs-on: ubuntu-latest
    needs: verify
    if: github.event_name == 'pull_request'
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with: { node-version: 20, cache: 'npm' }
      - run: npm ci && npm run build
      - run: npm run start &
      - run: npx wait-on http://localhost:3000
      - run: npx @lhci/cli@latest autorun
```

## deploy.yml — main 推送部署

```yaml
name: Deploy

on:
  push:
    branches: [main]

concurrency:
  group: deploy-production
  cancel-in-progress: false  # 部署不要互相取消

jobs:
  deploy:
    runs-on: ubuntu-latest
    environment: production
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with: { node-version: 20, cache: 'npm' }
      - run: npm ci
      - run: npm run build
        env:
          NEXT_PUBLIC_APP_URL: ${{ vars.APP_URL }}
          DATABASE_URL: ${{ secrets.DATABASE_URL }}
      - name: Deploy to Vercel
        run: npx vercel --prod --token=${{ secrets.VERCEL_TOKEN }}
      - name: Smoke test
        run: |
          sleep 10
          curl -f ${{ vars.APP_URL }}/api/health || exit 1
      - name: Notify Slack
        if: always()
        uses: slackapi/slack-github-action@v1
        with:
          payload: |
            { "text": "Deploy ${{ job.status }}: ${{ github.sha }}" }
        env:
          SLACK_WEBHOOK_URL: ${{ secrets.SLACK_WEBHOOK_URL }}
```

## CRITICAL 規則

| # | Rule | 說明 |
|---|---|---|
| CI1 | 用 `concurrency` 防同 branch 重複跑 | 節省資源 |
| CI2 | 用 `cache: 'npm'` 加速 | install 快 5x |
| CI3 | secret 用 `secrets.*`，public 配置用 `vars.*` | 安全 |
| CI4 | `environment: production` 啟用 review/approval | 防誤上 |
| CI5 | E2E 失敗保存 artifact（screenshot / trace） | debug |
| CI6 | smoke test 跑於部署後 | 確認真活著 |
| CI7 | 用 SHA 鎖 action 版本（高安全要求） | `uses: actions/checkout@v4` 改 SHA |
| CI8 | `permissions: read-all` 預設、寫權限按需開 | 最小權限 |

## 路徑過濾（節省 CI）

```yaml
on:
  pull_request:
    paths:
      - 'src/**'
      - 'package.json'
      - '.github/workflows/**'
```

## 矩陣測試

```yaml
strategy:
  matrix:
    node: [20, 22]
    os: [ubuntu-latest, windows-latest]
runs-on: ${{ matrix.os }}
```

## Cache patterns

```yaml
- uses: actions/cache@v4
  with:
    path: |
      ~/.npm
      ${{ github.workspace }}/.next/cache
    key: ${{ runner.os }}-nextjs-${{ hashFiles('**/package-lock.json') }}-${{ hashFiles('**/*.{js,ts,tsx}') }}
```

## Anti-Pattern

| Bad | Good |
|---|---|
| 在 CI 跑 `npm install`（不鎖版本） | `npm ci` |
| secret 用明文 echo | 永遠 mask |
| 多個工作各自 install | 用 `needs` + 共享 artifact |
| 部署不限 environment | 用 environment + reviewers |
| token 範圍過大 | fine-grained PAT |

## 與 Codex / Claude Code 整合

可在 GitHub Actions 加入：
- `ultrareview` PR 自動 review
- AI 產生 release notes
- Issue → branch 自動產生
