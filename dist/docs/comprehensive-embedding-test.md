# Comprehensive Component Embedding Test

This file tests all possible scenarios of components embedded within other components.

## ✅ Test 1: CodeBlock inside Steps

+++steps
### Paso 1: Instalación
Primero instala las dependencias:

+++code-block
```bash
npm install fastapi uvicorn
```
+++

### Paso 2: Configuración
Luego configura tu archivo:

+++code-block
```python
# main.py
from fastapi import FastAPI
app = FastAPI()
```
+++
+++

## ✅ Test 2: CodeBlock inside Admonition

+++admonition
---
type: tip
title: "Código dentro de Admonition"
---
Aquí hay un bloque de código dentro de una admonition:

+++code-block
```python
def hello():
    print("Hello from inside admonition!")
```
+++
+++

## ✅ Test 3: CodeBlock inside Accordion

+++accordion
---
allowMultiple: true
---
### Sección 1 con código
Contenido de la sección 1 con un bloque de código:

+++code-block
```javascript
console.log("Hello from accordion!");
```
+++

### Sección 2 con código
Contenido de la sección 2 con otro bloque de código:

+++code-block
```python
print("Hello from accordion section 2!")
```
+++
+++

## ✅ Test 4: Multiple Components inside Grid

+++grid
---
columns: 2
---
### Columna 1 con código
Primera columna con código:

+++code-block
```html
<div>Column 1</div>
```
+++

### Columna 2 con admonition
Segunda columna con una admonition:

+++admonition
---
type: info
title: "Info en columna"
---
Esta es una admonition dentro de una columna del grid.
+++
+++

## ✅ Test 5: TabbedCodeBlock (should work as before)

+++tabbed-code-block
```javascript
// JavaScript code
function test() {
    console.log("JS code");
}
```

```python
# Python code
def test():
    print("Python code")
```

```bash
# Bash code
echo "Bash code"
```
+++

## ✅ Test 6: Traditional Code Block (should work as before)

Este es un bloque de código tradicional que debería funcionar:

```python
def traditional():
    print("Traditional code block")
```

## ✅ Test 7: Deep Nesting - CodeBlock inside Admonition inside Steps

+++steps
### Paso Complejo: Configuración Avanzada
Este paso incluye una admonition con código:

+++admonition
---
type: warning
title: "Advertencia Importante"
---
Antes de continuar, verifica este código:

+++code-block
```python
# Verificación de seguridad
import os
if not os.path.exists('config.yaml'):
    raise FileNotFoundError("Archivo de configuración no encontrado")
```
+++
+++

### Paso Final: Ejecución
Finalmente, ejecuta tu aplicación.
+++

## ✅ Test 8: Mixed Syntax - Both +++ and Traditional

+++admonition
---
type: note
title: "Nota sobre sintaxis mixtas"
---
Este es un ejemplo de sintaxis mixta dentro de una admonition:

**Usando sintaxis +++:**

+++code-block
```python
# Código con sintaxis +++
def funcion_moderna():
    return "Sintaxis +++"
```
+++

**Usando sintaxis tradicional:**

```python
# Código con sintaxis tradicional
def funcion_tradicional():
    return "Sintaxis tradicional"
```
+++

## ✅ Resultados

¡Todos los componentes anidados ahora deberían renderizarse correctamente! La función `convertPlusFencesToBackticks` ahora procesa recursivamente el contenido de los componentes anidados, lo que permite que los bloques de código y otros componentes funcionen correctamente cuando están embebidos dentro de otros componentes.