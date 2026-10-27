# Instrucciones

## Ejecución

Sigue estos pasos en orden.

### 1. Resultado de moderación

Aquí se configuran los modelos Pydantic de los agentes de moderación. Son los esquemas de salida de los agentes de texto, audio, imagen y video con los que se trabaja después.

1. Edita `types/moderation_result.py` y sustituye las secciones `# TODO` por tu código.
2. Ejecuta la prueba desde la raíz del proyecto inicial: `uv run pytest tests/test_moderation_result.py -vv`. Si da error, léelo con cuidado: indica qué corregir.

**No continúes a menos que la prueba pase.**

### 2. Agente de texto

Aquí se completa la configuración del agente de moderación de texto.

1. Edita `agents/text_agent.py` y sustituye las secciones `# TODO`.
2. Ejecuta: `uv run pytest tests/test_text_agent.py -vv`.

**No continúes a menos que la prueba pase.**

### 3. Agente de imagen

Igual que con el texto, pero para imágenes. Como es un agente multimodal, se usan las capacidades multimodales de Pydantic AI y Gemini.

1. Edita `agents/image_agent.py` y sustituye las secciones `# TODO`.
2. Ejecuta: `uv run pytest tests/test_image_agent.py -vv`.

**No continúes a menos que la prueba pase.**

### 4. Agentes de video y audio

Ya están completos. Solo hay que probarlos: `uv run pytest tests/test_video_agent.py` y `uv run pytest tests/test_audio_agent.py`.

### 5. Aplicación Gradio

Aquí se junta todo en una app Gradio, la interfaz de usuario de la solución.

1. Edita `gradio_app.py` y sustituye todas las secciones `# TODO`. Hay bastantes, de arriba abajo. Antes de seguir, busca `TODO` en el archivo (Ctrl+F o Command+F) para no dejarte ninguna.
2. Ejecuta: `uv run pytest tests/test_gradio_app.py -vv`.

**No continúes a menos que la prueba pase.**

### 6. Ejecutar todas las pruebas

Ejecuta todas las pruebas del proyecto a la vez:

```bash
uv run pytest tests/ -vv --ignore=tests/test_env_setup.py
```

Todas deberían pasar. Si alguna falla, corrígela antes de entregar, y **guarda esta salida**: es lo que va en el archivo de evidencia de la entrega.

**Qué comprueban estas pruebas.** Ninguna llama a Gemini (las pruebas de agentes usan un modelo simulado), así que no cuestan nada y comprueban el cableado: que cada agente esté configurado y conectado bien. Aun así, necesitan un `.env` con `GEMINI_API_KEY`, `USER_API_KEY` y `DEFAULT_GOOGLE_MODEL` no vacíos, o se paran con `ValueError: … required but not set`.

`tests/test_env_setup.py` se excluye porque sí llama a Gemini para comprobar tu configuración, igual que las evaluaciones del paso 7, que miden lo bien que tus agentes juzgan el contenido. Con las credenciales puestas, puedes incluirla quitando el `--ignore`:

```bash
uv run pytest tests/ -vv
```

### 7. Evaluaciones

Con el sistema completo, hay que ejecutar evaluaciones para entender y medir su comportamiento.

1. Edita `evals/text/test_cases.py` y sustituye todas las secciones `# TODO`. Lo mismo con `evals/image/test_cases.py`.
2. Ejecuta `uv run evals/text/test_cases.py` para ver los resultados en texto. **Nota:** la puntuación **no** siempre será 100 %, y eso no es un error: el agente de texto no es perfecto.
3. Ejecuta las evaluaciones de los demás medios: `uv run evals/image/test_cases.py`, `uv run evals/audio/test_cases.py` y `uv run evals/video/test_cases.py`.

#### Cómo interpretar los resultados

A diferencia de las pruebas, las evaluaciones sí llaman a Gemini: en cada caso, una vez para el agente evaluado y otra para un juez LLM (`LLMJudge`) que califica la justificación del agente. Los dos son modelos de lenguaje, así que el mismo caso puede pasar en una ejecución y fallar en la siguiente. Medir esa variación es el propósito de las evaluaciones, y por eso se espera menos del 100 %, lo cual no es un defecto de la entrega.

`EVAL_NUM_REPEATS` en `.env` fija cuántas veces se ejecuta cada caso. `env.example` lo pone a `1`, rápido y barato: déjalo así mientras escribes los casos. Subirlo (por ejemplo a 5) da una puntuación más estable pero multiplica las llamadas a la API, que en el espacio de trabajo salen del presupuesto de tu clave `voc-`. (Si tu `.env` dice `5`, viene de un `env.example` antiguo.) Cada evaluación lee `.env` de nuevo, así que no hay que reiniciar nada.

Qué mirar: que cada caso se ejecute sin error, que los casos inaceptables se marquen casi siempre y que los aceptables casi nunca. Cuando un caso falla, lee la justificación del agente en la salida: suele indicar si se equivoca el agente o tu valor esperado.

## Probar la aplicación

### Conversación

Al terminar, la app debería funcionar.

En el espacio de trabajo, ejecuta primero esto para que Phoenix se vea a través del proxy (ver *Abrir la app y Phoenix en el navegador* en la [configuración del entorno](02-configuracion-del-entorno.md)). En tu propia máquina, sáltatelo.

```bash
export PHOENIX_HOST_ROOT_PATH=/proxy/6006
```

Luego inicia la app:

```bash
uv run multimodal-moderation
```

Esto arranca tres servicios: la app de chat en el puerto 7860, la API del backend en el 8000 y Phoenix en el 6006. Espera a que la terminal muestre una línea `Phoenix UI:` y otra `Running on local URL`, y abre la app de chat: en tu máquina en `http://localhost:7860/`; en el espacio de trabajo, en tu dirección `VSCODE_PROXY_URI` con `7860` en lugar de `{{port}}`. Puedes usarla libremente; como inspiración, una conversación podría ir así:

```
YOU> Welcome to ACME Customer Service. How can I help?
CUSTOMER-LLM> ... [will complain about the product not working]
YOU> I am sorry to hear that. Is this the product you are talking about? [attach evals/test_data/professional_image.jpg]
CUSTOMER-LLM> ... [will say something about wanting a refund]
YOU> I am sorry but I absolutely cannot offer a refund
------> message will be flagged as rude
YOU> I am going to help you solving your issue. I am authorized to offer you a replacement. Would you be willing to accept it?
CUSTOMER-LLM> ... [probably won't accept]
[you can now close the conversation by clicking on the End Conversation button]
```

### Ver las trazas

Después de una conversación como la de arriba, abre Phoenix: en tu máquina en `http://localhost:6006/projects`; en el espacio de trabajo, en tu dirección `VSCODE_PROXY_URI` con `6006` en lugar de `{{port}}`, seguida de `projects`. (En el espacio de trabajo la terminal imprime `Phoenix UI: http://localhost:6006/proxy/6006`, pero esa dirección es interna y no se abre en tu navegador.) Haz clic en el proyecto por defecto y mira las trazas y los spans, los metadatos y los informes. **Esta es la vista que hay que capturar para el archivo de evidencia.**

Si Phoenix sale en blanco en el espacio de trabajo, la app se inició sin `PHOENIX_HOST_ROOT_PATH`. Párala con Ctrl+C, ejecuta `export PHOENIX_HOST_ROOT_PATH=/proxy/6006`, iníciala de nuevo y haz otra conversación para que haya trazas.

### Ver las APIs del backend

Ve a `http://localhost:8000/docs` para ver la documentación de las APIs de moderación. (En el espacio de trabajo, usa tu dirección `VSCODE_PROXY_URI` con `8000` en lugar de `{{port}}`, seguida de `docs`. Si sale «Failed to load API definition», tu espacio de trabajo se configuró antes de que se corrigiera esto: sáltate esta parte, no hace falta para la entrega.) Para probarlas, haz clic en **Authorize** arriba a la derecha y pon tu `USER_API_KEY` (la de `.env`). Luego elige un endpoint (por ejemplo `/api/v1/moderate-text`) y haz clic en **Try it out**. Verás un JSON como:

```json
{
  "text": "string"
}
```

Cambia `string` por un mensaje y haz clic en **Execute**: el mensaje se modera. Baja un poco para ver el resultado.

### Parar y reiniciar la app

`uv run multimodal-moderation` sigue en marcha hasta que lo pares. Usa cuatro puertos: 7860 (app de chat), 8000 (API del backend), 6006 (panel de Phoenix y recolector de trazas) y 4317 (segundo recolector de trazas de Phoenix).

- **Para pararla**, haz clic en su terminal y pulsa Ctrl+C. Eso para los tres servicios. Cerrar la pestaña del navegador no los para, y cerrar la terminal puede dejarlos en marcha.
- **Reinicia tras cada cambio en `.env`:** Ctrl+C y otra vez `uv run multimodal-moderation`. La app solo lee `.env` al arrancar. Reinicia también tras cambiar el código de un agente o de la app de chat antes de probarlo en la app (las pruebas siempre usan tu código más reciente).
- **«Address already in use»** (por ejemplo `Failed to bind to address [::]:4317`, o lo mismo con 6006, 7860 u 8000) significa que sigue en marcha una copia anterior: normalmente una iniciada en una terminal que cerraste, o un notebook de demo de Phoenix de una lección anterior. Para la copia antigua con:

  ```bash
  pkill -f multimodal-moderation
  ```

  Si el error sigue, probablemente un notebook ocupa el puerto: apaga su kernel (en Jupyter, *Kernel → Shut Down Kernel*) e inicia la app de nuevo.
- **En una terminal nueva** (en el espacio de trabajo), ejecuta primero `cd /voc/work/code/project/starter/` y `source .venv/bin/activate` (si no, no se encuentra `uv`), luego `export PHOENIX_HOST_ROOT_PATH=/proxy/6006` y por último inicia la app.

## Instrucciones para la entrega

Se puede entregar desde el espacio de trabajo, en un ZIP o con un repositorio de GitHub.

Junto con el código, hay que incluir en el nivel superior un archivo de evidencia llamado **`RUN_EVIDENCE.pdf`** con estas cuatro cosas:

1. **Pegado como texto:** la salida de `uv run pytest tests/ -vv --ignore=tests/test_env_setup.py`. Este único comando cubre todas las pruebas de la rúbrica.
2. **Pegado como texto:** la salida de una evaluación, por ejemplo `uv run evals/text/test_cases.py`. Se espera menos del 100 % y no es un defecto.
3. **Una captura de pantalla:** una conversación en la interfaz de Gradio donde la moderación marcó un mensaje, mostrando la justificación tal como apareció.
4. **Una captura de pantalla:** las trazas en el panel de Phoenix, con los nombres de los spans. En el espacio de trabajo, abre Phoenix como se explica en *Ver las trazas*; si sale en blanco, sigue la nota de ese apartado antes de capturar.

**Por qué cambian los formatos y dónde hay margen.** Los puntos 1 y 2 son salidas de una herramienta, así que es mejor pegar el texto que una imagen: el evaluador puede leerlo y buscar en él, y tú lo copias de una vez. Si ya hiciste una captura legible de esa salida, también vale. Los puntos 3 y 4 son distintos: solo una pantalla puede demostrar que la interfaz mostró la marca y su justificación, y que los spans llegaron a Phoenix. Una descripción en palabras solo demuestra que sabes lo que debería haber pasado, así que para esos dos hace falta captura.

**Un único PDF es lo más seguro.** Se puede entregar `RUN_EVIDENCE.md` con las capturas al lado, pero los `.png` o `.jpg` sueltos pueden no aparecer en la lista de archivos del evaluador.

`tests/test_env_setup.py` se excluye del comando anterior porque comprueba tus credenciales y llama de verdad a la API de Gemini.

Este archivo forma parte de la entrega, no es un extra: el evaluador revisa el código y la evidencia, y una entrega sin ella se devuelve para otro intento.
