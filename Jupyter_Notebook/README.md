## Contenido :dart:
En estos archivos encontraras información de las pruebas aplicadas para la validación de las hipótesis.

## Prueba de significancia :bulb:
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

![](/Imagenes/Prueba_significancia_resultados.png)

## Regresión lineal simple :bar_chart:

Contiene el detalle de las consultas empleadas para predecir el valor de una variable en función de la otra, y el cálculo del error cuadrático medio (MSE), coeficiente de determinación (R2), intercepción y coeficiente.

Se pueden visualizar los gráficos de dispersión con la recta de regresión lineal.
