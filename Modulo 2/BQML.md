### Temas que se van abordar

- Crear un modelo de regresión logística binaria
- Evaluar el modelo
- Usar el modelo para hacer predicciones



Haga clic en **Editor de consultas** y agregue esta consulta para crear un modelo que prediga si un visitante hará una transacción:



```sql
#standardSQL
CREATE OR REPLACE MODEL `bqml_lab.sample_model`
OPTIONS(model_type='logistic_reg') AS
SELECT
  IF(totals.transactions IS NULL, 0, 1) AS label,
  IFNULL(device.operatingSystem, "") AS os,
  device.isMobile AS is_mobile,
  IFNULL(geoNetwork.country, "") AS country,
  IFNULL(totals.pageviews, 0) AS pageviews
FROM
  `bigquery-public-data.google_analytics_sample.ga_sessions_*`
WHERE
  _TABLE_SUFFIX BETWEEN '20160801' AND '20170631'
LIMIT 100000;
```



Aquí se utiliza el sistema operativo del dispositivo del visitante (si dicho dispositivo es un dispositivo móvil), el país del visitante y el número de páginas vistas como criterio para determinar si se realizó una transacción.

En este caso, `bqml_lab` es el nombre del conjunto de datos y `sample_model` es el nombre del modelo. El tipo de modelo especificado es **regresión logística binaria**. En este caso, `label` es lo que está intentado ajustar.



Los datos de prueba se limitan a los recolectados entre el 1 de agosto de 2016 y el 30 de junio de 2017. Esto se hace a fin de guardar el último mes de datos para la "predicción". Se limita, además, a 100,000 datos para ahorrar tiempo.

La ejecución del comando `CREATE MODEL` crea un trabajo de consulta que se ejecutará de manera asíncrona para que pueda, por ejemplo, cerrar o actualizar la ventana de la IU de **BigQuery**.



**Salida :** 

![](https://res.cloudinary.com/xaiop/image/upload/c_scale,w_397/v1591583306/Modulo2/a_q3anw5.png)





![](https://res.cloudinary.com/xaiop/image/upload/c_scale,w_394/v1591583305/Modulo2/b_kq3rf1.png)



Podemos observar distintos atributos de nuestro modelo, como la **perdida** que se tubo durante el entrenamiento, y también su aprendizaje.





### Evalúe el modelo



Ahora, reemplace la consulta con lo siguiente:

```sql
#standardSQL
SELECT
*
FROM
  ml.EVALUATE(MODEL `bqml_lab.sample_model`, (
SELECT
  IF(totals.transactions IS NULL, 0, 1) AS label,
  IFNULL(device.operatingSystem, "") AS os,
  device.isMobile AS is_mobile,
  IFNULL(geoNetwork.country, "") AS country,
  IFNULL(totals.pageviews, 0) AS pageviews
FROM
  `bigquery-public-data.google_analytics_sample.ga_sessions_*`
WHERE
  _TABLE_SUFFIX BETWEEN '20170701' AND '20170801'));
```



Si la utiliza con un modelo de **regresión logística**, la consulta anterior muestra las siguientes columnas:

- `precision`, `recall`
- `accuracy`, `f1_score`
- `log_loss`, `roc_auc`

Si deseas saber que significan dichas variables, te dejo el glosario de google [https://developers.google.com/machine-learning/glossary]()



**Salida:**

![](https://res.cloudinary.com/xaiop/image/upload/c_scale,w_608/v1591583769/Modulo2/F4xj1Rol9xUq416nkDE_aruyNryT1P9vhfZLpRbXWxI_mt3frg.png)





### Use el modelo

##### **Prediga compras por país**

Con esta consulta, intentará predecir el número de transacciones realizadas por visitantes de cada país, ordenar los resultados y seleccionar los 10 países que realizaron más compras:



```sql
#standardSQL
SELECT
  country,
  SUM(predicted_label) as total_predicted_purchases
FROM
  ml.PREDICT(MODEL `bqml_lab.sample_model`, (
SELECT
  IFNULL(device.operatingSystem, "") AS os,
  device.isMobile AS is_mobile,
  IFNULL(totals.pageviews, 0) AS pageviews,
  IFNULL(geoNetwork.country, "") AS country
FROM
  `bigquery-public-data.google_analytics_sample.ga_sessions_*`
WHERE
  _TABLE_SUFFIX BETWEEN '20170701' AND '20170801'))
GROUP BY country
ORDER BY total_predicted_purchases DESC
LIMIT 10;
```



Esta consulta es muy parecida a la consulta de evaluación demostrada en la sección anterior. En lugar de `ml.EVALUATE`, utiliza `ml.PREDICT` y la parte BQML de la consulta se une con comandos de SQL estándar. En este lab, le interesan el país y la suma de compras por país, por lo que se utilizan `SELECT`, `GROUP BY` y `ORDER BY`. `LIMIT` para garantizar que obtenga solo los 10 resultados principales.

Debería ver una tabla similar a la siguiente:



**Salida:**

![](https://res.cloudinary.com/xaiop/image/upload/v1591583999/Modulo2/87OvTSvXnVFthzil2BzMoTlTz8KUtWYezliUJhZ1m6s_pvzj0z.png)



##### **Prediga compras por usuario**

A continuación, se muestra otro ejemplo. Esta vez, intentará predecir el número de transacciones realizada por cada visitante, ordenar los resultados y seleccionar los 10 visitantes que más transacciones realizan:



```sql
#standardSQL
SELECT
  fullVisitorId,
  SUM(predicted_label) as total_predicted_purchases
FROM
  ml.PREDICT(MODEL `bqml_lab.sample_model`, (
SELECT
  IFNULL(device.operatingSystem, "") AS os,
  device.isMobile AS is_mobile,
  IFNULL(totals.pageviews, 0) AS pageviews,
  IFNULL(geoNetwork.country, "") AS country,
  fullVisitorId
FROM
  `bigquery-public-data.google_analytics_sample.ga_sessions_*`
WHERE
  _TABLE_SUFFIX BETWEEN '20170701' AND '20170801'))
GROUP BY fullVisitorId
ORDER BY total_predicted_purchases DESC
LIMIT 10;
```



**Salida:**



![](https://res.cloudinary.com/xaiop/image/upload/v1591584088/Modulo2/zk0mE_w87K3wW2_ZJSrb0b9xoKiLSgnLe6gVtugr1I_boepeh.png)



**¡enhorabuena!** 😃 aprendiste sobre regresión logística y como clasificar por usuario y país en **BigQuey** **Machine Learning**

👍 #PuntosPorEsfuerzo