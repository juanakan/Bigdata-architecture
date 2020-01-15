# Practica Bigdata-architecture


### Idea general.
. buscar los 10 mejores restaurantes Japoneses de Madrid para recomendar los airbnb mas cercanos.

### Nombre del producto.
. Recomendador de airbnb Turismo Japones (los restaurantes Japoneses mejor valorados de Madrid)

### Estrategia del DAaas.
. Voy a utilizar herramientas en la nube para realizar un reporte que puedan utilizar paginas de viajes con los 50 airbnb mas cercanos a dichos restaurantes

### Arquitectura.
Arquitectura Cloud basada en Google Cloud Storage + HIVE + Dataproc
Recoger datos de la pagina el tenedor y crear CSV.
Tanto el CSV del tenedor como el dataset de airbnb los colocare en un segmento de
Google Cloud.
Desde Google Storage cogere los datos para crear 2 tablas de HIVE, y realizare
una query con un JOIN que reste las distancias entre cada airbnb y los 10 mejores restaurantes Japoneses.
De la query obtendre el TOP de apartamentos de airbnb de menos de 100 euros la noche.
El resultado de la query lo metere en Google Storage.

### Operating model
Hay un operador que soy yo, voy a recoger los datos diariamente de la pagina el tenedor y guardare el resultado en un directorio del segmento llamado "input_tenedor".
En el segmento siempre habra un directorio llamado "input_airbnb".
Seguire el standard de levantar el Cluster solamente cuando quiera regenerar el TOP.
Una vez al dia, levantare el CLUSTER a mano, enviare las tareas de:

. crear tabla de airbnb  
. crear tabla del Tenedor  
. LOAD DATA INPATH 'gs://practica-big-data-arquitectura/input_tenedor.csv' INTO TABLE tenedor;  
. LOAD DATA INPATH 'gs://practica-big-data-arquitectura/input_airbnb.csv' INTO TABLE airbnb;  
. SELECT JOIN INTO DIRECTORY 'gs://output/results'  


. Apago el cluster y me voy a dormir.  
. Voy a montar una web con un link directo al Google Storage Segment Object.  

### Desarrollo

##### Creación del Storage con los datos de airbnb y del tenedor:
![Pantallazo del Storage](https://github.com/juanakan/Bigdata-architecture/blob/master/google%20storage.PNG)

##### Creación de los cluster:
![Pantallazo de los cluster](https://github.com/juanakan/Bigdata-architecture/blob/master/cluster%20hadoop.PNG)

##### Query de Hive:
INSERT OVERWRITE DIRECTORY 'gs://practica-big-data-arquitectura/output/result' ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' select airbnb.Listing_Url, tenedor.nombre from airbnb left join tenedor on airbnb.City = tenedor.ciudad where airbnb.geolocation is not null and tenedor.nombre is not null limit 50;
![Pantallazo de Hive](https://github.com/juanakan/Bigdata-architecture/blob/master/select%20hive.PNG)

##### Resultado final:
[Enlace al archivo creado](https://storage.cloud.google.com/practica-big-data-arquitectura/output/result/000000_0?hl=es)
![Carpeta resultado](https://github.com/juanakan/Bigdata-architecture/blob/master/creando%20el%20output.PNG)



### Diagrama
![Diagrama](https://github.com/juanakan/Bigdata-architecture/blob/master/Diagrama.png)




