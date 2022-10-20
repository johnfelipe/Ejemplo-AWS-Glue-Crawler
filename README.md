# Un ejemplo de AWS Glue Crawler

Este repositorio se creó para investigar el comportamiento de AWS Glue Crawler.
Y hacer una pequeña demostracion para la entrevista de bairesdev para el opening.
En mi concepto AWS Glue Crawler no está bien documentado y tiene un comportamiento misterioso.

Aunque para un caso de uso simple,
de modo que todos los archivos JSON de un bucket de S3 tengan la misma estructura,
Glue Crawler produce solo un esquema de tabla,
cuando los archivos JSON no tienen la misma estructura,
Producirá más de un esquema de tabla.

## Datasets

Este repositorio tiene cinco conjuntos de datos que se pueden cargar a AWS Glue Crawler:

*   json-data-example
*   Clave plana y una clave común
*   llaves disjuntas
*   non-hive-disjoint-keys
*   Teclas superpuestas

Estos conjuntos de datos se definen en *glue_crawler_example/datos.py*.

El conjunto de datos `json-data-example` solo tienen un archivo JSON.
Glue Crawler genera solo un esquema de tabla.

El conjunto de datos `flat-and-one-common-key` tiene archivos JSON puestos en la raíz.
Los archivos JSON tienen la clave común `id`
Pero cada archivo tiene una clave adicional única para el archivo.
Glue Crawler genera tablas para cada archivo JSON.

El conjunto de datos `disjoint-keys` tener archivos JSON con una clave única para cada archivo.
El prefijo de los archivos está en formato Hive.
Glue Crawler genera muchas tablas.

El conjunto de datos `non-hive-disjoint-keys` es una versión del conjunto de datos `disjoint-keys`,
donde el prefijo no está en el formato de Hive.
Glue Crawler genera muchas tablas.

El conjunto de datos `overlapping-keys` tienen archivos JSON con las siguientes claves:

*   `id`
*   `description`
*   `q0` o `q1`

Glue Crawler genera solo una tabla para este conjunto de datos.

## Cómo implementar

Primero, crea un virtualenv de Python:

```console
$ python3 -m venv .venv
```

En segundo lugar, activa su virtualenv después de la inicialización:

```console
$ source .venv/bin/activate
```

En tercer lugar, instale las dependencias:

```console
$ pip install -r requirements.txt
```

A continuación, puede implementar la pila:

```console
$ cdk deploy
```

## Cómo ejecutar los rastreadores

Después de la implementación, puede enumerar los rastreadores mediante el siguiente comando:

```console
$ inv list-crawlers
```

Luego ejecuta los rastreadores y espera a que se completen:

```console
$ inv start-crawlers
```

El siguiente comando muestra los esquemas de tabla generados:

```console
$ inv show-tables
```

Le gustaría limpiar las bases de datos en AWS Glue:

```console
$ inv delete-databases
```
