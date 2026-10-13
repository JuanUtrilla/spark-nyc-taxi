# Viajes de taxi de Nueva York con Apache Spark

Análisis de los once millones y medio de viajes de taxi amarillo de Nueva York en enero de
2015: a qué horas y qué días se piden más taxis, y en qué barrio empieza y termina cada
trayecto. Con tantos registros un solo ordenador se queda corto, así que el trabajo se
reparte entre varias máquinas con Apache Spark. Es la práctica del módulo de Spark del
Máster en Big Data, Data Science e IA de la Universidad Complutense (curso 2025-26).

![Mapa de calor de la demanda por hora y día de la semana](img/heatmap_demanda_hora_dia.png)

Entre semana la demanda se concentra en las horas punta; el fin de semana se desplaza a
la madrugada.

El cuaderno limpia los registros imposibles, calcula hora, día, duración y velocidad de
cada viaje y estudia la demanda con diagramas de caja y mapas de calor. Después dibuja los
distritos de la ciudad con GeoPandas y Folium. La última parte averigua en qué distrito cae
cada punto de recogida y de llegada, millones de comprobaciones de "este punto está dentro
de este polígono". Para eso se usa Apache Sedona, una extensión de Spark para datos
geográficos que reparte el cálculo entre las máquinas.

## Los datos no están en el repo

Pesan demasiado. Hay que bajarlos de su fuente:

- `yellow_tripdata_2015-01.csv`, de la [NYC Taxi & Limousine Commission](https://www.nyc.gov/site/tlc/about/tlc-trip-record-data.page).
- `NYC_Community_Districts.geojson`, del [portal de datos abiertos de NYC](https://opendata.cityofnewyork.us/).

## Cómo ejecutarlo

Se desarrolló en Databricks Free Edition, pero sirve cualquier clúster con Spark y Sedona:

1. Subir los dos ficheros a un Volume del catálogo (o a HDFS/S3, según el entorno).
2. Importar `spark_nyc_taxi.ipynb` en el workspace.
3. Ajustar `taxi_csv_path` y `geojson_path`.
4. Instalar Apache Sedona en el clúster antes de lanzar el último bloque. Si no, falla ahí.
