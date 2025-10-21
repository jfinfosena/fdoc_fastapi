---
title: "Diseño y Organización de Código"
position: 6
date: 2025-12-01
---

# Diseño y organización de código

```admonition
type: warning
title: Principios clave
---
SRP (Responsabilidad Única), OCP (Abierto/Cerrado), KISS (Simplicidad), y favorecer composición sobre herencia cuando sea posible.
```

```steps
### Paso 1: Separar responsabilidades
Divide por dominios (modelos, servicios, repositorios) para reducir acoplamiento.

### Paso 2: Interfaces y contratos
Define Protocols o clases base para servicios y repositorios.

### Paso 3: Organizar módulos
Agrupa en paquetes y usa imports explícitos, evitando dependencias circulares.
```

## Estructura sugerida

```file-tree
---
highlight:
  - "dominio/"
  - "servicios/"
annotations:
  "dominio/": "Entidades y lógica del negocio"
  "servicios/": "Casos de uso/servicios que orquestan"
---
project-root/
├── dominio/
│   ├── modelos.py
│   └── repositorios.py
├── servicios/
│   ├── crear_usuario.py
│   └── enviar_notificacion.py
└── app.py
```

## Explicación detallada
- Separación de capas: el paquete `dominio` contiene entidades (modelos de negocio) y contratos de persistencia; `servicios` implementa casos de uso orquestando entidades y repositorios.
- Acoplamiento vs cohesión: minimizar dependencias entre módulos, maximizar que cada módulo tenga una responsabilidad clara.
- Contratos explícitos: usar `Protocol` o clases base para permitir pruebas con dobles (mocks/stubs) y cambiar implementaciones sin tocar consumidores.
- Gestión de dependencias: inyectar dependencias (por constructor) en servicios para facilitar testeo y evitar dependencias globales.
- Importaciones: preferir importaciones absolutas dentro del proyecto, y evitar que `dominio` importe `servicios` (dirección de dependencia unidireccional).

## Ejemplo: Protocolos con typing

```python
from typing import Protocol

class ReposUsuario(Protocol):
    def guardar(self, usuario: dict) -> None: ...

class RepoMemoria:
    def guardar(self, usuario: dict) -> None:
        print("guardado", usuario)

class ServicioCrearUsuario:
    def __init__(self, repo: ReposUsuario):
        self.repo = repo
    def ejecutar(self, usuario: dict):
        self.repo.guardar(usuario)
```

## Buenas prácticas
- Nombres claros: `ServicioCrearUsuario`, `RepoMemoria` describen exactamente su función.
- Evita ciclos: si `servicios` necesita funciones de `dominio`, no hagas que `dominio` dependa de `servicios`.
- Tests por capas: prueba `dominio` sin I/O; prueba `servicios` con repositorios falsos (dobles) para aislar comportamiento.
- Documenta contratos: docstrings con precondiciones, efectos y excepciones.

## Errores comunes
- Mezclar lógica de negocio con I/O (por ejemplo, llamadas HTTP dentro de entidades).
- Heredar para reutilizar donde composición sería más flexible (pérdida de encapsulación y aumento del acoplamiento).
- Importaciones implícitas y dependencias no declaradas que dificultan el testeo y la reutilización.

