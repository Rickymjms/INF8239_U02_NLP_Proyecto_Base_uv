# INF-8239 — Ciencia de Datos II: Proyecto Base de PLN

**Asignatura:** INF-8239 - Ciencia de Datos II  
**Unidad 02:** Procesamiento de Lenguaje Natural, redes y visión  
**Autor / Estudiante:** Manuel De Jesus Mañana Santana  
**Universidad:** Universidad Autónoma de Santo Domingo (UASD)  
**Gestor de entorno:** `uv` (Python 3.12)  
**Corpus Seleccionado:** Amazon Reviews Multi ES (`SetFit/amazon_reviews_multi_es`)  

---

## Inicio Rápido

```bash
uv python install 3.12
uv sync
uv run pytest -q
uv run python scripts/audit_data.py
uv run python scripts/train_text.py
uv run streamlit run app/streamlit_app.py
```

Copie `.env.example` como `.env` y configure el dataset aprobado. Datos de configuracion en DATASET_CARD.md

---

##  Descripción del Proyecto

Este repositorio constituye la base reproducible para la Unidad 02 de PLN. Implementa un pipeline completo para la clasificación automatizada de opiniones en español sobre un corpus masivo de 200,000 registros:

1. **LAB04 — Auditoría y Contratos de Datos:** Evaluación de candidatos, descarga local, verificación de integridad con SHA-256 y pruebas unitarias de contratos con `pytest`.
2. **LAB05 — Clasificación de Texto y Diagnóstico:** Entrenamiento comparativo de clasificadores (Dummy, Naive Bayes y Regresión Logística con TF-IDF), evaluación multiclase (F1-macro), análisis de matriz de confusión, auditoría cualitativa de errores y despliegue interactivo con Streamlit.

---

##  Evaluación y Selección del Corpus Público (Paso 4)

### Enlaces Oficiales de los Candidatos
- **Candidato 1 (Aprobado):** [SetFit/amazon_reviews_multi_es (Hugging Face Datasets)](https://huggingface.co/datasets/SetFit/amazon_reviews_multi_es)
- **Candidato 2 (Descartado):** [SMS Spam Collection (UCI Machine Learning Repository)](https://archive.ics.uci.edu/dataset/228/sms+spam+collection)

### Matriz de Evaluación según Criterios de Aprobación

| Criterio / Pregunta de aprobación | Candidato 1 (Aprobado): Amazon Reviews Multi (es) | Candidato 2 (Descartado): SMS Spam Collection (UCI) |
| :--- | :--- | :--- |
| **1. Pertinencia**<br>*¿Los textos y etiquetas permiten responder la pregunta propuesta?* | **Sí.** Contiene textos de reseñas de productos en español (`text` / `label_text`) con etiquetas explícitas de calificación en estrellas (1 a 5), idóneas para clasificación multiclase. | **Parcial.** Textos breves de mensajes SMS con etiquetas binarias (`ham`/`spam`). No responde al análisis de sentimiento en opiniones de e-commerce. |
| **2. Licencia**<br>*¿Se permite descargar, analizar y publicar resultados derivados?* | **Sí.** Publicado bajo licencia abierta permisiva **CC-BY-4.0**. Permite libre descarga, procesamiento local, modificación y publicación. | **Sí.** Licencia pública para investigación académica atribuida al repositorio de la Universidad de California en Irvine (UCI). |
| **3. Representatividad**<br>*¿Qué población, idioma, periodo o grupos quedan fuera?* | **Idioma:** Español. Refleja sintaxis escrita de usuarios en comercio electrónico. *Excluye:* Lenguaje oral no estandarizado y dominios no comerciales. | **Idioma:** Inglés. *Excluye:* Toda la población hispanohablante. Además, está sesgado a patrones de mensajería SMS informal de los años 2000. |
| **4. Calidad**<br>*¿Hay nulos, duplicados, etiquetas ambiguas o clases extremadamente pequeñas?* | **Alta.** Clases balanceadas (20% por categoría), sin nulos en el cuerpo de la reseña y con baja frecuencia de duplicados (~0.87%). | **Regular.** Presenta un acentuado desbalance de clases (~87% mensajes legítimos vs ~13% spam) e informalidades léxicas. |
| **5. Reproducibilidad**<br>*¿Otra persona puede obtener la misma versión?* | **Sí.** Accesible mediante la API de Hugging Face Datasets (`SetFit/amazon_reviews_multi_es`) o enlace público directo. | **Sí.** Disponible de forma estática y permanente en el portal oficial de UCI. |
| **6. Riesgo**<br>*¿Contiene información personal, sensible o potencialmente dañina?* | **Bajo.** Comentarios de acceso público previamente anonimizados en la plataforma Amazon. No contiene datos personales identificables (PII). | **Bajo.** Corpus anonimizado de origen para fines de investigación académica. |
| **Decisión Final** | **APROBADO / SELECCIONADO** | **DESCARTADO** |

### Justificación Metodológica
- **Aprobación del Candidato 1:** Cumple con la totalidad de requisitos de pertinencia en español, equilibrio de clases para evitar métricas engañosas (como *accuracy* aislado), licencia permisiva CC-BY-4.0 y fácil integración en contratos de datos.
- **Descarte del Candidato 2:** Se rechaza por estar restringido al idioma inglés, circunscribirse únicamente a mensajes SMS cortos y presentar un marcado desbalance de clases.

---

##  Estructura del Repositorio

```text
INF8239_U02_NLP_Proyecto_Base_uv/
├── .env.example              # Plantilla de variables de entorno
├── .gitignore                # Exclusiones de Git (.venv, cache, datos pesados)
├── pyproject.toml            # Configuración del proyecto y dependencias en uv
├── uv.lock                   # Bloqueo exacto de versiones de paquetes
├── requirements-colab.txt    # Dependencias adaptadas para Google Colab
├── requirements-cloud.txt    # Dependencias para despliegue en la nube
├── README.md                 # Documentación principal del repositorio
├── app/
│   └── streamlit_app.py      # Aplicación web interactiva para inferencia local/nube
├── data/
│   ├── raw/                  # Datos crudos (data/raw/dataset.csv - excluido de Git)
│   ├── sample/               # Datos de demostración (demo_text.csv)
│   └── processed/            # Datos limpios y procesados
├── docs/
│   ├── DATASET_CARD.md       # Ficha técnica y diccionario de datos del corpus
│   └── Analisis_Errores.md   # Auditoría cualitativa detallada de 50 errores
├── models/
│   └── text_model.joblib     # Artefacto del modelo ganador serializado (LogisticRegression)
├── reports/
│   ├── text_metrics.csv      # Reporte detallado de métricas por modelo y clase
│   ├── confusion_text.png    # Matriz de confusión visual
│   └── error_analysis.csv    # Muestra exportada de predicciones incorrectas
├── scripts/
│   ├── download_data.py      # Script de descarga automatizada
│   ├── audit_data.py         # Script de auditoría (SHA-256, nulos, distribución)
│   └── train_text.py         # Pipeline de entrenamiento y evaluación comparativa
├── src/
│   └── inf8239_u02/          # Módulos y lógica reutilizable
└── tests/
    ├── test_data_contract.py # Pruebas unitarias de contratos de datos
    └── test_text_model.py    # Pruebas unitarias del pipeline e inferencia del modelo
```

---

##  Obtención y Auditoría del Dataset

### 1. Descarga e Independización Local
Para obtener el corpus masivo `SetFit/amazon_reviews_multi_es` (200,000 filas) y exportarlo a `data/raw/dataset.csv`:

```powershell
uv run python -c "from datasets import load_dataset; ds = load_dataset('SetFit/amazon_reviews_multi_es', split='train'); df = ds.to_pandas(); df.to_csv('data/raw/dataset.csv', index=False); print('Dataset guardado exitosamente con', len(df), 'filas')"
```

### 2. Ejecutar la Auditoría
```powershell
uv run python scripts/download_data.py
uv run python scripts/audit_data.py
```

**Métricas de Calidad Auditadas:**
- **Hash SHA-256:** `45896e29a23e374cdb46962ab3fd1a3dd4101f7448ef9179832e13433392d3e0`
- **Registros totales:** 200,000 filas y 4 columnas.
- **Valores nulos:** 0 en `text` y `label`.
- **Duplicados:** 1,736 registros (~0.87%).
- **Balanceo de clases:** 20.0% por cada categoría (`0`, `1`, `2`, `3` y `4`).

---

##  Validación de Contratos de Datos

Ejecuta las pruebas unitarias automatizadas con `pytest` para verificar la conformidad sintáctica del dataset:

```powershell
uv run pytest -q
```

---

##  Portabilidad Local / Nube (Google Colab)

1. **Sin datos pesados en Git:** La carpeta `data/raw/` y la carpeta `.venv/` están correctamente ignoradas en `.gitignore`.
2. **Ejecución en Nube:** Para correr este proyecto en Google Colab o entornos sin `uv`, instala las dependencias utilizando el archivo exportado:
   ```bash
   pip install -r requirements-colab.txt
   ```
3. **Documentación:** Revisa la ficha técnica completa y el diccionario de variables en [`docs/DATASET_CARD.md`](docs/DATASET_CARD.md).

---

##  Clasificación de Texto y Métricas (LAB05)

### 1. Desempeño Comparativo de Modelos
Se evaluaron tres clasificadores sobre una partición estratificada fija (`random_state=42`) encapsulando `TfidfVectorizer` dentro del pipeline para evitar la fuga de información (*data leakage*):

| Modelo / Pipeline | Precisión (macro) | Recall (macro) | F1-score (macro) | Accuracy |
| :--- | :-: | :-: | :-: | :-: |
| **DummyClassifier (Baseline)** | 0.040 | 0.200 | 0.067 | 0.200 |
| **Complement Naive Bayes** | 0.490 | 0.500 | 0.485 | 0.500 |
| **Logistic Regression (Ganador)** | **0.530** | **0.530** | **0.529** | **0.530** |

*La Regresión Logística superó al baseline por +0.462 en F1-macro y a Naive Bayes por +0.044.*

---

### 2. Inspección de la Matriz de Confusión (`reports/confusion_text.png`)

- **Aciertos en la diagonal principal:** Clase 0 ($6,908$), Clase 1 ($4,551$), Clase 2 ($4,021$), Clase 3 ($4,074$), Clase 4 ($6,875$).
- **Clase con menor Recall:** La **Clase 2 (3 estrellas / Neutro)** obtuvo el recall más bajo (**0.404 / 40.4%**) con **5,924 falsos negativos**, confundida principalmente con las clases adyacentes 1 ($2,621$ casos) y 3 ($1,825$ casos).
- **Mayor confusión fuera de la diagonal:** Clase Real 3 predicha como Clase 4 (**2,994 casos**).
- **Costo de Negocio:** Confundir opiniones de 4 estrellas con 5 estrellas oculta críticas menores sobre los productos, sobreestima los índices de satisfacción (NPS/CSAT) e impide resolver insatisfacciones moderadas antes de que escalen a valoraciones negativas.

---

##  Resumen del Análisis de Errores

Se realizó una auditoría cualitativa sobre una muestra de **50 predicciones incorrectas** extraídas de `reports/error_analysis.csv`.

### Distribución de Categorías de Error

| Categoría de Error | Porcentaje (%) | Factor Técnico / Causa Lingüística |
| :--- | :-: | :--- |
| **1. Ambigüedad / Opinión Mixta** | **28.0%** | La bolsa de palabras no capta el orden secuencial ni el giro final del discurso. |
| **2. Texto insuficiente** | **18.0%** | Reseñas breves sin términos de alta carga explícita en TF-IDF. |
| **3. Tema fuera del dominio** | **14.0%** | Reclamaciones sobre logística de envíos o empaques de Amazon. |
| **4. Mal etiquetado por el modelo** | **8.0%** | Omisión de palabras clave explícitas de baja frecuencia. |
| **5. Etiqueta discutible (Ruido)** | **8.0%** | Incoherencias en la nota asignada por el comprador en el dataset original. |
| **6. Dialecto / Expresiones regionales** | **6.0%** | Modismos y coloquialismos no contemplados en el vocabulario. |
| **7. Frontera entre clases adyacentes** | **6.0%** | Zona gris semántica entre calificaciones contiguas (ej. 2 vs 3 estrellas). |
| **8. Ironía / Sarcasmo** | **4.0%** | Interpretación literal de expresiones sarcásticas. |
| **9. Queja de aspecto secundario** | **4.0%** | Quejas de manuales o empaques que no comprometen el producto. |
| **10. Negación compleja / Tokenización** | **4.0%** | Alcance de negaciones y errores de signos pegados a palabras. |

>  **Documento Completo:** Para examinar el desglose individual de los 50 casos etiquetados con sus respectivas justificaciones lingüísticas, consulte [`docs/Analisis_Errores.md`](docs/Analisis_Errores.md).

---

##  Validación de Contratos y Pruebas Unitarias

Ejecute la suite de pruebas unitarias con `pytest` para certificar los contratos de datos y la funcionalidad del modelo serializado:

```bash
uv run pytest -q
```

**Verificación de Inferencia del Modelo Guardado (`joblib`):**
```powershell
uv run python -c "import joblib; m=joblib.load('models/text_model.joblib'); print(m.predict(['Excelente servicio']))"
# Salida esperada: [4] (5 estrellas)
```

---

##  Aplicación Local Interactiva (Streamlit)

Inicie la interfaz gráfica interactiva para evaluar el modelo en tiempo real con frases personalizadas:

```powershell
uv run streamlit run app/streamlit_app.py
```

---

##  Cierre Interpretativo

### **Resultado Principal:**
El pipeline de **Regresión Logística + TF-IDF** se consolidó como el mejor modelo clásico para la clasificación de sentiment/estrellas en el corpus `amazon_reviews_multi_es`, alcanzando un **F1-macro de 0.529**. Superó al baseline trivial por $+0.462$ y a Naive Bayes por $+0.044$.

### **Hallazgos Clave:**
1. **Dificultad en Clases Intermedias:** Las opiniones neutras (3 estrellas) presentan el mayor nivel de error ($recall = 40.4\%$), confundiéndose con bordes positivos y negativos.
2. **Limitación de Bolsa de Palabras:** El $46\%$ de los errores analizados se debe a la incapacidad de TF-IDF para interpretar la estructura secuencial del texto, cambios de contexto y la densidad léxica en frases cortas.
3. **Ruido Inevitable en los Datos:** Un $22\%$ de los errores corresponde a temas ajenos al producto (logística de envíos) o etiquetas erróneas colocadas por los usuarios originales (*Etiqueta discutible*).

### **Decisión de Despliegue:**
Se recomienda no desplegar el modelo de forma autónoma en entornos de producción. Se propone utilizarlo como herramienta asistida de triaje con **supervisión humana en clases 1, 2 y 3**, y avanzar hacia experimentos con **Embeddings Contextuales (Transformers / BETO)** para resolver las ambigüedades secuenciales identificadas.

---

##  Portabilidad Local / Nube

Para ejecutar este repositorio en Google Colab, Streamlit Cloud o entornos alternativos sin `uv`:

```bash
pip install -r requirements-cloud.txt
```