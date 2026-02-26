# INFORME DE ANÁLISIS DE CANCELACIÓN DE CLIENTES (CHURN)
**Autores:** Antony Aroni

------------------------------------------------------------------------

## 1. RESUMEN

El presente análisis tuvo como objetivo identificar los factores más
influyentes en la cancelación de clientes (churn) y desarrollar modelos
predictivos para anticipar este comportamiento.

### Métricas Globales del Dataset

-   **Total de clientes analizados:** 7,043  
-   **Tasa de cancelación global:** 25.72%  
-   **Clientes que NO cancelaron:** 74.28%  
-   **Clientes que SÍ cancelaron:** 25.72%  
-   **Ratio (No cancelados / Cancelados):** 2.89:1

El desbalance de clases (25.72% de cancelaciones) fue tratado mediante
la técnica **SMOTE**, generando datos sintéticos de la clase minoritaria
para balancear el conjunto de entrenamiento (50/50).

### Mejor Modelo Obtenido

-   **Modelo seleccionado:** Regresión Logística  
-   **ROC-AUC:** 0.8326  
-   **F1-Score:** 0.6073  
-   **Recall:** 66.58% de las cancelaciones son detectadas  
-   **Precisión:** 55.83% de las alertas son correctas

------------------------------------------------------------------------

## 2. ANÁLISIS COMPARATIVO DE MODELOS

### 2.1 Métricas de Rendimiento por Modelo

| Modelo              | Accuracy | Precisión | Recall | F1-Score | ROC-AUC |
|---------------------|----------|-----------|--------|----------|---------|
| Regresión Logística | 0.7785   | 0.5583    | 0.6658 | 0.6073   | 0.8326  |
| Random Forest       | 0.7641   | 0.5398    | 0.5615 | 0.5505   | 0.8156  |
| KNN                 | 0.7283   | 0.4826    | 0.7781 | 0.5957   | 0.8091  |

### 2.2 Matrices de Confusión

#### Regresión Logística

|             | Predicho No (0) | Predicho Sí (1) |
|-------------|-----------------|-----------------|
| Real No (0) | 883             | 197             |
| Real Sí (1) | 125             | 249             |

#### Random Forest

|             | Predicho No (0) | Predicho Sí (1) |
|-------------|-----------------|-----------------|
| Real No (0) | 901             | 179             |
| Real Sí (1) | 164             | 210             |

#### KNN

|             | Predicho No (0) | Predicho Sí (1) |
|-------------|-----------------|-----------------|
| Real No (0) | 768             | 312             |
| Real Sí (1) | 83              | 291             |

### 2.3 Análisis de Errores por Modelo

| Modelo              | Falsos Negativos | Falsos Positivos | Error más frecuente  |
|---------------------|------------------|------------------|----------------------|
| Regresión Logística | 125              | 197              | Falsas alarmas (61%) |
| Random Forest       | 164              | 179              | Balanceado           |
| KNN                 | 83               | 312              | Falsas alarmas (79%) |

### 2.4 Evaluación de Overfitting / Underfitting

| Modelo              | F1-Train | F1-Test | Diferencia | Diagnóstico             |
|---------------------|----------|---------|------------|-------------------------|
| Regresión Logística | 0.6123   | 0.6073  | +0.0050    | Generalización adecuada |
| Random Forest       | 0.6123   | 0.5505  | +0.0618    | Leve overfitting        |
| KNN                 | 0.6123   | 0.5957  | +0.0166    | Generalización adecuada |

------------------------------------------------------------------------

## 3. FACTORES QUE INFLUYEN EN LA CANCELACIÓN

### 3.1 Ranking Combinado de Variables (Top 15)

| Ranking | Variable                    | RL Rank | RF Rank | KNN Rank | Promedio |
|---------|-----------------------------|---------|---------|----------|----------|
| 1       | Cuentas_Diarias             | 3       | 3       | 1.5      | 2.5      |
| 2       | Charges.Monthly             | 4       | 4       | 1.5      | 3.2      |
| 3       | tenure                      | 7       | 2       | 3.0      | 4.0      |
| 4       | Charges.Total               | 12      | 1       | 4.0      | 5.7      |
| 5       | InternetService_Fiber optic | 2       | 10      | 5.0      | 5.7      |
| 6       | TechSupport_1               | 8       | 6       | 17.0     | 10.3     |
| 7       | OnlineSecurity_1            | 9       | 7       | 16.0     | 10.7     |
| 8       | Contract_Two year           | 11      | 5       | 19.0     | 11.7     |
| 9       | OnlineBackup_1              | 10      | 12      | 14.0     | 12.0     |
| 10      | StreamingMovies_1           | 5       | 22      | 10.0     | 12.3     |
| 11      | StreamingTV_1               | 6       | 24      | 11.0     | 13.7     |
| 12      | MultipleLines_Yes           | 14      | 19      | 8.0      | 13.7     |
| 13      | DeviceProtection_1          | 13      | 16      | 13.0     | 14.0     |
| 14      | gender                      | 28      | 13      | 6.0      | 15.7     |
| 15      | Partner                     | 29      | 11      | 9.0      | 16.3     |

------------------------------------------------------------------------

## 4. INTERPRETACIÓN DE NEGOCIO

### 4.1 Factores de Riesgo

| Variable        | Coeficiente | Impacto  |
|-----------------|-------------|----------|
| Cuentas_Diarias | +3.0597     | MUY ALTO |
| Charges.Monthly | +3.0597     | MUY ALTO |
| Charges.Total   | +1.3719     | ALTO     |

A mayor gasto mensual y diario, mayor probabilidad de cancelación.

### 4.2 Factores Protectores

| Variable                    | Coeficiente | Impacto  |
|-----------------------------|-------------|----------|
| PhoneService                | -5.9164     | MUY ALTO |
| InternetService_Fiber optic | -4.6381     | MUY ALTO |
| StreamingMovies_1           | -2.0653     | ALTO     |
| StreamingTV_1               | -2.0463     | ALTO     |
| tenure                      | -1.8357     | ALTO     |
| TechSupport_1               | -1.8031     | ALTO     |
| OnlineSecurity_1            | -1.7824     | ALTO     |
| OnlineBackup_1              | -1.5611     | ALTO     |
| Contract_Two year           | -1.5121     | ALTO     |
| DeviceProtection_1          | -1.2962     | MODERADO |

------------------------------------------------------------------------

## 5. ESTRATEGIAS DE RETENCIÓN PROPUESTAS

### 5.1 Estrategias por Segmento

#### Clientes de Alto Riesgo

-   Descuentos por permanencia vinculados a servicios adicionales.  
-   Programas de lealtad escalonados.  
-   Encuestas trimestrales.

#### Clientes de Riesgo Moderado

-   Bundles con descuento.  
-   Up-selling de servicios de valor agregado.

#### Clientes Leales

-   Beneficios exclusivos +2 años.  
-   Descuentos por fidelidad y referidos.

------------------------------------------------------------------------

## 6. CONCLUSIONES FINALES

1.  Regresión Logística es el modelo recomendado (ROC-AUC 0.8326).  
2.  Variables más predictivas: gasto y permanencia.  
3.  Múltiples servicios reducen significativamente la cancelación.  
4.  Principal error: falsas alarmas.  
5.  Estrategias deben enfocarse en contratos largos y servicios
    adicionales.

------------------------------------------------------------------------

