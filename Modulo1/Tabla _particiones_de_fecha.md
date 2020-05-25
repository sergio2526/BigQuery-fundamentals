# Tablas particionadas por fecha 



Esta consulta procesa **1.7 GB**, recorre toda nuestra base de datos buscando los datos que se cumplen según nuestra condición; en este caso estamos buscando todos los registros distintos durante la fecha **2018-07-08.**



```sql
SELECT DISTINCT
   fullVisitorId,
   date,
   city,
   pageTitle
FROM `data-to-insights.ecommerce.all_sessions_raw`
WHERE data = '20180708'
LIMIT 5
```

```sql
#result
This query will process 1.7 GB when run.
```



### ¿ Pero realmente que fue lo que paso ? 



Estos resultados para nosotros significa **"dinero"** estamos recorriendo toda la base de datos sin necesidad, ya que solo para algunos registros se cumple la condición.

```sql
WHERE data = '20180708'
```

El tiempo de ejecución es mayor y estamos consumiendo mas recursos sin necesidad alguna.



#### Solución

Debemos evitar este tipo de problemas, y esto lo haremos por medio de una partición de nuestra tabla, en este caso será por la fecha.

```sql
#standardSQL
 CREATE OR REPLACE TABLE ecommerce.partition_by_day
 PARTITION BY date_formatted
 OPTIONS(
   description="a table partitioned by date"
 ) AS

 SELECT DISTINCT
 PARSE_DATE("%Y%m%d", date) AS date_formatted,
 fullvisitorId
 FROM `data-to-insights.ecommerce.all_sessions_raw`
```



ahora observamos los resultados con la partición de nuestra tabla



```sql
#standardSQL
SELECT *
FROM `data-to-insights.ecommerce.partition_by_day`
WHERE date_formatted = '2018-07-08'
```

```sql
#result
This query will process 0 B when run
```



Nuestro proceso tiene un resultado de **0 B** por que en este caso no hay registros con respecto a la fecha asignada, no se recorrió registro por registro como lo teníamos en el resultado anterior.



##### Conclusión 

Estas particiones nos ayudan a mejorar el rendimiento y el costo de nuestras consultas.







**¡enhorabuena!** :smiley: aprendiste el particionamiento de tablas y saber cuales son sus beneficios en **BigQuey**

   :+1:  #PuntosPorEsfuerzo 