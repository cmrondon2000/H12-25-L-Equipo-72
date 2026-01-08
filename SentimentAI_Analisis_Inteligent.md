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

SentimentAI se implementa como una API desplegable en contenedores, integrando:

- Un **modelo de Machine Learning** (TF-IDF + Logistic Regression) entrenado y persistido con `joblib`
- Servicios de soporte en **Oracle** (para operación/almacenamiento/observabilidad según el entorno)
- Dos capacidades de **IA Generativa** vía **Cerebras**, usadas como módulos auxiliares para:
  1) traducir entradas al español (soporte multilenguaje)
  2) refinar la decisión en casos neutros (mejor precisión)
- Un módulo de **analítica y generación de reportes**, que resume resultados y entrega recomendaciones

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
2. **Traducción (Cerebras)** → español
3. **Limpieza + lematización** (NLP)
4. **Clasificación base (ML)** → positivo / neutro / negativo
5. **Verificación de neutro (Cerebras)** (solo si aplica)
6. **Salida estructurada** (clase final + texto traducido + metadatos)

---

### 16.2 Flujo para un CSV (Batch)

1. **Entrada**: archivo `.csv` con columna de comentarios (y opcionalmente id/metadata)
2. Procesamiento por filas:
   - Traducción a español (si se requiere)
   - NLP + ML + refinamiento neutros
3. **Consolidación**:
   - Tabla final con resultados por comentario
   - Estadísticas agregadas
4. **Salida**:
   - CSV/JSON de resultados
   - Reporte resumido (insights + recomendaciones)

---

## 17. Análisis Estadístico y Reportes Inteligentes

El sistema incorpora un módulo de análisis que produce reportes automáticos a partir de las predicciones obtenidas.

### 17.1 Resultados estadísticos típicos

- Conteo total de comentarios procesados
- Distribución por sentimiento:
  - % Positivo / % Neutro / % Negativo
- Ranking de términos frecuentes (sobre todo negativos)
- Detección de temas recurrentes en quejas (p.ej. entrega, atención, calidad, precio)
- Identificación de “drivers” de insatisfacción:
  - causas frecuentes
  - patrones de lenguaje asociados a frustración / queja

### 17.2 Reporte orientado a mejora (IA Generativa)

A través de IA (Cerebras), el sistema genera:
- Síntesis ejecutiva del sentimiento del cliente
- Principales hallazgos de comentarios negativos
- Recomendaciones prácticas:
  - acciones correctivas
  - sugerencias de comunicación con clientes
  - oportunidades de mejora del servicio/producto

Ejemplos de salida del reporte:
- “La mayoría de comentarios negativos se concentran en tiempos de respuesta…”
- “Se sugiere priorizar la solución de X y ajustar el proceso Y…”
- “Se detecta un patrón de quejas relacionado con Z…”

---

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




