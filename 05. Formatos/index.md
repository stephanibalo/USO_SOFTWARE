---
title       : Diferentes formatos de datos
subtitle    : Transformaciones, importaciones y exportaciones desde R.
author      : Valentín Vergara Hidd
job         : 
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : [mathjax, quiz, bootstrap]            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
logo        : escudoof.gif
biglogo     : marcaderecha.png

--- .segue bg:lightgreen
# Diferentes formatos de archivo

---

## Formatos

R puede leer prácticamente cualquier tipo de archivo. Desde texto plano hasta algunos propietarios como SPSS, SAS, Stata, etc.

La ventaja de esto es que no necesitamos más que R y algunos paquetes instalados para poder empezar a trabajar con datos que otros han desarrollado para sus propios objetivos (datos secundarios.)

Para ello, es fundamental trabajar con el paquete **foreign**. Se instala de esta forma.


```r
install.packages("foreign", repos = 'https://dirichlet.mat.puc.cl/')
```

```
## Installing package into '/usr/local/lib/R/site-library'
## (as 'lib' is unspecified)
```
Con este paquete, podremos leer archivos en formatos propietarios. 

--- .class #id 

## Primer ejemplo: SPSS.
El primer archivo que vamos a trabajar está hecho para SPSS. Se llama [examen.sav] y lo pueden encontrar en INFODA. Para leerlo, hay que cargar el paquete **foreign**, previamente instalado y a continuación creamos un objeto [spss1] que contenga los datos, utilizando para ello la función *read.spss*.


```r
library(foreign)
spss1<-read.spss("./examen.sav", to.data.frame = TRUE)
```

```
## re-encoding from latin1
```

---
Para conocer el contenido del objeto [spss1], podemos usar la función str.


```r
str(spss1)
```

```
## 'data.frame':	20 obs. of  3 variables:
##  $ puntaje : num  62 58 52 55 75 82 38 55 48 68 ...
##  $ horas   : num  40 31 35 26 51 48 25 37 30 44 ...
##  $ ansiedad: num  40 65 34 91 46 52 48 61 34 74 ...
##  - attr(*, "variable.labels")= Named chr  "Puntaje en el Examen" "Horas de preparación (estudio)" "Escala de Ansiedad"
##   ..- attr(*, "names")= chr  "puntaje" "horas" "ansiedad"
##  - attr(*, "codepage")= int 28591
```

---
O bien, si lo que nos interesa simplemente es el nombre de cada una de las variables del *dataframe* [spss1]:

```r
names(spss1)
```

```
## [1] "puntaje"  "horas"    "ansiedad"
```
Luego, para conocer sus dimensiones, la función es:

```r
dim(spss1)
```

```
## [1] 20  3
```
El resultado de esta función indica que hay 20 filas y 3 columnas; lo que es igual a 20 casos en 3 variables.

---
## Modificar el dataframe
Imaginemos que estamos interesados en  crear una nueva variable, que llamaremos [hombre] y cuyo valor será 1 si la persona es hombre; y 0 en cualquier otro caso. Como no poseemos la información, lo haremos de forma aleatoria. Para ello, vamos a crear un objeto [hombre], con 20 valores al azar.

```r
hombre<-sample(c(0,1),20,replace = TRUE)
```
Y luego, crearemos un objeto [spss2], donde se incluirá [hombre] a los contenidos de [spss1]

```r
spss2<-cbind(spss1,hombre)
```

---
Para asegurarnos que se creó, veamos las primeras 6 filas del objeto [spss2]:

```r
head(spss2)
```

```
##   puntaje horas ansiedad hombre
## 1      62    40       40      0
## 2      58    31       65      1
## 3      52    35       34      1
## 4      55    26       91      1
## 5      75    51       46      1
## 6      82    48       52      1
```

Luego, nos aseguramos que el objeto [spss2] sea aún un objeto de tipo *dataframe*

```r
is.data.frame(spss2)
```

```
## [1] TRUE
```

--- 
## Exportar los datos

En la mayoría de los casos, no debería ser necesario exportar los datos a otro formato, dado que todo lo que se ha hecho en una sesión de R queda guardado en el script (el archivo con extensión .R). Sin embargo, en los casos que se quiera exportar, el paquete **foreign** tiene la función write.foreign.


```r
write.foreign(spss2, "./datos.txt", "./datos.sps", package = "SPSS")
```
Los archivos resultantes se deberían poder leer desde SPSS.




