---
title: "Rutas, parámetros de ruta y parámetros de consulta (queries)"
position: 3
date: 2025-10-27
---


## 🚀 ¿Qué necesitas?
- Python 3.7+
- FastAPI instalado: `pip install fastapi`
- Un servidor ASGI como Uvicorn: `pip install uvicorn`

---

## 📁 Estructura básica de una app FastAPI

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
def home():
    return {"mensaje": "¡Hola desde FastAPI!"}
```

Guarda esto en un archivo, por ejemplo `main.py`, y ejecútalo con:

```bash
uvicorn main:app --reload
```

Visita `http://localhost:8000` en tu navegador.

---

## 🛣️ 1. Rutas (Endpoints)

En FastAPI, defines rutas usando decoradores como `@app.get()`, `@app.post()`, etc.

### Ejemplo básico:

```python
@app.get("/saludo")
def saludar():
    return {"texto": "¡Hola, mundo!"}
```

Visita: `http://localhost:8000/saludo`

---

## 🔢 2. Parámetros de Ruta (Path Parameters)

Son partes dinámicas de la URL. Se definen entre llaves `{}` en la ruta.

### Ejemplo:

```python
@app.get("/usuario/{user_id}")
def obtener_usuario(user_id: int):
    return {"usuario_id": user_id, "nombre": f"Usuario {user_id}"}
```

- **`{user_id}`** es un parámetro de ruta.
- FastAPI **convierte automáticamente** el valor a `int` gracias a la anotación de tipo.
- Si pones algo que no es número (ej. `/usuario/abc`), FastAPI devuelve un error 422.

> ✅ No necesitas Pydantic para esto. Solo usa tipos básicos: `str`, `int`, `float`, `bool`.

### Otro ejemplo con `str`:

```python
@app.get("/saludo/{nombre}")
def saludo_personalizado(nombre: str):
    return {"mensaje": f"¡Hola, {nombre}!"}
```

Visita: `http://localhost:8000/saludo/Alejandro`

---

## ❓ 3. Parámetros de Consulta (Query Parameters)

Son los parámetros que van después del `?` en la URL, como `?nombre=Ale&edad=30`.

Se definen como **parámetros de la función**, **pero no están en la ruta**.

### Ejemplo simple:

```python
@app.get("/buscar")
def buscar_usuario(nombre: str = None, edad: int = None):
    return {"nombre": nombre, "edad": edad}
```

- Si no se envía un parámetro, usa el valor por defecto (`None`).
- FastAPI los toma de la URL automáticamente.

Visita:
- `http://localhost:8000/buscar?nombre=Ana`
- `http://localhost:8000/buscar?nombre=Carlos&edad=25`

### Valores por defecto obligatorios vs opcionales

- **Opcional** (puede no enviarse):
  ```python
  def buscar(nombre: str = None)
  ```

- **Requerido** (debe enviarse):
  ```python
  def buscar(nombre: str)
  ```
  → Si no lo envías, FastAPI responde con error 422.

- **Con valor por defecto no nulo**:
  ```python
  def buscar(pagina: int = 1, limite: int = 10):
      return {"pagina": pagina, "limite": limite}
  ```

Visita: `http://localhost:8000/buscar` → usa `pagina=1`, `limite=10`.

---

## 🔄 Combinando Ruta + Query

¡Puedes usar ambos al mismo tiempo!

```python
@app.get("/usuario/{user_id}/posts")
def obtener_posts(user_id: int, activo: bool = True, categoria: str = None):
    return {
        "usuario_id": user_id,
        "activo": activo,
        "categoria": categoria
    }
```

Ejemplo de URL:
- `http://localhost:8000/usuario/5/posts?activo=false&categoria=tecnologia`

> FastAPI convierte `"false"` → `False`, `"true"` → `True` automáticamente.

---

## 📌 Notas importantes (sin Pydantic)

- **No necesitas modelos Pydantic** para rutas simples.
- FastAPI usa **anotaciones de tipo** (`: str`, `: int`, etc.) para:
  - Validar datos.
  - Generar documentación automática (en `/docs`).
- Si necesitas datos más complejos (como JSON en el cuerpo de una petición POST), ahí sí necesitarás Pydantic. Pero para rutas y queries, **no es obligatorio**.

---

## 📚 Documentación automática

FastAPI genera documentación interactiva en:

- Swagger UI: `http://localhost:8000/docs`
- ReDoc: `http://localhost:8000/redoc`

¡Puedes probar tus endpoints directamente desde ahí!

---

## ✅ Ejemplo completo (todo junto)

```python
from fastapi import FastAPI
from fastapi.middleware.cors import CORSMiddleware  

app = FastAPI()

# Configura CORS
app.add_middleware(
    CORSMiddleware,
    allow_origins=["*"],  
    allow_credentials=True,
    allow_methods=["*"],  
    allow_headers=["*"],  
)


@app.get("/")
def home():
    return {"mensaje": "Bienvenido a FastAPI sin Pydantic"}

@app.get("/item/{item_id}")
def leer_item(item_id: int, q: str = None, activo: bool = True):
    return {
        "item_id": item_id,
        "q": q,
        "activo": activo
    }

@app.get("/saludo/{nombre}")
def saludo(nombre: str):
    return {"saludo": f"¡Hola, {nombre}!"}

@app.get("/config")
def configuracion(modo: str = "produccion", version: float = 1.0):
    return {"modo": modo, "version": version}
```

---

## 🧪 Prueba tus endpoints

| URL | Resultado esperado |
|-----|--------------------|
| `GET /` | `{"mensaje": "Bienvenido..."}` |
| `GET /item/42` | `{"item_id": 42, "q": null, "activo": true}` |
| `GET /item/10?q=busqueda&activo=false` | `{"item_id": 10, "q": "busqueda", "activo": false}` |
| `GET /saludo/María` | `{"saludo": "¡Hola, María!"}` |
| `GET /config?modo=dev&version=2.5` | `{"modo": "dev", "version": 2.5}` |

---

## Probar la API (interactivo)

Usa el explorador interactivo para enviar solicitudes al ejercicio final y validar tus rutas sin salir del tutorial.

````api-explorer
---
baseUrl: http://localhost:8000
endpoints:
  - path: /
    method: GET
    title: Estado de la API
    description: Verifica que la API esté activa
  - path: /item/{item_id}
    method: GET
    title: Leer item por ID
    description: Devuelve datos del item y acepta queries opcionales
    parameters:
      - name: item_id
        in: path
        description: ID del item
        required: true
        schema:
          type: integer
          example: 42
      - name: q
        in: query
        description: Texto de búsqueda opcional
        required: false
        schema:
          type: string
          example: "busqueda"
      - name: activo
        in: query
        description: Estado activo del recurso
        required: false
        schema:
          type: boolean
          example: true
  - path: /saludo/{nombre}
    method: GET
    title: Saludo personalizado
    description: Devuelve un saludo usando el nombre en la ruta
    parameters:
      - name: nombre
        in: path
        description: Nombre para el saludo
        required: true
        schema:
          type: string
          example: "María"
  - path: /config
    method: GET
    title: Configuración
    description: Devuelve configuración usando parámetros de consulta
    parameters:
      - name: modo
        in: query
        description: Modo de la aplicación
        required: false
        schema:
          type: string
          example: "dev"
      - name: version
        in: query
        description: Versión de la aplicación
        required: false
        schema:
          type: number
          example: 2.5
---
````

