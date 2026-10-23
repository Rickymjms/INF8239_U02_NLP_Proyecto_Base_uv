# Paso 4: Cambiar un parámetro con una hipótesis

## 1. Registro de Resultados Comparativos (`window=5` vs `window=2`)

Al modificar únicamente el tamaño de la ventana de contexto de `window=5` a `window=2` en el script `scripts/embeddings_network.py` manteniendo la misma semilla (`seed=42`) y el término objetivo (`servicio`), se obtuvieron las siguientes métricas y listas de vecinos más cercanos:

| Métrica / Parámetro | Línea Base (`window=5`) | Experimento (`window=2`) | Variación Observada |
| :--- | :-: | :-: | :--- |
| **Tamaño de Vocabulario** | 55,793 | 55,793 | Sin cambio (`min_count=1`) |
| **Cobertura del Corpus** | 1.0 (100%) | 1.0 (100%) | Sin cambio |
| **Vecino 1** | `trato` (0.8432) | `trato` (0.7732) | Disminución de 0.0700 |
| **Vecino 2** | `distribuidor` (0.7066) | `distribuidor` (0.7380) | Incremento de 0.0314 |
| **Vecino 3** | `proveedor` (0.6995) | `proveedor` (0.7250) | Incremento de 0.0255 |
| **Vecino 4** | `reparto` (0.6970) | `reparto` (0.6929) | Variación mínima (-0.0041) |
| **Vecino 5** | `vendedor` (0.6756) | `cliente` (0.6879) | Reordenamiento de rango |
| **Vecino 6** | `cliente` (0.6719) | `producto` (0.6874) | Reordenamiento de rango |
| **Vecino 7** | `producto` (0.6629) | `vendedor` (0.6852) | Reordenamiento de rango |
| **Vecino 8** | `servio` (0.6574) | **`envío`** (0.6602) | **Término nuevo en top 10** |
| **Vecino 9** | `artículo` (0.6482) | **`transporte`** (0.6591) | **Término nuevo en top 10** |
| **Vecino 10** | `feedback` (0.6226) | `artículo` (0.6580) | Exclusión de `feedback` |

---

## 2. Salida Raw de Consola (`window=2`)

```text
Vocabulario: 55793 Cobertura: 1.0
Vecinos: [('trato', 0.7731908559799194), 
          ('distribuidor', 0.7379704713821411), 
          ('proveedor', 0.7250124216079712), 
          ('reparto', 0.6929163336753845), 
          ('cliente', 0.6879100799560547), 
          ('producto', 0.6874405145645142), 
          ('vendedor', 0.6852468848228455), 
          ('envío', 0.660215437412262), 
          ('transporte', 0.6590898633003235), 
          ('artículo', 0.6579985618591309)]
```

---

## 3. Explicación Lingüística y Metodológica del Cambio de Contexto

El ajuste de la ventana modifica la granularidad de las relaciones coocurrentes aprendidas por el modelo Word2Vec:

1. **Ventana amplia (`window=5`): Captura de dominialidad y temática global**  
   Al examinar 5 palabras a la izquierda y 5 a la derecha, el algoritmo aprende asociaciones a nivel de párrafo u oración completa. Por ello, aparecen términos de evaluación de opinión global como `feedback` o errores de tecleo lejanos como `servio`.

2. **Ventana estrecha (`window=2`): Captura de relaciones sintácticas e inmediatas**  
   Al restringir el radio de búsqueda a solo 2 palabras contiguas, el modelo prioriza colocaciones funcionales e inmediatas del sustantivo. Esto explica el ingreso de los términos **`envío`** y **`transporte`**, los cuales acompañan directamente a la palabra "servicio" en construcciones frecuentes como *"servicio de envío"* o *"servicio de transporte"*.

3. **Estabilidad del Vocabulario y Cobertura:**  
   Dado que el parámetro `min_count=1` se mantuvo constante, el tamaño del vocabulario (55,793 tokens) y la cobertura (1.0) permanecieron idénticos, demostrando que el cambio responde exclusivamente a la geometría del espacio latente de representación y no a la pérdida de tokens.

---

## 4. Documentación del Valor Seleccionado

* **Valor seleccionado para la versión final:** `window=5`.
* **Justificación:** Para tareas de minería de opiniones y análisis de sentimiento en e-commerce, una ventana de tamaño 5 permite agrupar tópicos y asociaciones semánticas más amplias de la experiencia del usuario, mientras que `window=2` se limita en exceso a colocaciones sintácticas inmediatas.