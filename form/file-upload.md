---
id: web_form_file_upload
name: web_form_file_upload
description: 檔案上傳（presigned URL、進度、分塊、安全）
---

# 檔案上傳

## 三大架構選擇

| 方式 | 適用 | 缺點 |
|------|------|------|
| **Presigned URL（推薦）** | 大檔、S3/R2/Cloudflare 直傳 | 需設 CORS |
| 走自家 server 中轉 | 小檔 + 需要伺服器處理 | 佔頻寬、慢 |
| Edge function 串流上傳 | 中檔 + 邊緣處理 | 平台限制（Vercel 4.5MB） |

## 推薦：Presigned URL（S3 / R2 / B2）

### Server：產生 presigned

```ts
// app/api/upload/route.ts
import { S3Client, PutObjectCommand } from '@aws-sdk/client-s3';
import { getSignedUrl } from '@aws-sdk/s3-request-presigner';
import { NextResponse } from 'next/server';
import { z } from 'zod';

const s3 = new S3Client({
  region: 'auto',
  endpoint: process.env.R2_ENDPOINT,
  credentials: {
    accessKeyId: process.env.R2_ACCESS_KEY!,
    secretAccessKey: process.env.R2_SECRET!,
  },
});

const schema = z.object({
  filename: z.string().max(255),
  contentType: z.enum(['image/jpeg', 'image/png', 'image/webp', 'application/pdf']),
  size: z.number().max(20 * 1024 * 1024), // 20MB
});

export async function POST(req: Request) {
  const user = await getCurrentUser(req);
  if (!user) return new Response('Unauthorized', { status: 401 });

  const body = schema.parse(await req.json());

  const key = `users/${user.id}/${crypto.randomUUID()}-${body.filename}`;
  const cmd = new PutObjectCommand({
    Bucket: process.env.R2_BUCKET!,
    Key: key,
    ContentType: body.contentType,
    ContentLength: body.size,
  });

  const url = await getSignedUrl(s3, cmd, { expiresIn: 60 }); // 1 分鐘
  return NextResponse.json({ url, key });
}
```

### Client：直接上傳到 S3

```tsx
'use client';
import { useState } from 'react';

export function FileUpload() {
  const [progress, setProgress] = useState(0);

  async function upload(file: File) {
    // 1. 取 presigned URL
    const { url, key } = await fetch('/api/upload', {
      method: 'POST',
      body: JSON.stringify({
        filename: file.name,
        contentType: file.type,
        size: file.size,
      }),
    }).then((r) => r.json());

    // 2. 直傳 S3，含進度
    await new Promise<void>((resolve, reject) => {
      const xhr = new XMLHttpRequest();
      xhr.upload.onprogress = (e) => {
        if (e.lengthComputable) setProgress(Math.round((e.loaded / e.total) * 100));
      };
      xhr.onload = () => xhr.status >= 200 && xhr.status < 300 ? resolve() : reject(new Error('Upload failed'));
      xhr.onerror = () => reject(new Error('Network error'));
      xhr.open('PUT', url);
      xhr.setRequestHeader('Content-Type', file.type);
      xhr.send(file);
    });

    // 3. 通知 server 紀錄
    await fetch('/api/files', {
      method: 'POST',
      body: JSON.stringify({ key, name: file.name }),
    });
  }

  return (
    <div>
      <input type="file" accept="image/*,.pdf"
        onChange={(e) => e.target.files?.[0] && upload(e.target.files[0])} />
      {progress > 0 && progress < 100 && <progress value={progress} max="100" />}
    </div>
  );
}
```

## CRITICAL 規則

| # | Rule | 為何 |
|---|---|---|
| FU1 | 用 presigned URL，不過自家 server | 省頻寬、快 |
| FU2 | server 端**必**驗 contentType / size | 防惡意上傳 |
| FU3 | key 用 UUID + user prefix | 防 clash + 隔離 |
| FU4 | presigned URL 短時效（≤5min） | 安全 |
| FU5 | client 顯示進度（XHR `onprogress`） | UX |
| FU6 | 大檔（>100MB）用 multipart upload | 失敗可續傳 |
| FU7 | 圖片在 server / Edge function 處理（resize / strip metadata） | 隱私 + 效能 |
| FU8 | 不直接公開 S3 bucket，用 CDN + signed URL | 安全 |

## 進度條 + 多檔（用 Uppy）

```bash
npm i @uppy/core @uppy/dashboard @uppy/aws-s3
```

```ts
import Uppy from '@uppy/core';
import Dashboard from '@uppy/dashboard';
import AwsS3 from '@uppy/aws-s3';

const uppy = new Uppy({ restrictions: { maxFileSize: 20 * 1024 * 1024, allowedFileTypes: ['image/*'] } })
  .use(Dashboard, { target: '#drop-zone', inline: true })
  .use(AwsS3, {
    getUploadParameters: async (file) => {
      const res = await fetch('/api/upload', {
        method: 'POST',
        body: JSON.stringify({ filename: file.name, contentType: file.type, size: file.size }),
      });
      const { url } = await res.json();
      return { method: 'PUT', url, headers: { 'Content-Type': file.type } };
    },
  });
```

## Multipart（>100MB）

S3 支援分塊：每塊 5MB-5GB，最多 10000 塊。用 `CreateMultipartUpload` + 多個 `UploadPart` + `CompleteMultipartUpload`。

> 一般 web 應用建議用 [Uppy](https://uppy.io) 或 [`@vercel/blob`](https://vercel.com/docs/storage/vercel-blob) 簡化。

## Vercel Blob（最簡）

```bash
npm i @vercel/blob
```

```ts
// app/api/upload/route.ts
import { handleUpload, type HandleUploadBody } from '@vercel/blob/client';

export async function POST(req: Request) {
  const body = (await req.json()) as HandleUploadBody;
  return Response.json(await handleUpload({
    body, request: req,
    onBeforeGenerateToken: async () => ({
      allowedContentTypes: ['image/*', 'application/pdf'],
      maximumSizeInBytes: 20 * 1024 * 1024,
    }),
    onUploadCompleted: async ({ blob }) => { /* 紀錄到 DB */ },
  }));
}
```

```tsx
'use client';
import { upload } from '@vercel/blob/client';

const blob = await upload(file.name, file, {
  access: 'public',
  handleUploadUrl: '/api/upload',
});
```

## Anti-Pattern

| Bad | Good |
|---|---|
| 大檔走自家 API route | presigned URL |
| 不驗 mime / size | server 必驗 |
| 把 S3 bucket 設 public | 用 signed URL 或 CDN proxy |
| presigned URL 24h | ≤5 分鐘 |
| 不顯示進度 | XHR onprogress 或 Uppy |
| 直接信任 client filename | sanitize + UUID 重命名 |
