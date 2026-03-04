# 📊 TelecomX Churn Prediction — Machine Learning Analysis

Este proyecto desarrolla un modelo de **Machine Learning para predecir el abandono de clientes (Churn)** en una empresa de telecomunicaciones.

El objetivo es **identificar clientes con alto riesgo de cancelar el servicio**, permitiendo a la empresa aplicar estrategias de retención temprana.

El análisis incluye:

- Exploratory Data Analysis (EDA)
- Preprocesamiento de datos
- Modelos de Machine Learning
- Optimización de hiperparámetros
- Evaluación comparativa de modelos

---

# 📁 Dataset

El dataset contiene **7043 clientes** y **18 variables predictoras** relacionadas con:

- Información demográfica
- Servicios contratados
- Facturación
- Tipo de contrato
- Método de pago

Distribución de la variable objetivo:

| Clase | Clientes |
|------|------|
| No Churn | 5174 |
| Churn | 1869 |

**Tasa de churn:**  
**26.5%**

Esto implica un **dataset desbalanceado**, lo que requiere técnicas específicas de modelado.

---

# 🔎 Exploratory Data Analysis (EDA)

El análisis exploratorio tuvo cuatro objetivos principales:

1. Analizar la distribución de la variable objetivo
2. Comparar variables numéricas entre clientes que abandonan y los que no
3. Calcular tasas de churn por categoría
4. Detectar multicolinealidad entre variables numéricas

## Variables Numéricas Analizadas

- Tenure (antigüedad del cliente)
- ChargesMonthly
- ChargesDaily
- ChargesTotal

Se utilizaron:

- Histogramas
- Boxplots
- Matriz de correlación

---

# ⚠️ Multicolinealidad detectada

Se identificaron correlaciones muy altas entre variables de facturación.


ChargesDaily ↔ ChargesMonthly : r = 1.00
ChargesTotal ↔ Tenure : r = 0.83


Esto ocurre porque:


ChargesDaily = ChargesMonthly / 30
ChargesTotal ≈ ChargesMonthly × Tenure


Por lo tanto se decidió eliminar:

- `ChargesDaily`
- `ChargesTotal`

Variables finales conservadas:

- `Tenure`
- `ChargesMonthly`

---

# ⚙️ Preprocesamiento de Datos

El pipeline de preprocesamiento incluye:

### Eliminación de variables

CustomerID
ChargesDaily
ChargesTotal


### Codificación de la variable objetivo


Churn
Yes → 1
No → 0


### Transformaciones

| Tipo de variable | Transformación |
|---|---|
| Numéricas | StandardScaler |
| Categóricas | OneHotEncoder |

Esto se implementa mediante **ColumnTransformer dentro de un Pipeline**, evitando **data leakage**.

---

# ✂️ División Train/Test

Se utilizó un **split estratificado** para mantener la proporción de churn.


Train: 5634 registros
Test : 1409 registros
Churn rate: 26.5%

train_test_split(..., stratify=y)


---

# 🤖 Modelos de Machine Learning

Se entrenaron dos modelos principales:

## 1️⃣ Logistic Regression

Modelo lineal interpretable adecuado para relaciones aproximadamente lineales entre variables y churn.

Configuración:


class_weight = "balanced"


Esto compensa el desbalance del dataset.

---

## 2️⃣ Random Forest

Modelo basado en **ensemble de árboles de decisión**, capaz de capturar interacciones no lineales entre variables.

También se utilizó:


class_weight = "balanced"


---

# 📊 Métricas de Evaluación

Para problemas de churn, la métrica más importante es **Recall**, ya que el objetivo es **detectar la mayor cantidad posible de clientes que abandonarán**.

Métricas utilizadas:

- Recall ⭐
- ROC-AUC
- F1-Score
- Classification Report
- Confusion Matrix
- ROC Curve

---

# 📈 Resultados — Modelos Base

| Modelo | ROC-AUC | Recall | F1 |
|------|------|------|------|
| Logistic Regression | **0.8422** | **0.7834** | 0.6168 |
| Random Forest | 0.8416 | 0.7193 | **0.6270** |

Observación:

- La **Regresión Logística obtiene mejor Recall**
- Random Forest obtiene ligeramente mejor F1

---

# 🔧 Optimización de Modelos

Se utilizó **RandomizedSearchCV** con:


scoring = "recall"
cv = StratifiedKFold(5)
n_iter = 40


Esto permite explorar eficientemente el espacio de hiperparámetros.

---

# 🏆 Mejores Hiperparámetros

## Logistic Regression


C = 0.00355
penalty = l2
solver = liblinear
class_weight = balanced


Recall CV:


0.798


---

## Random Forest


n_estimators = 445
max_depth = 5
min_samples_split = 10
min_samples_leaf = 2
max_features = sqrt
class_weight = balanced


Recall CV:


0.8107


---

# 📊 Resultados — Modelos Optimizados

| Modelo | ROC-AUC | Recall | F1 |
|------|------|------|------|
| Logistic Regression Optimizada | 0.8400 | 0.7968 | **0.6307** |
| Random Forest Optimizado | 0.8418 | **0.8102** | 0.6228 |

---

# 🏆 Comparación Final

| Modelo | ROC-AUC | Recall | F1 |
|------|------|------|------|
| LR Base | **0.8422** | 0.7834 | 0.6168 |
| RF Base | 0.8416 | 0.7193 | 0.6270 |
| LR Optimizada | 0.8400 | 0.7968 | **0.6307** |
| RF Optimizado | 0.8418 | **0.8102** | 0.6228 |

### Mejores métricas


Mejor Recall → Random Forest Optimizado
Mejor ROC-AUC → Logistic Regression Base
Mejor F1 Score → Logistic Regression Optimizada


---

# 📊 Validación Cruzada

Se utilizó **Stratified K-Fold (5 folds)** para estimar la estabilidad del modelo.

Objetivos:

- Reducir varianza del holdout set
- Detectar sobreajuste
- Obtener estimaciones más robustas

---

# 🧠 Interpretación del Modelo

En este dataset las relaciones entre variables y churn son **principalmente lineales**.

Ejemplos:


Más Tenure → menor churn
Más ChargesMonthly → mayor churn
Contrato mensual → mayor churn
Sin seguridad online → mayor churn


Por esta razón, **Logistic Regression compite muy bien contra Random Forest**, a pesar de ser un modelo más simple.

---

# 💡 Conclusiones

Principales hallazgos:

- El dataset presenta **26.5% churn**, lo que requiere manejo del desbalance.
- **Random Forest optimizado obtiene el mayor Recall (0.81)**.
- **Logistic Regression ofrece alta interpretabilidad y resultados competitivos.**
- La optimización mejora especialmente el rendimiento del Random Forest.

---

# 🚀 Posibles mejoras futuras

- Modelos adicionales (XGBoost, LightGBM)
- Feature engineering adicional
- Interpretabilidad con SHAP
- Optimización de umbral de decisión
- Modelos de retención segmentados

---

# 🛠️ Tecnologías Utilizadas

- Python
- Pandas
- NumPy
- Scikit-learn
- Matplotlib
- Seaborn
- Jupyter Notebook

---

# 👨‍💻 Autor

Rodrigo Muñoz  
GitHub:  
https://github.com/RodrigoMunoz-dev
