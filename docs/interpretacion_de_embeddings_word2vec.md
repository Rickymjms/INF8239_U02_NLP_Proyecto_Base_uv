# Interpretación de Resultados: Entrenamiento de Word2Vec (`--word servicio`)

## 1. Métricas Globales del Modelo

* **Tamaño del Vocabulario:** `55,793` tokens únicos retenidos tras procesar el corpus de 200,000 reseñas. La configuración con `min_count=1` retiene todos los términos con al menos una aparición.
* **Cobertura:** `1.0` (100%). Indica que todos los tokens contenidos en las oraciones procesadas existen dentro del espacio vectorial latente sin omisiones.

---

## 2. Análisis Semántico de los Vecinos Cercanos (Similitud Coseno)

Conforme a la hipótesis distribucional, las palabras que comparten contextos de aparición similares tienden a posicionarse cerca en el espacio vectorial.

| Rango | Término Vecino | Similitud Coseno | Categoría Semántica / Interpretación |
| :-: | :--- | :-: | :--- |
| **1** | `trato` | **0.8432** | **Atributo directo:** Máxima proximidad. Refleja alta coocurrencia en evaluaciones sobre atención al cliente. |
| **2** | `distribuidor` | **0.7066** | **Actor de cadena:** Entidad intermediaria responsable de la ejecución de la entrega o soporte. |
| **3** | `proveedor` | **0.6995** | **Actor de cadena:** Entidad emisora o prestadora del servicio. |
| **4** | `reparto` | **0.6970** | **Proceso logístico:** Dimensión operativa vinculada a la entrega del producto. |
| **5** | `vendedor` | **0.6756** | **Actor comercial:** Sujeto evaluado en paralelo con la calidad del servicio. |
| **6** | `cliente` | **0.6719** | **Receptor:** Sujeto destinatario en la interacción de compra. |
| **7** | `producto` | **0.6629** | **Entidad complementaria:** Refleja el contraste recurrente en e-commerce entre atención recibida y el bien físico. |
| **8** | `servio` | **0.6574** | **Ruido / Errata:** Error de tecleo (*typo*) frecuente en el corpus que comparte idéntica posición sintáctica que "servicio". |
| **9** | `artículo` | **0.6482** | **Sinónimo funcional:** Objeto del servicio en el contexto comercial de la plataforma. |
| **10** | `feedback` | **0.6226** | **Anglicismo / Modismo:** Término usado por los compradores para referirse a la retroalimentación. |

---

## 3. Observaciones e Implicaciones Metodológicas

1. **Estructura del Dominio de E-commerce:**  
   El modelo Word2Vec agrupa coherentemente actores (`vendedor`, `cliente`, `proveedor`), procesos (`reparto`, `feedback`) y atributos (`trato`), demostrando que captura la estructura semántica propia de reseñas comerciales en español.

2. **Captura de Erratas y Ruido Léxico:**  
   La presencia del término `servio` ($0.6574$) evidencia la capacidad del algoritmo para asociar errores ortográficos frecuentes con la forma correcta, debido a la equivalencia de sus contextos inmediatos.

3. **Límites Interpretativos:**  
   * **Similitud no equivale a sinonimia:** Términos como `vendedor` y `cliente` son conceptos opuestos/complementarios en la realidad, pero obtienen alta similitud coseno porque aparecen en posiciones sintácticas y oracionales equivalentes.
   * **Dependencia estricta del corpus:** Las distancias vectoriales corresponden al dominio específico de reseñas de e-commerce y no representan definiciones lingüísticas universales.