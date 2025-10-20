---
title: "Gestión de dependencias"
position: 2
---

## Gestión de dependencias con pip y requirements.txt

Trabajar en un entorno virtual facilita instalar, actualizar y reproducir dependencias de forma controlada.

## Operaciones básicas

- Instalar un paquete
```bash
pip install <paquete>
```
- Desinstalar
```bash
pip uninstall <paquete>
```
- Actualizar
```bash
pip install --upgrade <paquete>
```
- Ver paquetes instalados
```bash
pip list
```

## Congelar e instalar desde `requirements.txt`

- Guardar dependencias exactas del proyecto
```bash
pip freeze > requirements.txt
```
- Reproducir el entorno en otra máquina
```bash
pip install -r requirements.txt
```


