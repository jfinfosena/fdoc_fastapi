---
title: "Tipos de Datos y Operadores"
position: 2
date: 2025-08-01
---

# Tipos de Datos y Operadores

```admonition
type: info
title: Qué cubriremos
---
Verás tipos básicos y avanzados, junto con operadores aritméticos, de comparación, lógicos, bit a bit, pertenencia e identidad.
```

## Tipos de datos

### Tipos básicos
Python ofrece números (`int`, `float`, `complex`), cadenas (`str`), booleanos (`bool`) y `None` para indicar ausencia de valor. Son la base para construir cualquier programa.

```python
# Números
entero = 42              # int
real = 3.1415            # float
complejo = 2 + 3j        # complex

# Cadenas
texto = "Hola\nMundo"    # str con salto de línea
multilinea = """Línea 1
Línea 2"""               # str multilínea

# Booleanos y None
verdadero = True
falso = False
nada = None

print(type(entero), type(real), type(complejo))
print(texto)
print(multilinea)
print(verdadero, falso, nada)
```

### Bytes
Para trabajar con datos binarios, usa `bytes` (inmutable) y `bytearray` (mutable). Son útiles para archivos binarios, redes y criptografía.

```python
# Bytes y bytearray
b = b"ABC"               # inmutable
ba = bytearray(b)        # mutable
ba[0] = ord("Z")
print(b, ba)
```

## Operadores

### Aritméticos
Permiten operaciones matemáticas básicas y avanzadas: suma, resta, multiplicación, división, división entera, módulo y potencia.

```python
a, b = 7, 3
print(a + b, a - b, a * b, a / b)   # suma, resta, mul, div
print(a // b, a % b, a ** b)        # división entera, módulo, potencia
```

### Comparación y lógicos
Compara valores y combina condiciones con `and`, `or`, `not`. Útiles para expresiones condicionales.

```python
x, y = 5, 7
print(x == y, x != y, x < y, x <= y, x > y, x >= y)
p, q = True, False
print(p and q, p or q, not p)
```

### Pertenencia e identidad
`in` y `not in` verifican si un elemento está en una colección. `is` evalúa si dos referencias apuntan al mismo objeto.

```python
lista = [1, 2, 3]
print(2 in lista, 4 not in lista)
s1 = "hola"; s2 = "hola"
print(s1 is s2, s1 is not s2)  # identidad (referencia)
```

### Bit a bit
Operan sobre la representación binaria: AND (`&`), OR (`|`), XOR (`^`), NOT (`~`), desplazamientos (`<<`, `>>`).

```python
m, n = 0b1010, 0b0011
print(m & n, m | n, m ^ n, ~m, m << 1, m >> 1)
```

```admonition
type: tip
title: Conversión y lectura
---
Convierte con `int()`, `float()`, `str()`, `bool()`. Lee texto con `input()` (devuelve `str`) y convierte según necesites.
```
