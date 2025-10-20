---
title: "Problemas comunes y buenas prácticas"
position: 7
---

## Windows: activación de entorno

Si PowerShell bloquea scripts:
```powershell
Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned
```
Reabre la terminal y activa `.venv`.

## `pip` vs `python -m pip`

Usa siempre:
```bash
python -m pip install <paquete>
```
Evita confusiones de versiones o alias.

## Selección de intérprete en VS Code

- Abre la paleta de comandos y selecciona `Python: Select Interpreter`.
- Elige el de tu proyecto: `.venv`.

## Organización de proyecto

- Crea `.venv` en la raíz del proyecto.
- Ignora `.venv` en Git (`.gitignore`).
- Usa `requirements.txt` con versiones fijadas para producción.

## Mantenimiento

- Actualiza `pip` regularmente:
```bash
python -m pip install --upgrade pip
```
- Desactiva el entorno al terminar:
```bash
deactivate
```
- Borra y recrea `.venv` si hay conflictos de dependencias.

