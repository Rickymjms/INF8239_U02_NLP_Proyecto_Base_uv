## Analisis de errores

### Metodología de Inspección
Se abrió y examinó el archivo de predicciones erróneas `reports/error_analysis.csv`[cite: 4]. Para comprender las limitaciones intrínsecas del modelo lineal (`LogisticRegression` con representación vectorial `TF-IDF`), se extrajo y clasificó manualmente una muestra representativa de **50 errores**, evaluando el texto de la reseña, la etiqueta real (*ground truth*) y la predicción del clasificador[cite: 4, 8].

---

### Clasificación Muestral de Errores ($N = 50$)

| # | Texto de la Reseña | Real | Pred. | Categoría de Error |
| :-: | :--- | :-: | :-: | :--- |
| **1** | *"Es muy duro comparándolo con el original"* | **0** | **4** | **Ambigüedad** |
| **2** | *"Me encanta su color, pero queda pequeño y lo he visto bastante más barato."* | **0** | **4** | **Ambigüedad** |
| **3** | *"Es la segunda que compro, la primera vino perfecta... Hoy me llego la nueva toda rayada y partes de la madera rota..."* | **0** | **4** | **Ambigüedad** |
| **4** | *"Me encanta, es bonito, práctico... pero tras 3 meses de uso ha dejado de salir jabón..."* | **0** | **4** | **Ambigüedad** |
| **5** | *"Me compré unos hace poco... El sonido es muy bueno... Actualización. Pasado poco más de un mes, empieza a tener problemas de carga... Por todo ello, he tramitado la devolución..."* | **0** | **4** | **Ambigüedad** |
| **6** | *"Poco más se puede añadir. Si lo has visto alguna vez, ya sabes de qué se trata. No engaña ni promete nada que no pueda cumplir."* | **4** | **0** | **Ambigüedad** |
| **7** | *"Cuando compres un cable para un dispositivo de Apple... Éste cable lo está. Precio inigualable, garantía Amazon y certificado para su uso."* | **4** | **0** | **Ambigüedad** |
| **8** | *"se equivocaron mandando otro sabor y lejos de reclamar su devolucion me abonaron su valor... bravo amazon"* | **4** | **0** | **Ambigüedad** |
| **9** | *"Pica Deferencia en los tamaños y la distinta dureza."* | **0** | **4** | **Dialecto** |
| **10** | *"No es de muy buena calidad"* | **0** | **4** | **Dialecto** |
| **11** | *"Me va genial, no se mueve para nada la secadora"* | **0** | **4** | **Dialecto** |
| **12** | *"Dejado en el buzón. También la fecha de entrega cambia cuando estoy trabajando... Sois los mejores!"* | **0** | **4** | **Ironía** |
| **13** | *"Todo fue enviado de forma correcta. Satisface la necesidad del cliente garantizando el servicio..."* | **4** | **0** | **Mal etiquetado por el modelo** |
| **14** | *"No hace ningun efecto... los mosquitos en mi caso siguen volando tranquilamente sin problema."* | **0** | **4** | **Mal etiquetado por el modelo** |
| **15** | *"Comprarlas fue un error. Son de una silicona blanducha."* | **0** | **4** | **Mal etiquetado por el modelo** |
| **16** | *"Las peores esponjas que he probado"* | **0** | **4** | **Mal etiquetado por el modelo** |
| **17** | *"Son muy buenos, llevo años usándolos"* | **0** | **4** | **Etiqueta discutible** |
| **18** | *"em... me a gustado por que era muy divertido"* | **0** | **4** | **Etiqueta discutible** |
| **19** | *"Me ha gustado mucho y la talla es exacta y muy original"* | **0** | **4** | **Etiqueta discutible** |
| **20** | *"Llevo unos meses con la pieza puesta y ha vuelto a fallar. Que tiempo tiene de garantía esta pieza?"* | **0** | **4** | **Tema fuera del dominio** |
| **21** | *"Es un ventilador de mesita de noche... Que le hace suponer al vendedor que me tiene que regalar unos pendientes?"* | **0** | **4** | **Tema fuera del dominio** |
| **22** | *"-Del producto nada que decir... -Opciones de envío y en concreto horario de entrega pésimo..."* | **0** | **4** | **Tema fuera del dominio** |
| **23** | *"Tengo un problema con la manta ya compré hace 2 meses y ya no calienta como antes. qué solución podríais darme."* | **0** | **4** | **Tema fuera del dominio** |
| **24** | *"Me preguntan si me tengo buena relación con los vecinos?"* | **0** | **4** | **Tema fuera del dominio** |
| **25** | *"...Pero llevo esperando que me solucionen una incidencia desde enero... sres de Amazon, todo el mundo mira para otro lado..."* | **0** | **4** | **Tema fuera del dominio** |
| **26** | *"Como si fueran originales"* | **4** | **0** | **Texto insuficiente** |
| **27** | *"Llevo con el meses y como el primer dia"* | **4** | **0** | **Texto insuficiente** |
| **28** | *"se nota que es la propia marca quien se encarga y no revendedores"* | **4** | **0** | **Texto insuficiente** |
| **29** | *"Xiaomi en estado puro"* | **4** | **0** | **Texto insuficiente** |
| **30** | *"A los pocos días no lo reconoce tu iPhone"* | **4** | **0** | **Texto insuficiente** |
| **31** | *"Me los puse en casa e hice movimientos... La terminación del hierro roza en el dedo."* | **0** | **4** | **Texto insuficiente** |
| **32** | *"Duro 24 horas en funcionamiento el aire lo hace fresquito"* | **0** | **4** | **Texto insuficiente** |
| **33** | *"Soy un 40 y pedi un 41 y me quedaron muy justas"* | **0** | **4** | **Texto insuficiente** |
| **34** | *"Recipiente práctico y de una calidad normal. Lo que no me acaba de convencer es que el fondo..."* | **2** | **3** | **Ambigüedad** |
| **35** | *"Buena pero no la veo cosa de otro mundo. En realidad me esperaba algo más suave..."* | **3** | **2** | **Frontera entre clases adyacentes** |
| **36** | *"Pros: -Diseño atractivo y compacto... Contras: -No lee Mp3. -El cd hace saltos..."* | **2** | **1** | **Ambigüedad** |
| **37** | *"Me gustó mucho sus funciones y colores. Pero con respecto a la calidad es bastante mala..."* | **2** | **1** | **Ambigüedad** |
| **38** | *"El producto funciona a ratos puede esta 30 minutos funcionando y de repente dejar de sonar..."* | **1** | **0** | **Frontera entre clases adyacentes** |
| **39** | *"No me ha resultado buena compra, probablemente porque es complicado dar con la talla exacta."* | **0** | **2** | **Negación compleja** |
| **40** | *"al principio estabamos encantados con ellos eran muy bonitos... pero nos dejo de funcionar..."* | **2** | **1** | **Ambigüedad** |
| **41** | *"No.me gustó. Nada naturales"* | **1** | **0** | **Errores de tokenización / Puntuación** |
| **42** | *"no viene manual de instrucción en español y muy dificil la instalación!"* | **2** | **1** | **Queja de aspecto secundario** |
| **43** | *"Buen Jamón en general, ha llegado rápido y el jamón calidad-precio es correcto"* | **4** | **3** | **Frontera entre clases adyacentes** |
| **44** | *"Me dieron problemas con el envío, y tras esperar un mes y medio... decidí cancelar el envío"* | **0** | **2** | **Tema fuera del dominio** |
| **45** | *"Cómo decoración.... monísima. En cuanto sales de ciudad no tienes recepción de emisoras ninguna!"* | **0** | **2** | **Ironía** |
| **46** | *"Me llegó con la caja protectora rota, mientras q la caja de cartón estaba intacta..."* | **2** | **0** | **Queja de aspecto secundario** |
| **47** | *"Llegó bien de tiempo, la tela es de muy buena calidad. A la hora de montarla..."* | **0** | **1** | **Ambigüedad** |
| **48** | *"todo perfecto en tiempo y forma"* | **3** | **4** | **Etiqueta discutible** |
| **49** | *"Los cortos están fenomenal, pero hubiese quitado los comentarios del director..."* | **2** | **3** | **Ambigüedad** |
| **50** | *"No puedo valorarlo ya que no lo he utilizado, lo compré por si lo pudiera necesitar"* | **2** | **0** | **Texto insuficiente** |

---

### Distribución de Frecuencias de Errores

| Categoría de Error | Frecuencia ($n$) | Porcentaje (%) | Causa Técnica Principal |
| :--- | :-: | :-: | :--- |
| **1. Ambigüedad / Opinión Mixta** | 14 | **28.0%** | La bolsa de palabras no pondera la cláusula final de quiebre o actualización[cite: 8]. |
| **2. Texto insuficiente** | 9 | **18.0%** | Frases breves carentes de adjetivos de alta carga TF-IDF[cite: 8]. |
| **3. Tema fuera del dominio** | 7 | **14.0%** | Reclamos orientados a envíos o paquetería de Amazon y no al producto[cite: 8]. |
| **4. Mal etiquetado por el modelo** | 4 | **8.0%** | Fallo del vectorizador en detectar adjetivos negativos directos[cite: 8]. |
| **5. Etiqueta discutible** | 4 | **8.0%** | Ruido en la anotación original del dataset (*ground truth*)[cite: 4, 8]. |
| **6. Dialecto / Expresiones regionales** | 3 | **6.0%** | Modismos o coloquialismos no representados en el vocabulario[cite: 8]. |
| **7. Frontera entre clases adyacentes** | 3 | **6.0%** | Mínima diferencia semántica entre valoraciones contiguas (ej. 2 vs 3 estrellas)[cite: 4]. |
| **8. Ironía / Sarcasmo** | 2 | **4.0%** | Interpretación literal de términos positivos usados sarcásticamente[cite: 8]. |
| **9. Queja de aspecto secundario** | 2 | **4.0%** | Inconvenientes menores de empaque o manuales que no afectan la función principal[cite: 8]. |
| **10. Negación compleja** | 1 | **2.0%** | Negaciones de largo alcance que alteran el sentido del adjetivo[cite: 8]. |
| **11. Errores de tokenización** | 1 | **2.0%** | Signos de puntuación pegados a palabras (ej. *"No.me"*) que alteran el token[cite: 8]. |
| **Total Auditado** | **50** | **100.0%** | Muestra representativa de errores del modelo `LogisticRegression`[cite: 4]. |

---

### Interpretación Cualitativa e Hipótesis Verificable

#### **Análisis del Comportamiento del Modelo:**
El análisis cualitativo evidencia que el **46.0% de las equivocaciones** pertenecen a las categorías de **Ambigüedad** ($28.0\%$) y **Texto insuficiente** ($18.0\%$)[cite: 8]. Esto demuestra la limitación fundamental de las representaciones numéricas no secuenciales como TF-IDF: al tratar el texto como una bolsa plana de palabras unigramas y bigramas, el modelo lineal es incapaz de evaluar el orden temporal de las ideas (por ejemplo, reseñas que inician con elogios pero concluyen reportando una avería o devolución)[cite: 8].

Por otro lado, un **22.0% de los errores** provienen de **Temas fuera del dominio** ($14.0\%$) y **Etiquetas discutibles** ($8.0\%$), lo que señala que una porción significativa del margen de error es ajena al algoritmo y responde al ruido inherente de las opiniones escritas en entornos web no moderados[cite: 4, 8].

#### **Hipótesis Verificable para el Siguiente Experimento:**
> **Hipótesis:** *"Dado que el 46% de los errores se concentran en ambigüedades contextuales y falta de densidad léxica, la sustitución de TF-IDF por una representación vectorial densa basada en Embeddings Contextuales (Word2Vec / Transformers como BETO) o redes recurrentes/atención reducirá en al menos un 15% las confusiones en clases intermedias y reseñas con giros de discurso, al considerar la secuencialidad y el contexto completo de la oración."*[cite: 4, 8]