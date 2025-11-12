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

## 📝 Parámetros de ruta y consulta

En FastAPI, los **parámetros de ruta** (*path parameters*) y los **parámetros de consulta** (*query parameters*) son dos formas distintas de pasar datos a una API RESTful, cada una con propósitos y características específicas. A continuación, explico sus diferencias fundamentales, cuándo usar cada uno y ejemplos prácticos.

### **Diferencias fundamentales**

1. **Ubicación en la URL**:
   - **Parámetros de ruta**: Forman parte de la estructura de la URL, definidos dentro del path de la ruta. Se indican entre llaves `{}` en la definición de la ruta.
     - Ejemplo: `/mascotas/{mascota_id}` → `mascota_id` es un parámetro de ruta.
   - **Parámetros de consulta**: Se añaden al final de la URL después de un signo de interrogación (`?`), en formato clave-valor (`clave=valor`), separados por `&` si hay varios.
     - Ejemplo: `/mascotas?especie=perro&vacunado=true` → `especie` y `vacunado` son parámetros de consulta.

2. **Propósito**:
   - **Parámetros de ruta**: Identifican un recurso específico o único en la API. Son esenciales para definir la jerarquía o estructura de los recursos.
     - Ejemplo: `/mascotas/123` identifica la mascota con ID 123.
   - **Parámetros de consulta**: Filtran, ordenan o personalizan la respuesta de un recurso o conjunto de recursos. No son parte de la identidad del recurso, sino que modifican cómo se devuelve.
     - Ejemplo: `/mascotas?especie=perro` filtra la lista de mascotas para mostrar solo perros.

3. **Obligatoriedad**:
   - **Parámetros de ruta**: Son obligatorios, ya que forman parte del path. Si no se proporcionan, la ruta no coincide y la solicitud falla (404).
     - Ejemplo: Si defines `/mascotas/{mascota_id}`, el cliente debe proporcionar un valor para `mascota_id`.
   - **Parámetros de consulta**: Son opcionales por defecto, a menos que se especifique lo contrario (por ejemplo, con validaciones en FastAPI).
     - Ejemplo: En `/mascotas?especie=perro`, el parámetro `especie` puede omitirse.

4. **Tipado y validación**:
   - Ambos permiten tipado y validación en FastAPI, pero los **parámetros de ruta** suelen ser más estrictos, ya que identifican recursos específicos (por ejemplo, un ID como entero).
   - Los **parámetros de consulta** suelen ser más flexibles, ya que se usan para filtrar o configurar (por ejemplo, cadenas, booleanos, listas).

5. **Semántica REST**:
   - **Parámetros de ruta**: Reflejan la jerarquía de recursos en una API RESTful, alineándose con la idea de identificar recursos únicos.
   - **Parámetros de consulta**: Ajustan o filtran la representación de un recurso, siguiendo las convenciones de REST para personalizar respuestas.

### **Cuándo y por qué usar cada uno**

- **Usa parámetros de ruta** cuando:
  - Necesitas identificar un recurso específico o único.
  - La información es esencial para localizar el recurso en la estructura de la API.
  - Quieres que la URL sea clara y refleje la jerarquía de recursos.
  - Ejemplo: Obtener los detalles de una mascota específica por su ID.

- **Usa parámetros de consulta** cuando:
  - Quieres filtrar, ordenar o personalizar una lista de recursos o una respuesta.
  - Los parámetros son opcionales o tienen valores por defecto.
  - La información no es parte de la identidad del recurso, sino un ajuste a la respuesta.
  - Ejemplo: Filtrar una lista de mascotas por especie o estado de vacunación.

### **Ejemplos en FastAPI**

#### **Ejemplo 1: Parámmetro de ruta**
Supongamos que queremos obtener los detalles de una mascota específica por su ID.

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/mascotas/{mascota_id}")
async def obtener_mascota(mascota_id: int):
    return {"mensaje": f"Mascota con ID {mascota_id} obtenida"}
```

- **Explicación**:
  - La ruta `/mascotas/{mascota_id}` usa `mascota_id` como parámetro de ruta.
  - Es obligatorio y se espera un entero (`int`).
  - Ejemplo de solicitud: `GET /mascotas/123` → Respuesta: `{"mensaje": "Mascota con ID 123 obtenida"}`.
  - **Por qué usarlo**: El ID identifica un recurso único (una mascota específica), y la ruta refleja claramente la estructura del recurso.

#### **Ejemplo 2: Parámmetro de consulta**
Supongamos que queremos obtener una lista de mascotas, filtrada opcionalmente por especie.

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/mascotas")
async def listar_mascotas(especie: str | None = None):
    if especie:
        return {"mensaje": f"Lista de mascotas de especie {especie}"}
    return {"mensaje": "Lista de todas las mascotas"}
```

- **Explicación**:
  - La ruta `/mascotas` acepta un parámetro de consulta opcional `especie`.
  - Se define como `str | None` con valor por defecto `None`, lo que lo hace opcional.
  - Ejemplo de solicitud:
    - `GET /mascotas` → Respuesta: `{"mensaje": "Lista de todas las mascotas"}`.
    - `GET /mascotas?especie=perro` → Respuesta: `{"mensaje": "Lista de mascotas de especie perro"}`.
  - **Por qué usarlo**: El parámetro `especie` filtra la lista de mascotas, pero no identifica un recurso único. Es opcional y ajusta la respuesta.


