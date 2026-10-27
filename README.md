# Proyecto: moderación de contenido con IA para ACME Enterprise (Udacity Generative AI, sección 3)

Proyecto de la sección 3 del curso **Generative AI** de Udacity.

**Reto:** completar un sistema de moderación de contenido para el servicio al cliente de ACME Enterprise. Un agente en formación chatea con un cliente enfadado simulado por un LLM, y cada mensaje, imagen, video o audio que envía pasa antes por cuatro agentes de moderación con Google Gemini. Estos agentes detectan datos personales, tono poco profesional y medios perturbadores o de baja calidad, y devuelven resultados estructurados con Pydantic. Todo se sirve con Gradio (frontend), FastAPI (backend) y Arize Phoenix (trazado).

## Documentación

Documentación de la formación, en el mismo orden en que aparece en el curso:

| Archivo | Qué es |
|---|---|
| [docs/01-resumen-del-proyecto.md](docs/01-resumen-del-proyecto.md) | Resumen del proyecto: qué modera el sistema, el cliente simulado, la arquitectura en siete piezas y cómo avanzar paso a paso con las pruebas. |
| [docs/02-configuracion-del-entorno.md](docs/02-configuracion-del-entorno.md) | Entorno: instalación con uv y Python 3.12, el archivo `.env` (`GEMINI_API_KEY`, `USER_API_KEY`, `GOOGLE_GEMINI_BASE_URL`) y cómo abrir la app (puerto 7860) y Phoenix (puerto 6006), en el espacio de trabajo de Udacity y en la máquina local. |
