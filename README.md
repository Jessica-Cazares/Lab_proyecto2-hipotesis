# Laboratoria - Proyecto 2

# HIPÓTESIS: Plataformas Musicales

La validación de hipótesis es un proceso fundamental en la toma de decisiones basadas en evidencia, y no solamente en creencias u opiniones. Validar hipótesis (confirmar o refutar) se consigue con técnicas y métodos diseñados para determinar si los resultados observados en los datos, son estadísticamente significativos o si pueden atribuirse al azar.

# Temas

- [Introducción](#introducción)
- [Objetivo](#objetivo)
- [Herramientas](#herramientas)
- [Lenguajes](#lenguajes)
- [Procesamiento y análisis](#procesamiento-y-análisis)
- [Prueba de significancia](#prueba-de-significancia)
- [Resultados y conclusiones](#resultados-y-conclusiones)
- [Recomendaciones](#recomendaciones)
- [Recursos](#recursos)

## Introducción
En un mundo en el que la industria musical es extremadamente competitiva y está en permanente evolución, la capacidad de tomar decisiones basadas en datos se ha convertido en un activo invaluable. En este contexto, una discográfica se enfrenta al emocionante desafío de lanzar un nuevo artista en el escenario musical global. Afortunadamente, cuenta con una herramienta poderosa: un extenso dataset de Spotify con información sobre las canciones más escuchadas en 2023.

## Objetivo

Validar o refutar las hipótesis planteadas por la discográfica sobre qué hace que una canción sea más escuchada.
  
  1. Las canciones con un mayor BPM (Beats Por Minuto) tienen más éxito en términos de cantidad de streams en Spotify.
  2. Las canciones más populares en el ranking de Spotify también tienen un comportamiento similar en otras plataformas como Deezer.
  3. La presencia de una canción en un mayor número de playlists se relaciona con un mayor número de streams.
  4. Los artistas con un mayor número de canciones en Spotify tienen más streams.
  5. Las características de la música influyen en el éxito en términos de cantidad de streams en Spotify.

## Herramientas

* BigQuery
* Power BI
* Google Docs
* Google Slide

## Lenguajes

* SQL en BigQuery
* Python en Google Colab

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

## Prueba de significancia
1. Planteamiento de la hipótesis:
* _Hipótesis nula (H0)_ Las características de la música no influyen en el éxito en términos de cantidad de streams en Spotify.

  La diferencia de streams promedio en la categoría de cada característica son estadísticamente iguales.

  Streams promedio categoría alto = Streams promedio categoría bajo

* _Hipótesis Alternativa (H1):_ Las características de la música influyen en el éxito en términos de cantidad de streams en Spotify.

  La diferencia de streams promedio en la categoría de cada característica son significativamente diferentes.

  Streams promedio categoría alto != Streams promedio categoría bajo

2. Elegir nivel de confianza (alpha)
* Para evaluar la hipótesis, seleccionamos un nivel de confianza del 95% (α = 0.05). 

3. Elegir estadístico de contraste adecuado y calcular el pvalor
  Utilizamos dos pruebas estadísticas:
* __Test t de Student:__ prueba parámetrica.
* __Test Wilcoxon (Mann-Whitney U):__ prueba no paramétrica.

5. Comparar el p valor con el de alpha y concluir si aceptamos o rechazamos la H0

## Resultados y conclusiones

### Análisis exploratorio

* __Histograma bpm:__ Los valores de BPM más comunes se encuentran entre aproximadamente 85 y 145. Hay menos ocurrencias de valores de BPM por debajo de 80 y por encima de 180. La distribución parece simétrica, con una ligera tendencia hacia valores de BPM más altos.
  
* __Histograma Streams:__ Hay una frecuencia muy alta de streams en el rango de 0 a 500,000,000 mil millones, con más de 500 ocurrencias. Los valores de streams mayores 1,000,000,000 mil millones son mucho menos comunes. La distribución es asimétrica y está sesgada hacia la izquierda, con una larga cola hacia la derecha.

* __Histograma participación playlist:__ La mayor frecuencia de datos está entre 0 y 5000 mil playlists. La distribución de los datos es asimétrica y está sesgada a la izquierda. Hay muy pocos datos con valores altos de participación en playlist.


### Correlación entre variables y regresión lineal

__Hipótesis 1: Las canciones con un mayor BPM (Beats Por Minuto) tienen más éxito en términos de cantidad de streams en Spotify.__

* __Correlación__
* r= -0.00320018576

* __Interpretación:__
El valor de r es muy cercana a 0, lo que indica que no hay una relación lineal significativa entre las dos variables. El signo negativo indica que, si existiera alguna relación, sería una relación negativa, donde un aumento en una variable estaría asociado con una disminución en la otra. Sin embargo, dado que la magnitud es tan pequeña, esta relación negativa es prácticamente insignificante.

* __Regresión lineal__
Error cuadrático medio (MSE): 3.1890603295274906e+17
Coeficiente de determinación (R^2): -0.0003296256420366461
Intercepción: 518079372.14366764
Coeficiente: -14448.855151167847

* __Interpretación:__
Un MSE tan grande indica que las predicciones están muy lejos de los valores reales, hay un mal ajuste del modelo.
Un R² negativo indica que el modelo no tiene poder predictivo.
Intercepción: Este es el valor de streams cuando el bpm es 0.
Coeficiente: Este coeficiente indica que por cada incremento unitario en bpm, los streams disminuyen en promedio aproximadamente 14448.85 unidades. Dado que esto no parece razonable y el R² es negativo, esto también sugiere que el modelo no es adecuado.

* __Conclusión:__ Se refuta la hipótesis inicial.

__Hipótesis 2: Las canciones más populares en el ranking de Spotify tienen comportamiento similar en otras plataformas como Deezer, Apple, Shazam.__

* __Correlación__
* Spotify vs Deezer: r= 0.6076780201308
* Spotify vs Apple: r= 0.55269053270411
* Spotify vs Schazam: r= 0.6055409034998

* __Interpretación:__
Los valores de correlación de 0.6077 (Spotify vs Deezer), 0.5527 (Spotify vs Apple Music), y 0.6055 (Spotify vs Shazam) indican relaciones positivas moderadas entre Spotify y cada una de las otras plataformas. Estos valores sugieren que las tendencias de popularidad de música en Spotify tienden a reflejarse también en Deezer, Apple Music, y Shazam. Ninguna de estas correlaciones es extremadamente alta, lo que indica que, aunque hay una relación, no es perfecta. Esto es esperable ya que cada plataforma puede tener su propia base de usuarios con preferencias ligeramente diferentes.

* __Regresión lineal__
* Spotify vs Deezer
Error cuadrático medio (MSE): 21.5543438226604
Coeficiente de determinación (R^2): 0.4004457933944051
Intercepción: 0.5037406320271827
Coeficiente: 0.18002131752879133

* __Interpretación:__
El MSE de 21.55 sugiere que aunque el modelo hace un trabajo razonable prediciendo charts, hay un margen considerable de error. Sugiere que hay variabilidad en los datos que no está siendo capturada.
El R² de 0.40 indica que hay una relación moderada entre in_spotify_charts y in_deezer_charts, pero no es lo suficientemente fuerte como para ser el único predictor de charts.
El coeficiente positivo de 0.18 confirma que, en general, una mayor posición en spotify está asociado con una mayor posición en deezer.
Conclusión: Se valida la hipótesis inicial.

* __Conclusión:__ Se válida la hipótesis inicial.

## Recomendaciones

Después del análisis y revisión de las hipótesis para que el lanzamiento de un artista sea exitoso y que sus canciones sean las más escuchadas se puede recomendar lo siguiente:

* Estar disponible en todas las plataformas musicales, teniendo enfoque principal en Spotify, ya que el comportamiento de está plataforma influye en las demás.
* Es relevante estar presente en un mayor número de playlists y en los rankings musicales para aumentar la visibilidad de la canción.
* Aumentar rápidamente la cantidad de canciones que se lanzan al mercado, ya que hay una correlación positiva muy fuerte entre la cantidad de canciones y los streams, es decir, a mayor número de canciones mayor número de streams.
* No hay una relación clara en que alguna de las características de la canción pueda influir en su éxito.
* Hacer campañas de marketing y colaboración con los artistas más populares del momento.

