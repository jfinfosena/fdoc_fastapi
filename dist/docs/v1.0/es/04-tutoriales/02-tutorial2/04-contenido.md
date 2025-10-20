---
title: "Funciones y Módulos"
position: 4
date: 2025-08-01
---

# Funciones y Módulos

```admonition
type: example
title: Definir y usar funciones
---
Una función encapsula lógica reutilizable. Usa parámetros y valores de retorno para componer tu programa.
```

## Ejemplos de funciones

### funciones.py
Antes de usar una función, debemos definirla. Aquí creamos `sumar` y `saludar`. La segunda acepta un parámetro opcional para cambiar el estilo del saludo.

```python
def sumar(a, b):
    return a + b

def saludar(nombre, formal=False):
    if formal:
        return f"Buenos días, {nombre}."
    return f"Hola, {nombre}!"
```

### main.py
Para usar las funciones definidas en otro archivo, las importamos y luego las invocamos. Observa cómo cambiamos el resultado según el parámetro `formal`.

```python
from funciones import sumar, saludar

print("Suma:", sumar(3, 4))
print(saludar("Ana"))
print(saludar("Luis", formal=True))
```

## Punto de entrada
Cuando un archivo puede ser importado o ejecutado directamente, define el punto de entrada con `if __name__ == "__main__":`. Así evitas que se ejecute código principal al importar.

```python
def main():
    print("Ejecutando script...")

if __name__ == "__main__":
    main()
```

## Creación e importación de módulos
Un **módulo** es simplemente un archivo `.py` con funciones, variables o clases. Puedes importarlo desde otros archivos para reutilizar código.

### Módulo simple
Creamos `mimodulo.py` con una constante y una función.

```python
# mimodulo.py
PI = 3.14159

def area_circulo(radio: float) -> float:
    return PI * (radio ** 2)
```

### Importación absoluta
En `app.py` importamos el módulo y usamos sus elementos. La importación **absoluta** indica el nombre del módulo desde la raíz del proyecto.

```python
# app.py
import mimodulo

print("PI:", mimodulo.PI)
print("Área:", mimodulo.area_circulo(2))
```

### Paquetes y submódulos
Un **paquete** es una carpeta con un `__init__.py`. Dentro puedes organizar submódulos.

Estructura:
- utilidades/
  - __init__.py
  - strings.py

`strings.py` define funciones de texto:

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

```admonition
type: tip
title: Importaciones relativas (en paquetes)
---
Dentro de un paquete, puedes usar importaciones relativas: `from .strings import en_mayus`. Úsalas cuando el código se ejecuta como parte del paquete y no como script suelto.
```

