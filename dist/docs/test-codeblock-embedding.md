# Test de CodeBlock Embedding

Este archivo prueba si el componente CodeBlock puede ser embebido correctamente.

## Test 1: CodeBlock dentro de Steps

+++steps
### Paso 1: Introducción

Aquí mostramos un ejemplo de código:

+++codeblock
```javascript
function greet(name) {
  console.log(`Hello, ${name}!`);
}
greet('World');
```
+++

### Paso 2: Código Python

Otro ejemplo con Python:

+++codeblock
```python
def greet(name):
    return f"Hello, {name}!"

print(greet("World"))
```
+++
+++

## Test 2: CodeBlock dentro de Admonition

+++admonition type="tip" title="Consejo de Código"
Aquí hay un ejemplo útil:

+++codeblock
```typescript
interface User {
  name: string;
  age: number;
}

const user: User = { name: "John", age: 25 };
console.log(user);
```
+++
+++

## Test 3: CodeBlock dentro de Accordion

+++accordion
### Sección 1: JavaScript

+++codeblock
```javascript
// JavaScript moderno
const add = (a, b) => a + b;
console.log(add(5, 3));
```
+++

### Sección 2: Python

+++codeblock
```python
# Python simple
def add(a, b):
    return a + b

print(add(5, 3))
```
+++
+++