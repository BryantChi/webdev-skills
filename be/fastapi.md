---
name: FastAPI 指南
description: FastAPI 開發最佳實踐
---

# ⚡ FastAPI 指南

## 專案建立

```bash
pip install fastapi uvicorn
```

---

## 專案結構

```
app/
├── main.py
├── routers/
│   ├── users.py
│   └── posts.py
├── models/
│   └── user.py
├── schemas/
│   └── user.py
├── services/
│   └── user_service.py
└── database.py
```

---

## 基本應用

```python
from fastapi import FastAPI, HTTPException
from pydantic import BaseModel

app = FastAPI(title="My API")

class User(BaseModel):
    id: int
    name: str
    email: str

class CreateUser(BaseModel):
    name: str
    email: str

users = []

@app.get("/users", response_model=list[User])
async def get_users():
    return users

@app.post("/users", response_model=User, status_code=201)
async def create_user(user: CreateUser):
    new_user = User(id=len(users) + 1, **user.dict())
    users.append(new_user)
    return new_user

@app.get("/users/{user_id}", response_model=User)
async def get_user(user_id: int):
    user = next((u for u in users if u.id == user_id), None)
    if not user:
        raise HTTPException(status_code=404, detail="User not found")
    return user
```

---

## 路由模組化

```python
# routers/users.py
from fastapi import APIRouter

router = APIRouter(prefix="/users", tags=["users"])

@router.get("/")
async def get_users():
    return []

# main.py
from fastapi import FastAPI
from routers import users

app = FastAPI()
app.include_router(users.router)
```

---

## 依賴注入

```python
from fastapi import Depends

async def get_db():
    db = SessionLocal()
    try:
        yield db
    finally:
        db.close()

@app.get("/users")
async def get_users(db: Session = Depends(get_db)):
    return db.query(User).all()
```

---

## 執行

```bash
uvicorn app.main:app --reload
```

---

## 最佳實踐

1. **Pydantic Schema 驗證**
2. **依賴注入模式**
3. **路由模組化**
4. **async/await 非同步**
5. **自動 API 文件**
