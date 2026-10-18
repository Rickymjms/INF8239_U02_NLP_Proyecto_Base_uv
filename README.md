# INF-8239 · Unidad 02 · Proyecto NLP

Autor académico: Edwin Ramón José Nolasco
Replicado y realizado por: Manuel Mañana Santana

Proyecto base para LAB04–LAB06. No sustituya la comprensión por ejecución mecánica.

## Inicio rápido

```bash
uv python install 3.12
uv sync
uv run pytest -q
uv run python scripts/audit_data.py
```

Copie `.env.example` como `.env` y configure el dataset aprobado.

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

## Dataset

Complete `docs/DATASET_CARD.md`, el diccionario de datos (`docs/dataset_candidates.csv`), la licencia y el procedimiento de obtención.
El archivo `data/sample/demo_text.csv` solamente comprueba la arquitectura.

## Entrenamiento

```bash
uv run python scripts/train_text.py
uv run streamlit run app/streamlit_app.py
```

## Interpretación

Toda conclusión debe separar observación, evidencia, interpretación y decisión.