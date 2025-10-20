---
title: "Encapsulación y Propiedades"
position: 2
date: 2025-08-01
---

# Encapsulación y propiedades

```admonition
type: info
title: Controlar el acceso
---
Usa nombres con `_` para indicar atributos internos y `@property` para exponerlos de forma controlada.
```

```steps
### Paso 1: Atributos internos
Usa `_atributo` para señalar que no debe tocarse directamente.

### Paso 2: Propiedad de solo lectura
Expón el valor con `@property`.

### Paso 3: Valida con setter
Usa `@<prop>.setter` para validar entradas antes de asignar.
```

## Explicación detallada
- Encapsulación en Python es por convención: `_interno` indica uso interno; `__mangle` activa name-mangling.
- `@property` permite exponer un atributo calculado o protegido como un atributo normal manteniendo control.
- Setter/Getter: usar solo si hay lógica (validación, normalización). Si no, preferir acceso directo para simplicidad.
- Inmutabilidad: usa propiedades sin setter para estado de solo lectura; si necesitas más, considera `dataclasses` congeladas.

## Ejemplo: Cuenta bancaria

```python
class Cuenta:
    def __init__(self, saldo: float = 0):
        self._saldo = saldo  # convención de atributo interno

    @property
    def saldo(self) -> float:
        return self._saldo

    @saldo.setter
    def saldo(self, valor: float) -> None:
        if valor < 0:
            raise ValueError("El saldo no puede ser negativo")
        self._saldo = valor
```

## Uso

```python
c = Cuenta(100)
c.saldo = 150
print(c.saldo)
```

## Buenas prácticas
- Evita getters/setters verbales (`get_x`, `set_x`) si `@property` basta.
- Expón mínimamente: publica solo lo necesario y guarda invariantes en setters.
- Documenta qué valida cada setter y qué excepciones puede lanzar.

## Errores comunes
- Sobrecargar lógica trivial con getters/setters innecesarios.
- Usar `__privado` con name-mangling y luego no entender por qué no se puede acceder (evitar salvo casos raros).
