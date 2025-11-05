---
title: "Principios REST"
position: 5
date: 2025-11-05
---

Antes de sumergirnos en los ejemplos de FastAPI, es clave entender qué hace que una API sea verdaderamente RESTful. A continuación, se presento los **6 principios REST** que actúan como la brújula para diseñar servicios web robustos, escalables y fáciles de mantener. Conocerlos te permitirá aprovechar al máximo las capacidades de FastAPI y construir APIs que se comporten exactamente como los clientes esperan.

## Requisitos previos

1. Tener **Python 3.7+** instalado.
2. Instalar FastAPI y Uvicorn:

```bash
pip install fastapi uvicorn
```


## ¿Qué es REST?

**REST** (Representational State Transfer) es un estilo arquitectónico para diseñar APIs.  
Se basa en **6 principios clave**:

1. **Cliente-Servidor**
2. **Sin estado (Stateless)**
3. **Cacheable**
4. **Interfaz uniforme**
5. **Sistema en capas**
6. **Código bajo demanda** *(opcional)*

Vamos a ver **cada uno con un ejemplo claro en FastAPI**.

---

## 1. Cliente-Servidor

> **Separación clara entre cliente (frontend, móvil, etc.) y servidor (API).**

El cliente **no sabe cómo funciona el servidor**, solo hace peticiones HTTP.

### Ejemplo: Cliente pide datos, servidor responde

```python
# main.py
from fastapi import FastAPI

app = FastAPI()

@app.get("/saludo")
def saludar():
    return {"mensaje": "¡Hola desde el servidor!"}
```

**Prueba con cliente (navegador o `curl`):**

```bash
curl http://127.0.0.1:8000/saludo
# → {"mensaje": "¡Hola desde el servidor!"}
```

> El cliente solo pide, el servidor responde. ¡Separación clara!

---

## 2. Sin estado (Stateless)

> **Cada petición contiene toda la información necesaria. El servidor no guarda estado entre peticiones.**

No hay sesiones en el servidor.

### Ejemplo: Autenticación por token en cada petición

```python
from fastapi import FastAPI, HTTPException

app = FastAPI()

# Simulamos una base de datos de usuarios
USUARIOS = {
    "juan": {"nombre": "Juan", "rol": "admin"},
    "ana": {"nombre": "Ana", "rol": "usuario"}
}

@app.get("/perfil/{usuario_id}")
def obtener_perfil(usuario_id: str, token: str):
    if token != "token-secreto-123":
        raise HTTPException(status_code=401, detail="Token inválido")
    
    if usuario_id not in USUARIOS:
        raise HTTPException(status_code=404, detail="Usuario no encontrado")
    
    return USUARIOS[usuario_id]
```

**Prueba:**

```bash
# Correcto
curl "http://127.0.0.1:8000/perfil/juan?token=token-secreto-123"

# → {"nombre": "Juan", "rol": "admin"}

# Token malo
curl "http://127.0.0.1:8000/perfil/juan?token=mal"
# → 401: Token inválido
```

> El servidor **no recuerda** al usuario. Cada petición trae el `token`.

---

## 3. Cacheable

> **Las respuestas deben indicar si se pueden almacenar en caché.**

Usamos encabezados como `Cache-Control`.

### Ejemplo: Respuesta cacheable

```python
from fastapi import FastAPI
from fastapi.responses import JSONResponse

app = FastAPI()

@app.get("/datos-publicos")
def datos_publicos():
    headers = {"Cache-Control": "max-age=3600"}  # 1 hora en caché
    return JSONResponse(
        content={"dato": "Este dato no cambia mucho"},
        headers=headers
    )
```

> El cliente (navegador, CDN) puede guardar esta respuesta por 1 hora.

---

## 4. Interfaz uniforme

> **Reglas claras para interactuar con la API:**

- Recursos identificados por **URLs** (URIs)
- Uso de **métodos HTTP** (GET, POST, PUT, DELETE)
- **Representaciones** (normalmente JSON)

### Ejemplo: CRUD de libros

```python
from fastapi import FastAPI, HTTPException
from pydantic import BaseModel
from typing import List

app = FastAPI()

# Modelo del recurso
class Libro(BaseModel):
    id: int
    titulo: str
    autor: str

# Base de datos en memoria
libros_db = [
    Libro(id=1, titulo="1984", autor="George Orwell"),
    Libro(id=2, titulo="Cien años de soledad", autor="García Márquez")
]

# GET: Listar todos
@app.get("/libros", response_model=List[Libro])
def listar_libros():
    return libros_db

# GET: Uno por ID
@app.get("/libros/{libro_id}", response_model=Libro)
def obtener_libro(libro_id: int):
    for libro in libros_db:
        if libro.id == libro_id:
            return libro
    raise HTTPException(status_code=404, detail="Libro no encontrado")

# POST: Crear
@app.post("/libros", response_model=Libro, status_code=201)
def crear_libro(libro: Libro):
    for l in libros_db:
        if l.id == libro.id:
            raise HTTPException(status_code=400, detail="ID ya existe")
    libros_db.append(libro)
    return libro

# PUT: Actualizar
@app.put("/libros/{libro_id}", response_model=Libro)
def actualizar_libro(libro_id: int, datos: Libro):
    for i, libro in enumerate(libros_db):
        if libro.id == libro_id:
            libros_db[i] = datos
            return datos
    raise HTTPException(status_code=404, detail="Libro no encontrado")

# DELETE: Eliminar
@app.delete("/libros/{libro_id}", status_code=204)
def eliminar_libro(libro_id: int):
    for i, libro in enumerate(libros_db):
        if libro.id == libro_id:
            libros_db.pop(i)
            return
    raise HTTPException(status_code=404, detail="Libro no encontrado")
```

**Pruebas rápidas:**

```bash
# Listar
curl http://127.0.0.1:8000/libros

# Crear
curl -X POST http://127.0.0.1:8000/libros \
     -H "Content-Type: application/json" \
     -d '{"id": 3, "titulo": "El principito", "autor": "Saint-Exupéry"}'

# Actualizar
curl -X PUT http://127.0.0.1:8000/libros/3 \
     -H "Content-Type: application/json" \
     -d '{"id": 3, "titulo": "El principito", "autor": "Antoine"}'
```

> ¡Interfaz uniforme! Mismos patrones para todos los recursos.

---

## 5. Sistema en capas

> **El cliente no sabe si se conecta directamente al servidor final o a un intermediario (proxy, load balancer, caché).**

FastAPI no controla esto directamente, pero **lo soporta perfectamente**.

### Ejemplo: API detrás de un proxy

Tu API puede estar:

```
Cliente → Nginx (proxy) → FastAPI → Base de datos
```

FastAPI no cambia su código. Solo responde a peticiones HTTP.

> **Tú no necesitas hacer nada en FastAPI**. Solo saber que es posible.

---

## 6. Código bajo demanda (Code on Demand) - *Opcional*

> **El servidor puede enviar código ejecutable al cliente (ej. JavaScript).**

Rara vez usado en APIs REST puras, pero posible.

### Ejemplo: Devolver un script

```python
from fastapi import FastAPI
from fastapi.responses import HTMLResponse

app = FastAPI()

@app.get("/widget", response_class=HTMLResponse)
def obtener_widget():
    return """
    <div id="mi-widget">Cargando...</div>
    <script>
      document.getElementById("mi-widget").innerText = "¡Hola desde el servidor!";
    </script>
    """
```

> El cliente recibe HTML + JS que se ejecuta. ¡Código bajo demanda!

---

## Ejecuta la API

Guarda todo en `main.py` y ejecuta:

```bash
uvicorn main:app --reload
```

Abre: [http://127.0.0.1:8000/docs](http://127.0.0.1:8000/docs)  
→ ¡Documentación interactiva automática con Swagger!

---

## Principios REST con FastAPI

| Principio | ¿Cumplido? | Ejemplo en FastAPI |
|---------|-----------|---------------------|
| 1. Cliente-Servidor | Sí | `@app.get("/saludo")` |
| 2. Sin estado | Sí | Token en cada petición |
| 3. Cacheable | Sí | `Cache-Control` en headers |
| 4. Interfaz uniforme | Sí | CRUD con `/libros` |
| 5. Sistema en capas | Sí | Compatible con proxies |
| 6. Código bajo demanda | Sí | Devolver JS/HTML |

---





