---
title: "Conceptos y Clase/Objeto"
position: 1
date: 2025-12-01
---

# Conceptos básicos de POO

```admonition
type: note
title: Clase vs Objeto
---
Una clase define estructura y comportamiento; un objeto es una instancia concreta con estado propio.
```

```steps
### Paso 1: Definir una clase
Identifica atributos (datos) y métodos (comportamiento).

### Paso 2: Crear instancias
Usa el constructor `__init__` para inicializar estado.

### Paso 3: Invocar métodos
Los métodos operan sobre `self` (la instancia).
```

## Explicación detallada
- Clase: plantilla que agrupa datos (atributos) y operaciones (métodos) coherentes.
- Objeto: instancia de una clase con valores concretos; cada objeto mantiene su propio estado.
- `self`: referencia a la instancia actual; se pasa implícitamente al invocar métodos.
- Atributos de clase vs instancia: los atributos de clase se comparten; los de instancia son únicos por objeto.
- Tipado: usar anotaciones (`type hints`) mejora legibilidad y ayuda a herramientas estáticas.

## Ejemplo: Persona
Definimos una clase sencilla con atributos y un método.

```python
class Persona:
    def __init__(self, nombre: str, edad: int):
        self.nombre = nombre
        self.edad = edad

    def saludar(self) -> str:
        return f"Hola, soy {self.nombre}"
```

## Uso
Creamos y utilizamos objetos de la clase `Persona`.

```python
p = Persona("Ana", 21)
print(p.saludar())
print(p.nombre, p.edad)
```

## Diferencia entre atributo de clase e instancia

```python
class Contador:
    total = 0  # atributo de clase
    def __init__(self):
        self.individual = 0  # atributo de instancia

c1 = Contador(); c2 = Contador()
Contador.total += 1
c1.individual += 1
print(Contador.total, c1.individual, c2.individual)  # 1, 1, 0
```

## Buenas prácticas
- Nombra claramente atributos y métodos; usa verbos para acciones (`saludar`, `guardar`).
- Prefiere atributos de instancia salvo que el valor deba compartirse entre todas las instancias.
- Documenta con docstrings qué hace cada método y qué retorna.

## Errores comunes
- Olvidar `self` en la definición de métodos de instancia.
- Reutilizar atributos de clase para estado mutable por instancia (puede causar efectos compartidos inesperados).

