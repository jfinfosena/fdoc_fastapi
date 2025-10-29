---
title: "Actividad 2: Construye una API Básica con FastAPI"
position: 2
date: 2025-10-22
---

Esta actividad consiste en crear una API utilizando exclusivamente los conceptos y ejemplos presentados en:


```cards
---
columns: 1
items:
  - title: "FastAPI: Rutas y Operaciones HTTP"
    icon: "RocketIcon"
    href: "/01-contenido/02-contenido.md"
    content: "Diseñar y construir una API funcional con rutas básicas (`GET`, `POST`, `PUT`, `DELETE`)."
---
```

```admonition
type: info
title: Objetivos
---
- Diseñar y construir una API funcional con rutas básicas (`GET`, `POST`, `PUT`, `DELETE`).
- Seguir estrictamente el enfoque del tutorial [FastAPI: Rutas y Operaciones HTTP](/01-contenido/02-contenido.md): sin Pydantic, sin bases de datos, y usando estructuras en memoria (listas/diccionarios).
- Probar los endpoints localmente y documentar su uso.
```

## Alcance y restricciones
- Basarse únicamente en el contenido del tutorial de rutas y operaciones HTTP.
- Usar `main.py` como archivo principal con `FastAPI()` y ejecución con `uvicorn`.
- No usar modelos Pydantic, ORMs ni bases de datos externas.
- Emplear estructuras simples en memoria para simular datos.

## Proyectos (elige uno por grupo)
Cada grupo debe seleccionar un proyecto y construir su API con rutas básicas. 

1) Gestor de Usuarios Básico
- Requisitos mínimos: listar usuarios, crear usuario, obtener usuario por ID, actualizar usuario, eliminar usuario.
- Datos: `id`, `nombre`, `email`.

2) Inventario de Productos Simple
- Requisitos mínimos: listar productos, crear producto, obtener producto por ID, actualizar producto, eliminar producto.
- Datos: `id`, `nombre`, `precio`.

3) Biblioteca de Libros
- Requisitos mínimos: listar libros, registrar libro, obtener libro por ID, actualizar datos del libro, eliminar libro.
- Datos: `id`, `titulo`, `autor`.

4) Agenda de Tareas
- Requisitos mínimos: listar tareas, crear tarea, obtener tarea por ID, actualizar tarea, eliminar tarea.
- Datos: `id`, `titulo`, `descripcion`.

5) Registro de Asistencia
- Requisitos mínimos: listar asistencias, crear registro, obtener registro por ID, actualizar registro, eliminar registro.
- Datos: `id`, `fecha`, `nombre`, `estado`.

## Pasos 
- Elegir el proyecto y definir los datos mínimos.
- Diseñar los endpoints principales (`/`, `/recurso`, `/recurso/{id}`) siguiendo el tutorial.
- Implementar la lógica en memoria (listas/diccionarios) para crear, leer, actualizar y eliminar.
- Probar localmente con `uvicorn main:app --reload`.
- Validar los endpoints con `http://localhost:8000` y `http://localhost:8000/docs`.
- Utilizar Postman o ThunderClient para probar los endpoints.
- Documentar el uso en un `README.md` (endpoints y ejemplos de uso).

## Requisitos técnicos
- Archivo `main.py` con la app `FastAPI()`.
- Endpoints para `GET`, `POST`, `PUT`, `DELETE` operando sobre la estructura en memoria.
- Estructura y nombres consistentes con el proyecto elegido.

## Formato de entrega
- Enlace al repositorio GitHub (público).
- Nombre sugerido: `sena-fastapi-actividad2`.
- Debe incluir: `main.py`, `README.md` con instrucciones de ejecución y descripción de endpoints.
- Evidencias en Google Drive por el líder del grupo.



