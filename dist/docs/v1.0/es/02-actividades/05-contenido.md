---
title: "Actividad 5: Diseño conceptual Libre de API RESTful con FastAPI"
position: 5
date: 2025-11-05
---


**Duración:** **90 min trabajo** + **15 min exposición**  
**Equipos:** 3–5 personas | **Cualquier contexto**

---

```steps
### PASO 1: Tema + Valor
**Tarea:** Elige un tema real y escribe tu propuesta de valor.

**Preguntas guía:**
- ¿Qué problema resuelve?
- ¿Para quién?
- ¿Por qué es mejor?

### PASO 2: Usuarios y Historias
**Tarea:** Define los roles y escribe historias de usuario.

**Formato sugerido:**
> Como [rol] quiero [acción] para [beneficio]

**Preguntas guía:**
- ¿Quién usa la API?
- ¿Qué hacen?
- ¿Quién administra o aprueba?

### PASO 3: Modelo de Datos
**Tarea:** Define las entidades y sus relaciones.

**Preguntas guía:**
- ¿Qué guardas?
- ¿Cómo se conectan?
- ¿Qué es obligatorio, único, sensible?

### PASO 4: Endpoints
**Tarea:** Diseña los endpoints que necesites.

**Estructura sugerida:**
| Método | Ruta | Qué hace | Status |
|--------|------|----------|--------|

**Preguntas guía:**
- ¿Qué operaciones son clave?
- ¿Rutas claras y RESTful?
- ¿Filtros, paginación, errores?

### PASO 5: Arquitectura y Tecnologías
**Tarea:** Elige tu stack y justifícalo.

**Preguntas guía:**
- ¿Por qué FastAPI?
- ¿Qué base de datos? ¿Auth? ¿Despliegue?
- ¿Qué facilita tu visión?

### PASO 6: Presentación con diapositivas (15 min ensayo)
**Obligatorio:** Preparar un deck de 5 diapositivas para la exposición.

**Contenido sugerido:**
| Diap. | Contenido |
|------|-----------|
| 1 | Problema + Propuesta de valor |
| 2 | Usuarios + Historias clave |
| 3 | Modelo de datos |
| 4 | Endpoints principales |
| 5 | Stack + Visión final |

### PASO 7: Entregable
**Entrega obligatoria:**
- Archivo de diapositivas de la presentación (\*.pdf / \*.pptx o enlace a Google Slides).
- Archivo de audio de la exposición (15 min) en formato \*.mp3.
```


## **Ejemplo Completo: API de Turnos para Barbería**  
**"BarberBook API"**  
**Diseño conceptual listo para FastAPI**  
**Equipo: 3 personas | 90 min + 15 min exposición**

---

### **PASO 1: Tema + Valor**  
**Tema:** Sistema de reservas online para barberías pequeñas.  

**Propuesta de valor:**  
> *"Como cliente, reservo mi turno en 20 segundos desde el celular. Como barbero, veo mi agenda en tiempo real sin llamadas."*

**Problema real:**  
- Clientes llaman → barbero pierde tiempo → agenda en papel → errores.  

**Mejor que ahora:**  
- App simple, sin instalar nada, 24/7, recordatorios por WhatsApp.

---

### **PASO 2: Usuarios y Historias**  
#### Roles  
| Rol | Permisos |
|-----|----------|
| **Cliente** | Ver horarios, reservar, cancelar |
| **Barbero** | Ver agenda, confirmar, rechazar |
| **Admin** | Gestionar barberos, horarios |

#### 3 Historias Clave  
1. **Como cliente** quiero **ver horarios libres de hoy** para **elegir sin esperar**.  
2. **Como barbero** quiero **recibir notificación de nuevo turno** para **prepararme**.  
3. **Como cliente** quiero **cancelar con 1 hora de antelación** para **liberar el espacio**.

---

### **PASO 3: Modelo de Datos**  

```bash
Usuario
├── id (int)
├── nombre (str) *
├── teléfono (str) *
├── rol (cliente/barbero/admin)

Horario
├── id (int)
├── día_semana (lun, mar...) *
├── hora_inicio (time) *
├── hora_fin (time) *
├── barbero_id (FK)

Turno
├── id (int)
├── fecha_hora (datetime) *
├── estado (pendiente/confirmado/cancelado) *
├── cliente_id (FK)
├── horario_id (FK)
```

**Relaciones:**  
- 1 Barbero → muchos Horarios  
- 1 Horario + fecha → 1 Turno  
- 1 Cliente → muchos Turnos

---

### **PASO 4: Endpoints**  
| Método | Ruta | Qué hace | Status |
|--------|------|----------|--------|
| GET | `/api/v1/horarios` | Horarios libres de un barbero | 200 |
| POST | `/api/v1/turnos` | Reservar turno | 201 / 400 |
| GET | `/api/v1/turnos/mis` | Mis turnos (cliente) | 200 |
| PUT | `/api/v1/turnos/42` | Confirmar/cancelar (barbero) | 200 |
| DELETE | `/api/v1/turnos/42` | Cancelar (cliente) | 204 |

#### Ejemplo JSON (POST /turnos)  
**Request:**  
```json
{
  "horario_id": 7,
  "fecha": "2025-11-10",
  "hora": "10:30"
}
```

**Response (201):**  
```json
{
  "id": 101,
  "fecha_hora": "2025-11-10T10:30:00",
  "estado": "pendiente",
  "barbero": "Juan Pérez",
  "servicio": "Corte clásico"
}
```
---

### **PASO 5: Arquitectura y Tecnologías**  
| Componente | Tecnología | Justificación |
|-----------|------------|---------------|
| **API** | FastAPI | Docs automáticas, validación con Pydantic |
| **DB** | SQLite | Ideal para MVP, sin servidor |
| **Auth** | JWT | Login seguro, sin sesión |
| **Notificaciones** | WhatsApp (Twilio) | Clientes ya lo usan |
| **Despliegue** | Render / Railway | Gratis, 1 clic |

**Diagrama simple:**  
```
Cliente (App) → FastAPI → SQLite
                    ↓
                JWT Auth
                    ↓
             Twilio WhatsApp
```
---

### **PASO 6: Presentación 15 Min**  
| Min | Diapositiva | Qué decir |
|-----|-------------|-----------|
| 0-3 | 1 | "Imagina: llegas, te sientas, te cortan. Sin esperar. Esto es BarberBook." |
| 3-6 | 2 | "Cliente reserva → barbero ve → notificación → listo." |
| 6-9 | 3 | "3 tablas. Simple. Escalable." |
| 9-12 | 4 | "5 endpoints. Todo lo que necesitas. Nada más." |
| 12-15 | 5 | "Con FastAPI, en 1 semana tenemos MVP. En 3 meses, 50 barberías." |

**Cierre épico:**  
> _"BarberBook no es una API. Es tiempo ganado."_

---
