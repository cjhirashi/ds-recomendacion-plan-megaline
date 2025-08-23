# 📱 Proyecto Sprint 9 - Clasificación de Planes Megaline

[![Python](https://img.shields.io/badge/Python-3.x-blue?logo=python)](https://www.python.org/)
[![Pandas](https://img.shields.io/badge/Pandas-Data%20Analysis-green?logo=pandas)](https://pandas.pydata.org/)
[![Scikit-Learn](https://img.shields.io/badge/Scikit--Learn-ML%20Models-orange?logo=scikitlearn)](https://scikit-learn.org/stable/)
[![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange?logo=jupyter)](https://jupyter.org/)

---

## 🚀 Descripción

Proyecto del **Sprint 9** en **TripleTen**. Desarrollo de un modelo de **Machine Learning** para clasificar clientes de Megaline y recomendar el plan óptimo (**Smart** o **Ultra**). El modelo debe alcanzar una exactitud mínima de **0.75**.

---

## ✨ Objetivos principales

* Analizar y preparar los datos de comportamiento de usuarios.
* Entrenar diferentes modelos de clasificación.
* Ajustar hiperparámetros para optimizar el rendimiento.
* Evaluar la calidad del modelo en conjunto de prueba.
* Realizar prueba de cordura para verificar consistencia.

---

## 🧰 Tecnologías utilizadas

* [Python 3](https://www.python.org/)
* [Pandas](https://pandas.pydata.org/)
* [NumPy](https://numpy.org/)
* [Scikit-Learn](https://scikit-learn.org/stable/)
* [Jupyter Notebook](https://jupyter.org/)
* [Conda](https://docs.conda.io/) – gestión de entornos y dependencias

---

## ✅ PASOS DEL PROYECTO

1. **Carga y revisión inicial de datos**  
   - Vista de columnas, tipos, valores faltantes y estadísticos descriptivos.  
   - Revisión del balance de la variable objetivo `is_ultra` (0 = Smart, 1 = Ultra).

2. **Partición estratificada (train/valid/test)**  
   - División sugerida: `60% / 20% / 20%`.  
   - Uso de `random_state` para asegurar reproducibilidad.  
   - Separación entre `features` y `target`.

3. **Preparación mínima de datos**  
   - Conversión de tipos de datos si es necesario.  
   - Escalado de características (solo si aplica, por ejemplo en `LogisticRegression`).  
   - Revisión de outliers o anomalías.

4. **Prueba de cordura (baseline)**  
   - Entrenar un **DummyClassifier** como modelo base.  
   - Establecer un `accuracy` mínimo de referencia.

5. **Entrenamiento y ajuste de modelos**  
   - **DecisionTreeClassifier**: búsqueda de `max_depth`, `criterion`, `min_samples_split`, etc.  
   - **RandomForestClassifier**: variación de `n_estimators`, `max_depth`, `max_features`, etc.  
   - **LogisticRegression**: prueba de distintos `solver`, `C`, `penalty`, `max_iter`.  
   - Registro de resultados en validación.

6. **Selección del mejor modelo (validación)**  
   - Comparación de `accuracy` en *valid*.  
   - Selección del modelo con mejor equilibrio entre rendimiento y simplicidad.

7. **Evaluación final en test**  
   - Medición del `accuracy` en el conjunto de prueba.  
   - Reporte adicional: matriz de confusión, `classification_report` (precisión, recall, f1-score).  
   - Confirmar si se supera el umbral de **0.75**.

8. **Conclusiones y recomendaciones**  
   - Identificación del mejor modelo entrenado.  
   - Observaciones sobre limitaciones (posible sobreajuste o desbalance).  
   - Próximos pasos sugeridos: más datos, validación cruzada, ingeniería de características.


---

## ⚙️ Estructura del proyecto

```bash
.
├── datasets/
│   └── users_behavior.csv     # Dataset principal
├── notebooks/
│   └── sprint9_analysis.ipynb # Notebook con el análisis y modelos
├── environment.yml            # Archivo para recrear el entorno con conda
└── README.md
```

---

## 📑 Dataset

**Archivo:** `/datasets/users_behavior.csv`

**Columnas principales:**

* `calls` → Número de llamadas
* `minutes` → Duración total de llamadas en minutos
* `messages` → Número de SMS enviados
* `mb_used` → Tráfico de internet usado en MB
* `is_ultra` → Plan actual (Ultra = 1, Smart = 0)

---

## ▶️ Instalación y uso

1. **Clonar repositorio**

   ```bash
   git clone https://github.com/cjhirashi/proyecto-sprint-9.git
   cd proyecto-sprint-9
   ```

2. **Crear entorno con Conda**

   ```bash
   conda env create -f environment.yml
   conda activate tp-sprint-9
   ```

3. **Ejecutar Notebook**

   ```bash
   jupyter notebook notebooks/sprint9_analysis.ipynb
   ```

---

## ♻️ Gestión del entorno con Conda

* **Eliminar entorno** (para liberar espacio):

  ```bash
  conda env remove -n tp-sprint-9
  ```

* **Recrear entorno** (a partir del archivo `environment.yml`):

  ```bash
  conda env create -f environment.yml
  conda activate tp-sprint-9
  ```

Esto asegura que el proyecto se pueda reabrir en cualquier máquina con las versiones correctas de Python y librerías.

---

## 👨‍💻 Autor

**Carlos Jiménez Hirashi**
💼 Data Scientist Jr. | Python & Machine Learning
📧 [cjhirashi@gmail.com](mailto:cjhirashi@gmail.com) · 🌐 [LinkedIn](https://www.linkedin.com/in/cjhirashi)
