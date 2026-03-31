---
name: Node.js 指南
description: Node.js + Express 開發最佳實踐
---

# 🟢 Node.js 指南

## 專案建立

```bash
mkdir my-app && cd my-app
npm init -y
npm install express cors dotenv
npm install -D typescript @types/node @types/express
```

---

## 專案結構

```
src/
├── controllers/
├── middlewares/
├── models/
├── routes/
├── services/
├── utils/
├── config/
└── app.ts

tests/
package.json
tsconfig.json
```

---

## Express 基本設定

```typescript
// src/app.ts
import express from 'express';
import cors from 'cors';
import { errorHandler } from './middlewares/error';
import { userRoutes } from './routes/users';

const app = express();

app.use(cors());
app.use(express.json());

// 路由
app.use('/api/users', userRoutes);

// 錯誤處理
app.use(errorHandler);

export default app;
```

---

## 路由

```typescript
// src/routes/users.ts
import { Router } from 'express';
import { UserController } from '../controllers/UserController';
import { auth } from '../middlewares/auth';

const router = Router();
const controller = new UserController();

router.get('/', controller.getAll);
router.get('/:id', controller.getById);
router.post('/', auth, controller.create);
router.put('/:id', auth, controller.update);
router.delete('/:id', auth, controller.delete);

export { router as userRoutes };
```

---

## Controller

```typescript
// src/controllers/UserController.ts
import { Request, Response, NextFunction } from 'express';
import { UserService } from '../services/UserService';

export class UserController {
  private userService = new UserService();

  getAll = async (req: Request, res: Response, next: NextFunction) => {
    try {
      const users = await this.userService.findAll();
      res.json({ success: true, data: users });
    } catch (error) {
      next(error);
    }
  };

  create = async (req: Request, res: Response, next: NextFunction) => {
    try {
      const user = await this.userService.create(req.body);
      res.status(201).json({ success: true, data: user });
    } catch (error) {
      next(error);
    }
  };
}
```

---

## 錯誤處理

```typescript
// src/middlewares/error.ts
import { Request, Response, NextFunction } from 'express';

export function errorHandler(
  err: Error,
  req: Request,
  res: Response,
  next: NextFunction
) {
  console.error(err.stack);
  
  res.status(500).json({
    success: false,
    error: {
      message: err.message || 'Internal Server Error',
    },
  });
}
```

---

## 最佳實踐

1. **TypeScript 增加型別安全**
2. **分層架構 (Controller/Service/Repository)**
3. **統一錯誤處理**
4. **環境變數管理**
5. **Async/Await 處理非同步**
6. **驗證中間件 (Joi/Zod)**
