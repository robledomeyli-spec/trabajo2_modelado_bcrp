# Trabajo 2: Modelado Supervisado - Expectativas Económicas en Perú
**Curso**: Introducción a Ciencia de Datos y Machine Learning con Python
## Descripción de nuestro Proyecto

Análisis de modelos supervisados para predecir y clasificar las expectativas económicas de empresas peruanas en función de variables macroeconómicas clave del periodo 2015-2025.

### Pregunta de Investigación
**¿Cómo influyen el tipo de cambio interbancario, la inflación mensual, el PBI mensual y la tasa de referencia del BCRP en el índice de expectativas de la economía en el Perú?**

## Estructura del Proyecto

```
Trabajo2/
│
├── Trabajo2_COMPLETO.ipynb          # Notebook principal con análisis completo
├── dataset_bcrp_limpio.csv          # Dataset curado del Trabajo 1
├── README.md                        # Este archivo
└── requirements.txt                 # Dependencias del proyecto
```

## Variables del Dataset

### Variable Objetivo
- **`Expectativas_Economia`**: Índice de expectativas económicas (continuo)
- **`Expectativas_Binaria`**: Clasificación binaria (1: Optimismo >50, 0: Pesimismo ≤50)

### Variables Predictoras
- **`Tasa_Referencia_PM`**: Tasa de referencia de política monetaria del BCRP
- **`Tipo_Cambio_Promedio`**: Tipo de cambio interbancario promedio mensual
- **`PBI_Mensual`**: Producto Bruto Interno mensual
- **`IPC`**: Índice de Precios al Consumidor (inflación)

## Instrucciones de Ejecución

### Prerrequisitos
```bash
pip install -r requirements.txt
```

### Librerías Principales
```python
pandas==1.5.3
numpy==1.21.5
scikit-learn==1.2.2
statsmodels==0.13.5
matplotlib==3.5.2
seaborn==0.11.2
```

### Ejecución del Notebook
1. Clonar el repositorio
2. Subir `dataset_bcrp_limpio.csv` al entorno de ejecución
3. Ejecutar todas las celdas del notebook en orden

## Metodología

### 1. Separación de Datos
- **Train/Test Split**: 75%/25% (95/32 filas)
- **Shuffle**: `False` (preservar orden temporal)
- **Random State**: 42 (reproducibilidad)

### 2. Modelos Implementados

#### Ruta A: Comparación de Modelos de Regresión
- **OLS Simple**: Tipo_Cambio_Promedio → Expectativas_Economia
- **OLS Complejo**: Todas las variables → Expectativas_Economia

#### Ruta B: Regresión vs Clasificación  
- **OLS Simple**: Modelo de regresión lineal
- **Logit**: Modelo de clasificación binaria

### 3. Validación y Métricas

#### Regresión
- **Métrica principal**: MSE (Mean Squared Error)
- **Validación**: TimeSeriesSplit (k=5)
- **Baseline**: Predicción de la media

#### Clasificación
- **Métrica principal**: F1-Score (balance entre precisión y recall)
- **Métricas secundarias**: Accuracy, ROC-AUC
- **Baseline**: Clase mayoritaria (Dummy Classifier)
- **Validación**: TimeSeriesSplit (k=5)

### 4. Diagnósticos de Modelos
- **Statsmodels**: Coeficientes, p-values, R²
- **Residuos**: Análisis de heterocedasticidad (Breusch-Pagan)
- **Multicolinealidad**: Número de condición

## Resultados Clave

### Modelo Ganador: Regresión Logística
- **Accuracy Test**: 78.1%
- **F1-Score**: 75.9%
- **ROC-AUC**: 84.3%
- **Mejora vs Baseline**: +24.6% en accuracy

### Variables Significativas
1. **Tipo_Cambio_Promedio**: Efecto negativo significativo (p < 0.05)
2. **IPC**: Efecto positivo significativo (p < 0.05)  
3. **PBI_Mensual**: Efecto positivo (significativo en OLS)
4. **Tasa_Referencia_PM**: No significativa en modelos finales

## Decisiones de Modelado

### 1. Justificación de TimeSeriesSplit
Los datos son mensuales (serie temporal 2015-2025), por lo que:
- No se mezclan cronológicamente (`shuffle=False`)
- Se preservan dependencias temporales
- Se usa `TimeSeriesSplit` en lugar de K-Fold estándar

### 2. Elección de Forma Funcional
- **Lineal**: Relaciones teóricamente lineales entre variables macroeconómicas
- **Sin transformaciones polinomiales**: Mantener interpretabilidad económica
- **Logit**: Para clasificación binaria natural del índice (>50 vs ≤50)

### 3. Manejo de Variables
- **Sin one-hot encoding**: Todas las variables son numéricas
- **Sin escalado**: Modelos lineales son invariantes a escala
- **Manejo de NAs**: Dataset ya curado del Trabajo 1

## Limitaciones y Consideraciones

### Limitaciones del Estudio
- Periodo limitado (2015-2025)
- No incluye shocks externos (políticos, internacionales)
- Relaciones asumidas lineales pueden simplificar dinámicas complejas

### Supuestos del Modelo
- Relaciones lineales entre variables
- Independencia condicional de observaciones
- Estacionariedad aproximada de series temporales

## Próximos Pasos Potenciales

1. **Modelos no lineales**: Random Forest, XGBoost para capturar relaciones complejas
2. **Variables adicionales**: Expectativas sectoriales, indicadores globales
3. **Modelos dinámicos**: Incorporar rezagos temporales
4. **Análisis de causalidad**: Modelos de ecuaciones simultáneas

## Integrantes

- Abad Aniceto Anguiela Brilletth
- Meyli Yanely Robledo Jimenez  
- Damaris Belen Navarro Lozada
- Alberto Sebastian Pizarro Otero


