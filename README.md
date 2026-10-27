# Proyecto: OmniTrainer, moderación de contenido con IA para ACME Enterprise (Udacity Generative AI, sección 3)

Proyecto de la sección 3 del curso **Generative AI** de Udacity.

**Reto:** completar un sistema de moderación de contenido para el servicio al cliente de ACME Enterprise. Un agente en formación chatea con un cliente enfadado simulado por un LLM, y cada mensaje, imagen, video o audio que envía pasa antes por cuatro agentes de moderación con Google Gemini. Estos agentes detectan datos personales, tono poco profesional y medios perturbadores o de baja calidad, y devuelven resultados estructurados con Pydantic. Todo se sirve con Gradio (frontend), FastAPI (backend) y Arize Phoenix (trazado).

## Contenido

| Archivo | Qué es |
|---|---|
| [RUN_EVIDENCE.pdf](RUN_EVIDENCE.pdf) | **Evidencia de la entrega:** salida de las pruebas, salida de la evaluación de texto, captura de Gradio con un mensaje marcado y su justificación, y capturas de Phoenix con los spans (`conversation`, `chat_turn`, `moderate_text`, `moderate_image`, `llm_customer` y el atributo `feedback`). |
| [multimodal_moderation/](multimodal_moderation/) | La aplicación. Los TODO completados están en `types/moderation_result.py`, `agents/text_agent.py`, `agents/image_agent.py` y `gradio_app.py`. |
| [evals/](evals/) | Evaluaciones con Pydantic Evals; los TODO completados están en `evals/text/test_cases.py` y `evals/image/test_cases.py`. |
| [tests/](tests/) | Pruebas proporcionadas, sin modificar. |
| [env.example](env.example) | Plantilla del `.env`. El `.env` real no se sube porque contiene la clave. |

**Ejecutar** (con `.env` creado a partir de `env.example`, ver [docs/02-configuracion-del-entorno.md](docs/02-configuracion-del-entorno.md)):

```bash
uv sync --dev
uv run pytest tests/ -vv --ignore=tests/test_env_setup.py
uv run evals/text/test_cases.py
uv run multimodal-moderation
```

## Documentación

Documentación de la formación, en el mismo orden en que aparece en el curso:

| Archivo | Qué es |
|---|---|
| [docs/01-resumen-del-proyecto.md](docs/01-resumen-del-proyecto.md) | Resumen del proyecto: qué modera el sistema, el cliente simulado, la arquitectura en siete piezas y cómo avanzar paso a paso con las pruebas. |
| [docs/02-configuracion-del-entorno.md](docs/02-configuracion-del-entorno.md) | Entorno: instalación con uv y Python 3.12, el archivo `.env` (`GEMINI_API_KEY`, `USER_API_KEY`, `GOOGLE_GEMINI_BASE_URL`) y cómo abrir la app (puerto 7860) y Phoenix (puerto 6006), en el espacio de trabajo de Udacity y en la máquina local. |
| [docs/03-instrucciones.md](docs/03-instrucciones.md) | Instrucciones: los siete pasos en orden con su prueba (`types/moderation_result.py`, `agents/text_agent.py`, `agents/image_agent.py`, video y audio, `gradio_app.py`, todas las pruebas y las evaluaciones), cómo usar la app, Phoenix y la API, y la entrega con `RUN_EVIDENCE.pdf`. |
| [docs/04-rubrica.md](docs/04-rubrica.md) | Rúbrica oficial: modelos de salida estructurada, agentes de texto e imagen, interfaz Gradio con el cliente simulado, spans de trazado en Phoenix y evaluaciones con Pydantic Evals. |
| [docs/05-entrega.md](docs/05-entrega.md) | Entrega: fecha límite (14 de enero de 2027), los cuatro elementos de `RUN_EVIDENCE.pdf` y las formas de envío (ZIP o repositorio público de GitHub). |
