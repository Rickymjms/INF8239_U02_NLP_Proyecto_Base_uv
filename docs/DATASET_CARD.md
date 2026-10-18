# Dataset Card

## Identificación
- **Nombre:** Amazon Reviews Multi ES (SetFit)
- **Fuente original:** `SetFit/amazon_reviews_multi_es` (Hugging Face Datasets)
- **Responsable:** Amazon (recolección original) / SetFit Team & Hugging Face (curaduría) / Estudiante de INF8239 (descarga, transformación y auditoría local)
- **Versión o fecha:** Octubre 2026 / Split `train` (versión cruda descargada y exportada a CSV)
- **Licencia:** Creative Commons Attribution-ShareAlike 4.0 International (CC BY-SA 4.0)
- **Idioma:** Español (`es`)

## Propósito y variable objetivo
- **Uso previsto / Propósito:** Construcción y evaluación reproducible de pipelines de Clasificación de Texto y Análisis de Sentimientos en español para el proyecto INF8239 (LAB04).
- **Variable objetivo:** La columna `label` es la variable objetivo. Representa una calificación ordinal discreta de 5 niveles (de 0 a 4) equivalente a la valoración en estrellas del producto.

## Diccionario de datos

| Columna | Tipo | Descripción / Significado | Valores o unidad / Categorías válidas |
|---|---|---|---|
| `text` | `string` | Cuerpo de la reseña escrita por el usuario. | Cadena de texto plano en español (longitud: mín 16, p50 120, máx 3,086 caracteres). |
| `label` | `int64` | Sentimiento o estrellas asignadas al producto. | Enteros $\{0, 1, 2, 3, 4\}$ correspondientes a 1, 2, 3, 4 y 5 estrellas.

## Procedimiento de obtención y transformaciones
- **Obtención:** Extracción automatizada del *split* `train` de Hugging Face usando la librería `datasets` en un entorno virtual aislado con `uv` (Python 3.12).
- **Transformaciones realizadas:**
  1. Conversión de la estructura original `DatasetDict` de Hugging Face a un objeto `pandas.DataFrame`.
  2. Exportación e independización local a formato CSV en la ruta `data/raw/dataset.csv` para garantizar reproducibilidad sin dependencia de la red.
  3. Verificación de inmutabilidad mediante el cálculo del Hash SHA-256 (`45896e29a23e374cdb46962ab3fd1a3dd4101f7448ef9179832e13433392d3e0`).

## Ejemplos anonimizados

| # | Texto de la reseña (`text`) | Etiqueta (`label`) | Categoría equivalente |
|---|---|---|---|
| 1 | *"El producto llegó rápido y bien empaquetado. Funciona perfectamente."* | `4` | 5 estrellas (Excelente) |
| 2 | *"Es una buena compra por el precio ofertado, sin embargo tarda en cargar."* | `3` | 4 estrellas (Bueno) |
| 3 | *"Cumple con su función aunque los materiales se sienten algo sencillos."* | `2` | 3 estrellas (Regular / Neutral) |
| 4 | *"El empaque llegó abierto y faltaba un accesorio."* | `1` | 2 estrellas (Malo) |
| 5 | *"Mala calidad, se rompió al segundo día de uso. No lo recomiendo."* | `0` | 1 estrella (Muy malo) |

## Calidad observada
- **Volumen total:** 200,000 filas y 4 columnas.
- **Valores nulos:** 0 nulos en las columnas clave `text` y `label`.
- **Duplicados:** 1,736 registros con texto idéntico (~0.87% del total).
- **Distribución de clases:** Balance perfecto (20.0% por cada una de las 5 categorías: 40,000 observaciones por clase).
- **Estadísticas de longitud (caracteres):** Promedio 151.3, mediana (p50) 120, p90 295, p99 665 y máximo 3,086.

## Población cubierta y excluida
- **Población cubierta:** Consumidores de la plataforma e-commerce de Amazon que redactaron opiniones en español.
- **Población excluida:** Usuarios no digitalizados, comentarios en redes sociales con dialectos locales no estandarizados o modismos caribeños/dominicanos, y dominios especializados (médico, legal, financiero).

## Riesgos, sesgos y usos prohibidos
- **Riesgos:** Posible fuga de información (*data leakage*) si los 1,736 textos duplicados terminan repartidos entre entrenamiento y evaluación.
- **Sesgos previsibles:** Sesgo hacia vocabulario técnico de e-commerce (envíos, empaque, atención al cliente) que limita la generalización hacia otros dominios.
- **Usos prohibidos:** Generación de reseñas sintéticas falsas, manipulación de reputación comercial de terceros o toma de decisiones individuales sobre personas sin supervisión humana.