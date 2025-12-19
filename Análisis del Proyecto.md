# H12-25-L-Equipo 72  
## Proyecto 1: SentimentAPI — Análisis de Sentimientos de Feedbacks ONE

---

## 1. Análisis del proyecto

### 1.1 Problema de negocio
Las empresas reciben grandes volúmenes de comentarios (reseñas, encuestas, redes sociales) y no pueden analizarlos manualmente. Esto genera:  
- Lentitud en la atención al cliente  
- Pérdida de oportunidades de mejora  
- Dificultad para medir satisfacción en el tiempo  

### 1.2 Solución propuesta
Una API de análisis de sentimiento que:  
- Recibe un texto  
- Clasifica el sentimiento (Positivo / Neutro / Negativo o binario)  
- Devuelve la predicción con su probabilidad  
- Puede integrarse fácilmente con otros sistemas  

### 1.3 Valor para el cliente
Incluso con un modelo simple:  
- Prioriza comentarios negativos  
- Monitorea campañas de marketing  
- Detecta tendencias de satisfacción  
- Reduce esfuerzo manual  

### 1.4 Alcance técnico adecuado al hackathon
- Modelo clásico de NLP: TF-IDF + Regresión Logística o Naive Bayes  
- API REST simple  
- Integración clara entre DS y BE  
- MVP funcional > sofisticación excesiva  

---

## FASE 1 – Definición y alineación (DS + BE)

### 1.1 Definir alcance funcional (conjunto)
**Responsables:** DS + BE  
**Dependencias:** ninguna  

Decidir:  
- Clasificación binaria o ternaria  
- Idioma (Español / Portugués / ambos)  

Definir el contrato de integración:  
- Entrada  
- Salida  

> Este contrato desbloquea el trabajo en paralelo  

---

## FASE 2 – Data Science: preparación del modelo

### 2.1 Recolección del dataset
**Responsable:** Data Science  
**Dependencias:** Fase 1  

- Reviews públicas, tweets, encuestas, etc.  
- Dataset con:  
  - Texto  
  - Etiqueta de sentimiento  

### 2.2 EDA y limpieza de datos
**Responsable:** Data Science  
**Dependencias:** 2.1  

- Eliminar duplicados  
- Normalizar texto (minúsculas, símbolos)  
- Análisis de distribución de clases  
- Visualización básica  

### 2.3 Feature engineering (TF-IDF)
**Responsable:** Data Science  
**Dependencias:** 2.2  

- Vectorización de texto con `TfidfVectorizer`  
- Ajuste de:  
  - n-grams  
  - stopwords  
  - max_features (si aplica)  

### 2.4 Entrenamiento del modelo
**Responsable:** Data Science  
**Dependencias:** 2.3  

- Entrenar:  
  - Logistic Regression o  
  - Naive Bayes  
- Separación train/test  

### 2.5 Evaluación del modelo
**Responsable:** Data Science  
**Dependencias:** 2.4  

- Métricas:  
  - Accuracy  
  - Precision  
  - Recall  
  - F1-score  
- Decidir si el modelo es suficiente para MVP  

### 2.6 Serialización del modelo
**Responsable:** Data Science  
**Dependencias:** 2.5  

- Guardar pipeline completo  
- Entregar al equipo BE:  
  - Archivo `.joblib`  
  - Ejemplo de inferencia en Python  
  - Mapeo de etiquetas (ej. 0 = Negativo)  

> Este paso es un bloqueo crítico para Back-End  

---

## FASE 3 – Back-End: construcción de la API

### 3.1 Crear proyecto Spring Boot
**Responsable:** Back-End  
**Dependencias:** Fase 1  

- Configuración básica:  
  - Spring Web  
  - Validation  
  - Logging  
- Estructura limpia (controller, service, dto)  

### 3.2 Implementar endpoint `/sentiment`
**Responsable:** Back-End  
**Dependencias:** 3.1  

- `POST /sentiment`  
- DTO de entrada  

### 3.3 Validación y manejo de errores
**Responsable:** Back-End  
**Dependencias:** 3.2  

- Texto vacío o muy corto → error 400  
- Mensajes claros y consistentes en JSON  

### 3.4 Integración con el modelo DS
**Responsable:** Back-End  
**Dependencias:** 2.6  

**Opción A (más simple):**  
- Microservicio Python (FastAPI) con el modelo  
- BE llama vía HTTP  

**Opción B (avanzada):**  
- Cargar modelo exportado (ONNX)  

> La integración depende directamente del entregable DS  

---

## FASE 4 – Pruebas y validación

### 4.1 Pruebas manuales (Postman / cURL)
**Responsables:** BE + DS  
**Dependencias:** Fase 3 completa  

- 3 ejemplos reales:  
  - Positivo  
  - Neutro  
  - Negativo  

### 4.2 Logs y estabilidad
**Responsable:** Back-End  
**Dependencias:** 4.1  

- Logs de requests  
- Manejo de excepciones inesperadas  

---

## FASE 5 – Documentación y entrega

### 5.1 Notebook final (DS)
**Responsable:** Data Science  
**Dependencias:** Fase 2 completa  

- Código limpio  
- Comentarios claros  
- Resultados y conclusiones  

### 5.2 README del proyecto
**Responsables:** DS + BE  
**Dependencias:** Fases 2–4  

Incluir:  
- Cómo ejecutar el modelo  
- Cómo levantar la API  
- Ejemplos de request/response  
- Dependencias y versiones  

### 5.3 Preparación de la demo
**Responsables:** DS + BE  
**Dependencias:** todo lo anterior  

- Mostrar:  
  - Llamada al endpoint  
  - Respuesta del modelo  
  - Explicación simple del flujo  
