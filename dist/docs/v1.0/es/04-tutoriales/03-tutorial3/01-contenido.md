---
title: "Exponer tu servidor local a internet"
position: 1
date: 2025-10-27
---

## 🎯 ¿Para qué sirve ngrok?

Imagina que estás desarrollando una app en tu computadora (por ejemplo, en `http://localhost:8000`), pero necesitas que alguien de otro país la vea, o que un servicio externo (como WhatsApp, Stripe o Telegram) pueda enviarle datos.  
**ngrok** crea un **enlace público** (como `https://abc123.ngrok.io`) que redirige al servidor en tu máquina.

---

## ✅ Paso 1: Instalar ngrok

### Opción A: Descargar desde el sitio oficial (recomendado)

1. Ve a: [https://ngrok.com/download](https://ngrok.com/download)
2. Descarga la versión para tu sistema:
   - **Windows**: archivo `.zip`
   - **Mac**: `.zip` o usa Homebrew (`brew install ngrok/ngrok/ngrok`)
   - **Linux**: `.zip` o con snap (`sudo snap install ngrok`)
3. Descomprime el archivo. Obtendrás un ejecutable llamado `ngrok` (sin extensión en Mac/Linux) o `ngrok.exe` (en Windows).

> 💡 **Consejo**: Pon el archivo `ngrok` en una carpeta fácil de recordar, como `C:\herramientas\` o `~/Apps/`.

---

## 🔑 Paso 2: Crear una cuenta y autenticar

1. Ve a [https://dashboard.ngrok.com/signup](https://dashboard.ngrok.com/signup) y crea una cuenta gratuita.
2. Una vez logueado, ve a: [https://dashboard.ngrok.com/get-started/your-authtoken](https://dashboard.ngrok.com/get-started/your-authtoken)
3. Copia tu **authtoken** (es una cadena larga como `2FbXa9...`).
4. Abre tu terminal (o CMD/PowerShell en Windows) y ejecuta:

```bash
./ngrok config add-authtoken TU_TOKEN_AQUÍ
```

> En Windows (CMD):
> ```cmd
> ngrok.exe config add-authtoken TU_TOKEN_AQUÍ
> ```

✅ Esto vincula ngrok con tu cuenta y te da más funcionalidades (como URLs más estables en la versión gratuita).

---

## 🚀 Paso 3: Iniciar tu servidor local

Antes de usar ngrok, asegúrate de que tu app esté corriendo localmente.

### Ejemplo con FastAPI:

```bash
uvicorn main:app --reload --port 8000
```

Ahora tu app está en: `http://localhost:8000`

> Puedes usar cualquier puerto: 3000 (React), 5000 (Flask), 8080, etc.

---

## 🔗 Paso 4: Exponer tu servidor con ngrok

En otra terminal (o pestaña), navega a la carpeta donde está `ngrok` y ejecuta:

```bash
./ngrok http 8000
```

> En Windows:
> ```cmd
> ngrok.exe http 8000
> ```

Reemplaza `8000` por el puerto que uses.

### 🎉 ¡Listo! Verás algo como esto:

```bash
Session Status                online
Account                       tu@email.com (Plan: Free)
Version                       3.x.x
Region                        United States (us)
Forwarding                    https://abc123-456.ngrok.io -> http://localhost:8000
Forwarding                    http://abc123-456.ngrok.io -> http://localhost:8000
```

👉 **Copia la URL que empieza con `https://`** (ej: `https://abc123-456.ngrok.io`).

Ahora **cualquiera en internet** puede acceder a tu app local usando esa URL.

---

## 🌍 Pruébalo

1. Abre un navegador en tu celular (con datos móviles, no WiFi).
2. Pega la URL de ngrok.
3. ¡Deberías ver tu app!

También puedes compartir esa URL con un amigo o usarla en servicios como:
- Webhooks de WhatsApp
- Pruebas de APIs
- Integraciones con Zapier, etc.

---

## 📊 Panel de control en tiempo real

Mientras ngrok está activo, visita:  
👉 **http://127.0.0.1:4040**

Verás una interfaz web con:
- Todas las solicitudes entrantes
- Headers, parámetros, respuestas
- Reenvío de peticiones (¡muy útil para pruebas!)

---

## ❌ Detener ngrok

Presiona `Ctrl + C` en la terminal donde se ejecuta ngrok.

---

## 💡 Consejos útiles

| Necesitas... | Comando |
|-------------|--------|
| Usar otro puerto | `ngrok http 3000` |
| Ver logs en consola | Ya los ves por defecto |
| Acceder al panel web | `http://localhost:4040` |
| URL fija (solo en plan pago) | En gratis, la URL cambia cada vez |

> ⚠️ **Importante**: La versión gratuita genera una URL **aleatoria cada vez**. Si necesitas una URL fija, debes pagar o usar alternativas como **localhost.run** o **cloudflared**, pero ngrok es el más fácil.

---

## 🛑 Seguridad

- **No expongas apps con datos sensibles** (contraseñas, bases de datos).
- ngrok en plan gratuito es seguro para pruebas, pero **no para producción**.
- Si usas autenticación (tu token), ngrok encripta el tráfico (HTTPS).

---

