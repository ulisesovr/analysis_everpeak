# Análisis de Comportamiento de Usuarios de Telecomunicaciones

## 🎯 Objetivo del proyecto

Analizar el comportamiento de uso (llamadas, mensajes y minutos de llamada) de una base de usuarios de un servicio de telecomunicaciones en Latinoamérica, con el fin de:

- Limpiar y consolidar la información proveniente de distintas fuentes de datos.
- Identificar patrones de uso, valores atípicos y su relación con el tipo de plan contratado (Básico vs. Premium).
- Segmentar a los usuarios por edad y nivel de uso.
- Generar insights accionables para la toma de decisiones de negocio (marketing, retención y estrategias de upsell).

## 📂 Datasets utilizados

| Archivo           | Descripción                                                                                               |
| ----------------- | --------------------------------------------------------------------------------------------------------- |
| `plans.csv`       | Información sobre los planes disponibles (Básico y Premium).                                              |
| `users_latam.csv` | Datos demográficos de los usuarios: `user_id`, `age`, `city`, `plan`, `reg_date`, `churn_date`.           |
| `usage.csv`       | Registro de actividad por usuario: llamadas y mensajes, incluyendo `type`, `duration`, `length` y `date`. |

## 🔍 Etapas del análisis

1. **Carga y exploración inicial de los datos**
   Revisión de tipos de datos, dimensiones y estructura de cada dataset.

2. **Limpieza de datos**
   2. Detección y corrección de valores centinela (`-999` en `age`) mediante recodificación a `NaN` e imputación por mediana.
   3. Estandarización de valores faltantes representados como texto (`"?"`, `"NA"`) a `NaN` real.
   4. Conversión de columnas de fecha a formato `datetime` y corrección de fechas inválidas (fuera de rango esperado).
   5. Corrección de inconsistencias estructurales en `usage.csv` (valores de `duration` en registros de tipo `text` y viceversa).

3. **Transformación y agregación**
   2. Creación de columnas auxiliares (`is_text`, `is_call`) para contabilizar mensajes y llamadas.
   3. Agregación de `usage.csv` por `user_id` (total de mensajes, llamadas y minutos de llamada).
   4. Combinación (`merge`) de la tabla agregada con `users_latam.csv` para construir el DataFrame `user_profile`.

4. **Detección de valores atípicos (outliers)**
   2. Visualización mediante boxplots de las variables numéricas (`age`, `cant_mensajes`, `cant_llamadas`, `cant_minutos_llamada`).
   3. Cálculo de límites mediante el método IQR.
   4. Análisis cruzado de outliers contra la variable `plan`, para diferenciar entre errores de datos y comportamiento real de usuarios de alto consumo.

5. **Segmentación de usuarios**
   2. Creación de la columna `grupo_uso` (Bajo uso / Uso medio / Alto uso) según cantidad de llamadas y mensajes.
   3. Creación de la columna `grupo_edad` para análisis demográfico.

6. **Visualización y análisis exploratorio**
   2. Histogramas de `age`, `cant_mensajes`, `cant_llamadas` y `cant_minutos_llamada`, segmentados por `plan`.
   3. Gráficos de distribución por `grupo_uso` y `grupo_edad`.

7. **Generación de insights y recomendaciones**
   Interpretación de los hallazgos y traducción a recomendaciones orientadas a negocio (marketing, retención, upsell a Premium).

## ▶️ Cómo ejecutar el notebook

1. Clona este repositorio:
```bash
[git clone https://github.com/<tu_usuario>/<nombre_repo>.git](https://github.com/ulisesovr/sprint7-final-project)
cd <sprint7-final-project>
```

2. Instala las dependencias necesarias:
```bash
pip install pandas numpy matplotlib seaborn jupyter
```

3. Abre el notebook con Jupyter Notebook o JupyterLab:
```bash
jupyter notebook nombre_del_notebook.ipynb
```
   o bien:
```bash
jupyter lab nombre_del_notebook.ipynb
```

4. Asegúrate de que los archivos `plans.csv`, `users_latam.csv` y `usage.csv` estén ubicados en la misma carpeta que el notebook (o ajusta las rutas de carga según corresponda).

5. Ejecuta el notebook de principio a fin usando **Kernel → Restart Kernel and Run All Cells**, para garantizar que todas las celdas corran en el orden correcto y sin dependencias rotas.

## 🔁 Guía de reproducción

Para reproducir este análisis con tus propios datos:

1. Asegúrate de que tus archivos de origen sigan una estructura similar (identificador de usuario, tipo de plan, y registros de actividad con tipo, fecha y duración/longitud).
2. Ajusta los nombres de columnas en las primeras celdas del notebook si difieren de los usados aquí (`user_id`, `type`, `duration`, `length`, `plan`, `age`, `city`).
3. Revisa los umbrales de limpieza (por ejemplo, el valor centinela `-999`) y ajústalos según los valores que uses tu propio dataset para representar datos faltantes.
4. Los rangos usados para `grupo_uso` (Bajo/Medio/Alto) y `grupo_edad` pueden modificarse directamente en las celdas correspondientes según el criterio de negocio que necesites aplicar.
5. Todas las visualizaciones están construidas con `matplotlib` y `seaborn`; si agregas nuevas variables, puedes replicar el mismo patrón de bucles `for` usado para generar los boxplots e histogramas de forma automática.

## 🛠️ Tecnologías utilizadas

- Python 3.9
- pandas
- NumPy
- Matplotlib
- Seaborn
- Jupyter Notebook

## 👤 Autor

Ulises — Proyecto desarrollado como parte de la certificación de Data Analyst en TripleTen.
