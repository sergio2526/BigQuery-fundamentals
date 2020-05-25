# Linea de comandos

Antes de empezar con esta clase, debemos configurar nuestro entorno de trabajo, lo puedes hacer de las siguientes formas:

1. **SDK de GCP**
2. **Cloud Shell**

Para este ejercicio practico vamos a utilizar **Cloud Shell**, igualmente si escoges por medio del **SDK** te comparto un enlace donde puedes observar como hacer la respectiva configuración.

Install Cloud SDK

https://cloud.google.com/sdk/install



## **¡ Empecemos !**

Dirígete a la consola de tu cuenta, y das clic en la **cloud Shell**

![](https://res.cloudinary.com/xaiop/image/upload/c_scale,h_120,w_600/v1590362397/Modulo1/vdY5e_an9ZGXw5a_ZMb1agpXhRGozsOadHURcR8thAQ_cdlqrr.png)

 

Una vez estés ahí, se te va a desplegar la consola, donde puedes observar el Id de tu proyecto



![](https://res.cloudinary.com/xaiop/image/upload/c_scale,h_120,w_600/v1590362397/Modulo1/hmMK0W41Txk_20bQyuDP9g60vCdBajIS_52iI2f4bYk_nvuc9m.png)



por defecto tu proyecto debería estar iniciado, si no es así, entonces ejecute la siguiente linea con el Id de tu proyecto.

`gcloud config set project ID_de_tu_proyecto`



## Crear una tabla



Vamos a crear nuestro conjunto de datos, donde vamos almacenar nuestras tablas, para ello ejecuta la siguiente linea:

```
bq mk bebes
```

denomine al conjunto de datos "bebes" por que en este caso vamos a trabajar con una base de datos con registros de bebes.

Antes de poder crear la tabla, debe agregar el conjunto de datos a su proyecto.

```sql
wget http://www.ssa.gov/OACT/babynames/names.zip
```

Ahora lo vamos a descomprimir

```
unzip names.zip
```

**salida:**

![](https://res.cloudinary.com/xaiop/image/upload/v1590369609/Modulo1/fechas_xle140.png)

Contamos con registros desde los años **1880** hasta **2018**



**Creamos nuestra tabla**

```sql
bq load bebes.names2018 yob2018.txt
name:string,gender:string,count:integer
```

Por preferencia cree la tabla con los datos del año 2018



Ejecute `bq ls bebes`para confirmar que la tabla ahora tiene el conjunto de datos:

**salida:**

![](https://res.cloudinary.com/xaiop/image/upload/c_scale,h_60,w_600/v1590370072/Modulo1/table-creada_zoeqln.png)



Ahora observamos la estructura de nuestra tabla:



![](https://res.cloudinary.com/xaiop/image/upload/c_scale,h_80,w_600/v1590370408/Modulo1/estructura_vqrzwj.png)



Podemos deducir que nuestra tabla tiene **32.033** registros, sus atributos son **name, gender, count**.



## Ejecutar consultas

**Genial!** ahora ya estamos listos para generar consultas en nuestro conjunto de datos



Vamos a mostrar el top 5 de los nombres femeninos mas populares durante el periodo 2018

```sql
bq query "SELECT name,count FROM bebes.names2018 WHERE = 'F'
          ORDER BY count DESC LIMIT 5"
```

**Salida**



![](https://res.cloudinary.com/xaiop/image/upload/c_scale,h_207,w_220/v1590370741/Modulo1/resultado_wv4b8u.png)





**¡enhorabuena!** :smiley: aprendiste a crear una tabla y ejecutar consultas por linea de comandos en **BigQuey**

   :+1:  #PuntosPorEsfuerzo 



