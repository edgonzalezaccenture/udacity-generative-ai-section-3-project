# Configuración del entorno

## Espacio de trabajo de Udacity

### Configurar el entorno

En el panel de la derecha, haz clic en **Cloud Resources**. Luego selecciona **Start Cloud Resource** y después **Open Cloud Console**.

En VS Code, selecciona el menú **File** y luego **Open Folder**. Elige el directorio de esta tarea: `/voc/work/code/project/starter/`.

### Instalar la app en modo de edición

En VS Code, abre una terminal.

Este proyecto necesita Python 3.12 o más reciente. El Python del espacio de trabajo es más antiguo, y no pasa nada: `uv sync` descarga Python 3.12 en el entorno virtual del proyecto.

Se instala la app en modo de edición para que los cambios en los archivos se reflejen de inmediato en la app instalada:

```bash
python --version
cd /voc/work/code/project/starter/
pip install uv
python -m uv sync
source .venv/bin/activate
python --version
uv pip install -e .
```

El primer `python --version` muestra el Python del espacio de trabajo (3.10 cuando se escribió esto). El segundo, después de `source .venv/bin/activate`, debe mostrar Python 3.12.x. Si sigue mostrando la versión anterior, el entorno virtual no está activo: vuelve a ejecutar `source .venv/bin/activate` desde `/voc/work/code/project/starter/`. No bajes `requires-python` en `pyproject.toml` para que funcione la instalación: el proyecto necesita 3.12.

### Configurar credenciales

En la terminal, ve a la raíz del repositorio y copia `env.example` a `.env`:

```bash
cp env.example .env
```

Abre `.env`, pega tu clave `voc-` en `GEMINI_API_KEY` y pon en `USER_API_KEY` una contraseña inventada, por ejemplo `my-api-key`. No puede estar vacía (más abajo se explica qué es).

La clave `voc-` se te asigna para este programa: haz clic en **Cloud Resources** en el panel derecho del aula (el mismo botón que usaste para iniciar el espacio de trabajo) y ahí aparece junto con tu presupuesto restante. La página *Get a Vocareum Gemini API Key*, en la lección *Introduction* del curso, lo explica paso a paso.

Deja la línea `GOOGLE_GEMINI_BASE_URL` de `.env` exactamente como está: en el espacio de trabajo la clave `voc-` pasa por el proxy de Gemini de Udacity, y si borras esa línea deja de funcionar. Una clave de Google AI Studio no sirve aquí; esa es la ruta de la máquina local, más abajo.

**Acerca de `USER_API_KEY`.** No es una credencial de Udacity, Google ni Vocareum: es una contraseña que inventas para tu propia app. La API del backend (`fastapi_app.py`) rechaza cualquier solicitud que no la incluya, y la app de chat la envía con cada solicitud. Como la inventas tú, nunca reutilices aquí una contraseña real ni una clave de API, y nunca hagas commit de `.env`, que también contiene tu clave `voc-` (el `.gitignore` del proyecto inicial ya excluye `.env`). Para este proyecto basta con un valor compartido en `.env`; una app real de producción daría a cada cliente su propia credencial y la guardaría de forma segura.

### Abrir la app y Phoenix en el navegador

En el espacio de trabajo, la app se ejecuta en la máquina del espacio de trabajo, así que las direcciones `localhost` de las instrucciones no se abren en tu navegador. Se accede a ellas a través de la dirección proxy del espacio de trabajo. La barra de direcciones no la muestra (muestra `labs.vocareum.com`), así que se pregunta a la terminal:

```bash
echo $VSCODE_PROXY_URI
```

Imprime algo como `https://vocproxy-1-23.us-west-2.vocareum.com/proxy/{{port}}/`. Sustituye `{{port}}` por el puerto que quieras:

| Para abrir | Usa |
|---|---|
| La app de chat | La dirección con `7860` en lugar de `{{port}}` |
| El panel de Phoenix | La dirección con `6006` en lugar de `{{port}}`, seguida de `projects` |

Phoenix necesita un paso más para verse a través del proxy. Antes de iniciar la app, ejecuta en la misma terminal:

```bash
export PHOENIX_HOST_ROOT_PATH=/proxy/6006
```

Sin eso, la página de Phoenix sale en blanco. Los espacios de trabajo más nuevos ya leen este ajuste desde `.env`; ejecutar el `export` de todos modos no hace daño, y es lo que lo hace funcionar en un espacio de trabajo configurado antes. En tu propia máquina no lo pongas: abre `http://localhost:6006/projects` directamente.

Cada terminal nueva empieza de cero. Si abres otra terminal (o vuelves al espacio de trabajo más tarde), ejecuta primero `cd /voc/work/code/project/starter/` y `source .venv/bin/activate` (si no, verás `uv: command not found`), y vuelve a ejecutar el `export` antes de iniciar la app.

## Máquina local

Abre una terminal en la carpeta del kit inicial.

### Instalar uv

Este proyecto usa [uv](https://docs.astral.sh/uv/) para gestionar Python y las dependencias, y necesita Python 3.12 o más reciente. No hace falta instalar Python 3.12: el paso `uv sync` lo descarga en el entorno virtual del proyecto si tu máquina no lo tiene. Instala uv una sola vez siguiendo su guía de instalación, por ejemplo con `pip install uv` o, en macOS y Linux, con `curl -LsSf https://astral.sh/uv/install.sh | sh`. Compruébalo con `uv --version`.

### Crear el entorno virtual e instalar las dependencias

```bash
uv sync --dev
```

Comprueba que funcionó: `uv run python --version` debe mostrar Python 3.12 o más reciente.

### Instalar la app en modo de edición

```bash
uv pip install -e .
```

### Usar el entorno virtual como intérprete del proyecto

En VS Code, pulsa `Shift+Ctrl+P` (o `Shift+Command+P` en Mac), selecciona **Python: Select Interpreter**, luego **Enter Interpreter Path** y escribe `.venv/bin/python`. Ayuda con el resaltado de sintaxis y otras cosas.

### Configurar credenciales

En la raíz del repositorio, copia `env.example` a `.env`:

```bash
cp env.example .env
```

Abre `.env`, pon tu credencial en `GEMINI_API_KEY` y en `USER_API_KEY` una contraseña inventada, por ejemplo `my-api-key`. No puede estar vacía.

En tu propia máquina usa una clave de Google AI Studio: la clave `voc-` del espacio de trabajo no funciona fuera de él. `env.example` también pone `GOOGLE_GEMINI_BASE_URL` apuntando al proxy de Gemini de Udacity, que solo funciona en el espacio de trabajo: borra o comenta esa línea, o tu clave de Google AI Studio se enviará al proxy y no se autenticará. Si `.env` tiene líneas `PHOENIX_HOST_ROOT_PATH` o `API_ROOT_PATH`, bórralas también: solo sirven para el proxy del espacio de trabajo y, en tu máquina, rompen las direcciones de Phoenix y de la documentación de la API que usan estas instrucciones.

**Acerca de `USER_API_KEY`.** No es una credencial de Google ni de Udacity: es una contraseña que inventas para tu propia app, y la API del backend la comprueba en cada solicitud. Nunca reutilices aquí una contraseña real ni una clave de API, y nunca hagas commit de `.env` (el `.gitignore` del proyecto inicial ya lo excluye). Una app real de producción daría a cada cliente su propia credencial y la guardaría de forma segura.
