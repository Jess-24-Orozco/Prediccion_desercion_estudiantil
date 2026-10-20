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

**Calidad de los datos.** El dataset tiene 4.424 estudiantes y 35 columnas, sin valores nulos ni duplicados. Varias columnas numéricas son en realidad códigos de categorías (carrera, estado civil, nacionalidad), por lo que se tratan como categóricas.

**Variable objetivo.** Las tres clases no están balanceadas: *Enrolled* es la más pequeña (17,9 %). Por eso se usan métricas como F1 macro y pesos balanceados.

![Distribución del Target](eda_target.png)

**Rendimiento del primer semestre.** El 40 % de quienes abandonaron no aprobó ninguna materia, frente al 3,5 % de quienes se graduaron. Los 718 estudiantes con nota 0 son estudiantes sin materias aprobadas, no datos faltantes. La mediana de materias aprobadas es 2 entre quienes abandonaron y 6 entre quienes se graduaron.

![Rendimiento del primer semestre según Target](eda_rendimiento_primer_semestre.png)

**Situación económica.** Abandona el 86,6 % de quienes no tienen la matrícula al día (24,7 % entre quienes sí), el 62,0 % de quienes tienen deuda (28,3 % sin deuda) y el 38,7 % de quienes no tienen beca (12,2 % con beca).

![Tasa de abandono según situación económica](eda_abandono_por_variable.png)

Variables como ser estudiante internacional o tener necesidades educativas especiales casi no cambian la tasa de abandono y tienen muy pocos casos (110 y 51). Estos resultados muestran asociaciones, no causas.

## Resultados

Evaluados en 1.106 estudiantes que los modelos no vieron al entrenar:

| Modelo | F1 macro | AUC | Recall macro | Recall Dropout |
|---|---|---|---|---|
| Regresión Logística | 0,699 | 0,874 | 0,702 | 0,724 |
| Random Forest | 0,654 | 0,878 | 0,648 | 0,766 |
| **Gradient Boosting** | **0,704** | **0,883** | **0,709** | 0,704 |

Gradient Boosting obtuvo el mejor F1 macro y AUC, pero queda casi empatado con la Regresión Logística, por lo que la diferencia no es concluyente. La clase *Enrolled* fue la más difícil de predecir en los tres modelos.

## Hallazgos principales

- El 32,1 % de los estudiantes abandonó la carrera.
- El 40 % de quienes abandonaron no aprobó ninguna materia en el primer semestre, frente al 3,5 % de quienes se graduaron.
- Abandona el 86,6 % de quienes no tienen la matrícula al día y el 62,0 % de quienes tienen deuda.
- Con beca abandona el 12,2 %; sin beca, el 38,7 %.

## Limitaciones

- Los datos son de una institución de Portugal; no se pueden generalizar directamente a otros países.
- La evaluación usa una sola división de entrenamiento y prueba. Como siguiente paso: validación cruzada y ajuste de parámetros.

## Contenido del repositorio

- `Prediccion_desercion_estudiantil.ipynb`: cuaderno con todo el análisis y los modelos.
- `dataset.csv`: datos originales.
- `dataset_estudiantes_arreglado.xlsx`: los mismos datos en Excel, con un diccionario que explica cada código.
- `eda_target.png`, `eda_abandono_por_variable.png` y `eda_rendimiento_primer_semestre.png`: gráficos del análisis exploratorio.

