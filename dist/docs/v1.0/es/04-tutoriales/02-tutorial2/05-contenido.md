---
title: "Colecciones y Comprensiones"
position: 5
date: 2025-08-01
---

# Colecciones y Comprensiones

```admonition
type: info
title: Mutables vs inmutables
---
Listas, conjuntos y diccionarios son mutables; tuplas son inmutables.
```

## Colecciones básicas

### Listas
Estructuras ordenadas y mutables. Permiten añadir, extender, insertar y eliminar elementos; admiten slicing para sublistas.

```python
numeros = [1, 2, 3]
numeros.append(4)
numeros.extend([5, 6])
numeros.insert(0, 0)
numeros.remove(3)
print(numeros)       # [0,1,2,4,5,6]
print(numeros[1:4])  # slicing
```

### Tuplas
Secuencias ordenadas e inmutables. Útiles para registros y retornos múltiples.

```python
punto = (10, 20)
x, y = punto
print(x, y)
# tuplas son inmutables
# punto[0] = 5  # error
```

### Conjuntos (set)
Colecciones no ordenadas de elementos únicos. Excelentes para operaciones de teoría de conjuntos.

```python
A = {1, 2, 3}
B = {3, 4}
print(A | B)   # unión
print(A & B)   # intersección
print(A - B)   # diferencia
print(A ^ B)   # diferencia simétrica
```

### Diccionarios
Mapas clave-valor mutables. Itera con `items()` para obtener pares.

```python
usuario = {"nombre": "Ana", "edad": 21}
usuario["email"] = "ana@example.com"
print(usuario.get("edad", 0))
for k, v in usuario.items():
    print(k, v)
```

## Comprensiones

### List comprehension
Sintaxis compacta para construir listas a partir de iterables y condiciones.

```python
cuadrados = [n*n for n in range(6)]
pares = [n for n in range(10) if n % 2 == 0]
print(cuadrados, pares)
```

### Set y dict comprehensions
Permiten crear conjuntos y diccionarios de forma declarativa.

```python
unicos = {c.lower() for c in "AaBbCc"}
longitudes = {palabra: len(palabra) for palabra in ["python", "data", "class"]}
print(unicos, longitudes)
```

```admonition
type: tip
title: Copias y vistas
---
Usa `list.copy()` o `list(...)` para copia superficial; para estructuras anidadas considera `copy.deepcopy`.
```

