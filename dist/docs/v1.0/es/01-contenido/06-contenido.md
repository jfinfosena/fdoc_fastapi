---
title: "Organización de Rutas con APIRouter"
position: 6
date: 2025-11-10
---

## Objetivo

Comprender y aplicar `APIRouter` en un proyecto básico de FastAPI para organizar rutas por módulos, componer routers bajo un prefijo de versión y registrar todo en `main.py` de forma limpia.

## ¿Qué es APIRouter?

- `APIRouter` permite definir rutas de manera modular (por recurso o área) y luego componerlas bajo un prefijo común (por ejemplo, `/api/v1`).
- Facilita agrupar endpoints con `tags`, añadir prefijos y middlewares por router, y mantener una arquitectura escalable.

## Estructura mínima del proyecto

```bash
project_name/
├── app/
│   ├── main.py
│   └── api/
│       └── v1/
│           ├── routers.py
│           └── endpoints/
│               ├── users.py
│               └── items.py
└── requirements.txt
```

## Guía paso a paso

### 1) Crear router de usuarios
Define un router simple para `users` con prefijo y tags.

```python
# app/api/v1/endpoints/users.py
from fastapi import APIRouter

router = APIRouter(prefix="/users", tags=["users"])  # bajo /api/v1

@router.get("/")
def list_users():
    return [{"id": 1, "name": "Alice"}, {"id": 2, "name": "Bob"}]

@router.get("/{user_id}")
def get_user(user_id: int):
    return {"id": user_id, "name": f"User {user_id}"}
```

### 2) Crear router de ítems
Implementa un router para `items` con parámetros de ruta y consulta.

```python
# app/api/v1/endpoints/items.py
from fastapi import APIRouter

router = APIRouter(prefix="/items", tags=["items"])  # bajo /api/v1

@router.get("/")
def list_items(skip: int = 0, limit: int = 100, q: str | None = None):
    items = [
        {"id": 1, "title": "Book"},
        {"id": 2, "title": "Laptop"},
    ]
    return items[skip: skip + limit]

@router.get("/{item_id}")
def get_item(item_id: int):
    return {"id": item_id, "title": f"Item {item_id}"}
```

### 3) Componer routers bajo /api/v1
Crea un `routers.py` que agrupe los routers de `users` e `items` bajo el prefijo `/api/v1`.

```python
# app/api/v1/routers.py
from fastapi import APIRouter
from .endpoints import users, items

api_router = APIRouter(prefix="/api/v1")
api_router.include_router(users.router)
api_router.include_router(items.router)
```

### 4) Registrar en main.py
Crea la app, registra el router compuesto y expón endpoints globales básicos.

```python
# app/main.py
from fastapi import FastAPI
from app.api.v1.routers import api_router

app = FastAPI(title="Basic API")

@app.get("/health")
def health():
    return {"status": "ok"}

app.include_router(api_router)
```

### 5) Probar endpoints
Levanta el servidor y verifica rutas.

```bash
uvicorn app.main:app --reload
# http://127.0.0.1:8000/health
# http://127.0.0.1:8000/api/v1/users/
# http://127.0.0.1:8000/api/v1/users/1
# http://127.0.0.1:8000/api/v1/items/
# http://127.0.0.1:8000/api/v1/items/1
```

## Referencia

Este tutorial se basa en las recomendaciones del documento: [Rutas y Routers en FastAPI](/03-proyecto/07-rutas.md) , adaptado a un proyecto básico.


