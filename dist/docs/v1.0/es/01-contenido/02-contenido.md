---
title: "FastAPI: Rutas y Operaciones HTTP"
position: 2
date: 2025-10-22
---

# FastAPI: Rutas y Operaciones HTTP (Básico)

FastAPI es un framework web moderno y de alto rendimiento para construir APIs con Python. En este tutorial aprenderás a crear rutas básicas y manejar diferentes operaciones HTTP de manera simple.

## ¿Qué son las Rutas en FastAPI?

Las rutas son los endpoints de tu API que definen cómo responder a las solicitudes del cliente. Cada ruta está asociada a una función que se ejecuta cuando se accede a esa URL específica.

```admonition
---
type: info
title: Conceptos Clave
---
- **Ruta**: La URL específica donde tu API responde (ej: `/usuarios`, `/productos`)
- **Operación HTTP**: El método usado para interactuar con la ruta (GET, POST, PUT, DELETE, etc.)
- **Función de ruta**: La función Python que maneja la lógica de la ruta
- **Decorador**: La sintaxis `@app.get()` que conecta la función con la ruta
```

## Configuración Inicial

Antes de crear rutas, necesitas configurar FastAPI de forma simple:

```python
from fastapi import FastAPI
import uvicorn

# Crear la instancia de FastAPI
app = FastAPI()

# Ejecutar la aplicación
if __name__ == "__main__":
    uvicorn.run(app, host="0.0.0.0", port=8000)
```

## Decoradores en FastAPI para Métodos HTTP

En FastAPI, los **decoradores** son una forma elegante y simple de definir rutas (path operations) en tu API web. Estos decoradores se aplican directamente sobre funciones de Python y especifican tanto la ruta (path) como el método HTTP que la función manejará. La sintaxis general es `@app.método_http("/ruta")`, donde `app` es una instancia de la clase `FastAPI`.

FastAPI hereda de Starlette y convierte automáticamente las respuestas (como diccionarios, listas o cadenas) en JSON. Las funciones pueden ser síncronas (`def`) o asíncronas (`async def`), lo que las hace ideales para aplicaciones de alto rendimiento.

## Métodos HTTP Comunes y Sus Decoradores

FastAPI soporta todos los métodos HTTP estándar. Aquí va una tabla con los más usados, su propósito típico y el decorador correspondiente:

| Método HTTP | Decorador              | Uso Típico                          |
|-------------|------------------------|-------------------------------------|
| **GET**    | `@app.get(path)`      | Leer datos (consultas).             |
| **POST**   | `@app.post(path)`     | Crear nuevos datos.                 |
| **PUT**    | `@app.put(path)`      | Actualizar datos existentes (reemplazo completo). |
| **DELETE** | `@app.delete(path)`   | Eliminar datos.                     |
| **PATCH**  | `@app.patch(path)`    | Actualizar parcialmente datos.      |
| **OPTIONS**| `@app.options(path)`  | Describir opciones de comunicación. |
| **HEAD**   | `@app.head(path)`     | Obtener metadatos (sin cuerpo).     |
| **TRACE**  | `@app.trace(path)`    | Depuración (eco de la solicitud).   |

## Operaciones HTTP Básicas

### 1. GET - Obtener Datos

La operación GET se usa para recuperar información del servidor:

```python
# Lista simple de usuarios (simulada)
usuarios_db = [
    {"id": 1, "nombre": "Juan", "email": "juan@email.com"},
    {"id": 2, "nombre": "María", "email": "maria@email.com"},
    {"id": 3, "nombre": "Carlos", "email": "carlos@email.com"}
]

@app.get("/")
def root():
    """Ruta raíz de la API"""
    return {"mensaje": "¡Bienvenido a mi API con FastAPI!"}

@app.get("/usuarios")
def obtener_usuarios():
    """Obtener todos los usuarios"""
    return {"usuarios": usuarios_db}

@app.get("/usuarios/{usuario_id}")
def obtener_usuario(usuario_id: int):
    """Obtener un usuario específico por ID"""
    for usuario in usuarios_db:
        if usuario["id"] == usuario_id:
            return {"usuario": usuario}
    
    return {"error": f"Usuario con ID {usuario_id} no encontrado"}

@app.get("/salud")
def verificar_salud():
    """Endpoint para verificar el estado de la API"""
    return {
        "estado": "activo",
        "mensaje": "La API está funcionando correctamente"
    }
```

### 2. POST - Crear Datos

La operación POST se utiliza para enviar datos al servidor y crear nuevos recursos:

```python
# Variable para generar IDs únicos
contador_id = 4  # Empezamos en 4 porque ya tenemos 3 usuarios

@app.post("/usuarios")
def crear_usuario(nombre: str, email: str):
    """Crear un nuevo usuario"""
    global contador_id
    
    # Verificar si el email ya existe
    for usuario in usuarios_db:
        if usuario["email"] == email:
            return {"error": "El email ya está registrado"}
    
    # Crear nuevo usuario
    nuevo_usuario = {
        "id": contador_id,
        "nombre": nombre,
        "email": email
    }
    
    usuarios_db.append(nuevo_usuario)
    contador_id += 1
    
    return {"mensaje": "Usuario creado exitosamente", "usuario": nuevo_usuario}

@app.post("/productos")
def crear_producto(nombre: str, precio: float):
    """Crear un producto simple"""
    producto = {
        "id": 1,
        "nombre": nombre,
        "precio": precio
    }
    
    return {"mensaje": "Producto creado exitosamente", "producto": producto}
```

### 3. PUT - Actualizar Completamente

PUT se usa para actualizar completamente un recurso existente:

```python
@app.put("/usuarios/{usuario_id}")
def actualizar_usuario_completo(usuario_id: int, nombre: str, email: str):
    """Actualizar completamente un usuario existente"""
    
    # Buscar el usuario
    for i, usuario in enumerate(usuarios_db):
        if usuario["id"] == usuario_id:
            # Actualizar completamente
            usuarios_db[i] = {
                "id": usuario_id,
                "nombre": nombre,
                "email": email
            }
            return {"mensaje": "Usuario actualizado", "usuario": usuarios_db[i]}
    
    return {"error": f"Usuario con ID {usuario_id} no encontrado"}

@app.put("/productos/{producto_id}")
def actualizar_producto(producto_id: int, nombre: str, precio: float):
    """Actualizar un producto completamente"""
    producto_actualizado = {
        "id": producto_id,
        "nombre": nombre,
        "precio": precio
    }
    
    return {"mensaje": "Producto actualizado", "producto": producto_actualizado}
```

### 4. DELETE - Eliminar Datos

DELETE se usa para eliminar recursos del servidor:

```python
@app.delete("/usuarios/{usuario_id}")
def eliminar_usuario(usuario_id: int):
    """Eliminar un usuario por ID"""
    
    # Buscar y eliminar usuario
    for i, usuario in enumerate(usuarios_db):
        if usuario["id"] == usuario_id:
            usuario_eliminado = usuarios_db.pop(i)
            return {"mensaje": "Usuario eliminado", "usuario": usuario_eliminado}
    
    return {"error": f"Usuario con ID {usuario_id} no encontrado"}

@app.delete("/productos/{producto_id}")
def eliminar_producto(producto_id: int):
    """Eliminar un producto por ID"""
    return {"mensaje": f"Producto {producto_id} eliminado exitosamente"}
```

## Ejemplo Completo de Aplicación

Aquí tienes un ejemplo completo que puedes copiar y ejecutar:

```python
from fastapi import FastAPI
import uvicorn

# Crear la instancia de FastAPI
app = FastAPI()

# Lista simple de usuarios (simulada)
usuarios_db = [
    {"id": 1, "nombre": "Juan", "email": "juan@email.com"},
    {"id": 2, "nombre": "María", "email": "maria@email.com"},
    {"id": 3, "nombre": "Carlos", "email": "carlos@email.com"}
]

# Variable para generar IDs únicos
contador_id = 4

# Rutas GET
@app.get("/")
def root():
    return {"mensaje": "¡Bienvenido a mi API con FastAPI!"}

@app.get("/usuarios")
def obtener_usuarios():
    return {"usuarios": usuarios_db}

@app.get("/usuarios/{usuario_id}")
def obtener_usuario(usuario_id: int):
    for usuario in usuarios_db:
        if usuario["id"] == usuario_id:
            return {"usuario": usuario}
    return {"error": f"Usuario con ID {usuario_id} no encontrado"}

# Rutas POST
@app.post("/usuarios")
def crear_usuario(nombre: str, email: str):
    global contador_id
    
    # Verificar si el email ya existe
    for usuario in usuarios_db:
        if usuario["email"] == email:
            return {"error": "El email ya está registrado"}
    
    # Crear nuevo usuario
    nuevo_usuario = {
        "id": contador_id,
        "nombre": nombre,
        "email": email
    }
    
    usuarios_db.append(nuevo_usuario)
    contador_id += 1
    
    return {"mensaje": "Usuario creado exitosamente", "usuario": nuevo_usuario}

# Rutas PUT
@app.put("/usuarios/{usuario_id}")
def actualizar_usuario(usuario_id: int, nombre: str, email: str):
    for i, usuario in enumerate(usuarios_db):
        if usuario["id"] == usuario_id:
            usuarios_db[i] = {
                "id": usuario_id,
                "nombre": nombre,
                "email": email
            }
            return {"mensaje": "Usuario actualizado", "usuario": usuarios_db[i]}
    
    return {"error": f"Usuario con ID {usuario_id} no encontrado"}

# Rutas DELETE
@app.delete("/usuarios/{usuario_id}")
def eliminar_usuario(usuario_id: int):
    for i, usuario in enumerate(usuarios_db):
        if usuario["id"] == usuario_id:
            usuario_eliminado = usuarios_db.pop(i)
            return {"mensaje": "Usuario eliminado", "usuario": usuario_eliminado}
    
    return {"error": f"Usuario con ID {usuario_id} no encontrado"}

# Ejecutar la aplicación
if __name__ == "__main__":
    uvicorn.run(app, host="0.0.0.0", port=8000)
```

## Códigos de Estado HTTP

FastAPI permite especificar códigos de estado HTTP para diferentes situaciones:

```python
from fastapi import status

# Ejemplos de códigos de estado comunes
@app.get("/estado-ok", status_code=status.HTTP_200_OK)
def respuesta_ok():
    return {"mensaje": "Todo está bien"}

@app.post("/crear-recurso", status_code=status.HTTP_201_CREATED)
def crear_recurso():
    return {"mensaje": "Recurso creado"}

@app.put("/actualizar-recurso", status_code=status.HTTP_200_OK)
def actualizar_recurso():
    return {"mensaje": "Recurso actualizado"}

@app.delete("/eliminar-recurso", status_code=status.HTTP_204_NO_CONTENT)
def eliminar_recurso():
    # 204 No Content no retorna cuerpo de respuesta
    pass

# Manejo de errores con códigos específicos
@app.get("/recurso-no-encontrado")
def recurso_no_encontrado():
    raise HTTPException(
        status_code=status.HTTP_404_NOT_FOUND,
        detail="El recurso solicitado no existe"
    )

@app.post("/error-validacion")
def error_validacion():
    raise HTTPException(
        status_code=status.HTTP_422_UNPROCESSABLE_ENTITY,
        detail="Los datos proporcionados no son válidos"
    )
```

## Buenas Prácticas

Aquí tienes algunas recomendaciones para escribir mejor código con FastAPI:

```admonition
---
type: tip
title: Consejos Importantes
---
1. **Usa nombres descriptivos** para tus rutas y funciones
2. **Agrega documentación** con docstrings en tus funciones
3. **Valida los datos** antes de procesarlos
4. **Maneja errores** de forma consistente
5. **Organiza tu código** en módulos separados cuando crezca
```

### Ejemplo de Buenas Prácticas

```python
from fastapi import FastAPI

app = FastAPI(
    title="Mi API de Usuarios",
    description="Una API simple para gestionar usuarios",
    version="1.0.0"
)

# Base de datos simulada
usuarios_db = []
contador_id = 1

@app.get("/", tags=["General"])
def obtener_informacion_api():
    """
    Obtiene información básica de la API
    """
    return {
        "nombre": "API de Usuarios",
        "version": "1.0.0",
        "estado": "activo"
    }

@app.get("/usuarios", tags=["Usuarios"])
def listar_usuarios():
    """
    Obtiene la lista completa de usuarios registrados
    """
    return {
        "usuarios": usuarios_db,
        "total": len(usuarios_db)
    }

@app.post("/usuarios", tags=["Usuarios"])
def registrar_usuario(nombre: str, email: str):
    """
    Registra un nuevo usuario en el sistema
    
    - **nombre**: Nombre completo del usuario
    - **email**: Dirección de correo electrónico única
    """
    global contador_id
    
    # Validar que el email no esté vacío
    if not email or "@" not in email:
        return {"error": "Email inválido"}
    
    # Verificar email único
    for usuario in usuarios_db:
        if usuario["email"] == email:
            return {"error": "El email ya está registrado"}
    
    # Crear usuario
    nuevo_usuario = {
        "id": contador_id,
        "nombre": nombre,
        "email": email
    }
    
    usuarios_db.append(nuevo_usuario)
    contador_id += 1
    
    return {
        "mensaje": "Usuario registrado exitosamente",
        "usuario": nuevo_usuario
    }
```

## Probar la API (interactivo)

Usa el explorador interactivo para enviar solicitudes a tu API mientras sigues el tutorial. Edita los parámetros, envía la petición y observa la respuesta.

````api-explorer
---
baseUrl: http://localhost:8000
endpoints:
  - path: /
    method: GET
    title: Estado de la API
    description: Verifica que la API esté activa
  - path: /usuarios
    method: GET
    title: Listar usuarios
    description: Obtiene todos los usuarios registrados
  - path: /usuarios/{usuario_id}
    method: GET
    title: Obtener usuario por ID
    description: Busca un usuario específico
    parameters:
      - name: usuario_id
        in: path
        description: ID del usuario
        required: true
        schema:
          type: integer
          example: 1
  - path: /usuarios
    method: POST
    title: Crear usuario
    description: Crea un nuevo usuario usando parámetros de consulta
    parameters:
      - name: nombre
        in: query
        description: Nombre del usuario
        required: true
        schema:
          type: string
          example: "Ana Pérez"
      - name: email
        in: query
        description: Correo electrónico del usuario
        required: true
        schema:
          type: string
          example: "ana@example.com"
  - path: /usuarios/{usuario_id}
    method: PUT
    title: Actualizar usuario
    description: Actualiza completamente nombre y email
    parameters:
      - name: usuario_id
        in: path
        description: ID del usuario
        required: true
        schema:
          type: integer
          example: 1
      - name: nombre
        in: query
        description: Nuevo nombre
        required: true
        schema:
          type: string
          example: "Ana Actualizada"
      - name: email
        in: query
        description: Nuevo correo
        required: true
        schema:
          type: string
          example: "ana.actualizada@example.com"
  - path: /usuarios/{usuario_id}
    method: DELETE
    title: Eliminar usuario
    description: Elimina un usuario por su ID
    parameters:
      - name: usuario_id
        in: path
        description: ID del usuario
        required: true
        schema:
          type: integer
          example: 1
---
````


### Recursos Útiles

- **Documentación Oficial**: [https://fastapi.tiangolo.com/](https://fastapi.tiangolo.com/)
- **Tutorial Interactivo**: Usa `http://localhost:8000/docs` cuando ejecutes tu API
- **Ejemplos**: Experimenta modificando el código de este tutorial

## Cómo Ejecutar la Aplicación

Para ejecutar tu aplicación FastAPI:

1. **Instalar FastAPI y Uvicorn**:
```bash
pip install fastapi uvicorn
```

2. **Guardar el código** en un archivo llamado `main.py`

3. **Ejecutar la aplicación**:
```bash
uvicorn main:app --reload
```

4. **Abrir en el navegador**:
   - API: `http://localhost:8000`
   - Documentación automática: `http://localhost:8000/docs`


## Solucionar CORS en FastAPI: Guía Rápida y Sencilla

Si estás viendo errores de **CORS** (Cross-Origin Resource Sharing) al conectar tu frontend (ej. React, Vue) con FastAPI, es común cuando el origen (dominio/puerto) del cliente difiere del servidor. FastAPI lo resuelve fácil con un **middleware** dedicado.

## ¿Qué es CORS y por qué falla?
- **CORS** es una política de seguridad del navegador que bloquea requests de orígenes "cruzados" (ej. `http://localhost:3000` llamando a `http://localhost:8000`).
- Error típico: "Access to XMLHttpRequest at '...' from origin '...' has been blocked by CORS policy".
- Solución: Configura FastAPI para permitir orígenes específicos.

## Instalación
Necesitas instalar el middleware (viene en FastAPI, pero confirma):

```bash
pip install "fastapi[all]"  # Incluye CORSMiddleware
```

## Cómo Agregarlo a Tu App
En tu `main.py`, importa y agrega el middleware **después** de crear la app `FastAPI()`.

Ejemplo básico:

```python
from fastapi import FastAPI
from fastapi.middleware.cors import CORSMiddleware  # Importa aquí

app = FastAPI()

# Configura CORS
app.add_middleware(
    CORSMiddleware,
    allow_origins=["*"],  # O lista específica: ["http://localhost:3000", "https://tuapp.com"]
    allow_credentials=True,
    allow_methods=["*"],  # O ["GET", "POST", "PUT", "DELETE"]
    allow_headers=["*"],  # O ["X-Requested-With", "Content-Type"]
)

@app.get("/")
def read_root():
    return {"Mensaje": "¡CORS resuelto!"}
```

- **allow_origins=["*"]**: Permite **todos** los orígenes (solo para desarrollo; en prod, usa lista específica por seguridad).
- **allow_credentials=True**: Si usas cookies/auth.
- **allow_methods/headers=["*"]**: Permite todos (ajusta para prod).

Corre con `uvicorn main:app --reload` y prueba desde tu frontend. ¡El error debería desaparecer!
