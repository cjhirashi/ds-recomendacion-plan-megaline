<div align="center">

# 📡 Sprint 9 — Clasificación de Planes Megaline

**Modelo de Machine Learning para recomendar plan *Smart* (0) u *Ultra* (1)**

[![Python](https://img.shields.io/badge/Python-3.11-3776AB?logo=python\&logoColor=white)](https://www.python.org/)
[![Pandas](https://img.shields.io/badge/Pandas-Data%20Analysis-150458?logo=pandas\&logoColor=white)](https://pandas.pydata.org/)
[![Scikit--learn](https://img.shields.io/badge/scikit--learn-Modeling-F7931E?logo=scikitlearn\&logoColor=white)](https://scikit-learn.org/stable/)
[![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626?logo=jupyter\&logoColor=white)](https://jupyter.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-2ea44f)](#-licencia)

</div>

---

## 🚀 Descripción

La empresa **Megaline** desea migrar a sus clientes desde planes heredados hacia los nuevos **Smart** y **Ultra**. Con datos de comportamiento mensual de usuarios, construiremos un **modelo de clasificación** que recomiende el plan adecuado, cumpliendo un **umbral de exactitud ≥ 0.75**.

**Dataset:** `/datasets/users_behavior.csv`

**Columnas:** `calls`, `minutes`, `messages`, `mb_used`, `is_ultra` (1 = Ultra, 0 = Smart)

---

## 🧩 Objetivos del proyecto

* Preparar datos y particionar en **train / validation / test**
* Entrenar y comparar **árbol de decisión**, **bosque aleatorio** y **regresión logística**
* Optimizar hiperparámetros (búsqueda en rejilla / validación cruzada)
* **Evaluar** en el conjunto de prueba y documentar hallazgos
* Ejecutar **prueba de cordura** (sanity check) para validar que el modelo aprende señal real

---

## 🧰 Tecnologías utilizadas

* Python · Pandas · NumPy · Scikit‑learn · Matplotlib/Seaborn · Jupyter


---

## 🌐 Ver ejemplo en vivo

* Notebook principal: `notebooks/sprint9_megaline.ipynb` (por generar)
* Reporte HTML exportado: `reports/sprint9_megaline.html` (opcional)

---

## ⚙️ Instalación

```bash
# 1) Clonar
git clone https://github.com/cjhirashi/proyecto-sprint-9.git
cd proyecto-sprint-9

# 2) Crear entorno con pipenv
pip install pipenv
pipenv install
pipenv shell
```

**Estructura sugerida**

```
.
├── datasets/
│   └── users_behavior.csv
├── notebooks/
│   └── sprint9_megaline.ipynb
├── reports/
│   └── sprint9_megaline.html
├── src/
│   ├── data.py           # carga y partición de datos
│   ├── models.py         # definiciones y búsquedas de hiperparámetros
│   └── evaluation.py     # métricas, curvas y utilidades
├── requirements.txt
└── README.md
```

---

## 📦 Datos

Cada fila representa el comportamiento **mensual** de un usuario.

* `calls`: número de llamadas
* `minutes`: duración total (min)
* `messages`: SMS enviados
* `mb_used`: tráfico de Internet (MB)
* `is_ultra`: etiqueta objetivo (1 = Ultra, 0 = Smart)

**Nota:** Verifica valores atípicos y correlación `calls ↔ minutes` para evitar colinealidad fuerte al usar modelos lineales.

---

## 🔬 Metodología

1. **Carga y verificación**: tipos, nulos, rangos plausibles, estadísticos básicos.
2. **Partición estratificada**: por `is_ultra` (estratificación) en `train/valid/test` (70/15/15 o 60/20/20).
3. **Escalado** (si procede): `StandardScaler` para modelos lineales / distancia.
4. **Modelos**:

   * `DecisionTreeClassifier`: `max_depth`, `min_samples_split`, `min_samples_leaf`, `criterion`
   * `RandomForestClassifier`: `n_estimators`, `max_depth`, `max_features`, `min_samples_*`
   * `LogisticRegression`: `penalty`, `C`, `solver`, `class_weight`
5. **Selección de hiperparámetros**: `GridSearchCV` (o `RandomizedSearchCV`), validación cruzada estratificada.
6. **Métricas**: `accuracy` (principal), además `precision`, `recall`, `f1`, matriz de confusión.
7. **Evaluación final**: re‑entrenar el mejor modelo en `train+valid` y probar en `test`.
8. **Prueba de cordura**: comparar contra **baseline** (mayoría) y contra datos con **etiquetas barajadas**.

---

## 🧪 Bitácora de experimentos (ejemplo)

|  ID | Modelo        | Preproceso     | Hiperparámetros                   | Acc. (val) | Acc. (test) | Notas                       |
| --: | ------------- | -------------- | --------------------------------- | ---------: | ----------: | --------------------------- |
| E01 | MajorityClass | —              | —                                 |       0.50 |        0.50 | Baseline estratificado      |
| E02 | DecisionTree  | —              | depth=5, min\_samples\_leaf=5     |       0.74 |        0.73 | Sobreajuste leve si depth>8 |
| E03 | RandomForest  | —              | n=300, depth=None, max\_feat=sqrt |   **0.79** |    **0.78** | Mejor equilibrio            |
| E04 | LogisticReg   | StandardScaler | C=1.0, penalty=l2, solver=lbfgs   |       0.76 |        0.75 | Estable y explicable        |

> **Objetivo:** alcanzar **accuracy ≥ 0.75** en *test*.

---

## 📈 Métricas y visualizaciones

* **Matriz de confusión** y **clasification\_report**
* Curvas **ROC** (opcional, si se ajusta a umbrales)
* Importancias de características (árbol/bosque) o coeficientes (LR)

---

## ✅ Prueba de cordura (sanity check)

1. **Baseline de clase mayoritaria**: entrenar un clasificador que siempre prediga la clase más frecuente y registrar su `accuracy`.
2. **Etiquetas barajadas**: barajar `is_ultra` en `train` y re‑entrenar. Si el rendimiento ≈ baseline, el modelo original **sí** estaba aprendiendo señal real.
3. **Data leakage**: confirmar que no se usan variables derivadas del objetivo ni información de valid/test durante el entrenamiento.

---

## 🧪 Código de referencia (snippet)

```python
import pandas as pd
from sklearn.model_selection import train_test_split, GridSearchCV
from sklearn.metrics import accuracy_score, classification_report, ConfusionMatrixDisplay
from sklearn.tree import DecisionTreeClassifier
from sklearn.ensemble import RandomForestClassifier
from sklearn.linear_model import LogisticRegression

# 1) Carga
df = pd.read_csv('datasets/users_behavior.csv')
X = df[['calls','minutes','messages','mb_used']]
y = df['is_ultra']

# 2) Split estratificado
X_train, X_temp, y_train, y_temp = train_test_split(
    X, y, test_size=0.30, random_state=42, stratify=y
)
X_valid, X_test, y_valid, y_test = train_test_split(
    X_temp, y_temp, test_size=0.50, random_state=42, stratify=y_temp
)

# 3) Árbol (búsqueda básica)
param_grid = {
    'max_depth': [3,5,7,9,None],
    'min_samples_leaf': [1,3,5,10],
    'criterion': ['gini','entropy']
}
clf = GridSearchCV(DecisionTreeClassifier(random_state=42), param_grid, cv=5, n_jobs=-1)
clf.fit(X_train, y_train)
print('Mejor árbol:', clf.best_params_)

# 4) Evaluación en valid y test
y_pred_valid = clf.predict(X_valid)
y_pred_test  = clf.predict(X_test)
print('ACC valid:', accuracy_score(y_valid, y_pred_valid))
print('ACC test :', accuracy_score(y_test,  y_pred_test))
print(classification_report(y_test, y_pred_test))
```

---

## 📝 Entregables

* Notebook con el flujo completo: **EDA → Partición → Modelado → Optimización → Evaluación → Sanity check**
* README (este archivo) con resultados y recomendaciones
* (Opcional) Reporte HTML/PDF con tablas y gráficas clave

---

## 👤 Autor

**Carlos Jiménez Hirashi**
💼 Data Scientist Jr. | Python & Machine Learning
📧 [cjhirashi@gmail.com](mailto:cjhirashi@gmail.com) · 🌐 LinkedIn: [https://www.linkedin.com/in/cjhirashi](https://www.linkedin.com/in/cjhirashi) · 🔗 Portafolio: [https://cjhirashi.github.io](https://cjhirashi.github.io) · 💻 GitHub: [https://github.com/cjhirashi](https://github.com/cjhirashi)


