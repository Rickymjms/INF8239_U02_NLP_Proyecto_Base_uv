# INF-8239 — Ciencia de Datos II: Proyecto Base de PLN

**Asignatura:** INF-8239 - Ciencia de Datos II  
**Unidad 02:** Procesamiento de Lenguaje Natural, redes y visión  
**Autor / Estudiante:** Manuel De Jesus Mañana Santana  
**Universidad:** Universidad Autónoma de Santo Domingo (UASD)  
**Gestor de entorno:** `uv` (Python 3.12)  
**Corpus Seleccionado:** Amazon Reviews Multi ES (`SetFit/amazon_reviews_multi_es`)  

---

## Inicio rápido

```bash
uv python install 3.12
uv sync
uv run pytest -q
uv run python scripts/audit_data.py
```

Copie `.env.example` como `.env` y configure el dataset approved.

---

## 📌 Descripción del Proyecto

Este repositorio constituye la base reproducible para la Unidad 02 de PLN. Implementa la localización, evaluación, aprobación, descarga local, auditoría de calidad y validación automatizada mediante contratos de datos para un corpus masivo de clasificación de texto en español (200,000 registros).

El proyecto está diseñado bajo estándares estrictos de **reproducibilidad, portabilidad local/nube y control de versiones**, garantizando la inmutabilidad de los datos crudos y la ejecución aislada sin dependencia fija del sistema operativo.

---

## Evaluación y Selección del Corpus Público (Paso 4)

### Enlaces Oficiales de los Candidatos
- **Candidato 1 (Aprobado):** [SetFit/amazon_reviews_multi_es (Hugging Face Datasets)](https://huggingface.co/datasets/SetFit/amazon_reviews_multi_es)
- **Candidato 2 (Descartado):** [SMS Spam Collection (UCI Machine Learning Repository)](https://archive.ics.uci.edu/dataset/228/sms+spam+collection)

### Matriz de Evaluación según Criterios y Preguntas de Aprobación

| Criterio / Pregunta de aprobación | Candidato 1 (Aprobado): Amazon Reviews Multi (es) | Candidato 2 (Descartado): SMS Spam Collection (UCI) |
| :--- | :--- | :--- |
| **1. Pertinencia**<br>*¿Los textos y etiquetas permiten responder la pregunta propuesta?* | **Sí.** Contiene textos de reseñas de productos en español (`text` / `label_text`) con etiquetas explícitas de sentimiento o calificación en estrellas (1 a 5), idóneas para tareas complejas de PLN. | **Parcial.** Textos breves de mensajes de texto SMS con etiquetas binarias (`ham`/`spam`). Responde a detección de spam pero no al análisis de sentimiento/opinión. |
| **2. Licencia**<br>*¿Se permite descargar, analizar y publicar resultados derivados?* | **Sí.** Publicado bajo licencia abierta permisiva **CC-BY-4.0**. Permite libre descarga, procesamiento local, modificación y publicación de derivados. | **Sí.** Licencia pública para investigación académica atribuida al repositorio de la Universidad de California en Irvine (UCI). |
| **3. Representatividad**<br>*¿Qué población, idioma, periodo o grupos quedan fuera?* | **Idioma:** Español. Refleja la sintaxis y opinión escrita de usuarios en comercio electrónico. *Excluye:* Lenguaje oral no estandarizado y dominios no comerciales. | **Idioma:** Inglés. *Excluye:* Toda la población hispanohablante. Además, está sesgado a patrones de mensajería SMS informal de los años 2000. |
| **4. Calidad**<br>*¿Hay nulos, duplicados, etiquetas ambiguas o clases extremadamente pequeñas?* | **Alta.** Clases balanceadas (mismo volumen por número de estrellas), sin nulos en el cuerpo de la reseña y con baja frecuencia de duplicados. | **Regular.** Presenta un acentuado desbalance de clases (~87% mensajes legítimos vs ~13% spam) y alto nivel de abreviaturas e informalidades. |
| **5. Reproducibilidad**<br>*¿Otra persona puede obtener la misma versión?* | **Sí.** Accesible mediante la API de Hugging Face Datasets (`SetFit/amazon_reviews_multi_es`) o mediante enlace directo público sin autenticación privada. | **Sí.** Disponible de forma estática y permanente en el portal oficial de UCI Machine Learning Repository. |
| **6. Riesgo**<br>*¿Contiene información personal, sensible o potencialmente dañina?* | **Bajo.** Comentarios de acceso público previamente anonimizados en la plataforma Amazon. No contiene datos personales identificables (PII). | **Bajo.** Corpus anonimizado de origen para fines de investigación académica. |
| **Decisión Final** | **APROBADO / SELECCIONADO** | **DESCARTADO** |

### Justificación Metodológica

- **Aprobación de Candidate 1 (`SetFit/amazon_reviews_multi_es`):** Cumple con todos los requisitos de pertinencia en español, equilibrio de clases para evitar métricas engañosas (como el accuracy aislado), licencia abierta CC-BY-4.0 y fácil integración en los contratos de validación de datos.
- **Descarte de Candidate 2 (`SMS Spam Collection`):** Se rechaza principalmente por estar restringido al idioma inglés, limitar el ámbito de análisis únicamente a mensajes SMS cortos binarios y presentar un marcado desbalance de clases.

---

## 📁 Estructura del Repositorio

```text
INF8239_U02_NLP_Proyecto_Base_uv/
├── .env.example              # Plantilla de variables de entorno
├── .gitignore                # Reglas de exclusión para Git (.venv, cache, datos pesados)
├── pyproject.toml            # Definición del proyecto y dependencias de uv
├── uv.lock                   # Bloqueo de versiones exactas de dependencias
├── requirements-colab.txt    # Exportación de dependencias para Google Colab
├── README.md                 # Documentación principal del proyecto
├── data/
│   ├── raw/                  # Datos crudos descargados (data/raw/dataset.csv - excluido en git)
│   ├── sample/               # Datos de demostración (demo_text.csv)
│   └── processed/            # Datos limpios preparados para modelado
├── docs/
│   └── DATASET_CARD.md       # Ficha técnica y diccionario de datos del corpus
├── scripts/
│   ├── download_data.py      # Script automatizado de descarga
│   └── audit_data.py         # Script de auditoría (Hash SHA-256, nulos, duplicados, distribución)
├── src/
│   └── inf8239_u02/          # Módulos y lógica reutilizable del proyecto
└── tests/
    └── test_data_contract.py # Pruebas unitarias y contratos de datos (pytest)
```

---

## 📊 Obtención y Auditoría del Dataset

### 1. Descarga e Independización Local
Para obtener el corpus masivo `SetFit/amazon_reviews_multi_es` (200,000 filas) y exportarlo a `data/raw/dataset.csv`:

```powershell
uv run python -c "from datasets import load_dataset; ds = load_dataset('SetFit/amazon_reviews_multi_es', split='train'); df = ds.to_pandas(); df.to_csv('data/raw/dataset.csv', index=False); print('Dataset guardado exitosamente con', len(df), 'filas')"
```

### 2. Ejecutar la Auditoría
Valida el volumen de datos, valores nulos, duplicados, distribución de clases y longitud de texto:

```powershell
uv run python scripts/download_data.py
uv run python scripts/audit_data.py
```

**Métricas del Corpus Auditado:**
- **Hash SHA-256:** `45896e29a23e374cdb46962ab3fd1a3dd4101f7448ef9179832e13433392d3e0`
- **Registros totales:** 200,000 filas y 4 columnas.
- **Valores nulos:** 0 en `text` y `label`.
- **Duplicados:** 1,736 registros (~0.87%).
- **Balanceo de clases:** Perfectamente balanceado (20.0% por cada una de las 5 categorías: `0`, `1`, `2`, `3` y `4`).

---

## 🧪 Validación de Contratos de Datos

Ejecuta las pruebas unitarias automatizadas con `pytest` para verificar la conformidad sintáctica del dataset:

```powershell
uv run pytest -q
```

---

## 🌐 Portabilidad Local / Nube (Google Colab)

1. **Sin datos pesados en Git:** La carpeta `data/raw/` y la carpeta `.venv/` están correctamente ignoradas en `.gitignore`.
2. **Ejecución en Nube:** Para correr este proyecto en Google Colab o entornos sin `uv`, instala las dependencias utilizando el archivo exportado:
   ```bash
   pip install -r requirements-colab.txt
   ```
3. **Documentación:** Revisa la ficha técnica completa y el diccionario de variables en [`docs/DATASET_CARD.md`](docs/DATASET_CARD.md).