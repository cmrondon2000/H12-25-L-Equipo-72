# SentimentAI – Análisis Inteligente de Sentimientos

## 1. Descripción General

**SentimentAI – Análisis Inteligente** es un sistema de análisis de sentimientos basado en técnicas de *Procesamiento de Lenguaje Natural (NLP)* y *Machine Learning*, diseñado para clasificar comentarios de clientes en tres categorías:

- **Positivo**
- **Neutro**
- **Negativo**

La solución está orientada a integrarse como una **API de análisis de sentimientos**, permitiendo a empresas procesar grandes volúmenes de texto de forma automática, consistente y escalable.

---

## 2. Objetivo del Sistema

El objetivo principal de SentimentAI es:

- Automatizar el análisis de opiniones y comentarios de clientes  
- Priorizar retroalimentación negativa y neutra  
- Apoyar la toma de decisiones en atención al cliente, marketing y mejora de productos  

---

## 3. Fuentes de Datos

Para el entrenamiento y evaluación del modelo se utilizaron **múltiples datasets**, combinando datos abiertos y datos propios, con el fin de mejorar la robustez y generalización del modelo.

### 3.1 Dataset Multilingüe (Hugging Face)

- Fuente: `tyqiangz/multilingual-sentiments`
- Idioma utilizado: **Español**
- Formato original: `parquet`
- Etiquetas originales:
  - `0`: Negativo
  - `1`: Neutro
  - `2`: Positivo

Estas etiquetas fueron mapeadas a clases semánticas legibles y unificadas al esquema final del proyecto.

---

### 3.2 Dataset Propio – Comentarios Etiquetados

- Fuente: Repositorio GitHub del proyecto
- Contenido:
  - Título del comentario
  - Texto del comentario
  - Etiqueta manual de sentimiento
- Procesamiento:
  - Unión de `titulo + comentario` en una sola variable textual (`texto`)
  - Normalización de etiquetas

---

### 3.3 Dataset Unificado

- Dataset adicional con etiquetas en inglés:
  - `positive`
  - `neutral`
  - `negative`
- Traducción y estandarización de etiquetas al español:
  - `positivo`
  - `neutro`
  - `negativo`

---

### 3.4 Consolidación Final

- Unión de todos los datasets en un único DataFrame
- Eliminación de duplicados por combinación `texto + sentimiento`
- Resultado:
  - Dataset balanceado y multifuente
  - Mayor diversidad semántica y contextual

---

## 4. Preprocesamiento de Texto (NLP)

Antes del entrenamiento, los textos fueron sometidos a un proceso riguroso de limpieza y normalización.

### 4.1 Limpieza Básica

- Conversión a minúsculas
- Eliminación de:
  - Menciones (`@usuario`)
  - URLs
  - Caracteres especiales
- Normalización de acentos

---

### 4.2 Stopwords Personalizadas

Se utilizaron *stopwords* en español (NLTK), conservando palabras clave de negación que aportan significado semántico:

**Ejemplos conservados**:
- no
- ni
- nunca
- sin
- jamás
- pero

---

### 4.3 Lematización

- Modelo utilizado: `spaCy – es_core_news_sm`
- Se aplicó lematización para reducir variaciones morfológicas
- Resultado:
  - Texto más compacto
  - Mejor representación semántica
  - Reducción de ruido

---

## 5. Representación del Texto

Para convertir texto a variables numéricas se utilizó:

### TF-IDF (Term Frequency – Inverse Document Frequency)

- Eliminación de términos poco frecuentes (`min_df`)
- Eliminación de términos demasiado comunes (`max_df`)
- Evaluación de:
  - Unigramas
  - Bigramas

---

## 6. Modelos de Machine Learning Evaluados

Se realizó una comparación exhaustiva entre distintos algoritmos supervisados:

- **Logistic Regression (Multinomial)**
- **Support Vector Machines (SVM)**
- **Random Forest**
- **Gradient Boosting**
- **XGBoost**

### Criterios de selección:
- Precisión global
- Balance entre clases
- Interpretabilidad
- Tiempo de entrenamiento
- Facilidad de despliegue en API

---

## 7. Modelo Final Seleccionado

### Logistic Regression + TF-IDF (Pipeline)

El modelo con mejor desempeño global y estabilidad fue:

- **TF-IDF Vectorizer**
- **Logistic Regression Multinomial**
- Optimización mediante **GridSearchCV**

#### Hiperparámetros optimizados:
- `max_features`
- `ngram_range`
- `C`
- `class_weight`

---

## 8. Evaluación del Modelo Multiclase

### Métricas utilizadas:

- Accuracy
- Precision
- Recall
- F1-score
- Matriz de confusión

El modelo mostró:
- Buen desempeño en clases **Positivo** y **Negativo**
- Mayor complejidad en la clase **Neutro**, inherente a la ambigüedad semántica del lenguaje natural

---

## 9. Modelo Alternativo: Clasificación Binaria

Además del modelo ternario, se desarrolló un **modelo binario**:

- Clases:
  - Positivo
  - Negativo
- Exclusión de la clase Neutro

### Ventajas del modelo binario:
- Mayor precisión
- Menor ambigüedad
- Útil para escenarios de decisión rápida

---

## 10. Análisis de Errores

Se realizó un análisis detallado de los errores del modelo binario:

- Identificación de textos mal clasificados
- Comparación entre:
  - Texto original
  - Texto limpio
  - Etiqueta real
  - Predicción del modelo
- Exportación a archivo CSV para revisión manual

Este análisis permitió:
- Detectar ironía y sarcasmo
- Identificar ambigüedad contextual
- Proponer mejoras futuras (modelos contextuales)

---

## 11. Persistencia del Modelo

Los modelos finales fueron almacenados utilizando `joblib`:

- `modelo_sentimiento_optimizado.pkl`
- `modelo_sentimiento_binario_optimizado.pkl`

Esto permite:
- Carga rápida en producción
- Integración directa en una API REST
- Escalabilidad del sistema

---

## 12. Conclusiones Técnicas Análisis de Datos

- La combinación **TF-IDF + Logistic Regression** ofrece un balance óptimo entre desempeño y simplicidad
- La unión de múltiples datasets mejora significativamente la generalización
- El preprocesamiento lingüístico es crítico para el rendimiento final
- SentimentAI está listo para ser expuesto como un servicio de análisis de sentimientos

## 14. Arquitectura de la Solución (API + ML + IA)

SentimentAI se implementa como una API desplegable en contenedores, integrando un enfoque híbrido de **Machine Learning clásico** e **Inteligencia Artificial generativa**, con el objetivo de maximizar la precisión, la trazabilidad y el valor analítico del sistema.

La arquitectura combina:

- Un modelo de **Machine Learning** (TF-IDF + Logistic Regression) como clasificador base  
- Un módulo de **Inteligencia Artificial Generativa (Cerebras)** como capa de verificación inteligente  
- Un módulo de **analítica estadística y perfilamiento del cliente**  
- Servicios de soporte e integración sobre **infraestructura Oracle**


### 14.1 Componentes principales

1. **API SentimentAI (Backend)**
   - Orquesta el flujo: entrada → traducción → limpieza → inferencia ML → verificación neutros → salida
   - Expone endpoints para análisis individual y por lote (CSV)

2. **Módulo ML (Clasificación base)**
   - Modelo: `modelo_sentimiento_optimizado.pkl` (ternario) y/o `modelo_sentimiento_binario_optimizado.pkl` (binario)
   - Vectorización: TF-IDF dentro de un Pipeline
   - Salidas:
     - Clase predicha (positivo / neutro / negativo)
     - Probabilidades (si se habilitan en el modelo)

3. **IA Generativa – Cerebras (Traducción)**
   - Permite que el sistema reciba comentarios en **cualquier idioma**
   - Estandariza el texto a **español** antes del preprocesamiento NLP y clasificación ML
   - Mejora consistencia del análisis, evitando depender de un modelo multilingüe específico

4. **IA Generativa – Cerebras (Refinamiento de neutros)**
   - Se activa cuando el modelo ML entrega:
     - Predicción **Neutro**, o
     - Probabilidad cercana entre clases (baja confianza)
   - Objetivo:
     - Reducir falsos neutros
     - Mejorar precisión en textos ambiguos (p.ej. quejas suaves, ironía, ambivalencia)

   **Criterios de activación:**
     - Predicción inicial = **Neutro**, o
     - **Precisión / probabilidad máxima del modelo ML < 80%**

   **Objetivos del módulo IA:**
     - Validar y refinar la clasificación inicial
     - Reducir errores en textos ambiguos, irónicos o contextuales
     - Minimizar falsos neutros y clasificaciones inestables
     - Entregar una decisión final más robusta y explicable

 Este enfoque permite que el sistema:
     - Mantenga la velocidad del ML clásico
     - Utilice IA únicamente cuando realmente agrega valor

---

5. **Módulo de Analítica y Reportes**
   - A partir del conjunto de predicciones:
     - Calcula estadísticas (distribución por sentimiento, tendencias, palabras frecuentes, tópicos)
     - Genera un reporte con hallazgos
     - Produce recomendaciones y consejos de mejora centrados en comentarios negativos

---

## 15. Contenerización y Despliegue (Docker + Oracle)

### 15.1 Docker

El despliegue se realiza con **Docker**, encapsulando:

- Dependencias de NLP (spaCy, NLTK, modelos)
- Pipeline de ML (scikit-learn, joblib)
- Servicios y configuración de la API
- Integración con servicios Oracle del entorno

Beneficios:
- Reproducibilidad
- Portabilidad entre ambientes (dev / test / prod)
- Escalabilidad horizontal
- Aislamiento de dependencias

### 15.2 Integración con Oracle

SentimentAI se integra con **servicios Oracle** como parte de la infraestructura de operación. Dependiendo del diseño del cliente, Oracle puede soportar:

- Persistencia de resultados (predicciones, trazabilidad)
- Almacenamiento de reportes generados
- Gestión de usuarios / auditoría / logs (si aplica)
- Integración con ecosistemas empresariales existentes

> Nota: la integración exacta (Oracle DB, Oracle Cloud, colas, storage, etc.) dependerá del stack del cliente; la API está preparada para conectarse a estos servicios.

---

## 16. Flujo de Procesamiento (End-to-End)

### 16.1 Flujo para un comentario (Single)

1. **Entrada**: comentario en cualquier idioma
  - Metadatos opcionales: edad, género, fecha, canal
2. **Traducción (Cerebras)** → español
3. **Preprocesamiento** (NLP)
   - Limpieza
   - Lematización
   - Normalización
4. **Clasificación base (ML)** → positivo / neutro / negativo
   - Predicción inicial: positivo / neutro / negativo
   - Cálculo de probabilidad (confianza)
5. **Verificación de neutro (Cerebras)** (solo si aplica)
   - Se ejecuta si:
     - Predicción = Neutro, o
     - Confianza ML < 80%
   - Retorna una clasificación refinada
6. **Salida estructurada** (clase final + texto traducido + metadatos)
   - Sentimiento final
   - Probabilidades
   - Texto traducido
   - Indicador de verificación IA
   - Metadatos asociados

---

### 16.2 Flujo para un CSV (Batch)

1. **Entrada** 
- Archivo `.csv` con:
     - Comentario
     - Edad
     - Género
     - Fecha / época del año
     - Identificador del cliente (si aplica)
2. Procesamiento por filas:
   - Traducción → NLP → ML → verificación IA (si < 80%)

3. **Consolidación**:
   - Resultados por comentario
   - Dataset enriquecido con:
     - Sentimiento final
     - Confianza
     - Variables demográficas y temporales   

4. **Salida**:
   - CSV/JSON de resultados
   - Reporte estadístico automático
   - Insights y recomendaciones

---

## 17. Análisis Estadístico, Perfilamiento del Cliente y Reportes Inteligentes

SentimentAI incorpora un módulo avanzado de **analítica estadística y perfilamiento**, diseñado para extraer valor estratégico a partir de los comentarios procesados.

---

### 17.1 Variables Analizadas

Además del sentimiento, el sistema permite analizar:

- Edad del cliente
- Género
- Época del año:
  - Mes
  - Trimestre
  - Temporadas (alta / baja)
- Volumen de comentarios
- Distribución de sentimientos por segmento
- Otras variables suministradas en el CSV (canal, producto, región, etc.)

---

### 17.2 Resultados estadísticos generados

- Cantidad total de comentarios analizados
- Distribución de sentimientos:
  - Global
  - Por edad
  - Por género
  - Por época del año
- Identificación de:
  - Periodos con mayor cantidad de comentarios negativos
  - Periodos con mayor satisfacción del cliente
- Comparación entre segmentos:
  - ¿Qué grupos generan más comentarios negativos?
  - ¿En qué épocas se concentran las quejas?
- Detección de patrones temporales y demográficos

---

### 17.3 Perfilamiento del Cliente Asociado al Comentario

A partir del cruce entre sentimiento y variables contextuales, el sistema construye un **perfil analítico del cliente**, permitiendo:

- Identificar segmentos críticos de insatisfacción
- Detectar grupos con alta recurrencia de comentarios negativos
- Asociar el tipo de lenguaje con el perfil demográfico
- Priorizar acciones correctivas según impacto real

---



### 17.4 Reportes Inteligentes con IA Generativa

Mediante IA generativa (Cerebras), el sistema produce reportes interpretativos que incluyen:

- Síntesis ejecutiva del estado del cliente
- Hallazgos clave por segmento
- Análisis de tendencias temporales
- Recomendaciones accionables:
  - Mejoras operativas
  - Ajustes en atención al cliente
  - Estrategias de comunicación focalizadas

**Ejemplos de salida:**
- “Los comentarios negativos aumentan significativamente en clientes entre 25 y 35 años durante el segundo trimestre del año…”
- “Se recomienda priorizar mejoras en X para el segmento Y…”
- “La mayor satisfacción se registra en el periodo Z…”

## 18. Modos de Uso del Sistema

SentimentAI soporta dos modalidades principales:

1. **Modo Single (Comentario único)**
   - Ideal para integraciones en tiempo real (chatbots, CRM, formularios)

2. **Modo Batch (CSV)**
   - Ideal para análisis masivo (encuestas, redes sociales, exportaciones de atención al cliente)
   - Genera resultados por registro + analítica agregada + reporte

---

## 19. Consideraciones Técnicas Relevantes

- El uso de traducción previa permite analizar textos en múltiples idiomas usando el mismo pipeline.
- La verificación de neutros reduce errores típicos del análisis automático cuando el texto es ambiguo.
- El enfoque híbrido (ML clásico + IA generativa) busca:
  - precisión y rapidez del modelo ML
  - flexibilidad y contextualización de la IA para casos complejos

---

## 20. Conclusiones

SentimentAI – Análisis Inteligente de Sentimientos se consolida como una solución integral para el análisis automatizado de comentarios de clientes, al integrar técnicas de Procesamiento de Lenguaje Natural (NLP), Machine Learning supervisado e Inteligencia Artificial generativa dentro de una arquitectura escalable y orientada a servicios.

El modelo base TF-IDF combinado con Logistic Regression demostró ser una alternativa eficiente y confiable, ofreciendo un adecuado balance entre precisión, interpretabilidad y facilidad de despliegue en entornos productivos. La utilización de múltiples fuentes de datos y un preprocesamiento lingüístico riguroso permitió mejorar la capacidad de generalización del modelo y su desempeño frente a distintos contextos semánticos.

La incorporación de una capa de verificación mediante Inteligencia Artificial, activada cuando la confianza del modelo de Machine Learning es inferior al 80% o cuando se presentan clasificaciones ambiguas, fortalece la robustez del sistema y reduce errores asociados a neutralidad, ironía o ambigüedad del lenguaje natural. Este enfoque híbrido permite aprovechar la velocidad del modelo clásico y la capacidad contextual de la IA generativa únicamente cuando es necesario.

Adicionalmente, el módulo de análisis estadístico amplía el alcance del análisis de sentimientos tradicional al integrar variables demográficas y temporales, como edad, género y época del año, facilitando la identificación de patrones, tendencias y perfiles de clientes asociados a comentarios positivos y negativos. Esto aporta un valor estratégico adicional para la toma de decisiones en áreas como atención al cliente, marketing y mejora continua.

Finalmente, la contenerización del sistema mediante Docker y su preparación para integrarse con infraestructura Oracle garantizan portabilidad, reproducibilidad y escalabilidad, posicionando a SentimentAI como una solución lista para producción, adaptable a distintos volúmenes de datos, idiomas y necesidades empresariales.



