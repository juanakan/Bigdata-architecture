# Practica Bigdata-architecture

### Idea general.
. buscar los 10 mejores restaurantes Japoneses de Madrid para recomendar los airbnb mas cercanos.

### Nombre del producto.
. Recomendador de airbnb Turismo Japones (los restaurantes Japoneses mejor valorados de Madrid)

### Estrategia del DAaas.
. Voy a utilizar herramientas en la nube para realizar un reporte que puedan utilizar paginas de viajes con los 50 airbnb mas cercanos a dichos restaurantes

### Arquitectura.
Arquitectura Cloud basada en Google Cloud Storage + HIVE + Dataproc
Recoger datos de la pagina el tenedor y crear CSV. lo ejecuto
en un Cloud Function.
Insertar el dataset de airbnb en HIVE.
Tanto el CSV del tenedor como el dataset de airbnb los colocare en un segmento de
Google Cloud.
Desde Google Storage cogere los datos para crear 2 tablas de HIVE, y realizare
una query con un JOIN que reste las distancias entre cada airbnb y los 10 mejores restaurantes Japoneses.
De la query obtendre el TOP de apartamentos de airbnb de menos de 100 euros la noche.
El resultado de la query estara en Google Storage.

### Operating model
Hay un operador que soy yo, voy a recoger los datos diariamente de la pagina el tenedor y guardare el resultado en un directorio del segmento llamado "input_tenedor".
En el segmento siempre habra un directorio llamado "input_airbnb".
Seguire el standard de levantar el Cluster solamente cuando quiera regenerar el TOP.
Una vez al dia, levantare el CLUSTER a mano, enviare las tareas de:

crear tabla de airbnb
crear tabla del Tenedor
load data inpath de gs://XXXX:input_tenedor/ into table yelp
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




