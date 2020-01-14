# Practica Bigdata-architecture

### Idea general.
. buscar los 10 mejores restaurantes Japoneses de Madrid para recomendar los airbnb mas cercanos al turismo Japones.

### Nombre del producto.
. Recomendador de airbnb Turismo Japones (los restaurantes Japoneses mejor valorados de Madrid)

### Estrategia del DAaas.
. Voy a utilizar herramientas en la nube para realizar un reporte que puedan utilizar paginas de viajes con los 100 airbnb mas cercanos a dichos restaurantes
### Arquitectura.
Arquitectura Cloud basada en Scrapy + Google Cloud Storage + HIVE + Dataproc
Crawler con scrapy que lee de el tenedor, me escribe los resultados en csv. Y lo ejecuto
en un Cloud Function.
Insertar el dataset de airbnb en HIVE.
Tanto el resultado del scrap como el dataset de airbnb los colocare en un segmento de
Google Cloud.
Desde Google Storage cogere los datos para crear 2 tablas de HIVE, y realizare
una query con un JOIN que reste las distancias entre cada airbnb y los 10 mejores restaurantes Japoneses.
De la query obtendre el TOP de apartamentos de airbnb de menos de 100 euros la noche.
El resultado de la query estara en Google Storage

### Operating model
Hay un operador que soy yo, voy a disparar el cloud function todas las mananas con
mi Google Home (le dire: "Ok Google crawlea yelp"). Esto disparara el cloud function
y guardara el resultado en un directorio del segmento llamado "input_yelp".
En el segmento siempre habra un directorio llamado "input_airbnb".
Seguire el standard de levantar el Cluster solamente cuando quiera regenerar el TOP.
Una vez al dia, levantare el CLUSTER a mano, enviare las tareas de:

crear tabla de airbnb
crear tabla de yelp
load data inpath de gs://XXXX:input_yelp/ into table yelp
load data inpath de gs://XXXX:input_airbnb/ into table airbnb
SELECT JOIN INTO DIRECTORY 'gs://output/results'

Apago el cluster y me voy a dormir.
Voy a montar una web con un link directo al Google Storage Segment Object.

### Desarrollo
Tengo el cloud function aqui:
Query de HIVE
Pantallazo final

### Diagrama
[https://docs.google.com/drawings/d/1FJflTqXcQr2vmYA0-uEI04GQZUWMOU_00-u2jxAGdMQ/edit?usp=sharing]


