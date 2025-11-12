---
title: "FastAPI y Pydantic"
position: 4
date: 2025-10-28
---


## 🌟 Introducción: ¿Qué son FastAPI y Pydantic?

Imagina que estás en un **restaurante**:

- **FastAPI** es como el **mesero**. Recibe tus pedidos (por ejemplo: "Quiero una pizza") y te trae lo que pediste.
- **Pydantic** es como el **chef que revisa tu pedido**. Antes de cocinar, dice: “¿Estás seguro de que quieres una pizza? ¿Con qué ingredientes? ¿Sin piña, verdad?”. Si tu pedido no tiene sentido (“Quiero una pizza de aire”), el chef **no cocina nada** y le dice al mesero: “¡Este pedido está mal!”.

En programación:

- **FastAPI** es una herramienta para crear **APIs** (una forma en que programas se comunican entre sí por internet).
- **Pydantic** es una herramienta que **verifica que los datos que recibe tengan el formato correcto** (por ejemplo: que un “edad” sea un número, no una palabra).

Juntos, hacen que tu API sea **rápida, segura y fácil de entender**.

---

## 🛠️ Primeros pasos: Instalación y primer servidor

### Paso 1: Crea una carpeta para tu proyecto

En tu computadora, crea una carpeta nueva. Por ejemplo: `mi_primer_api`.

Dentro de esa carpeta, vamos a crear un archivo llamado `main.py`. Puedes usar cualquier editor de texto (como Bloc de notas, VS Code, etc.).

### Paso 2: Instala FastAPI y un servidor

FastAPI necesita un **servidor web** para funcionar. Usaremos `uvicorn`, que es muy rápido y fácil.

Abre tu **terminal** (o símbolo del sistema), ve a la carpeta `mi_primer_api` y escribe:

```bash
pip install fastapi uvicorn
```

> 💡 `pip` es como una tienda de apps para Python. Con `pip install` instalas herramientas.

> ✅ Nota: Pydantic ya viene incluido con FastAPI, ¡así que no necesitas instalarlo por separado!

### Paso 3: Escribe tu primera API

Abre el archivo `main.py` y escribe esto:

```python
# main.py
from fastapi import FastAPI

# Creamos una "app" (nuestra API)
app = FastAPI()

# Definimos una ruta: cuando alguien vaya a la página principal ("/"), 
# le devolvemos un mensaje
@app.get("/")
def hola_mundo():
    return {"mensaje": "¡Hola, mundo!"}
```

### Paso 4: Ejecuta el servidor

En la terminal (dentro de la carpeta `mi_primer_api`), escribe:

```bash
uvicorn main:app --reload
```

> 🔍 ¿Qué significa esto?
> - `main`: el nombre del archivo (`main.py`)
> - `app`: el nombre de la variable que creamos (`app = FastAPI()`)
> - `--reload`: hace que el servidor se reinicie automáticamente si cambias el código

Verás un mensaje como:

```
INFO:     Uvicorn running on http://127.0.0.1:8000
```

Ahora abre tu navegador y ve a: [http://127.0.0.1:8000](http://127.0.0.1:8000)

¡Deberías ver esto!

```json
{"mensaje": "¡Hola, mundo!"}
```

> 💡 Consejo: También puedes ver una interfaz bonita en [http://127.0.0.1:8000/docs](http://127.0.0.1:8000/docs). Es la **documentación automática** de FastAPI (llamada Swagger UI). La usaremos mucho.

---

## 🧱 Entendiendo Pydantic: Los Modelos (`BaseModel`)

Ahora vamos a aprender **Pydantic**.

### ¿Qué es un "modelo"?

Un **modelo** es como una **plantilla** o **molde** para los datos. Por ejemplo:

> “Un usuario debe tener: nombre (texto), edad (número), y si es mayor de edad (sí/no)”.

En Pydantic, creamos esa plantilla con una **clase** que hereda de `BaseModel`.

### Paso 1: Crea tu primer modelo

Agrega esto al final de tu archivo `main.py`:

```python
from pydantic import BaseModel

class Usuario(BaseModel):
    nombre: str
    edad: int
    es_adulto: bool
```

> 🔍 Explicación:
> - `from pydantic import BaseModel`: importamos la herramienta `BaseModel`.
> - `class Usuario(BaseModel)`: creamos una clase llamada `Usuario` que usa `BaseModel`.
> - `nombre: str` → el campo `nombre` debe ser un **texto** (`str` = string).
> - `edad: int` → debe ser un **número entero** (`int` = integer).
> - `es_adulto: bool` → debe ser **verdadero o falso** (`bool` = boolean).

### Paso 2: Prueba el modelo (fuera de FastAPI)

Agrega temporalmente esto al final de `main.py` (solo para probar):

```python
# Solo para ver cómo funciona Pydantic
usuario_prueba = Usuario(nombre="Ana", edad=25, es_adulto=True)
print(usuario_prueba)
print(usuario_prueba.nombre)
```

Guarda el archivo y ejecuta en la terminal:

```bash
python main.py
```

Verás:

```
nombre='Ana' edad=25 es_adulto=True
Ana
```

¡Funciona! Pero si intentas esto:

```python
# Esto daría ERROR (coméntalo o bórralo después de probar):
# usuario_malo = Usuario(nombre=123, edad="veinticinco", es_adulto="sí")
```

Pydantic **gritaría**: “¡El nombre debe ser texto, no número!”.

> ✅ **Conclusión**: Pydantic **fuerza** a que los datos cumplan con la plantilla.

---

## ✨ La Magia de la Validación de Datos

Pydantic no solo revisa tipos, ¡también corrige algunos errores pequeños!

Prueba esto en `main.py` (solo para ver):

```python
usuario_corregido = Usuario(nombre="Luis", edad="30", es_adulto="true")
print(usuario_corregido)
```

Aunque `edad` es un texto (`"30"`) y `es_adulto` es `"true"` (texto), Pydantic **lo convierte automáticamente** a `int` y `bool`.

Resultado:

```
nombre='Luis' edad=30 es_adulto=True
```

¡Pero cuidado! Si le das algo que **no puede convertir**, como `edad="treinta"`, fallará con un error claro.

---

## 📥 Integración con FastAPI: El Cuerpo de la Solicitud (`Request Body`)

Ahora, usemos Pydantic **dentro de FastAPI** para recibir datos.

### ¿Qué es el "cuerpo de la solicitud"?

Cuando usas una app web o móvil, a veces **envías datos** (por ejemplo, al registrarte). Ese paquete de datos se llama **cuerpo de la solicitud** (*request body*).

FastAPI usa Pydantic para **leer y validar** ese cuerpo.

### Paso 1: Crea una ruta que reciba un `Usuario`

Agrega esta función a tu `main.py` (debajo de las otras):

```python
@app.post("/usuarios/")
def crear_usuario(usuario: Usuario):
    return {"mensaje": f"Usuario {usuario.nombre} creado con éxito", "edad": usuario.edad}
```

> 🔍 Explicación:
> - `@app.post("/usuarios/")`: esta ruta **recibe datos** (por eso es `POST`, no `GET`).
> - `usuario: Usuario`: FastAPI sabe que el cuerpo de la solicitud debe coincidir con el modelo `Usuario`.
> - Dentro de la función, `usuario` ya es un objeto Pydantic **validado**.

### Paso 2: Prueba en la documentación

Guarda el archivo y asegúrate de que el servidor esté corriendo (`uvicorn main:app --reload`).

Ve a: [http://127.0.0.1:8000/docs](http://127.0.0.1:8000/docs)

Verás una nueva ruta: **POST /usuarios/**

Haz clic en ella → **"Try it out"** → en el cuadro de texto, escribe:

```json
{
  "nombre": "Carlos",
  "edad": 28,
  "es_adulto": true
}
```

Luego haz clic en **"Execute"**.

Verás la respuesta:

```json
{
  "mensaje": "Usuario Carlos creado con éxito",
  "edad": 28
}
```

¡Funciona!

### Paso 3: ¿Qué pasa si envías datos malos?

En el mismo cuadro, escribe:

```json
{
  "nombre": "María",
  "edad": "veinticinco",
  "es_adulto": "sí"
}
```

Haz clic en **"Execute"**.

Verás un error **422 Unprocessable Entity** con un mensaje como:

```json
{
  "detail": [
    {
      "loc": ["body", "edad"],
      "msg": "value is not a valid integer",
      "type": "type_error.integer"
    }
  ]
}
```

> ✅ FastAPI + Pydantic te dice **exactamente qué está mal**, **dónde** y **por qué**.

---


## 🧩 Tipos de Datos Avanzados 

Pydantic soporta tipos más complejos. Vamos a ver tres muy útiles:

### 1. `Optional`: un campo que puede no existir

Ya lo vimos arriba:

```python
from typing import Optional

class Producto(BaseModel):
    nombre: str
    precio: float
    descuento: Optional[float] = None  # Puede no estar
```

### 2. `List`: una lista de elementos

```python
from typing import List

class Pedido(BaseModel):
    cliente: str
    productos: List[str]  # Lista de textos
```

Ejemplo de uso en FastAPI:

```python
@app.post("/pedido/")
def hacer_pedido(pedido: Pedido):
    return {"total_productos": len(pedido.productos), "cliente": pedido.cliente}
```

Prueba en Swagger:

```json
{
  "cliente": "Jorge",
  "productos": ["manzana", "pan", "leche"]
}
```

### 3. `Union`: un campo que puede ser de varios tipos

```python
from typing import Union

class Respuesta(BaseModel):
    resultado: Union[str, int]
```

Esto significa que `resultado` puede ser **texto o número**.

> ⚠️ Usa `Union` con cuidado. A veces es mejor separar en dos campos.

---

## ❌ Manejo de Errores (básico)

Cuando los datos no coinciden con el modelo, FastAPI **automáticamente** devuelve un error **422** con detalles.

Ejemplo de error:

```json
{
  "detail": [
    {
      "loc": ["body", "edad"],
      "msg": "field required",
      "type": "value_error.missing"
    }
  ]
}
```

- `loc`: dónde está el error (`body` = en el cuerpo; `edad` = el campo).
- `msg`: qué pasó.
- `type`: tipo de error.

Esto es **muy útil** para que quien use tu API sepa cómo corregir su solicitud.

---

## 📦 Código final completo (`main.py`)

Aquí está todo el código que usamos, listo para copiar:

```python
# main.py
from fastapi import FastAPI
from pydantic import BaseModel
from typing import Optional, List, Union

app = FastAPI()

# Modelo de usuario
class Usuario(BaseModel):
    nombre: str
    edad: int
    es_adulto: bool

# Modelo de pedido
class Pedido(BaseModel):
    cliente: str
    productos: List[str]

# Ruta básica
@app.get("/")
def hola_mundo():
    return {"mensaje": "¡Hola, mundo!"}

# Ruta POST con Request Body
@app.post("/usuarios/")
def crear_usuario(usuario: Usuario):
    return {"mensaje": f"Usuario {usuario.nombre} creado con éxito", "edad": usuario.edad}

# Ruta con Query Parameters
@app.get("/buscar/")
def buscar_usuario(nombre: str, edad: Optional[int] = None):
    if edad is not None:
        return {"mensaje": f"Buscando a {nombre} de {edad} años"}
    else:
        return {"mensaje": f"Buscando a {nombre}"}

# Ruta con List
@app.post("/pedido/")
def hacer_pedido(pedido: Pedido):
    return {"total_productos": len(pedido.productos), "cliente": pedido.cliente}
```

Guarda este archivo, ejecuta con `uvicorn main:app --reload`, y ¡explora en [http://127.0.0.1:8000/docs](http://127.0.0.1:8000/docs)!

---

## 🔍 Validación y Documentación con Pydantic en FastAPI

La integración de **Pydantic** en **FastAPI** optimiza la validación de datos, la generación de documentación y la mantenibilidad de APIs RESTful. A continuación, un resumen de cómo lo logra:

### **1. Validación de datos**
- **Entrada**: Pydantic valida automáticamente datos de solicitudes (JSON, query params) según modelos declarativos, devolviendo errores 422 si no cumplen (ej: tipo incorrecto, campo faltante).
- **Salida**: Garantiza que las respuestas coincidan con el esquema definido (usando `response_model`).
- **Beneficio**: Reduce código manual, mejora robustez y simplifica manejo de errores.
- **Ejemplo**:
  ```python
  from fastapi import FastAPI
  from pydantic import BaseModel

  app = FastAPI()

  class Mascota(BaseModel):
      nombre: str
      edad: int

  @app.post("/mascotas")
  async def crear_mascota(mascota: Mascota):
      return mascota
  ```
  - Valida que `nombre` sea string y `edad` sea entero; falla si no.

### **2. Documentación automática (OpenAPI/Swagger UI)**
- Pydantic genera esquemas OpenAPI a partir de modelos, que FastAPI usa para crear documentación interactiva en `/docs`.
- Muestra endpoints, parámetros, esquemas y permite pruebas en tiempo real.
- **Beneficio**: Documentación siempre actualizada, sin esfuerzo manual, accesible en `http://localhost:8000/docs`.


