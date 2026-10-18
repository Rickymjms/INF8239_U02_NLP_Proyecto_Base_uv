# Registro de Incidencias Técnicas y Soluciones de Reproducibilidad (U02.LAB04)

**Proyecto:** Localizar, aprobar y auditar un corpus público de NLP  
**Entorno de desarrollo:** Python 3.12 (`uv`) en Windows  
**Dataset seleccionado:** `SetFit/amazon_reviews_multi_es`  

---

## 1. Error HTTP 404 al intentar la descarga directa por URL
* **Incidencia:** Al ejecutar `download_data.py` con `DATA_SOURCE=url`, el script devolvió un error `requests.exceptions.HTTPError: 404 Client Error: Not Found`.
* **Causa:** Hugging Face reubicó el repositorio original de `amazon_reviews_multi` y migró su almacenamiento interno a archivos Parquet, imposibilitando el acceso directo a un archivo `.csv` crudo mediante peticiones HTTP convencionales.
* **Solución aplicada:** Se cambió el proyecto al **Modo Local** (`DATA_SOURCE=local` en el `.env`) y se utilizó la librería oficial `datasets` de Hugging Face para descargar el *split* `train` de `SetFit/amazon_reviews_multi_es`, exportándolo localmente como CSV estructurado en `data/raw/dataset.csv` (200,000 registros).

---

## 2. Desalineación de rutas en `.env` y lectura del dataset de prueba (*sample*)
* **Incidencia:** El script de auditoría (`audit_data.py`) ejecutaba la evaluación sobre 18 filas correspondientes a `data/sample/demo_text.csv` en lugar del dataset real de Amazon.
* **Causa:** En el archivo `.env`, la variable `DATASET_PATH` apuntaba a `data/raw/amazon_reviews_es.csv`, mientras que el script de extracción guardaba el archivo en `data/raw/dataset.csv`. Ante la discrepancia, la lógica del proyecto recurría al conjunto de prueba.
* **Solución aplicada:** Se estandarizó la variable en el archivo `.env` estableciendo explícitamente `DATASET_PATH=data/raw/dataset.csv`, logrando que `audit_data.py` leyera correctamente el corpus masivo auditado.

---

## 3. Bloqueo de permisos en Windows / OneDrive (`Access is denied - os error 5`)
* **Incidencia:** Al intentar sincronizar o instalar dependencias mediante `uv add datasets` o `uv sync`, la consola devolvió el error `failed to remove directory ... Access is denied. (os error 5)`.
* **Causa:** El proyecto se encuentra dentro de un directorio sincronizado en tiempo real por **Microsoft OneDrive** (`CI-MACDIA - Documents`). Además, servidores de lenguaje en segundo plano (como Pylance en VS Code) mantenían bloqueados los archivos del paquete local dentro de `.venv`.
* **Solución aplicada:**
  1. Se forzó el cierre de procesos de Python activos mediante PowerShell: `Stop-Process -Name "python*" -Force`.
  2. Se ejecutó la instalación directa en el entorno utilizando `uv pip install datasets`, evitando el intento de desinstalación/reconstrucción de paquetes que desencadenaba el error de permisos.

---

## 4. Ausencia del gestor `pip` dentro del entorno virtual (`ModuleNotFoundError: No module named pip`)
* **Incidencia:** La llamada `python -m pip install datasets` falló indicando que el módulo `pip` no existía.
* **Causa:** Los entornos virtuales gestionados por `uv` son ligeros y optimizados, omitiendo la instalación por defecto de `pip` dentro de `.venv`.
* **Solución aplicada:** Se utilizó la herramienta interna de resolución de paquetes del CLI de `uv` (`uv pip install datasets`), la cual gestiona las librerías sin requerir el módulo `pip` tradicional de Python.

---

## 5. Advertencias sobre Enlaces Simbólicos (*Symlinks*) en Windows
* **Incidencia:** Durante el proceso de descarga con `datasets`, Hugging Face mostró una advertencia de *symlinks* no soportados (`UserWarning: huggingface_hub cache-system uses symlinks...`).
* **Causa:** La configuración por defecto del sistema de archivos de Windows restringe la creación de enlaces simbólicos si no se ejecuta en Modo Desarrollador o con privilegios de Administrador.
* **Solución aplicada:** Se ignoró la advertencia, ya que el sistema realiza un respaldo automático a copia completa dentro del caché (`.cache/huggingface/hub`), permitiendo que la exportación final a `data/raw/dataset.csv` se generara sin pérdidas ni corrupción de datos.