---
title: "Introducción a FastAPI y entorno de desarrollo"
position: 1
date: 2025-10-13
---

+++hero-section
---
title: "¡Bienvenido a FastAPI!"
subtitle: "Aprende a construir APIs modernas, rápidas y autodocumentadas desde cero."
backgroundImage: "https://images.unsplash.com/photo-1555066931-4365d14bab8c?q=80&w=2070"
buttons:
  - text: "Comenzar Lección"
    url: "#introduccion"
    variant: "primary"
    icon: "RocketIcon"
  - text: "Ver Código de Ejemplo"
    url: "#ejemplo-hola-mundo"
    variant: "secondary"
---
+++

## 📌 Objetivos de la lección

Al finalizar esta lección, serás capaz de:

- Entender qué es FastAPI y por qué es una excelente opción para construir APIs.
- Configurar un entorno de desarrollo con Python y FastAPI.
- Crear y ejecutar tu primera API usando Uvicorn.
- Explorar la documentación automática generada por FastAPI.

---

## 🔍 ¿Qué es FastAPI?

FastAPI es un **framework web moderno, rápido (de alto rendimiento)** para construir APIs con Python 3.7+ basado en estándares abiertos como **OpenAPI** y **JSON Schema**.

### ✨ Principales ventajas

- **Rápido**: Uno de los frameworks más rápidos de Python, comparable con Node.js y Go.
- **Fácil**: Diseñado para ser intuitivo y fácil de aprender.
- **Autodocumentado**: Genera documentación interactiva automáticamente (Swagger UI y ReDoc).
- **Tipado**: Usa el tipado de Python para validación automática y mejor experiencia de desarrollo.

+++admonition
---
type: tip
title: ¿Por qué FastAPI y no Flask o Django?
---
FastAPI combina la simplicidad de Flask con la potencia del tipado estático y la generación automática de documentación. Es ideal para APIs modernas, microservicios y prototipado rápido.
+++

---

## 🛠️ Configuración del entorno de desarrollo

Sigue estos pasos para preparar tu entorno:


+++steps
### Paso 1: Instalar Python 3.8+
Asegúrate de tener Python 3.8 o superior instalado. Verifica con el comando `python --version` o `python3 --version` en tu terminal.


### Paso 2: Crear un entorno virtual
Crea un entorno virtual ejecutando `python -m venv fastapi-env`. Luego actívalo con:
- Linux/macOS: `source fastapi-env/bin/activate`
- Windows: `fastapi-env\Scripts\activate`


### Paso 3: Instalar FastAPI y Uvicorn
Instala las dependencias necesarias con el comando `pip install fastapi uvicorn`.


### Paso 4: Verificar la instalación
Comprueba que Uvicorn esté instalado ejecutando `uvicorn --version`.
+++

---

## 💡 Tu primer "Hola Mundo" con FastAPI

Crea un archivo llamado `main.py` con el siguiente contenido:

+++python
# main.py
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
def read_root():
    return {"mensaje": "¡Hola, Mundo!"}
+++

### Ejecutar la API

En tu terminal, ejecuta:

+++bash
uvicorn main:app --reload
+++

- `main`: nombre del archivo (`main.py`)
- `app`: nombre del objeto FastAPI
- `--reload`: reinicia el servidor al detectar cambios (solo para desarrollo)

Visita [http://localhost:8000](http://localhost:8000) en tu navegador. Verás:

+++json
{"mensaje": "¡Hola, Mundo!"}
+++

+++admonition
---
type: note
title: Puerto por defecto
---
Uvicorn corre por defecto en el puerto `8000`. Puedes cambiarlo con `--port 8080`.
+++

---

## 📚 Documentación automática

FastAPI genera automáticamente dos interfaces de documentación:

- **Swagger UI**: [http://localhost:8000/docs](http://localhost:8000/docs)
- **ReDoc**: [http://localhost:8000/redoc](http://localhost:8000/redoc)

Estas interfaces te permiten explorar y probar tus endpoints sin necesidad de Postman.

+++image-gallery
---
columns: 2
items:
  - src: "https://fastapi.tiangolo.com/img/index/index-03-swagger-docs.png"
    alt: "Swagger UI de FastAPI"
  - src: "https://fastapi.tiangolo.com/img/index/index-04-redoc-docs.png"
    alt: "ReDoc de FastAPI"
---
+++

---

## 🧪 Práctica: Crea tu propia API de bienvenida

**Ejercicio guiado**: Modifica tu `main.py` para que incluya un nuevo endpoint `/saludo/{nombre}` que devuelva un mensaje personalizado.

**Ejemplo esperado**:
+++json
{
  "saludo": "¡Hola, Ana!"
}
+++

### ¿Necesitas una pista?

Usa un parámetro de ruta como se muestra en el ejemplo de código:

+++python
@app.get("/saludo/{nombre}")
def saludar(nombre: str):
    return {"saludo": f"¡Hola, {nombre}!"}
+++

---

## 📝 Resumen

- FastAPI es un framework moderno, rápido y autodocumentado.
- Se instala con `pip install fastapi uvicorn`.
- El servidor de desarrollo se inicia con `uvicorn main:app --reload`.
- La documentación automática está disponible en `/docs` y `/redoc`.

+++stat-cards
---
columns: 3
items:
  - icon: "ZapIcon"
    value: "⚡"
    label: "Alto rendimiento"
    color: "blue"
  - icon: "BookOpenIcon"
    value: "✅"
    label: "Autodocumentación"
    color: "green"
  - icon: "ShieldCheckIcon"
    value: "🔒"
    label: "Validación automática"
    color: "purple"
---
+++

---

## ▶️ Próximos pasos

En la **Lección 2**, aprenderás a trabajar con:
- Métodos HTTP (`GET`, `POST`, etc.)
- Parámetros de ruta y consulta
- Tipado y validación con Pydantic

+++cta
---
title: "¿Listo para continuar?"
buttons:
  - text: "Ir a la Lección 2"
    url: "/leccion2"
    variant: "primary"
  - text: "Descargar código de esta lección"
    url: "/assets/leccion1.zip"
    variant: "secondary"
---
¡Sigue construyendo tu primera API profesional con FastAPI!
+++

---
