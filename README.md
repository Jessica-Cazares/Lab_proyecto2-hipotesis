# Laboratoria - Proyecto 2

# HIPÓTESIS: Plataformas Musicales

La validación de hipótesis es un proceso fundamental en la toma de decisiones basadas en evidencia, y no solamente en creencias u opiniones. Validar hipótesis (confirmar o refutar) se consigue con técnicas y métodos diseñados para determinar si los resultados observados en los datos, son estadísticamente significativos o si pueden atribuirse al azar.

![](/carpeta/nombreimagen.png)

# Temas

- [Introducción](#introducción)
- [Objetivo](#objetivo)
- [Herramientas](#herramientas)
- [Lenguajes](#lenguajes)
- [Procesamiento y análisis](#procesamiento-y-análisis)
- [Jupyter Nootebook](/Jupyter_Nootebook/README.md)
- [Ficha tecnica](/Ficha_tecnica/ficha_tecnica)
- [Visualizacion](/Visualizacion/README.md)
- [Presentacion](/Presentacion/README.md)
- [Prueba de significancia](#prueba-de-significancia)
- [Resultados y conclusiones](#resultados-y-conclusiones)
- [Recomendaciones](#recomendaciones)

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
* Jupyter Notebook

## Lenguajes

* SQL
* Python

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

Este proceso es fundamental para asegurar la calidad y precisión del análisis subsiguiente.

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
  
1. Error cuadrático medio (MSE): 3.1890603295274906e+17
2. Coeficiente de determinación (R^2): -0.0003296256420366461
3. Intercepción: 518079372.14366764
4. Coeficiente: -14448.855151167847

* __Interpretación:__
  
1. Un MSE tan grande indica que las predicciones están muy lejos de los valores reales, hay un mal ajuste del modelo.
2. Un R² negativo indica que el modelo no tiene poder predictivo.
3. Intercepción: Este es el valor de streams cuando el bpm es 0.
4. Coeficiente: Este coeficiente indica que por cada incremento unitario en bpm, los streams disminuyen en promedio aproximadamente 14448.85 unidades. Dado que esto no parece razonable y el R² es negativo, esto también sugiere que el modelo no es adecuado.

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

1. Error cuadrático medio (MSE): 21.5543438226604
2. Coeficiente de determinación (R^2): 0.4004457933944051
3. Intercepción: 0.5037406320271827
4. Coeficiente: 0.18002131752879133

* __Interpretación:__
  
1. El MSE de 21.55 sugiere que aunque el modelo hace un trabajo razonable prediciendo charts, hay un margen considerable de error. Sugiere que hay variabilidad en los datos que no está siendo capturada.
2. El R² de 0.40 indica que hay una relación moderada entre in_spotify_charts y in_deezer_charts, pero no es lo suficientemente fuerte como para ser el único predictor de charts.
3. El coeficiente positivo de 0.18 confirma que, en general, una mayor posición en spotify está asociado con una mayor posición en deezer.

* __Conclusión:__ Se válida la hipótesis inicial.

* __Regresión lineal__
  
* Spotify vs Apple
  
1. Error cuadrático medio (MSE): 1976.8643296982843
2. Coeficiente de determinación (R^2): 0.24968577987854879
3. Intercepción: 34.845478791842766
4. Coeficiente: 1.4310541598256816

* __Interpretación:__
  
1. El MSE de 1976.86 es un valor alto lo que indica que no hay un buen ajuste del modelo a los datos.
2. Un R² de 0.2497 indica que aproximadamente el 24.97% de los datos se pueden predecir por el modelo de regresión lineal.
3. Intercepción cuando la variable independiente es 0, el modelo predice que la variable dependiente será aproximadamente 34.85.
4. Coeficiente, sugiere una relación positiva, una mayor posición en spotify está asociado con una mayor posición en deezer.
   
* __Conclusión:__ Se válida la hipótesis inicial, pero la relación es débil como para ser un buen predictor por sí solo.

* __Regresión lineal__
  
* Spotify vs Shazam
  
1. Error cuadrático medio (MSE): 14735.855116718994
2. Coeficiente de determinación (R^2): 0.44691000277096626
3. Intercepción: -0.8709355811230566
4. Coeficiente: 4.905546289495083

* __Interpretación:__

1. El MSE de 14735.86 sugiere que aunque el modelo hace un trabajo razonable prediciendo "charts in Spotify", hay un margen considerable de error. 
2. El R² de 0.45 indica que hay una relación moderada, lo que sugiere que charts in Spotify es un predictor razonable del éxito en los charts de Shazam, pero hay otros factores importantes que también influyen.
3. El coeficiente positivo de 4.91 confirma que, en general, un mejor desempeño en los charts in spotify está asociado con un mejor desempeño en los charts in Shazam.

* __Conclusión:__ Se válida la hipótesis inicial.

__Hipótesis 3: La presencia de una canción con un mayor número de playlists se relaciona con un mayor número de streams.__

* __Correlación__ 
* r=0.7836803010789
  
* __Interpretación:__
Un valor de r indica una fuerte relación positiva. Esto sugiere que hay una relación considerablemente fuerte entre el número de playlists en las que aparece una canción y el número de streams que recibe.

* __Regresión lineal__
  
1. Error cuadrático medio (MSE): 1.275849895855914e+17
2. Coeficiente de determinación (R^2): 0.5997973331266275
3. Intercepción: 231242214.05101973
4. Coeficiente: 48560.98322485796

* __Interpretación:__

Los resultados sugieren que hay una relación positiva entre el número de playlists en las que aparece una canción y el número de streams que recibe. Aunque el R2 de 0.6 indica que esta relación explica una parte significativa de la variación en los streams, el MSE elevado muestra que hay otros factores no incluidos en el modelo que también influyen en el número de streams. Aún así, el coeficiente positivo y significativo refuerza la idea de que estar en más playlists se asocia con más streams

* __Conclusión:__ Se válida la hipótesis inicial.

__Hipótesis 4: Los artistas con un mayor número de canciones en Spotify tienen más streams.__

* __Correlación__ 
* r=0.80016684593280
  
* __Interpretación:__
El valor de r indica una correlación positiva fuerte por lo que es muy probable que los artistas con más canciones en Spotify tiendan a tener un mayor número de streams. Lo que sugiere que la cantidad de contenido que un artista tiene en la plataforma está estrechamente vinculada con su popularidad en términos de streams.

* __Regresión lineal__

1. Error cuadrático medio (MSE): 1.377236569956644e+18
2. Coeficiente de determinación (R^2): 0.7537531924491054
3. Intercepción: 194858748.11261463
4. Coeficiente: 453769865.76784766

* __Interpretación:__

Los resultados sugieren una relación positiva y fuerte entre el número de canciones de un artista en Spotify y el número de streams que recibe. Aunque el MSE elevado indica que hay otros factores que también influyen en el número de streams y no están incluidos en el modelo, el R2 de 0.75 muestra que una gran parte de la variación en los streams puede ser explicada por el número de canciones. El coeficiente positivo y significativo refuerza la hipótesis de que los artistas con más canciones tienden a tener más streams.

* __Conclusión:__ Se válida la hipótesis inicial.

__Hipótesis 5: Las características de la música influyen en el éxito en términos de cantidad de streams en spotify.__

* __Correlación__ 
* _Danceability:_ r= -0.10563589955055
* _Speechiness:_ r= -0.112773935150

* __Interpretación:__
Los valores de r indican una relación negativa muy débil. La relación inversa es mínima, a medida que aumenta danceability y speechiness, la otra variable tiende a disminuir ligeramente.

* __Correlación:__
* _Valence:_ r=-0.041797954869
* _Energy:_ r= -0.0257381767548
* _Acousticness:_ r=-0.00498576864
* _Instrumentalness:_ r=-0.0440399854154
* _Liveness:_ r=-0.051147025245

* __Interpretación:__
Para todas las características anteriores:
La dirección de la relación es inversa (negativa) en todos los casos.
La fuerza de la relación es extremadamente débil en todos los casos, con valores muy cercanos a 0, prácticamente no hay una relación lineal observable.
No hay una relación significativa entre Valence, Energy, Acousticness, Instrumentalness, y Liveness vs streams.

* __Interpretación regresión lineal para todas las características:__
  
La hipótesis de que las características de la música influyen en el éxito en términos de cantidad de streams en Spotify no se sostiene. El modelo muestra que tienen un impacto mínimo y, de hecho, parecen tener una relación negativa con la cantidad de streams. Además, la alta magnitud del MSE y el bajo R² indican que el modelo no es adecuado para predecir el éxito en términos de streams basándose en las características.

* __Conclusión:__ Se refuta la hipótesis inicial.

* __Regresión lineal Danceability:__

1. Error cuadrático medio (MSE): 3.124828603648244e+17
2. Coeficiente de determinación (R^2): 0.01981828366786642
3. Intercepción: 754197652.8166625
4. Coeficiente: -3561913.4172532973

* __Regresión lineal Speechiness:__

1. Error cuadrático medio (MSE): 3.1839496713280416e+17
2. Coeficiente de determinación (R^2): 0.0012734618744032478
3. Intercepción: 586839521.0596712
4. Coeficiente: -7004805.666278839

* __Regresión lineal Acousticness:__

1. Error cuadrático medio (MSE): 3.1889904031590746e+17
2. Coeficiente de determinación (R^2): -0.000307691463716786
3. Intercepción: 519052870.0753346
4. Coeficiente: -101639.24942977371

* __Regresión lineal Energy:__

1. Error cuadrático medio (MSE): 3.191199432101503e+17
2. Coeficiente de determinación (R^2): -0.0010006093977401598
3. Intercepción: 584343722.8663057
4. Coeficiente: -1060671.6164008593

* __Regresión lineal Instrumentalness:__

1. Error cuadrático medio (MSE): 3.186896882065557e+17
2. Coeficiente de determinación (R^2): 0.0003489945049873766
3. Intercepción: 521574809.3676341
4. Coeficiente: -3009595.266891558

* __Regresión lineal Liveness:__
  
1. Error cuadrático medio (MSE): 3.167159734862173e+17
2. Coeficiente de determinación (R^2): 0.006540051127653657
3. Intercepción: 542243879.0230006
4. Coeficiente: -1424267.01610165

* __Regresión lineal Valence:__

1. Error cuadrático medio (MSE): 3.182035009291737e+17
2. Coeficiente de determinación (R^2): 0.00187404416514092
3. Intercepción: 566138289.9384496
4. Coeficiente: -971945.7250246599

## Recomendaciones

Después del análisis y revisión de las hipótesis para que el lanzamiento de un artista sea exitoso y que sus canciones sean las más escuchadas se puede recomendar lo siguiente:

* Estar disponible en todas las plataformas musicales, teniendo enfoque principal en Spotify, ya que el comportamiento de está plataforma influye en las demás.
* Es relevante estar presente en un mayor número de playlists y en los rankings musicales para aumentar la visibilidad de la canción.
* Aumentar rápidamente la cantidad de canciones que se lanzan al mercado, ya que hay una correlación positiva muy fuerte entre la cantidad de canciones y los streams, es decir, a mayor número de canciones mayor número de streams.
* No hay una relación clara en que alguna de las características de la canción pueda influir en su éxito.
* Hacer campañas de marketing y colaboración con los artistas más populares del momento.
