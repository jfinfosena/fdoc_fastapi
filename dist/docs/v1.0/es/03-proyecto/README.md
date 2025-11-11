---
title: "Proyecto Integrador: Desarrollo de una API con FastAPI"
position: 0
date: 2025-11-10
---


### 1) Crear Fork del Repositorio
Solo el líder del grupo debe crear el repositorio para la actividad.  

```bash
https://github.com/jfinfosena/proyecto_integrador_ciencia_datos.git
```

Pulsa el botón **Fork** (esquina superior derecha) desde la cuenta del líder.

### 2) El líder clona su fork en su equipo:

```bash
git clone https://github.com/LIDER-DEL-GRUPO/proyecto_integrador_ciencia_datos.git
cd proyecto_integrador_ciencia_datos
```

### 3) El líder agrega a los integrantes como colaboradores en su fork (Settings → Collaborators) o comparte el enlace del fork para que ellos lo clonen directamente:

```bash
git clone https://github.com/LIDER-DEL-GRUPO/proyecto_integrador_ciencia_datos.git
```

### 3) Flujo de trabajo recomendado:
- Rama `main` protegida (sin commits directos).
- Cada integrante crea ramas por funcionalidad: `feature/<nombre>` (ej. `feature/descripcion-datos`).

### 4) Registro del grupo: info.json
Antes de iniciar la preparación, diligencia el archivo `info.json` en la raíz del repositorio del líder con la siguiente estructura:

```json
{
  "nombres": "",
  "apellidos": "",
  "grupo": "datos-#"
}
```

Indicaciones:
- `nombres` y `apellidos`: del líder del grupo.
- `grupo`: reemplaza `#` por el número asignado (ej. `datos-3`).
- Este archivo sirve para identificar el grupo durante la evaluación.
- Inclúyelo en el primer commit del fork y actualízalo si cambia el liderazgo.

## Indicaciones para el Proyecto Integrador con FastAPI

### Instrucciones Generales
Cada grupo debe desarrollar un proyecto integrador utilizando **FastAPI** que demuestre la aplicación de los conceptos de desarrollo de APIs RESTful vistos en clase. El proyecto consistirá en la creación de una API con **mínimo 3 recursos** basados en un tema o área de conocimiento seleccionado por el grupo.

### Base documental obligatoria del proyecto
Todo el proyecto integrador debe basarse y alinearse con los siguientes documentos, adaptándolos a la idea seleccionada por el grupo (dominio elegido), sin romper la arquitectura propuesta:

- `01 - Introducción y objetivos` (`03-proyecto/01-introduccion.md`)
- `02 - Instalación y ejecución` (`03-proyecto/02-instalacion-y-ejecucion.md`)
- `03 - Estructura del proyecto` (`03-proyecto/03-estructura-proyecto.md`)
- `04 - Configuración y variables de entorno` (`03-proyecto/04-configuracion.md`)
- `05 - Base de datos y sesiones` (`03-proyecto/05-base-datos.md`)
- `06 - Modelos y Schemas` (`03-proyecto/06-modelos-y-schemas.md`)
- `07 - Rutas y Routers` (`03-proyecto/07-rutas.md`)
- `08 - Servicios y lógica de negocio` (`03-proyecto/08-servicios.md`)
- `09 - Documentación con Scalar` (`03-proyecto/09-documentacion-scalar.md`)
- `10 - Scripts de utilidad y pruebas` (`03-proyecto/10-scripts.md`)
- `11 - Relaciones de la API` (`03-proyecto/11-relaciones-api.md`)
- `12 - Despliegue en Render.com (Docker)` (`03-proyecto/12-deploy-render.md`)

La implementación debe seguir estas guías y patrones, adaptando los modelos, rutas, servicios y pruebas al tema elegido por el grupo (por ejemplo: biblioteca, inventarios, cursos, reservas, etc.).

### Adaptación a la idea del grupo
- Define mínimo 3 recursos del dominio (ej.: `courses`, `students`, `enrollments`).
- Implementa CRUD completo por recurso y relaciones necesarias (1:1, 1:N, N:N) según aplique, siguiendo el esquema de `models/schemas/services/endpoints` propuesto.
- Expón la documentación en `GET /scalar` y valida la API con los scripts de prueba.
- Mantén la separación por capas y las buenas prácticas descritas en los documentos 03–08.

### Trabajo en equipo
- Usa ramas por funcionalidad y Pull Requests al fork del líder.
- Asegura consistencia de estilos y convenciones entre módulos.
- Revisa y prueba cada aporte antes de fusionarlo a `main`.


