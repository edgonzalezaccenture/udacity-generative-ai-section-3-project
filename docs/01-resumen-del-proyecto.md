# Resumen del proyecto

## Visión general

Este es un sistema de moderación de contenido impulsado por IA para interacciones de servicio al cliente en una empresa ficticia llamada ACME Enterprise.

**Qué hace:**

- Modera texto, imágenes, videos y audio antes de que se envíen a los clientes.
- Detecta problemas como: PII (información personal identificable), tono poco profesional, contenido poco amigable, imágenes/videos perturbadores y medios de baja calidad.
- Bloquea contenido dañino y proporciona explicaciones detalladas sobre por qué el contenido fue marcado.

La aplicación permite que un agente en formación interactúe con un cliente simulado enojado, interpretado por un modelo de lenguaje grande (LLM). El cliente LLM ha comprado un producto de ACME (el ACME Power Widget Pro) y el producto dejó de funcionar. El agente de servicio al cliente debe manejar este caso chateando con el cliente, y cada mensaje y contenido proporcionado por el agente en formación se modera y supervisa para garantizar que todas las comunicaciones cumplan con los estándares de la empresa.

## Arquitectura

1. **Agentes especializados:** cuatro agentes de moderación (texto, imagen, video, audio), cada uno usando IA de Google Gemini con prompts personalizados para verificar su tipo específico de contenido (los agentes están en la carpeta `agents`).
2. **LLM como cliente:** un agente (`agents/customer_agent.py`).
3. **Resultados estructurados:** cada agente devuelve un modelo Pydantic con indicadores específicos (por ejemplo, `contiene_pii`, `es_poco_amigable`) además de una justificación. Las definiciones están en `types/moderation_result.py`.
4. **Frontend:** interfaz de chat Gradio, una interfaz web interactiva donde los usuarios pueden chatear y subir archivos, con moderación en tiempo real. Esto es `gradio_app.py`.
5. **Backend:** API REST de FastAPI, con puntos finales HTTP para acceso programático (`/moderate/text`, `/moderate/image`, etc.). Se ofrecen tal cual: simplemente envuelven en puntos finales HTTP la funcionalidad de los agentes. Esto es `fastapi_app.py`. La división en servicios frontend/backend es típica de aplicaciones web y permite que diferentes frontends usen los mismos servicios del backend. Por ejemplo, tras la prueba de concepto con la app Gradio, podríamos pasar a una aplicación React, Vue o Angular más orientada a producción. Esa nueva aplicación puede usar el mismo backend, y ambas pueden coexistir hasta que se compruebe que la nueva funciona. No se necesita ningún cambio en los agentes de IA ni en el backend.
6. **Observabilidad:** integración con Phoenix para trazado y monitoreo del comportamiento de los agentes de IA. Parte de la configuración está en `tracing.py`.
7. **Un ejecutable** que inicia de forma conveniente los tres servicios: el backend (API REST de FastAPI), el frontend (la app Gradio) y Arize Phoenix para trazado.

## Cómo trabajar en el proyecto

Se te proporciona la estructura base. Completarás partes clave del código, aplicando lo que has aprendido, siguiendo una secuencia precisa de pasos.

Cada paso debe ejecutarse en orden. Puedes comprobar que el paso se completó correctamente ejecutando las pruebas indicadas (ya proporcionadas).

**No saltes pasos ni avances si las pruebas no pasan**, ya que los siguientes pasos dependen del paso anterior.
