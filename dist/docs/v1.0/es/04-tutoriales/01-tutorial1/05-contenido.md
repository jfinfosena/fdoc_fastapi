---
title: "Trabajo en equipo y control de versiones"
position: 6
---

## Actividad grupal: Flujo de trabajo con entornos virtuales

Esta guía resume cómo colaborar en equipo manteniendo dependencias sincronizadas.

## Preparación del repositorio

- Crea un repositorio en GitHub.
- Clona el repo y crea un entorno virtual local.
```bash
python -m venv .venv
# Activar entorno ...
pip install -r requirements.txt  # si ya existe
```

## Ramas y dependencias

- Trabaja en ramas por funcionalidad: `feature/asistencia-ui`, `fix/estadisticas-null`, etc.
- Si agregas librerías nuevas:
```bash
pip install <lib>
pip freeze > requirements.txt
git add requirements.txt
git commit -m "chore: actualiza dependencias"
```
- Comunica los cambios para que el equipo actualice:
```bash
pip install -r requirements.txt
```

## Buenas prácticas de colaboración

- Usa mensajes de commit claros y descriptivos.
- Evita subir la carpeta `.venv` (añadir a `.gitignore`).
- Automatiza pasos comunes con un `Makefile` o scripts (opcional).
- Revisa PRs verificando código y ejecución local.

## Entregables de equipo

- Código funcional, `requirements.txt` actualizado.
- `README.md` con instalación, uso y dependencias.
- Documenta responsabilidades y contribuciones.

