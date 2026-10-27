# Entrega

**Proyecto:** OmniTrainer: entrenador multimodal para servicio al cliente.

**Fecha límite:** 14 de enero de 2027.

Se entrega el proyecto inicial completado como archivo ZIP o como repositorio, junto con un archivo de evidencia en el nivel superior llamado **`RUN_EVIDENCE.pdf`** con estos cuatro elementos:

1. **Pegado como texto:** la salida de `uv run pytest tests/ -vv --ignore=tests/test_env_setup.py`. Este único comando cubre todas las pruebas de los criterios.
2. **Pegado como texto:** la salida de una evaluación, por ejemplo `uv run evals/text/test_cases.py`. Se espera menos del 100 % y no es un defecto.
3. **Una captura de pantalla:** una conversación en Gradio donde la moderación marcó un mensaje, con la justificación tal como apareció.
4. **Una captura de pantalla:** las trazas en el panel de Phoenix, con los nombres de los spans.

Los elementos 1 y 2 mejor como texto que como captura de la terminal: al evaluador le es más fácil leerlos y a ti producirlos. Se acepta una captura legible de la misma salida. Los elementos 3 y 4 tienen que ser capturas: describir lo que salió en pantalla no demuestra que saliera.

Un único PDF es lo más seguro. También se puede enviar `RUN_EVIDENCE.md` con las capturas al lado.

`tests/test_env_setup.py` se excluye del comando anterior porque comprueba tus credenciales y hace llamadas reales a la API de Gemini.

## Formas de envío

- **Archivo ZIP:** comprimir los archivos del proyecto en un único ZIP. Límite de 500 MB sin comprimir.
- **Repositorio público de GitHub.**
