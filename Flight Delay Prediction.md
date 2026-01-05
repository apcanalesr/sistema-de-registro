**✈️ Flight Delay Prediction – Baseline Model**

Sistema de predicción de retrasos aéreos utilizando **Machine Learning** y un pipeline productivo basado en **Regresión Logística**.

-----
**📌 Objetivo del proyecto**

Predecir si un vuelo se retrasará más de **15 minutos** al momento de llegada utilizando únicamente información disponible **antes del despegue**.

is\_delayed = 1  si ArrDelay > 15 minutos  

is\_delayed = 0  en caso contrario

Esto permite anticipar retrasos y optimizar la planificación operacional.

**🔍 Dataset**

Se utiliza el dataset:

**Sample\_DelayedFlights.csv**

Obtenido desde GitHub.

Se filtran vuelos cancelados y se conserva solo información disponible **antes del vuelo**.

-----
**🎯 Variable Objetivo**

Se define:

is\_delayed = 1 if ArrDelay > 15 else 0

Esto transforma el problema en uno de **clasificación binaria supervisada**.

-----

**🧠 Caracteristicas utilizadas (MVP)**

|**Tipo**|**Variables**|
| :-: | :-: |
|Categóricas|UniqueCarrier, Origin, Dest, DayOfWeek|
|Numéricas|CRSDepTime → dep\_hour, Distance|

Feature derivada:

dep\_hour = CRSDepTime // 100

-----
**⚙️ Pipeline de Preprocesamiento**

Implementado con ColumnTransformer:

|**Tipo de variable**|**Transformación**|
| :-: | :-: |
|Categóricas|OneHotEncoder|
|Numéricas|Passthrough|

Esto genera una matriz **sparse y de alta dimensionalidad**, ideal para regresión logística.

-----
**🤖 Modelo utilizado – Regresión Logística**

Pipeline([

`    `("preprocessor", preprocessor),

`    `("classifier", LogisticRegression(max\_iter=1000))

])

-----
**📌 ¿Por qué se eligió Regresión Logística?**

**1️	El problema es de clasificación binaria**

La regresión logística modela directamente la probabilidad:

P (is\_delayed=1∣X)P(is\\_delayed = 1 \mid X)P(is\_delayed=1∣X) 

-----
**2️	Alta interpretabilidad**

Permite responder preguntas clave:

|**Feature**|**Impacto**|
| :-: | :-: |
|Aerolínea|¿Cuáles se retrasan más?|
|Aeropuerto|¿Qué destinos generan más riesgo?|
|Hora del día|¿Qué franjas horarias son críticas?|

Los coeficientes pueden interpretarse como:

Incremento o disminución en la probabilidad de retraso.

-----
**3️	Excelente rendimiento con One-Hot Encoding**

El modelo:

- Tolera alta dimensionalidad.
- Funciona óptimamente con matrices dispersas.
- Es el estándar para features categóricas transformadas.
-----
**4️	Modelo baseline productivo ideal**

|**Criterio**|**Regresión Logística**|
| :-: | :-: |
|Velocidad|⚡ Muy alta|
|Memoria|🧠 Muy eficiente|
|Interpretabilidad|🔍 Muy alta|
|Overfitting|🛡️ Bajo|
|Mantenimiento|🛠️ Simple|
|Integración API|🚀 Directa|

-----
**5️	Predicción probabilística**

model.predict\_proba(X)

Esto permite:

- Definir umbrales dinámicos.
- Crear sistemas de alerta temprana.
- Implementar reglas de negocio basadas en riesgo.
-----
**6️	Preparado para escalar**

El pipeline permite fácilmente cambiar el modelo por:

- Random Forest
- Gradient Boosting
- XGBoost / LightGBM

sin modificar el preprocesamiento.

-----
**📈 Evaluación**

Se utiliza:

classification\_report(y\_test, y\_pred)

con división estratificada 80/20.

-----
**💾 Exportación del modelo**

El pipeline completo se guarda en:

ds/artifacts/model.joblib

Listo para ser consumido por un API (ej: FastAPI).

-----
**🏁 Conclusión**

La Regresión Logística se seleccionó por ser el mejor compromiso entre rendimiento, interpretabilidad, facilidad de mantenimiento y robustez productiva, estableciendo un baseline sólido y confiable para el sistema de predicción de retrasos aéreos.

