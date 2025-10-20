---
title: "Polimorfismo y Clases Abstractas"
position: 4
date: 2025-12-01
---

# Polimorfismo y clases abstractas

```admonition
type: tip
title: Interfaces por comportamiento
---
El polimorfismo permite usar distintos objetos intercambiablemente si comparten la misma interfaz de métodos (duck typing).
```

```steps
### Paso 1: Polimorfismo
Invoca el mismo método en diferentes clases.

### Paso 2: Clases Abstractas
Usa `abc.ABC` y `@abstractmethod` para definir contratos.

### Paso 3: Protocols
Usa `typing.Protocol` para interfaces estructurales sin herencia explícita.
```

## Explicación detallada
- Duck typing: “si camina como un pato y hace quack, es un pato”. Lo importante es la interfaz, no la herencia.
- Clases abstractas: obligan a implementar métodos; útiles cuando requieres estructura común y documentación del contrato.
- Protocols: definen la forma (métodos/atributos requeridos) sin acoplar jerarquías; ideal para testeo y flexibilidad.

## Ejemplo: Polimorfismo

```python
class PDF:
    def render(self) -> str:
        return "Renderizando PDF"

class HTML:
    def render(self) -> str:
        return "Renderizando HTML"

for doc in [PDF(), HTML()]:
    print(doc.render())
```

## Ejemplo: Clase Abstracta

```python
from abc import ABC, abstractmethod

class Repositorio(ABC):
    @abstractmethod
    def guardar(self, entidad) -> None:
        pass

class RepoMemoria(Repositorio):
    def guardar(self, entidad) -> None:
        print(f"Guardado en memoria: {entidad}")
```

## Ejemplo: Protocol

```python
from typing import Protocol

class Renderizable(Protocol):
    def render(self) -> str: ...

class Reporte:
    def mostrar(self, doc: Renderizable) -> None:
        print(doc.render())

Reporte().mostrar(PDF())
```

## Buenas prácticas
- Elige ABC para jerarquías claras; usa Protocols para acoplamiento bajo y testabilidad.
- Documenta contratos: entradas, salidas y efectos (side effects).
- Evita suponer tipos concretos; programa contra interfaces (métodos esperados).

## Errores comunes
- Usar `isinstance` donde basta con duck typing (interfaces por comportamiento).
- Acoplar fuertemente a clases concretas en lugar de depender de contratos (ABC/Protocol).

