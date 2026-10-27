# Rúbrica

Utilice esta rúbrica del proyecto para comprender y evaluar los criterios del proyecto.

## Salidas estructuradas de moderación

| Criterios | Requisitos de entrega |
|---|---|
| El proyecto define y utiliza un modelo estructurado de salida para moderación | • El modelo base `ModerationResult` define únicamente el campo compartido `rationale: str` (requerido, sin valor por defecto).<br>• Cada subclase por modalidad define sus propias banderas en la clase correcta y no añade banderas específicas de modalidad a la base: texto → `contains_pii`, `is_unfriendly`, `is_unprofessional`; imagen/video → `contains_pii`, `is_disturbing`, `is_low_quality`; audio → según el TODO de `AudioModerationResult`.<br>• Los campos de bandera son de tipo `bool` y requeridos, sin valor por defecto, exactamente como los definen las clases `TextModerationResult`, `ImageModerationResult` y `VideoModerationResult` proporcionadas; `rationale` es obligatorio.<br>• El modelo correcto se importa y se usa en cada agente de moderación correspondiente.<br>• Todas las pruebas de `tests/test_moderation_result.py` pasan sin modificaciones. |

## Agentes de moderación

| Criterios | Requisitos de entrega |
|---|---|
| El proyecto incluye un agente funcional de moderación de texto que analiza los mensajes del usuario | • El agente de `agents/text_agent.py` usa Gemini o un LLM similar para detectar información personal identificable (PII), tono poco profesional y contenido poco amigable.<br>• El agente devuelve resultados como una instancia válida de `ModerationResult`.<br>• El análisis del modelo y la lógica de respuesta funcionan según lo previsto.<br>• Todas las pruebas de `tests/test_text_agent.py` pasan sin modificaciones (lo cubre la salida de pruebas del archivo de evidencia). |
| El proyecto incluye un agente de moderación de imágenes que procesa entradas binarias y devuelve resultados estructurados | • El agente de `agents/image_agent.py` envuelve correctamente los bytes de la imagen subida en un objeto `BinaryContent`.<br>• El agente usa Gemini o un LLM equivalente para identificar contenido visual perturbador o inapropiado.<br>• La salida cumple con el modelo `ModerationResult`.<br>• Todas las pruebas de `tests/test_image_agent.py` pasan sin modificaciones (lo cubre la salida de pruebas del archivo de evidencia). |

## Interfaz de moderación multimodal

| Criterios | Requisitos de entrega |
|---|---|
| El proyecto incluye una interfaz de chat de Gradio que maneja entradas multimodales e integra moderación | • `gradio_app.py` implementa una `gr.ChatInterface` que admite texto y archivos subidos.<br>• Todos los mensajes del usuario y los archivos subidos se moderan antes de mostrarse.<br>• La interfaz mantiene el estado de la conversación entre los mensajes del usuario y los del cliente.<br>• Todos los TODO relacionados con la moderación están completados.<br>• `tests/test_gradio_app.py` pasa sin modificaciones (lo cubre la salida de pruebas del archivo de evidencia). |
| El proyecto integra un agente cliente LLM simulado que participa en las conversaciones | • La llamada `customer_agent.run(...)` está implementada de forma asíncrona y se usa para generar respuestas dinámicas del cliente.<br>• La conversación previa se pasa al agente cliente (el TODO `past_messages_state` de `gradio_app.py`), para que las respuestas mantengan coherencia con el contexto.<br>• La ruta de moderación está configurada para que los mensajes marcados se muestren con su justificación y no se envíen al agente cliente.<br>• El archivo de evidencia incluye una captura de una conversación en Gradio con un evento de moderación marcado, mostrando la justificación tal como apareció. |

## Observabilidad y trazado

| Criterios | Requisitos de entrega |
|---|---|
| El proyecto incluye instrumentación completa de trazado con OpenTelemetry y Arize Phoenix | • Se implementan spans para `moderate_text`, `chat_turn`, `conversation` (con `session.id`) y `feedback`.<br>• Los spans se crean con el trazador definido en `tracing.py`.<br>• Cada span incluye atributos relevantes, como identificadores de sesión o el contenido de la retroalimentación.<br>• El archivo de evidencia incluye una captura del panel de Phoenix que muestra estos spans de una ejecución. |

## Evaluaciones de moderación

| Criterios | Requisitos de entrega |
|---|---|
| El proyecto define y ejecuta casos de evaluación para los agentes de moderación con Pydantic Evals | • `evals/text/test_cases.py` y `evals/image/test_cases.py` incluyen casos de prueba con contenido aceptable e inaceptable.<br>• Las evaluaciones usan entradas estructuradas y devuelven resultados coherentes con `ModerationResult`.<br>• Todos los TODO de ambos archivos están completos, incluidos los valores esperados de las banderas de cada caso.<br>• El archivo de evidencia incluye la salida de una evaluación. Se espera una puntuación por debajo del 100 % y no es un defecto. |

## Sugerencias para hacer destacar el proyecto

- Ampliar la moderación con banderas adicionales (por ejemplo, discurso de odio, spam o desinformación).
- Añadir análisis visuales o paneles que resuman el contenido marcado usando las APIs del backend.
- Introducir variaciones de persona en el agente cliente LLM para simular distintos tonos emocionales o escenarios.
