---
title: "Herencia y Composición"
position: 3
date: 2025-08-01
---

# Herencia y composición

```admonition
type: important
title: Reutilizar y organizar
---
Herencia especializa comportamiento; composición ensambla objetos para lograr funcionalidades.
```

```steps
### Paso 1: Crear clase base
Define métodos comunes.

### Paso 2: Especializar con herencia
Sobrescribe métodos y usa `super()` para extender.

### Paso 3: Composición
Usa objetos internos para delegar responsabilidades.
```

## Explicación detallada
- Herencia (is-a): `Perro` es un `Animal`; reutiliza y especializa comportamiento.
- Composición (has-a): `Auto` tiene un `Motor`; delega responsabilidades a objetos internos.
- MRO (Method Resolution Order): Python resuelve métodos desde la clase concreta hacia arriba en su jerarquía.
- `super()`: invoca comportamiento de la superclase manteniendo el orden correcto según MRO.
- Múltiple herencia: posible en Python, pero complica MRO; usar con prudencia, considerar mixins para añadir capacidades.

## Ejemplo: Herencia

```python
class Animal:
    def hablar(self) -> str:
        return "..."

class Perro(Animal):
    def hablar(self) -> str:
        return "Guau"

class Gato(Animal):
    def hablar(self) -> str:
        return "Miau"

for a in [Perro(), Gato()]:
    print(a.hablar())
```

## Ejemplo: Composición

```python
class Motor:
    def arrancar(self) -> str:
        return "Motor en marcha"

class Auto:
    def __init__(self):
        self.motor = Motor()
    def iniciar(self) -> str:
        return self.motor.arrancar()

car = Auto()
print(car.iniciar())
```

## Buenas prácticas
- Prefiere composición para reutilizar comportamiento sin acoplar jerarquías rígidas.
- Usa mixins (clases pequeñas) para añadir capacidades (por ejemplo, `LoggableMixin`) sin formar jerarquías profundas.
- Limita niveles de herencia para mantener claridad y evitar efectos colaterales.

## Errores comunes
- Heredar solo para reutilizar una función; crea utilidades o usa composición.
- Sobrescribir métodos sin llamar a `super()` cuando la superclase mantiene invariantes.
