# Análisis exploratorio de datos

En este archivo encontrarás las consultas, tablas y vistas que se realizaron para el proceso de limpieza de datos:

1. Carga de datos
2. Exploración Inicial de los Datos
3. Manejo de Valores Faltantes
4. Corrección de Tipos de Datos
5. Detección y Manejo de Outliers
6. Creación de Nuevas Variables

Este proceso es fundamental para asegurar la calidad y precisión del análisis subsiguiente.

## Procesamiento y análisis
1. Conectar/importar datos a herramientas
* Se creó el proyecto2-hipotesis-lab y el conjunto de datos Dataset en BigQuery.
* Tablas importadas: track_in_competition, track_in_spotify, track_technical_info

2. Identificar y manejar valores nulos
* Se identifican valores nulos a través de comandos SQL COUNT, WHERE y IS NULL.
* track_in_competition: 50 valores nulos en la columna in_shazam_charts.
* track_in_spotify: 0 nulos.
* track_technical_info: 95 valores nulos en la columna key.

3. Identificar y manejar valores duplicados
* Se identifican duplicados a través de comandos SQL COUNT, GROUP BY, HAVING.
* track_in_competition: no hay valores duplicados.
* track_in_spotify: 4 track_name duplicadas con diferentes track_id. Para este análisis no se consideró la información de estos track_name porque no hay certeza de cuáles datos son correctos. La muestra total contiene 952 datos, se consideraron solo 944.
* track_technical_info: no hay valores duplicados.
  
4. Identificar y manejar datos fuera del alcance del análisis
* Se manejan variables que no son útiles para el análisis a través de comandos SQL SELECT EXCEPT.
* track_tecnical_info: se excluyó la columna key por tener muchos datos nulos (95) y la columna mode por no tener información relevante para el análisis.
  
5. Identificar y manejar datos discrepantes en variables categóricas
* Se identifican datos discrepantes utilizando el comandos de manejo de string, como REGEXP.
* track_in_spotify: Se reemplazaron por espacios vacíos los caracteres especiales de los track_name y artist_s__name , se creó una nueva columna track_name_limpio y artist_s__name_limpio.
  
6. Identificar y manejar datos discrepantes en variables numéricas
* Se identifican datos discrepantes utilizando comandos como MAX, MIN y AVG para las variables numéricas de interés para el estudio de cada base de datos.
  
7. Comprobar y cambiar tipo de dato
* track_in_spotify: Conversión de la variable streams de STRING a INTEGER usando comando SAFECAST, AS INT64 creando una nueva variable streams_numero, se calculan los valores MAX, MIN y AVG.
  
8. Crear nuevas variables
* track_in_spotify: Se creó la variable released, utilizando CONCAT.
  
9. Unir tablas
* Se crearon vistas de las tablas con los datos limpios, view_competition_limpia, view_technical_info_limpia y view_spotify_limpia
* Unión de las tablas limpias usando LEFT JOIN.
* Se creó la variable total_part_playlist usando SUM.
  
10. Construir tablas auxiliares
* Se creó tabla temporal para calcular el total de canciones por artista solista usando WITH.
  
11. Agrupar datos según variables categóricas
* Se importan los datos de BigQuery a Power BI.
* Se crearon tablas matrix con la cantidad de tracks por artista, cantidad de tracks por released_year y la cantidad de streams por año.
  
12. Visualizar las variables categóricas
* Se crearon gráficas de barras para la visualización de variables categóricas en Power BI.
  
13. Aplicar medidas de tendencia central y de dispersión
* Usando Matrix en Power BI se calcularon las medidas de tendencia central y dispersión para las variables bpm, streams spotify, playlist spotify, deezer, apple y total de participación playlists.
  
14. Visualizar distribución
* Utilizando Python se crearon histogramas para las variables bpm, streams_numero, total_part_playlist.
  
15. Visualizar el comportamiento de los datos a lo largo del tiempo
* Se crearon gráficos de línea en Power BI para evaluar el comportamiento de la cantidad de tracks y streams a lo largo del tiempo.
  
16. Calcular cuartiles, deciles o percentiles
* Se crearon categorías por cuartiles para las variables de características en BigQuery utilizando WITH, NTILE, IF.
* Se crearon las categorías alto y bajo para cada característica.
  
17. Calcular correlación entre variables
* En BigQuery se calculó la correlación en entre variables para cada una de las hipótesis utilizando el comando CORR.
