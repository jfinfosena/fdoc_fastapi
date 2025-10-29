---
title: "Actividad 4: Validación con Pydantic y Request Body"
position: 4
date: 2025-10-28
---

Esta actividad está basada en el contenido de:

```cards
---
columns: 1
items:
  - title: "FastAPI y Pydantic"
    icon: "RocketIcon"
    href: "/01-contenido/04-contenido.md"
    content: "FastAPI y Pydantic: validación con modelos BaseModel y tipos Optional, List, Union."
---
```


Los ejemplos aquí propuestos son distintos a los del tutorial para que apliques los mismos conceptos con otros casos.

## Objetivo
- Construir endpoints que reciban y validen datos con modelos Pydantic (`BaseModel`).
- Practicar tipos `Optional`, `List`, `Union` y validación automática (422) en FastAPI.
- Probar las rutas desde `/docs` y con el bloque interactivo.

## Alcance y restricciones
- No pegues soluciones completas; implementa el código en tu proyecto local.
- Mantén la estructura mínima con `FastAPI()` y ejecuta `uvicorn`.
- Puedes agregar `CORSMiddleware` si necesitas probar desde un frontend.

## Requisitos de la API (ejemplos diferentes al tutorial)
Implementa las siguientes rutas, usando modelos propios (nombres y campos como se indican):

```steps
### 1) GET /
Devuelve un mensaje de bienvenida de tu API de ejemplo.

### 2) POST /productos/
Crea un producto.

Modelo sugerido: `Producto(BaseModel)` con `nombre: str`, `precio: float`, `en_stock: bool`, `tags: Optional[List[str]] = None`.
Respuesta sugerida: confirma creación y eco de datos válidos.

### 3) GET /productos/buscar/
Búsqueda por consulta.

Query: `nombre: str`, `categoria: Optional[str] = None`.
Devuelve un objeto con los filtros recibidos.

### 4) POST /orden/
Recibe una orden de compra.

Modelo sugerido: `Orden(BaseModel)` con `cliente: str`, `items: List[str]`.
Devuelve el número de items y el cliente.

### 5) POST /respuesta/
Demuestra `Union`.

Modelo sugerido: `Respuesta(BaseModel)` con `valor: Union[str, int]`.
Devuelve el tipo detectado y el valor.

### 6) POST /perfil/
Modelo con campos opcionales.

Modelo sugerido: `Perfil(BaseModel)` con `usuario: str`, `bio: Optional[str] = None`, `intereses: Optional[List[str]] = None`.
Devuelve un saludo con `usuario` y los campos opcionales.

### 7) POST /calificacion/
Conversión de tipos.

Modelo sugerido: `Calificacion(BaseModel)` con `curso: str`, `nota: float`.
Envía `nota` como texto convertible (ej. "4.5") para observar la conversión.

### 8) POST /configuracion/
Valores por defecto.

Modelo sugerido: `Config(BaseModel)` con `modo: str = "produccion"`, `version: float = 1.0`.
Permite enviar vacío para recibir valores por defecto.

### 9) POST /validacion/estricta
Error esperado 422.

Modelo sugerido: `Estricto(BaseModel)` con `cantidad: int`.
Envía un valor inválido (por ejemplo, `"cantidad": "abc"`) y verifica el error.
```

## Pasos sugeridos (sin soluciones completas)
1. Crea `main.py` y define `app = FastAPI()`.
2. Declara los modelos Pydantic indicados arriba.
3. Implementa las rutas con las firmas solicitadas y devuelve respuestas JSON claras.
4. Ejecuta: `uvicorn main:app --reload`.
5. Prueba en `http://localhost:8000/docs` y valida respuestas y errores 422.
6. (Opcional) Habilita CORS si tu frontend hace solicitudes a `localhost:8000`.

## Pruebas esperadas (I/O de ejemplo)
Estas pruebas no son los únicos casos posibles; te ayudan a validar comportamientos clave.

| Solicitud | Cuerpo/Query | Resultado esperado (forma) |
|---|---|---|
| `GET /` | – | `{ "mensaje": "Bienvenido..." }` |
| `POST /productos/` | `{ "nombre": "Teclado", "precio": 99.9, "en_stock": true, "tags": ["periférico"] }` | Eco de datos válidos y confirmación |
| `GET /productos/buscar/?nombre=monitor&categoria=oficina` | – | `{ "nombre": "monitor", "categoria": "oficina" }` |
| `POST /orden/` | `{ "cliente": "Sofía", "items": ["A1", "B2"] }` | `{ "total_items": 2, "cliente": "Sofía" }` |
| `POST /respuesta/` | `{ "valor": 123 }` | `{ "tipo": "int", "valor": 123 }` |
| `POST /respuesta/` | `{ "valor": "ok" }` | `{ "tipo": "str", "valor": "ok" }` |
| `POST /perfil/` | `{ "usuario": "leo", "bio": "hola" }` | Incluye `usuario` y `bio`, `intereses` ausente o `null` |
| `POST /calificacion/` | `{ "curso": "Python", "nota": "4.5" }` | `nota` convertida a número `4.5` |
| `POST /validacion/estricta` | `{ "cantidad": "abc" }` | `422 Unprocessable Entity` con detalle de tipo inválido |

## Probar la API (interactivo)
Usa el bloque para enviar solicitudes rápidamente mientras desarrollas.

````api-explorer
---
baseUrl: http://localhost:8000
endpoints:
  - path: /
    method: GET
    title: Estado API
    description: Verifica que el backend responde

  - path: /productos/
    method: POST
    title: Crear producto
    description: Crea un producto de ejemplo
    headers:
      - name: Content-Type
        value: application/json
    body: |
      {
        "nombre": "Teclado",
        "precio": 99.9,
        "en_stock": true,
        "tags": ["periférico"]
      }

  - path: /productos/buscar/
    method: GET
    title: Buscar productos
    description: Demuestra parámetros de consulta
    parameters:
      - name: nombre
        in: query
        required: true
        schema:
          type: string
          example: "monitor"
      - name: categoria
        in: query
        required: false
        schema:
          type: string
          example: "oficina"

  - path: /orden/
    method: POST
    title: Crear orden
    description: Envía items de compra
    headers:
      - name: Content-Type
        value: application/json
    body: |
      {
        "cliente": "Sofía",
        "items": ["A1", "B2"]
      }

  - path: /respuesta/
    method: POST
    title: Respuesta Union int
    headers:
      - name: Content-Type
        value: application/json
    body: |
      { "valor": 123 }

  - path: /respuesta/
    method: POST
    title: Respuesta Union str
    headers:
      - name: Content-Type
        value: application/json
    body: |
      { "valor": "ok" }

  - path: /perfil/
    method: POST
    title: Crear perfil opcional
    headers:
      - name: Content-Type
        value: application/json
    body: |
      {
        "usuario": "leo",
        "bio": "hola"
      }

  - path: /calificacion/
    method: POST
    title: Calificación con conversión
    headers:
      - name: Content-Type
        value: application/json
    body: |
      {
        "curso": "Python",
        "nota": "4.5"
      }

  - path: /validacion/estricta
    method: POST
    title: Error 422 esperado
    headers:
      - name: Content-Type
        value: application/json
    body: |
      {
        "cantidad": "abc"
      }
---
````

## Formato de entrega
- Enlace al repositorio GitHub (público).
- Nombre sugerido: `sena-fastapi-actividad4`.
- Debe incluir: `main.py`, `README.md` con instrucciones de ejecución y descripción de endpoints.
- Evidencias en Google Drive por el líder del grupo.

