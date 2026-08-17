# Procesamiento distribuido de viajes de taxi de Nueva York — Apache Spark

Análisis de millones de trayectos de taxi amarillo de Nueva York con **Apache Spark**,
incluyendo cruce geoespacial contra los distritos comunitarios de la ciudad.

Trabajo desarrollado en el módulo de *Spark* del Máster en Big Data, Data Science e
Inteligencia Artificial (Universidad Complutense de Madrid, curso 2025-26).

## Qué hace

| Bloque | Contenido |
|---|---|
| 1. Limpieza y enriquecimiento | Carga del dataset de viajes en Spark, saneado de registros inválidos y derivación de variables temporales (hora, día de la semana, duración, velocidad media). |
| 2. Distribución de la demanda | *Boxplots* facetados de la actividad por hora y por día de la semana. |
| 3. Patrones semanales | *Heatmaps* hora × día, que dejan ver los picos de hora punta y el patrón distinto del fin de semana. |
| 4. Cartografía de distritos | Representación de los *community districts* de NYC con GeoPandas y Folium a partir del GeoJSON oficial. |
| 5. *Spatial join* distribuido | Asignación de cada trayecto a su distrito mediante **Apache Sedona**, resolviendo el cruce punto-en-polígono a escala sin salir de Spark. |

El punto interesante del ejercicio 5: un *join* geoespacial hecho a mano en pandas no
escala a millones de puntos. Sedona indexa los polígonos y distribuye la operación sobre
el clúster, que es la diferencia entre un análisis que termina y uno que no.

## Datos

Los ficheros de entrada **no se incluyen en el repositorio** por tamaño:

- `yellow_tripdata_2015-01.csv` — viajes de taxi amarillo de enero de 2015, publicados
  por la [NYC Taxi & Limousine Commission](https://www.nyc.gov/site/tlc/about/tlc-trip-record-data.page).
- `NYC_Community_Districts.geojson` — límites de los distritos comunitarios, disponibles
  en el [portal de datos abiertos de NYC](https://opendata.cityofnewyork.us/).

## Ejecución

El notebook está pensado para un entorno Spark gestionado (se desarrolló sobre
**Databricks Free Edition**), aunque funciona en cualquier clúster con Spark y Sedona:

1. Subir los dos ficheros de datos a un *Volume* del catálogo (o a HDFS/S3 según entorno).
2. Importar `Spark_t2.ipynb` en el workspace.
3. Ajustar `taxi_csv_path` y `geojson_path` a las rutas reales.
4. Instalar Apache Sedona en el clúster antes de ejecutar el bloque 5.

## Stack

Apache Spark · Apache Sedona · GeoPandas · Folium · Python

---

**Juan Peñas Utrilla** — [LinkedIn](https://www.linkedin.com/in/juan-penas-utrilla/) · [GitHub](https://github.com/JuanUtrilla)
