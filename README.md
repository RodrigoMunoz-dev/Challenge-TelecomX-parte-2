Challenge Telecom X · Parte 2 — Alura Latam
Construcción de modelos predictivos de Machine Learning para anticipar la cancelación de clientes.

📑 Tabla de Contenidos

Propósito del Proyecto

Estructura del Repositorio

Contexto del Problema

Proceso de Preparación de Datos

Decisiones de Modelización

Modelos Implementados

Métricas de Evaluación

Exploración de Threshold

Principales Insights

Conclusiones Estratégicas

Instrucciones de Ejecución

Tecnologías Utilizadas

🎯 Propósito del Proyecto

Este proyecto forma parte del Challenge Telecom X - Parte 2 del programa de formación en Ciencia de Datos de Alura Latam.

El objetivo principal es construir modelos de clasificación capaces de predecir qué clientes tienen mayor probabilidad de cancelar su servicio (Churn), con el fin de ayudar a la empresa a implementar estrategias de retención proactivas.

Preguntas clave del análisis:

¿Quiénes son los clientes con mayor riesgo de evasión?

¿Qué variables influyen más en ese comportamiento?

¿Qué perfil de cliente requiere mayor atención?

📁 Estructura del Repositorio
telecomx-churn/


├── Challenge Alura TelecomX parte2.ipynb # Notebook principal con el análisis completo

├── README.md   # Documentación del proyecto

└── datos_tratados.csv  # Dataset preprocesado (output del Challenge Parte 1)

📊 Contexto del Problema

El dataset contiene información de 7.043 clientes de una empresa de telecomunicaciones.

Variable objetivo:

Churn (1 = canceló, 0 = permaneció)

Distribución:

No Churn: ~73.5%

Churn: ~26.5%

Este desbalance (~3:1) impacta directamente en las decisiones de modelado y evaluación.

🧹 Proceso de Preparación de Datos
1️⃣ Limpieza y selección de variables

Se eliminaron las siguientes columnas antes del modelado:

Columna	Motivo
CustomerID	Identificador sin valor predictivo
ChargesDaily	Correlación perfecta con ChargesMonthly (r = 1.00) — multicolinealidad
ChargesTotal	Correlación alta con Tenure (r = 0.83) — variable derivada

La eliminación se justificó mediante análisis de correlación (ver sección correspondiente en el notebook).
Mantener variables altamente correlacionadas puede inflar la varianza de coeficientes en Regresión Logística y distorsionar la interpretación.

Variables retenidas:

Numéricas: Tenure, ChargesMonthly

Categóricas: 16 variables (demográficas, contractuales y de servicios)

2️⃣ Codificación y estandarización

Implementadas dentro de un Pipeline con ColumnTransformer:

StandardScaler → variables numéricas

OneHotEncoder(handle_unknown='ignore') → variables categóricas

Esto evita data leakage y garantiza un preprocesamiento consistente.

3️⃣ Split estratificado
X_train, X_test, y_train, y_test = train_test_split(
    X, y,
    test_size=0.2,
    random_state=42,
    stratify=y
)

El parámetro stratify=y mantiene la proporción de churn en entrenamiento y prueba.

⚙️ Decisiones de Modelización
¿Por qué class_weight='balanced'?

El dataset presenta desbalance (~3:1).
Este parámetro ajusta los pesos de las clases de forma inversamente proporcional a su frecuencia.

¿Por qué optimizar por Recall?

En problemas de churn:

Falso negativo → cliente se va y no lo detectamos (alto costo).

Falso positivo → cliente es contactado sin necesidad (costo menor).

Por eso RandomizedSearchCV utiliza:

scoring='recall'
¿Por qué Regresión Logística puede rendir mejor?

En este dataset específico, las relaciones entre variables y churn muestran un comportamiento mayormente lineal (por ejemplo, mayor antigüedad → menor churn).

En este contexto, modelos lineales como la Regresión Logística pueden ser más estables y competitivos que modelos más complejos.

🤖 Modelos Implementados

Se entrenaron cuatro configuraciones:

Modelo	Descripción
LR Base	Regresión Logística con hiperparámetros por defecto
RF Base	Random Forest (200 árboles, max_depth=10)
LR Optimizado	RandomizedSearchCV (40 iteraciones, scoring=recall)
RF Optimizado	RandomizedSearchCV (40 iteraciones, scoring=recall)

Validación cruzada:

StratifiedKFold

5 folds

📈 Métricas de Evaluación

Recall ⭐ (principal)

ROC-AUC

F1-Score

Matriz de Confusión

Curva ROC

🎚 Exploración de Threshold

Por defecto, los clasificadores usan 0.5 como umbral.

Se exploraron los siguientes valores sobre el mejor modelo:

Threshold	Comportamiento
0.50	Más conservador (mayor precisión)
0.45	Balance intermedio
0.40	Mayor Recall
0.35	Recall máximo (más falsos positivos)

Reducir el threshold aumenta la capacidad de detectar clientes en riesgo.

La elección depende del contexto operativo:

Recursos amplios → threshold más agresivo (0.35–0.40)

Recursos limitados → threshold más conservador (0.45–0.50)

🔎 Principales Insights
1️⃣ Tipo de contrato

Clientes con contrato mensual presentan mayor tasa de churn.

2️⃣ Antigüedad

Los primeros 12 meses son críticos.

3️⃣ Servicios adicionales

Servicios como OnlineSecurity y TechSupport reducen churn.

4️⃣ Perfil crítico

Contrato mensual + alto cargo mensual + baja antigüedad = mayor riesgo.

🏁 Conclusiones Estratégicas
Acciones prioritarias

Score mensual de riesgo para clientes mensuales.

Migración a contrato anual con incentivos.

Programa de onboarding estructurado.

Bundle gratuito de servicios adicionales.

Revisión de pricing/experiencia en segmentos críticos.

🎯 Recomendación Final

El modelo permite transformar el churn en un problema preventivo.

Se recomienda su uso como sistema de alerta temprana, ejecutado periódicamente para activar campañas de retención antes de que el cliente cancele.

▶ Instrucciones de Ejecución
Requisitos
pip install pandas numpy matplotlib seaborn scikit-learn scipy
Versiones recomendadas

Python 3.9+

pandas 1.5+

scikit-learn 1.2+

Pasos
git clone https://github.com/tu-usuario/telecomx-churn.git
cd telecomx-churn
jupyter notebook TelecomX_Churn.ipynb

Ejecutar:

Kernel → Restart & Run All

⚠️ RandomizedSearchCV puede tardar algunos minutos.

🛠 Tecnologías Utilizadas

Python

Pandas / NumPy

Matplotlib / Seaborn

Scikit-learn

SciPy

Jupyter Notebook

Git / GitHub

Challenge Telecom X Parte 2 — Alura Latam · Ciencia de Datos
