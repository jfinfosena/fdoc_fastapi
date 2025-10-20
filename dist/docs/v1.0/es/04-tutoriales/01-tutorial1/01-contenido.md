---
title: "Entornos Virtuales en Python"
position: 1
---

## Entornos Virtuales en Python:

Los entornos virtuales en Python son una herramienta esencial para gestionar las dependencias de tus proyectos. Permiten crear un espacio aislado para cada proyecto, evitando conflictos entre las librerías que utiliza cada uno.

### **¿Para qué sirven?**

* **Aislamiento de dependencias:** Cada proyecto puede tener sus propias versiones de librerías, sin afectar a otros proyectos.
* **Control de versiones:** Puedes crear un entorno virtual para cada versión de un proyecto, asegurando compatibilidad.
* **Simplificación de la gestión de dependencias:** Facilita la instalación y actualización de librerías.

### **Creación de un entorno virtual:**

```bash
python -m venv .venv
```


### **Activación del entorno virtual:**

```tabs
---[tab title="Windows" lang="bash"]---
myenv\Scripts\activate

---[tab title="Linux/macOS" lang="bash"]---
source myenv/bin/activate
```

### **Comandos dentro del entorno virtual:**

### **Instalación de paquetes:**
```bash
pip install <nombre_del_paquete>
```
Ejemplo:
```bash
pip install requests
```

###  **Desinstalación de paquetes:**
```bash
pip uninstall <nombre_del_paquete>
```

###  **Actualización de paquetes:**
```bash
pip install --upgrade <nombre_del_paquete>
```

###  **Lista de paquetes instalados:**
```bash
pip freeze
```

###  **Guardar las dependencias en un archivo:**
```bash
pip freeze > requirements.txt
```
Este comando crea un archivo `requirements.txt` que lista todas las dependencias del proyecto.

###  **Instalación de paquetes desde un archivo:**
```bash
pip install -r requirements.txt
```

###  **Desactivación del entorno virtual:**
```bash
deactivate
```
