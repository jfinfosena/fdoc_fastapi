---
title: "Control de Flujo"
position: 3
date: 2025-08-01
---

# Control de Flujo (condicionales y bucles)

```admonition
type: important
title: Indentación en Python
---
La indentación (4 espacios) define bloques. Es obligatoria.
```

```steps
### Condicionales
Usa if, elif, else, y operador ternario para decidir.

### Bucles
Utiliza for para iterar sobre secuencias y while para repetir mientras se cumple una condición.
```

## Condicionales

### if / elif / else
Evalúa múltiples caminos según condiciones. Se ejecuta el primer bloque cuya condición es verdadera.

```python
x = 18
if x < 18:
    print("Menor")
elif x == 18:
    print("Justo 18")
else:
    print("Mayor")
```

### Ternario (condición inline)
Sintaxis compacta para asignar valores según una condición.

```python
edad = 20
msg = "Mayor" if edad >= 18 else "Menor"
print(msg)
```

### match-case (>=3.10)
Estructura de comparación por patrones. Útil para múltiples casos.

```python
# Requiere Python 3.10 o superior
codigo = "ES"
match codigo:
    case "ES":
        print("Español")
    case "EN":
        print("Inglés")
    case _:
        print("Otro")
```

## Bucles

### for: range y enumerate
`range` genera secuencias numéricas; `enumerate` aporta índices al iterar.

```python
# range
for i in range(3, 8, 2):  # 3,5,7
    print(i)
# enumerate
frutas = ["manzana", "pera", "uva"]
for idx, f in enumerate(frutas, start=1):
    print(idx, f)
```

### for: zip y dict
`zip` combina varias secuencias; los diccionarios se recorren con `items()` para clave/valor.

```python
# zip
nombres = ["Ana", "Luis"]
edades = [21, 19]
for nombre, edad in zip(nombres, edades):
    print(nombre, edad)
# dict items
usuario = {"nombre": "Ana", "edad": 21}
for clave, valor in usuario.items():
    print(clave, valor)
```

### while: break/continue/else
`while` repite mientras la condición sea verdadera. `continue` salta iteración; `break` rompe el bucle; `else` se ejecuta si no hubo `break`.

```python
n = 5
while n > 0:
    n -= 1
    if n == 2:
        continue        # salta el 2
    if n == 1:
        break           # rompe en 1
    print("n:", n)
else:
    print("Termina si no hubo break")
```

```admonition
type: tip
title: Buenas prácticas
---
Evita bucles infinitos; usa condiciones claras. En for, prefiere enumerate/zip antes que índices manuales.
```
