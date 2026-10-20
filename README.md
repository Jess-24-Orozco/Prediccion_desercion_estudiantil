# Predicción de abandono estudiantil

Proyecto de portafolio en ciencia de datos: un modelo de clasificación que predice si un estudiante abandonará la carrera, seguirá matriculado o se graduará, usando únicamente información disponible al terminar el primer semestre.

## Fuente de los datos

Dataset *Predict Students' Dropout and Academic Success* (UCI Machine Learning Repository), de Realinho, Machado, Baptista y Martins (2022). Contiene 4.424 estudiantes de una institución de educación superior de Portugal, ya anonimizados. Revisa la licencia y la cita en la página del dataset.

## Problema

Las instituciones de educación superior suelen detectar el abandono cuando ya ocurrió, sin un mecanismo que identifique de forma temprana a los estudiantes en riesgo.

## Objetivo

Construir un modelo de clasificación que prediga si un estudiante abandonará (*Dropout*), seguirá matriculado (*Enrolled*) o se graduará (*Graduate*), e identificar qué factores se asocian con el abandono.

## Alcance

**Incluye:** análisis exploratorio, comparación de tres modelos de clasificación, evaluación con métricas para clases desbalanceadas y recomendaciones. Se usan únicamente variables del primer semestre, para simular una alerta temprana.

**Fuera del alcance:** la puesta en producción del modelo y las conclusiones sobre las causas del abandono (solo se describen asociaciones).

## Proceso

1. Carga y revisión de la calidad de los datos (sin nulos ni duplicados).
2. Análisis exploratorio: estadística descriptiva, histogramas, boxplots y tasas de abandono por variable.
3. Selección de 28 variables, sin fuga de información (se excluyó el segundo semestre).
4. División en entrenamiento (75 %) y prueba (25 %), estratificada.
5. Preprocesamiento con pipeline: escalado de variables numéricas y codificación *one-hot* de las categóricas.
6. Entrenamiento de tres modelos con clases balanceadas: Regresión Logística, Random Forest y Gradient Boosting.
7. Evaluación con F1 macro, AUC, recall y matriz de confusión.

## Análisis exploratorio

Antes de modelar se revisaron los datos para entender su estructura y qué variables se relacionan con el abandono.

**Calidad de los datos.** El dataset tiene 4.424 estudiantes y 35 columnas, sin valores nulos ni duplicados. Varias columnas numéricas son en realidad códigos de categorías (carrera, estado civil,
