# Dataset Card

## Identificación
- **Nombre:** Amazon Reviews Multi ES (SetFit)
- **Fuente original:** `SetFit/amazon_reviews_multi_es` (Hugging Face Datasets)[cite: 1]
- **Responsable:** Amazon (recopilación original) / SetFit Team & Hugging Face (curaduría) / Estudiante (descarga y auditoría local para INF8239)[cite: 1]
- **Versión o fecha:** Octubre 2026 / Split `train`[cite: 1]
- **Licencia:** Creative Commons Attribution-ShareAlike 4.0 International (CC BY-SA 4.0)[cite: 1]
- **Idioma:** Español (`es`)[cite: 1]

## Propósito y variable objetivo
- **Propósito:** Proveer un conjunto de datos estandarizado y reproducible para el entrenamiento y evaluación de modelos de Clasificación de Texto y Análisis de Sentimientos en español (INF8239 LAB04)[cite: 1].
- **Variable objetivo:** La columna `label` es la variable objetivo[cite: 1]. Representa una calificación ordinal discreta de 5 niveles (de 0 a 4) equivalente a la valoración en estrellas del producto[cite: 1].

## Diccionario de datos

| Columna | Tipo | Descripción | Valores o unidad |
|---|---|---|---|
| `text` | `string` | Cuerpo de la reseña escrita por el usuario. | Texto plano en español (longitud: min 16, p50 120, max 3,086 caracteres)[cite: 1]. |
| `label` | `int64` | Sentimiento o estrellas asignadas al producto. | Enteros $\{0, 1, 2, 3, 4\}$ correspondientes a 1, 2, 3, 4 y 5 estrellas[cite: 1]. |

## Procedimiento de obtención
- Extracción automatizada del split `train` desde Hugging Face mediante la librería `datasets` en entorno aislado con `uv` (Python 3.12)[cite: 1].
- Exportación e independización local a formato CSV en la ruta `data/raw/dataset.csv`[cite: 1].
- Verificación de la integridad de los bytes mediante el cálculo del Hash SHA-256 (`45896e29a23e374cdb46962ab3fd1a3dd4101f7448ef9179832e13433392d3e0`)[cite: 1].

## Calidad observada
- **Volumen total:** 200,000 filas y 4 columnas[cite: 1].
- **Valores nulos:** 0 nulos en las columnas `text` y `label`[cite: 1].
- **Duplicados de texto:** 1,736 registros idénticos (~0.87% del corpus)[cite: 1].
- **Distribución de clases:** Balance perfecto (20.0% por cada categoría `0`, `1`, `2`, `3` y `4`, con 40,000 registros por clase)[cite: 1].
- **Estadísticas de longitud:** Promedio de 151.3 caracteres, p50 (mediana) de 120, p90 de 295, p99 de 665 y máximo de 3,086 caracteres[cite: 1].

## Población cubierta y excluida
- **Población cubierta:** Consumidores de la plataforma e-commerce de Amazon que redactaron reseñas en español[cite: 1].
- **Población excluida:** Usuarios no digitalizados, textos provenientes de redes sociales con alta carga dialectal local o modismos caribeños/regionales no estandarizados, y dominios especializados (médico, legal, financiero)[cite: 1].

## Riesgos, sesgos y usos prohibidos
- **Riesgos:** Posible fuga de información (*data leakage*) si los 1,736 textos duplicados se reparten entre entrenamiento y evaluación[cite: 1].
- **Sesgos:** Sesgo de vocabulario enfocado en experiencia de compra, envíos, empaque y calidad del producto[cite: 1].
- **Usos prohibidos:** Generación de reseñas falsas, manipulación de reputación comercial o toma de decisiones individuales sobre personas sin supervisión humana[cite: 1].