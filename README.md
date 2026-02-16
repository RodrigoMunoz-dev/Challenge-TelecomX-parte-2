# 📡 Challenge Telecom X - Parte 2: Prediciendo la Evasión de Clientes

Este repositorio contiene la segunda fase del proyecto de Ciencia de Datos para **Telecom X**. En esta etapa, el objetivo es evolucionar del análisis descriptivo hacia el **análisis predictivo**, utilizando herramientas de estadística avanzada y Machine Learning para anticipar el *churn* (cancelación) de clientes.

## 🎯 Propósito del Proyecto
Construir modelos predictivos que permitan a la empresa identificar clientes con alto riesgo de abandono. El enfoque principal es:

* **Identificar** las variables que más influyen en el comportamiento de los clientes.
* **Definir** perfiles de clientes que requieren atención prioritaria.
* **Fundamentar** estrategias de retención personalizadas basadas en datos.

## 📂 Estructura del Proyecto
* `/notebooks`: Contiene el archivo `.ipynb` con el análisis y modelado.
* `/data`: Incluye el archivo `TelecomX_Clean.csv` (dataset tratado en la Parte 1).
* `README.md`: Documentación del proyecto.

## ⚙️ Proceso de Preparación de Datos
Para asegurar modelos confiables, se realizaron las siguientes etapas:

1. **Clasificación de Variables**: Separación entre variables categóricas (contrato, servicios) y numéricas (*tenure*, cargos).
2. **Limpieza Avanzada**: Eliminación de columnas irrelevantes para la predicción y verificación de la proporción de clases (cancelación vs. permanencia).
3. **Codificación y Normalización**: Transformación de datos para que los algoritmos de ML puedan procesarlos eficientemente.
4. **División del Dataset**: Separación en conjuntos de entrenamiento y prueba para validar el rendimiento del modelo.

## 📊 Análisis y Modelado
El flujo de trabajo técnico incluye:

* **Análisis de Correlación**: Identificación de los factores críticos que impulsan el *churn*.
* **Regresión Lineal**: Modelado de las relaciones entre variables para entender su impacto individual.
* **Modelos de Clasificación**: Desarrollo de modelos de Machine Learning orientados a predecir si un cliente se irá o no.
* **Evaluación de Métricas**: Uso de matrices de confusión, precisión y *recall* para medir la efectividad del modelo.
