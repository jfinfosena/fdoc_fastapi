---
title: "Módulos e Importaciones"
position: 6
date: 2025-08-01
---

# Módulos e Importaciones en Python

```admonition
type: note
title: ¿Qué es un módulo?
---
Un módulo es un archivo `.py` que agrupa funciones, variables o clases. Importarlos permite reutilizar y organizar mejor tu código.
```

## Crear un módulo simple
Define funciones y constantes en un archivo y reutilízalas desde otros scripts.

```python
# mimodulo.py
PI = 3.14159

def area_circulo(radio: float) -> float:
    return PI * (radio ** 2)
```

## Importación absoluta (archivo principal)
Importa el módulo por su nombre desde la raíz del proyecto.

```python
# app.py
import mimodulo

print("PI:", mimodulo.PI)
print("Área:", mimodulo.area_circulo(2))
```

## Importar miembros específicos y alias
Usa `from ... import ...` para traer nombres concretos. Con `as` creas alias.

```python
from mimodulo import area_circulo, PI as PI_CONST

print(PI_CONST)
print(area_circulo(3))
```

## Paquetes: carpetas con `__init__.py`
Un paquete es un directorio con `__init__.py`. Dentro defines submódulos para organizar funcionalidades.

## Estructura de proyecto (file-tree)

```file-tree
---
highlight:
  - "utilidades/"
annotations:
  "mimodulo.py": "Módulo con constante y función"
  "utilidades/strings.py": "Funciones de texto"
---
mi_proyecto/
├── app.py
├── mimodulo.py
├── utilidades/
│   ├── __init__.py
│   ├── strings.py
│   └── formatos.py
└── extras/
    └── plugins/
        └── analizador.py
```

Contenido de `strings.py`:

```python
# utilidades/strings.py

def en_mayus(texto: str) -> str:
    return texto.upper()

def contar_vocales(texto: str) -> int:
    return sum(1 for c in texto.lower() if c in "aeiou")
```

Uso con importación absoluta:

```python
# app.py
from utilidades.strings import en_mayus, contar_vocales

print(en_mayus("hola"))
print(contar_vocales("documentación"))
```

## Importaciones relativas (dentro de paquetes)
Desde un submódulo de un paquete, puedes importar relativo respecto al paquete.

```python
# utilidades/formatos.py
from .strings import en_mayus

def saludo_formateado(nombre: str) -> str:
    return en_mayus(f"hola, {nombre}")
```

```admonition
type: tip
title: Cuándo usar relativas
---
Úsalas cuando el código se ejecuta como parte del paquete (no como script suelto). Para scripts, prefiere importaciones absolutas.
```

## `__name__ == "__main__"`: punto de entrada
Evita que se ejecute código principal al importar un archivo.

```python
# ejecutable.py

def main():
    print("Ejecutando script...")

if __name__ == "__main__":
    main()
```

## Ruta de búsqueda de importaciones
Python busca módulos en el directorio actual, el entorno y rutas de `sys.path`. Puedes inspeccionarlo y ampliarlo.

```python
import sys
print(sys.path)  # rutas donde Python busca módulos

# Añadir una ruta temporalmente
sys.path.append("./extras")
```

```admonition
type: note
title: PYTHONPATH
---
Puedes definir la variable de entorno `PYTHONPATH` para añadir rutas de búsqueda a nivel del sistema o sesión.
```

## Importaciones dinámicas con `importlib`
Carga módulos a demanda, útil para plugins o extensiones.

```python
import importlib

mod = importlib.import_module("mimodulo")
print(mod.area_circulo(5))
```

## Control de exportaciones con `__all__`
Define qué nombres se exportan cuando se usa `from paquete import *` (no recomendado). `__all__` es una lista de strings con los nombres públicos del módulo.

```python
# utilidades/__init__.py
__all__ = ["strings", "formatos"]
```

## Evita importaciones circulares
Una importación circular ocurre cuando `A` importa `B` y `B` importa `A`. Provoca errores o comportamientos inesperados. Soluciones:
- Mueve funciones compartidas a un tercer módulo.
- Importa dentro de funciones (lazy import) para romper el ciclo.
- Reestructura el diseño para reducir dependencias cruzadas.

## Buenas prácticas

- Evita `from modulo import *`; contamina el espacio de nombres.
- Prefiere imports al inicio del archivo.
- Usa alias cuando aclaren la intención (`import numpy as np`).
- Estructura tu proyecto en paquetes coherentes.

```admonition
type: tip
title: Resumen
---
Crea módulos para reutilizar código, organiza paquetes con `__init__.py`, usa importaciones absolutas por claridad y relativas dentro de paquetes. Controla `sys.path`/`PYTHONPATH` y recurre a `importlib` cuando necesites carga dinámica.
```

