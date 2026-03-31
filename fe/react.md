---
name: React Guide
description: React 18+ 開發最佳實踐
---

# ⚛️ React 指南

## 專案建立

```bash
# 使用 Vite (推薦)
npm create vite@latest my-app -- --template react-ts

# 使用 Create React App
npx create-react-app my-app --template typescript
```

---

## 專案結構

```
src/
├── components/
│   ├── ui/           # 基礎 UI 元件
│   │   ├── Button.tsx
│   │   └── Input.tsx
│   └── features/     # 功能元件
├── hooks/            # 自訂 Hooks
├── pages/            # 頁面元件
├── services/         # API 服務
├── stores/           # 狀態管理
├── types/            # TypeScript 型別
├── utils/            # 工具函式
└── App.tsx
```

---

## 元件模式

### 函式元件

```tsx
interface UserCardProps {
  name: string;
  email: string;
  avatar?: string;
}

export function UserCard({ name, email, avatar }: UserCardProps) {
  return (
    <div className="user-card">
      {avatar && <img src={avatar} alt={name} />}
      <h3>{name}</h3>
      <p>{email}</p>
    </div>
  );
}
```

### 使用 Hooks

```tsx
import { useState, useEffect } from 'react';

export function useUser(userId: string) {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    fetchUser(userId)
      .then(setUser)
      .catch(setError)
      .finally(() => setLoading(false));
  }, [userId]);

  return { user, loading, error };
}
```

---

## 狀態管理

### 本地狀態

```tsx
const [count, setCount] = useState(0);
const [form, setForm] = useState({ name: '', email: '' });
```

### 全域狀態 (Zustand)

```tsx
import { create } from 'zustand';

interface UserStore {
  user: User | null;
  setUser: (user: User) => void;
  logout: () => void;
}

export const useUserStore = create<UserStore>((set) => ({
  user: null,
  setUser: (user) => set({ user }),
  logout: () => set({ user: null }),
}));
```

---

## 最佳實踐

1. **元件單一職責**
2. **使用 TypeScript**
3. **自訂 Hook 抽取邏輯**
4. **避免 prop drilling，使用 Context 或狀態管理**
5. **使用 React.memo 優化效能**
6. **使用 useCallback 和 useMemo 適當快取**
