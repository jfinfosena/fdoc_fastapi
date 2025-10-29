---
title: "Actividad 3: Rutas, Parámetros de Ruta y Consulta"
position: 3
date: 2025-10-27
---

Esta actividad debe implementarse **estrictamente** con base en el contenido de:  
```cards
---
columns: 1
items:
  - title: "Rutas, parámetros de ruta y parámetros de consulta (queries)"
    icon: "RocketIcon"
    href: "01-contenido\03-contenido.md"
    content: "Rutas, parámetros de ruta y parámetros de consulta (queries)"
---
```

## Objetivo
- Construir una API con FastAPI que demuestre rutas básicas, **parámetros de ruta** y **parámetros de consulta**.
- Validar el comportamiento con el navegador y la documentación automática (`/docs`).
- Hacer pasar las **10 rutas de pruebas** definidas en la tabla.

## Alcance y restricciones
- Usar tipos simples: `str`, `int`, `float`, `bool` (sin Pydantic ni modelos).
- Implementar una aplicación mínima con `FastAPI()` y ejecutar con `uvicorn`.
- Opcional: si tu frontend hace peticiones cruzadas, puedes agregar CORSMiddleware como se sugiere en el tutorial.

## Requisitos mínimos de la API
Implementa los siguientes endpoints:

Tu API debe soportar las rutas y combinaciones necesarias para responder a las **10 pruebas esperadas** listadas más abajo.

## Pasos sugeridos 
- Crear `main.py` y definir tu instancia `app = FastAPI()`.
- Agregar las rutas del ejercicio con anotaciones de tipo en los parámetros.
- Ejecutar localmente: `uvicorn main:app --reload`.
- Probar en el navegador: `http://localhost:8000` y validar en `http://localhost:8000/docs`.
- Verificar que las consultas opcionales se manejen correctamente (valores por defecto y tipos).

## Plantilla base

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

```
---

## Pruebas esperadas
Comprueba que tus endpoints responden al menos como se describe:

| URL | Resultado esperado |
|-----|--------------------|
| `GET /` | `{"mensaje": "Bienvenido..."}` |
| `GET /item/42` | `{"item_id": 42, "q": null, "activo": true}` |
| `GET /item/10?q=busqueda&activo=false` | `{"item_id": 10, "q": "busqueda", "activo": false}` |
| `GET /saludo/María` | `{"saludo": "¡Hola, María!"}` |
| `GET /config?modo=dev&version=2.5` | `{"modo": "dev", "version": 2.5}` |
| `GET /item/0?q=&activo=true` | `{"item_id": 0, "q": "", "activo": true}` |
| `GET /saludo/José%20Luis` | `{"saludo": "¡Hola, José Luis!"}` |
| `GET /config` | `{"modo": "produccion", "version": 1.0}` |
| `GET /item/-7` | `{"item_id": -7, "q": null, "activo": true}` |
| `GET /config?modo=test&version=abc` | `422 Unprocessable Entity (error de tipo en \"version\")` |

## Probar la API (interactivo)
Puedes apoyarte en el bloque explorador del tutorial para enviar solicitudes.

````api-explorer
---
baseUrl: http://localhost:8000
endpoints:
  # 1) GET /
  - path: /
    method: GET
    title: Estado de la API
    description: Verifica que la API esté activa

  # 2) GET /item/42 (sin queries)
  - path: /item/{item_id}
    method: GET
    title: Item por ID (42)
    description: Devuelve datos del item con valores por defecto
    parameters:
      - name: item_id
        in: path
        required: true
        schema:
          type: integer
          example: 42

  # 3) GET /item/10?q=busqueda&activo=false
  - path: /item/{item_id}
    method: GET
    title: Item por ID (10) con queries
    description: Devuelve datos del item con consultas específicas
    parameters:
      - name: item_id
        in: path
        required: true
        schema:
          type: integer
          example: 10
      - name: q
        in: query
        required: false
        schema:
          type: string
          example: "busqueda"
      - name: activo
        in: query
        required: false
        schema:
          type: boolean
          example: false

  # 4) GET /saludo/María
  - path: /saludo/{nombre}
    method: GET
    title: Saludo María
    description: Devuelve un saludo personalizado
    parameters:
      - name: nombre
        in: path
        required: true
        schema:
          type: string
          example: "María"

  # 5) GET /config?modo=dev&version=2.5
  - path: /config
    method: GET
    title: Configuración dev 2.5
    description: Devuelve configuración con queries específicas
    parameters:
      - name: modo
        in: query
        required: false
        schema:
          type: string
          example: "dev"
      - name: version
        in: query
        required: false
        schema:
          type: number
          example: 2.5

  # 6) GET /item/0?q=&activo=true
  - path: /item/{item_id}
    method: GET
    title: Item ID 0 con q vacía
    description: Devuelve datos del item con q vacía y activo true
    parameters:
      - name: item_id
        in: path
        required: true
        schema:
          type: integer
          example: 0
      - name: q
        in: query
        required: false
        schema:
          type: string
          example: ""
      - name: activo
        in: query
        required: false
        schema:
          type: boolean
          example: true

  # 7) GET /saludo/José%20Luis
  - path: /saludo/{nombre}
    method: GET
    title: Saludo José Luis
    description: Devuelve un saludo con espacios en el nombre
    parameters:
      - name: nombre
        in: path
        required: true
        schema:
          type: string
          example: "José Luis"

  # 8) GET /config (sin queries)
  - path: /config
    method: GET
    title: Configuración por defecto
    description: Devuelve configuración con valores por defecto

  # 9) GET /item/-7 (ID negativo)
  - path: /item/{item_id}
    method: GET
    title: Item ID negativo (-7)
    description: Devuelve datos del item con ID negativo
    parameters:
      - name: item_id
        in: path
        required: true
        schema:
          type: integer
          example: -7

  # 10) GET /config?modo=test&version=abc (error esperado)
  - path: /config
    method: GET
    title: Configuración error en version
    description: Envía version no numérica para provocar error de validación
    parameters:
      - name: modo
        in: query
        required: false
        schema:
          type: string
          example: "test"
      - name: version
        in: query
        required: false
        schema:
          type: string
          example: "abc"
---
````

## Formato de entrega
- Enlace al repositorio GitHub (público).
- Nombre sugerido: `sena-fastapi-actividad3`.
- Debe incluir: `main.py`, `README.md` con instrucciones de ejecución y descripción de endpoints.
- Evidencias en Google Drive por el líder del grupo.

