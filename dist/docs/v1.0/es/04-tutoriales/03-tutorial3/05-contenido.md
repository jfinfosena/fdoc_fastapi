---
title: "Métodos Especiales y Dataclasses"
position: 5
date: 2025-08-01
---

# Métodos especiales y dataclasses

```admonition
type: note
title: Dunder methods
---
Los métodos especiales (como `__repr__`, `__eq__`, `__lt__`) integran tus clases con las operaciones del lenguaje.
```

```steps
### Paso 1: Representación
Usa `__repr__` para una representación útil.

### Paso 2: Comparaciones
Implementa `__eq__` y `__lt__` si quieres ordenar o comparar.

### Paso 3: Dataclasses
Usa `@dataclass` para clases de datos con menos código.
```

## Explicación detallada
- `__repr__` debería ser no-ambiguo y útil para depuración (idealmente reconstruible).
- `__str__` es para usuarios finales; si no lo defines, Python usa `__repr__`.
- Operadores de comparación: define solo los necesarios o usa `@dataclass(order=True)` para generarlos.
- Dataclasses: generan `__init__`, `__repr__`, `__eq__` automáticamente; soportan `frozen=True` (inmutables), `field(default=...)` y `__post_init__`.

## Ejemplo: Métodos especiales

```python
class Punto:
    def __init__(self, x: float, y: float):
        self.x = x
        self.y = y

    def __repr__(self) -> str:
        return f"Punto(x={self.x}, y={self.y})"

    def __eq__(self, other) -> bool:
        return isinstance(other, Punto) and self.x == other.x and self.y == other.y
```

## Ejemplo: Dataclass

```python
from dataclasses import dataclass

@dataclass(order=True)
class Producto:
    precio: float
    nombre: str

p1 = Producto(10.0, "Lapicero")
p2 = Producto(8.5, "Cuaderno")
print(p1, p1 > p2)
```

## Buenas prácticas
- Mantén `__repr__` informativo y `__str__` amigable.
- Evita implementar comparadores inconsistentes (ej. `__eq__` sin `__hash__` en objetos mutables).
- En dataclasses, usa `frozen=True` para objetos inmutables y seguros como claves.

## Errores comunes
- Sobrecargar `__eq__` sin considerar la semántica de identidad y mutabilidad.
- Usar `order=True` cuando el criterio natural de orden no está claro (puede ser confuso).

