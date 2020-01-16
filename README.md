# Práctica Bigdata-architecture


### Idea general.
Voy a buscar los 10 mejores restaurantes Japoneses de Madrid para recomendar los 50 airbnb más cercanos.

### Nombre del producto.
Recomendador de airbnb Turismo Japonés (los restaurantes Japoneses mejor valorados de Madrid y los airbnb más cercanos).

### Estrategia del DAaas.
Voy a usar herramientas en la nube para realizar un reporte que puedan utilizar páginas de viajes con los 50 airbnb más cercanos a dichos restaurantes.

### Arquitectura.
Arquitectura Cloud basada en Google Cloud Storage + HIVE + Dataproc
Recoger datos de la página "El tenedor" y crear CSV.
Tanto el CSV de "El tenedor" como el dataset de "airbnb" los colocaré en un segmento de
Google Cloud.
Desde Google Storage cogeré los datos para crear 2 tablas de HIVE, y realizaré
una query con un JOIN que busque los airbnb cerca de los mejores restaurantes Japoneses.
De la query obtendré los 50 apartamentos de "airbnb" más cercanos a los restaurantes.
El resultado de la query lo meteré en Google Storage.

### Operating model
Hay un operador, que soy yo, que va a recoger los datos diariamente de la página "El tenedor" y guardar el resultado en un directorio del segmento llamado "input_tenedor".
En el segmento siempre habrá un directorio llamado "input_airbnb".
Seguiré el standard de levantar el Cluster solamente cuando quiera regenerar el listado de los restaurantes y los airbnb.
Una vez al día levantaré el CLUSTER a mano y enviaré las tareas de:

. crear tabla de "airbnb";  
. crear tabla de "El Tenedor" 
. LOAD DATA INPATH 'gs://practica-big-data-arquitectura/input_tenedor.csv' INTO TABLE tenedor;  
. LOAD DATA INPATH 'gs://practica-big-data-arquitectura/input_airbnb.csv' INTO TABLE airbnb;  
. SELECT JOIN INTO DIRECTORY 'gs://output/results'  


. Apago el cluster y me voy a dormir.  
. Voy a montar una web con un link directo al Google Storage Segment Object.  

### Desarrollo

##### Creación del Storage con los datos de "airbnb" y de "El tenedor":
![Pantallazo del Storage](https://github.com/juanakan/Bigdata-architecture/blob/master/google%20storage.PNG)

##### Creación de los cluster:
![Pantallazo de los cluster](https://github.com/juanakan/Bigdata-architecture/blob/master/cluster%20hadoop.PNG)

##### Query de Hive:
INSERT OVERWRITE DIRECTORY 'gs://practica-big-data-arquitectura/output/result' ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' select airbnb.Listing_Url, tenedor.nombre from airbnb left join tenedor on airbnb.City = tenedor.ciudad where airbnb.Listing_Url is
 not null and tenedor.nombre is not null limit 50;
![Pantallazo de Hive](https://github.com/juanakan/Bigdata-architecture/blob/master/select.PNG)

##### Resultado final:
[Enlace al archivo creado](https://storage.cloud.google.com/practica-big-data-arquitectura/output/result/000000_0?hl=es)
![Carpeta resultado](https://github.com/juanakan/Bigdata-architecture/blob/master/creando%20el%20output.PNG)



### Diagrama
![Diagrama](https://github.com/juanakan/Bigdata-architecture/blob/master/Diagrama.png)




