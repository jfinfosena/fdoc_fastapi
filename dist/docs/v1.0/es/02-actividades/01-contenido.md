---
title: "Actividad: Aplicación Python con entornos virtuales y fundamentos"
position: 1
date: "2025-10-20"
---

# Actividad (Grupal): Aplicación Python con entornos virtuales y control de dependencias

```admonition
type: info
title: Objetivo
---
Crear una aplicación sencilla en Python que use conceptos fundamentales (variables, funciones, módulos) y gestione sus dependencias mediante un entorno virtual. El código debe versionarse y publicarse en un repositorio de GitHub.
```

```admonition
type: tip
title: Apóyate en los tutoriales
---
Usa los tutoriales disponibles:
- Fundamentos de Python
- Estructuras y módulos
```

```cards
---
columns: 2
items:
  - title: "Tutorial 1: Entornos Virtuales en Python"
    icon: "RocketIcon"
    href: "/04-tutoriales/01-tutorial1/01-contenido"
    content: "Entornos virtuales en Python: creación, activación y gestión."
  - title: "Tutorial 1: Fundamentos de Python"
    icon: "BookOpenIcon"
    href: "/04-tutoriales/02-tutorial2/01-contenido"
    content: "Introducción a Python: sintaxis, tipos, funciones y buenas prácticas."
  
---
```

## Requisitos
- Trabajo **grupal** (2–4 personas).
- Proyecto gestionado con **Git** y alojado en **GitHub**.
- Uso de **entorno virtual** (`venv`) para aislar dependencias.
- Archivo `requirements.txt` con las versiones usadas.
- Incluir un `README.md` explicando: propósito, cómo instalar dependencias, cómo ejecutar.
- Entrega: **hoy 2025-10-20**.

## Estructura sugerida del proyecto

```file-tree
---
highlight:
  - "src/"
  - "README.md"
annotations:
  "package.json": "(opcional si usas herramientas JS para scripts)"
  "requirements.txt": "Lista exacta de dependencias (con versiones)"
  "README.md": "Guía de instalación y uso"
---
project-root/
├── src/
│   ├── main.py
│   └── utils.py
├── requirements.txt
├── .gitignore
└── README.md
```

## Pasos (Windows y multiplataforma)

```steps
### 1. Crear entorno virtual
Windows PowerShell: `python -m venv .venv` y `./.venv/Scripts/Activate.ps1`

En macOS/Linux: `python3 -m venv .venv` y `source .venv/bin/activate`

### 2. Instalar dependencias
Ejemplos: `pip install requests` o `pip install fastapi uvicorn`

### 3. Congelar versiones
`pip freeze > requirements.txt`

### 4. Inicializar Git y publicar en GitHub
`git init`, conectar remoto (`git remote add origin ...`), primer commit y `git push -u origin main`

### 5. Documentar en README
Objetivo, pasos de instalación, ejecución y responsabilidades del equipo.
```

## .gitignore recomendado

```gitignore
# Entorno virtual
.venv/
# Cache de Python
__pycache__/
*.pyc
# Archivos temporales
.env
*.log
```

## Ejemplo mínimo de aplicación

```python
# src/main.py
from utils import saludar

if __name__ == "__main__":
    print(saludar("Equipo"))
```

```python
# src/utils.py
def saludar(nombre: str) -> str:
    return f"Hola, {nombre}!"
```

## Rúbrica de evaluación
- Configuración correcta de `venv` y `requirements.txt` (30%).
- Código funcional y organizado en módulos (`src/`) (25%).
- Uso de Git con commits claros y repo público en GitHub (25%).
- Documentación (`README.md`) clara y reproducible (20%).

## Entrega
- URL del repositorio GitHub.
- Comandos para instalar y ejecutar desde cero:
  - `python -m venv .venv && ./.venv/Scripts/Activate.ps1`
  - `pip install -r requirements.txt`
  - `python src/main.py`

```admonition
type: warning
title: Buenas prácticas y control de dependencias
---
- Evita instalar dependencias globalmente; usa siempre el entorno virtual.
- Congela versiones en `requirements.txt` para reproducibilidad.
- Usa ramas y PRs para colaborar: `feature/funcionalidad-x`.
- Escribe mensajes de commit descriptivos: `feat: añadir módulo utils con función saludar`.
```
