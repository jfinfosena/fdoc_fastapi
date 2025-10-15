---
title: "Actividad: Probando tu API con Clientes REST"
position: 1
date: "2025-12-15"
---

# 🧪 Actividad Práctica: Probando tu API con Diferentes Clientes REST

En esta actividad aprenderás a probar tu API de FastAPI usando diferentes herramientas de cliente REST. Esta es una habilidad esencial para cualquier desarrollador backend.

## 📋 Objetivos de la Actividad

Al completar esta actividad, serás capaz de:

- Probar endpoints de tu API con Postman
- Usar Thunder Client en VS Code
- Realizar pruebas con herramientas online
- Entender las respuestas HTTP y códigos de estado
- Enviar diferentes tipos de datos (JSON, parámetros, headers)

---

## 🚀 Preparación: Asegúrate de tener tu API corriendo

Antes de comenzar, verifica que tu API de FastAPI esté funcionando:

+++steps
### Paso 1: Verificar que tu API esté activa
Abre tu terminal y ejecuta:

`uvicorn main:app --reload`

Deberías ver un mensaje similar a:
```
INFO:     Uvicorn running on http://127.0.0.1:8000 (Press CTRL+C to quit)
```

### Paso 2: Probar el endpoint raíz
Abre tu navegador y visita:

`http://localhost:8000/`

Deberías ver:
`{"mensaje": "¡Hola, Mundo!"}`

### Paso 3: Verificar la documentación
Asegúrate de que Swagger UI esté disponible en:

`http://localhost:8000/docs`
+++

---

## 🎯 Cliente 1: Thunder Client (VS Code)

Thunder Client es una extensión ligera para VS Code perfecta para pruebas rápidas.

+++admonition
---
type: info
title: "Instalación de Thunder Client"
---
Si no tienes Thunder Client instalado:
1. Abre VS Code
2. Ve a Extensiones (Ctrl+Shift+X)
3. Busca "Thunder Client"
4. Haz clic en "Instalar"
---

### Prueba 1: GET al endpoint raíz

+++rest-client
---
method: "GET"
url: "http://localhost:8000/"
headers:
  Accept: "application/json"
---
+++

**Respuesta esperada:**
```json
{
  "mensaje": "¡Hola, Mundo!"
}
```

### Prueba 2: GET con parámetro de ruta

Si creaste el endpoint `/saludo/{nombre}` en la lección 1:

+++rest-client
---
method: "GET"
url: "http://localhost:8000/saludo/Ana"
headers:
  Accept: "application/json"
---
+++

**Respuesta esperada:**
```json
{
  "saludo": "¡Hola, Ana!"
}
```

---

## 🔧 Cliente 2: Postman

Postman es la herramienta más popular para pruebas de API. Vamos a crear una colección.

+++steps
### Paso 1: Crear una colección
1. Abre Postman
2. Haz clic en "New Collection"
3. Nómbrala "FastAPI Curso"
4. Agrega una descripción: "Pruebas de mi API de FastAPI"

### Paso 2: Crear tu primera petición
1. Dentro de la colección, haz clic en "Add Request"
2. Nombra la petición "GET Raíz"
3. Selecciona método GET
4. URL: `http://localhost:8000/`
5. Haz clic en "Send"

### Paso 3: Verificar la respuesta
En la parte inferior verás:
- **Status**: 200 OK
- **Time**: Tiempo de respuesta (debe ser muy rápido)
- **Size**: Tamaño de la respuesta
- **Body**: El JSON con el mensaje
+++

### Peticiones adicionales en Postman

+++admonition
---
type: tip
title: "Variables de entorno en Postman"
---
Crea una variable de entorno llamada `base_url` con valor `http://localhost:8000` para no tener que escribir la URL completa en cada petición.
---

---

## 🌐 Cliente 3: Herramientas Online

Para pruebas rápidas sin instalar nada, puedes usar herramientas online.

### Opción 1: Hoppscotch (https://hoppscotch.io)

+++rest-client
---
method: "GET"
url: "http://localhost:8000/docs"
headers:
  Accept: "text/html"
---
+++

### Opción 2: ReqBin (https://reqbin.com)

+++rest-client
---
method: "GET"
url: "http://localhost:8000/redoc"
headers:
  Accept: "text/html"
---
+++

+++admonition
---
type: warning
title: "Precaución con herramientas online"
---
Las herramientas online no pueden acceder a `localhost`. Solo úsalas cuando tu API esté desplegada en un servidor público.
---

---

## 📊 Análisis de Respuestas HTTP

Es importante entender los códigos de estado HTTP que recibes.

+++stat-cards
---
columns: 4
items:
  - icon: "CheckCircleIcon"
    value: "200"
    label: "OK - Todo bien"
    color: "green"
  - icon: "ExclamationTriangleIcon"
    value: "400"
    label: "Bad Request - Error del cliente"
    color: "yellow"
  - icon: "XCircleIcon"
    value: "404"
    label: "Not Found - No existe"
    color: "red"
  - icon: "ServerIcon"
    value: "500"
    label: "Server Error - Error del servidor"
    color: "red"
---
+++

---

## 🎯 Actividad Final: Crear tu propio endpoint

Ahora que dominas las herramientas, crea un nuevo endpoint en tu API.

+++steps
### Paso 1: Agregar un endpoint POST
Modifica tu `main.py` y agrega:

```python
from pydantic import BaseModel

class Usuario(BaseModel):
    nombre: str
    edad: int

@app.post("/usuarios/")
def crear_usuario(usuario: Usuario):
    return {
        "mensaje": f"Usuario {usuario.nombre} creado exitosamente",
        "datos": usuario
    }
```

### Paso 2: Probar con Thunder Client
1. Método: POST
2. URL: `http://localhost:8000/usuarios/`
3. Headers: `Content-Type: application/json`
4. Body (raw JSON):
```json
{
    "nombre": "Carlos",
    "edad": 25
}
```

### Paso 3: Verificar la respuesta
Deberías recibir:
```json
{
    "mensaje": "Usuario Carlos creado exitosamente",
    "datos": {
        "nombre": "Carlos",
        "edad": 25
    }
}
```
+++

---

## 📋 Checklist de Entrega

Para completar esta actividad, asegúrate de:

+++steps
### ✅ Realizar todas las pruebas
- [ ] Probar endpoint GET `/` con Thunder Client
- [ ] Probar endpoint GET `/saludo/{nombre}` con Thunder Client
- [ ] Crear colección en Postman con al menos 3 peticiones
- [ ] Probar tu nuevo endpoint POST con datos JSON
- [ ] Verificar que todos los códigos de estado sean 200 OK

### ✅ Documentar tus resultados
- [ ] Captura de pantalla de Thunder Client con resultados
- [ ] Captura de pantalla de Postman con tu colección
- [ ] Captura del nuevo endpoint POST funcionando
- [ ] Breve descripción de qué aprendiste
+++

---

## 🏆 Recursos Adicionales

+++admonition
---
type: success
title: "¡Felicidades!"
---
Al completar esta actividad, habrás dominado las herramientas esenciales para probar APIs REST. Estas habilidades son fundamentales para cualquier desarrollador backend moderno.
---

### Recursos útiles:
- [Documentación oficial de Thunder Client](https://docs.thunderclient.com)
- [Guía de Postman para principiantes](https://learning.postman.com)
- [Códigos de estado HTTP - MDN](https://developer.mozilla.org/es/docs/Web/HTTP/Status)

### Próximos pasos:
- Explora las funciones avanzadas de Postman (variables, ambientes, tests)
- Aprende a crear documentación automática con Swagger
- Practica con APIs públicas gratuitas

---

```cta
---
title: "¿Completaste la actividad?"
buttons:
  - text: "Ir a la Lección 2"
    url: "/v1.0/es/01-contenido/02-contenido"
    variant: "primary"
    icon: "ArrowRightIcon"
  - text: "Ver más actividades"
    url: "/v1.0/es/02-actividades"
    variant: "secondary"
---
¡Excelente trabajo! Ahora estás listo para aprender sobre métodos HTTP y validación de datos.
```

