# Proyecto: OmniTrainer, moderación de contenido con IA para ACME Enterprise (Udacity Generative AI, sección 3)

Proyecto de la sección 3 del curso **Generative AI** de Udacity.

**Reto:** completar un sistema de moderación de contenido para el servicio al cliente de ACME Enterprise. Un agente en formación chatea con un cliente enfadado simulado por un LLM, y cada mensaje, imagen, video o audio que envía pasa antes por cuatro agentes de moderación con Google Gemini. Estos agentes detectan datos personales, tono poco profesional y medios perturbadores o de baja calidad, y devuelven resultados estructurados con Pydantic. Todo se sirve con Gradio (frontend), FastAPI (backend) y Arize Phoenix (trazado).

## Documentación

Documentación de la formación, en el mismo orden en que aparece en el curso:

| Archivo | Qué es |
|---|---|
| [docs/01-resumen-del-proyecto.md](docs/01-resumen-del-proyecto.md) | Resumen del proyecto: qué modera el sistema, el cliente simulado, la arquitectura en siete piezas y cómo avanzar paso a paso con las pruebas. |
| [docs/02-configuracion-del-entorno.md](docs/02-configuracion-del-entorno.md) | Entorno: instalación con uv y Python 3.12, el archivo `.env` (`GEMINI_API_KEY`, `USER_API_KEY`, `GOOGLE_GEMINI_BASE_URL`) y cómo abrir la app (puerto 7860) y Phoenix (puerto 6006), en el espacio de trabajo de Udacity y en la máquina local. |
| [docs/03-instrucciones.md](docs/03-instrucciones.md) | Instrucciones: los siete pasos en orden con su prueba (`types/moderation_result.py`, `agents/text_agent.py`, `agents/image_agent.py`, video y audio, `gradio_app.py`, todas las pruebas y las evaluaciones), cómo usar la app, Phoenix y la API, y la entrega con `RUN_EVIDENCE.pdf`. |
| [docs/04-rubrica.md](docs/04-rubrica.md) | Rúbrica oficial: modelos de salida estructurada, agentes de texto e imagen, interfaz Gradio con el cliente simulado, spans de trazado en Phoenix y evaluaciones con Pydantic Evals. |
| [docs/05-entrega.md](docs/05-entrega.md) | Entrega: fecha límite (14 de enero de 2027), los cuatro elementos de `RUN_EVIDENCE.pdf` y las formas de envío (ZIP o repositorio público de GitHub). |
