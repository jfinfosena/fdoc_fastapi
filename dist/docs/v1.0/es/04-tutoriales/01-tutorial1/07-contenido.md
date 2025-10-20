---
title: "Chat en consola con Gemini"
position: 5
---

Este ejemplo muestra cómo crear un chat de consola utilizando la librería oficial `google-genai` y el modelo `gemini-2.0-flash`.

## Requisitos del entorno

```bash
python -m venv .venv
# Activar entorno ...
pip install -q -U google-genai
```

## Código de ejemplo (chat en consola)

```python
from google import genai

# Función que genera la respuesta
def generar_respuesta(prompt: str) -> str:
    if not prompt:
        return "Por favor, ingresa un tema o pregunta."
    try:
        client = genai.Client(api_key="AIzaSyBiiY7LJXnedAOsA1DM0Cv8lIj8-hnd_z0")  # Código original
        response = client.models.generate_content(
            model="gemini-2.0-flash", contents=prompt  # Código original con prompt dinámico
        )
        return response.text
    except Exception as e:
        return f"Error: {str(e)}"

# Lógica principal (interfaz de consola)
print("Chat con Gemini")
print("Ingresa un tema o pregunta para obtener una respuesta generada por Gemini.")
print("Escribe 'salir' para terminar.\n")

while True:
    prompt = input("Escribe tu pregunta o tema: ").strip()
    if prompt.lower() == 'salir':
        print("¡Hasta luego!")
        break
    if prompt:
        print("Generando respuesta...\n")
        respuesta = generar_respuesta(prompt)
        print("Respuesta:")
        print(respuesta)
        print("\n" + "="*50 + "\n")
    else:
        print("Por favor, ingresa un tema o pregunta válida.\n")
```

## Ejecución

```bash
python chat_gemini.py
```

- Escribe tus preguntas y recibe respuestas generadas por el modelo.
- Escribe `salir` para terminar el programa.

## Notas y recomendaciones

- Mantén tu clave segura: usa variables de entorno o un gestor de secretos.
- Si ves errores de autenticación, verifica que `GOOGLE_API_KEY` está definida y que la clave es válida.
- Para proyectos más grandes, separa la configuración y la lógica en módulos distintos.