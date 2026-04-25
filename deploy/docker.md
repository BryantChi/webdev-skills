---
id: web_deployment_docker
name: web_deployment_docker
description: Docker self-host（Next.js / Vue / Node API）多階段建置與最佳實踐
---

# Docker 部署

## Next.js Dockerfile（standalone output）

```dockerfile
# next.config.js: output: 'standalone'
FROM node:20-alpine AS base

FROM base AS deps
WORKDIR /app
COPY package.json package-lock.json ./
RUN npm ci

FROM base AS builder
WORKDIR /app
COPY --from=deps /app/node_modules ./node_modules
COPY . .
RUN npm run build

FROM base AS runner
WORKDIR /app
ENV NODE_ENV=production
RUN addgroup -S app && adduser -S app -G app
COPY --from=builder /app/public ./public
COPY --from=builder --chown=app:app /app/.next/standalone ./
COPY --from=builder --chown=app:app /app/.next/static ./.next/static
USER app
EXPOSE 3000
CMD ["node", "server.js"]
```

## `next.config.js`

```js
module.exports = {
  output: 'standalone',
  experimental: { outputFileTracingIncludes: { '/': ['./public/**/*'] } },
};
```

## Node API Dockerfile

```dockerfile
FROM node:20-alpine AS deps
WORKDIR /app
COPY package*.json ./
RUN npm ci --omit=dev

FROM node:20-alpine AS runner
WORKDIR /app
ENV NODE_ENV=production
COPY --from=deps /app/node_modules ./node_modules
COPY . .
USER node
EXPOSE 3000
HEALTHCHECK --interval=30s --timeout=3s CMD node -e "fetch('http://localhost:3000/health').then(r=>process.exit(r.ok?0:1))"
CMD ["node", "src/server.js"]
```

## docker-compose.yml

```yaml
services:
  app:
    build: .
    ports: ["3000:3000"]
    env_file: .env.production
    restart: unless-stopped
    healthcheck:
      test: ["CMD", "wget", "-qO-", "http://localhost:3000/health"]
      interval: 30s
    depends_on:
      db: { condition: service_healthy }

  db:
    image: postgres:16-alpine
    environment:
      POSTGRES_PASSWORD: ${DB_PASSWORD}
    volumes: ["pgdata:/var/lib/postgresql/data"]
    healthcheck:
      test: ["CMD", "pg_isready"]
      interval: 10s
    restart: unless-stopped

  nginx:
    image: nginx:alpine
    ports: ["80:80", "443:443"]
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf:ro
      - ./certs:/etc/nginx/certs:ro
    depends_on: [app]

volumes:
  pgdata:
```

## CRITICAL 規則

| # | Rule | Bad | Good |
|---|---|---|---|
| DK1 | 多階段建置 | 單一 stage | deps / builder / runner 分層 |
| DK2 | 用 `node:20-alpine`（小） | `node:20`（大） | alpine |
| DK3 | 跑非 root user | `USER root`（預設） | `USER node` 或 app |
| DK4 | `.dockerignore` 排除 node_modules、.git | 全打包 | 用 `.dockerignore` |
| DK5 | 機密用 build arg / env，不寫進 image | hardcode | `--build-arg` / runtime env |
| DK6 | layer 順序：先 deps 再 code | code 改動觸發 deps 重抓 | 分開 |
| DK7 | health check 必設 | 預設無 | `HEALTHCHECK` |
| DK8 | image 標 tag（不只 `latest`） | `latest` | sha / version |

## .dockerignore

```
node_modules
.next
.git
.env
.env.local
README.md
.vscode
*.log
coverage
```

## 安全掃描

```bash
docker scout cves my-image:latest
# 或
trivy image my-image:latest
```

## 推上 registry

```bash
docker build -t ghcr.io/user/app:v1.0.0 .
docker push ghcr.io/user/app:v1.0.0
```

CI 範例見 [`ci-github.md`](ci-github.md)。

## Anti-Pattern

| Bad | Good |
|---|---|
| 在 image 內裝 dev deps | 多階段 + `npm ci --omit=dev` |
| 把 .env 複製進 image | runtime 注入 |
| 直接 `latest` 部署 | tag 版本化 |
| 不限 memory / cpu | docker-compose 設 limits |
