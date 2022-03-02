# Importación de datos

No siempre todos los datos con los que queremos trabajar vienen dentro de un paquete de R. 
De hecho, la mayoría de veces vamos a querer trabajar con un archivo de datos que está en nuestra computadora. Pueden ser datos que hemos recolectado por nuestra cuenta o que hemos descargado de alguna fuente en internet. 

Estos archivos pueden encontrarse en diferentes formatos, siendo uno de los más populares el de Ms Excel. Otros tipos de archivos que quizás conozcas pueden ser SPSS o Stata.

Los ejemplos asumen que tienes tus datos dentro de una carpeta llamada "data" en tu proyecto.

La data a importar en la práctica se encuentra en una carpeta de [Google Drive](https://drive.google.com/drive/folders/1PStbhj2IXd1dx29QAKhjXzb0C_UNajFB?usp=sharing) para su descarga. No olvides ubicar todos los archivos descargados dentro de la carpeta "data" de tu proyecto. 

Ahora deberías contar con todos estos archivos en tu carpeta "data".


|archivos                       |
|:------------------------------|
|09_UNIVERSIDADES_CARATULA.SAV  |
|gapminder_comas.csv            |
|gapminder_comas2.txt           |
|gapminder_excel.xlsx           |
|gapminder_excel_col_names.xlsx |
|gapminder_excel_sheet.xlsx     |
|gapminder_excel_skip.xlsx      |
|gapminder_guiones.txt          |
|gapminder_michi.txt            |
|gapminder_slash.txt            |
|gapminder_tabs.tsv             |

En la carpeta "data" contamos con cinco tipos de archivos:

- \*.txt: Texto separado por un delimitador arbitrario.
- \*.tsv: Texto separado por tabulaciones
- \*.csv: Texto separado por comas
- \*.xlsx: Archivo de Excel
- \*.SAV: Archivo de SPSS

## Paquetes necesarios

Existen paquetes especializados para cada tipo de datos que deseamos descargar. 
En esta oportunidad, aprenderemos a usar los siguientes.

- `readr`: Para archivos de texto
- `readxl`: Para archivos de Excel
- `haven`: Para archivos de SPSS y Stata

Todos estos paquetes se descagaron cuando instalaste `tidyverse`.

Una característica común de todos los paquetes presentados es que al leer los datos en R, se crean como *tibbles*, un formato de trabajo para datos tabulares que existe sólo en R. Además de ello, sus funciones comparten elementos en su interfaz (API), lo que permite trabajar con distintos tipos de datos realizando cambios mínimos.

## Importación de archivos de texto

### Cargar readr

Para acceder a las funciones de un paquete, siempre debemos primero cargarlo haciendo uso de `library()`.


```r
library(readr)
```

Alternativamente, podemos cargarlo junto a toda la colección de paquetes del `tidyverse`.





```r
library(tidyverse)
#> -- Attaching packages ------------------- tidyverse 1.3.1 --
#> v ggplot2 3.3.5     v purrr   0.3.4
#> v tibble  3.1.5     v dplyr   1.0.7
#> v tidyr   1.1.4     v stringr 1.4.0
#> v readr   2.1.0     v forcats 0.5.1
#> -- Conflicts ---------------------- tidyverse_conflicts() --
#> x dplyr::filter() masks stats::filter()
#> x dplyr::lag()    masks stats::lag()
```

La función que nutre el núcleo de `readr` es `read_delim()`. Nos permitirá leer prácticamente todo tipo de archivos de texto siempre y cuando le indiquemos el tipo de delimitador.

Exploremos los archivos de texto en nuestra carpeta "data".

### Leer datos csv

Los archivos de valores separados por comas (**C**omma **S**eparated **V**alues) son de los más utilizados en el mundo del análisis de datos debido a que prácticamente cualquier software puede reconocerlos. 

Son archivos de texto que representan datos tabulares en los que los valores de cada columna están separados por comas.

Para leer estos archivos, usamos la función `read_csv()` del paquete `readr`.


```r
read_csv("data/gapminder_comas.csv")  
#> Rows: 1704 Columns: 6
#> -- Column specification ------------------------------------
#> Delimiter: ","
#> chr (2): country, continent
#> dbl (4): year, lifeExp, pop, gdpPercap
#> 
#> i Use `spec()` to retrieve the full column specification for this data.
#> i Specify the column types or set `show_col_types = FALSE` to quiet this message.
#> # A tibble: 1,704 x 6
#>    country     continent  year lifeExp      pop gdpPercap
#>    <chr>       <chr>     <dbl>   <dbl>    <dbl>     <dbl>
#>  1 Afghanistan Asia       1952    28.8  8425333      779.
#>  2 Afghanistan Asia       1957    30.3  9240934      821.
#>  3 Afghanistan Asia       1962    32.0 10267083      853.
#>  4 Afghanistan Asia       1967    34.0 11537966      836.
#>  5 Afghanistan Asia       1972    36.1 13079460      740.
#>  6 Afghanistan Asia       1977    38.4 14880372      786.
#>  7 Afghanistan Asia       1982    39.9 12881816      978.
#>  8 Afghanistan Asia       1987    40.8 13867957      852.
#>  9 Afghanistan Asia       1992    41.7 16317921      649.
#> 10 Afghanistan Asia       1997    41.8 22227415      635.
#> # ... with 1,694 more rows
```


Al ejecutar el código, obtenemos dos tipos de output. El primero nos indica que la lectura asignó a cada columna cierto tipo de datos. Este tipo de output es un simple mensaje (message).

El segundo output nos indica el objeto obtenido (un tibble), con la cantidad de filas y columnas que contiene. Además, nos brinda las diez primeras filas de la información obtenida, y el nombre y tipo de cada columna. Si este conjunto de datos tuviera más columnas, sólo se nos mostraría la cantidad de columnas que alcances en nuestra ventana. 

### Almacenar datos importados

Si deseamos usar los datos obtenidos para un análisis posterior, como probablemente sea el caso, es necesario almacenar la tabla generada haciendo uso del operador de asignación `<-`. El nombre indicado aparecerá en nuestro panel "Environment".


```r
datos_obtenidos_csv <- read_csv("data/gapminder_comas.csv")
#> Rows: 1704 Columns: 6
#> -- Column specification ------------------------------------
#> Delimiter: ","
#> chr (2): country, continent
#> dbl (4): year, lifeExp, pop, gdpPercap
#> 
#> i Use `spec()` to retrieve the full column specification for this data.
#> i Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

Vemos que volvemos a obtener el mensaje, pero esta vez no se nos muestran los datos. Esto sucede porque al asignarle un nombre, le hemos indicado a R que queremos acceder a los datos sólo cuando se lo indiquemos explícitamente. Para hacer ello, simplemente tipeamos el nombre que le asignamos al conjunto de datos.


```r
datos_obtenidos_csv
#> # A tibble: 1,704 x 6
#>    country     continent  year lifeExp      pop gdpPercap
#>    <chr>       <chr>     <dbl>   <dbl>    <dbl>     <dbl>
#>  1 Afghanistan Asia       1952    28.8  8425333      779.
#>  2 Afghanistan Asia       1957    30.3  9240934      821.
#>  3 Afghanistan Asia       1962    32.0 10267083      853.
#>  4 Afghanistan Asia       1967    34.0 11537966      836.
#>  5 Afghanistan Asia       1972    36.1 13079460      740.
#>  6 Afghanistan Asia       1977    38.4 14880372      786.
#>  7 Afghanistan Asia       1982    39.9 12881816      978.
#>  8 Afghanistan Asia       1987    40.8 13867957      852.
#>  9 Afghanistan Asia       1992    41.7 16317921      649.
#> 10 Afghanistan Asia       1997    41.8 22227415      635.
#> # ... with 1,694 more rows
```

En esta ocasión, sí obtenemos la información contenida en el conjunto de datos, pero no el mensaje. Esto sucede porque los mensajes sólo nos muestran información relevante al momento de ejecutar una función, como lo es `read_csv()`.

### Leer datos TSV

Otra manera de almacenar datos es usando uso de archivos de valores separados por tabulaciones (**T**ab **S**eparated **V**alues)

Para leer estos archivos, hacemos uso de la función `read_tsv()`.



```r
datos_obtenidos_tsv <- read_tsv("data/gapminder_tabs.tsv")
#> Rows: 1704 Columns: 6
#> -- Column specification ------------------------------------
#> Delimiter: "\t"
#> chr (2): country, continent
#> dbl (4): year, lifeExp, pop, gdpPercap
#> 
#> i Use `spec()` to retrieve the full column specification for this data.
#> i Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

Nuevamente, la función nos comunica mediante un mensaje qué tipo de columnas ha identificado.

Podemos inspeccionar el contenido de este conjunto de datos del mismo modo anterior.


```r
datos_obtenidos_tsv
#> # A tibble: 1,704 x 6
#>    country     continent  year lifeExp      pop gdpPercap
#>    <chr>       <chr>     <dbl>   <dbl>    <dbl>     <dbl>
#>  1 Afghanistan Asia       1952    28.8  8425333      779.
#>  2 Afghanistan Asia       1957    30.3  9240934      821.
#>  3 Afghanistan Asia       1962    32.0 10267083      853.
#>  4 Afghanistan Asia       1967    34.0 11537966      836.
#>  5 Afghanistan Asia       1972    36.1 13079460      740.
#>  6 Afghanistan Asia       1977    38.4 14880372      786.
#>  7 Afghanistan Asia       1982    39.9 12881816      978.
#>  8 Afghanistan Asia       1987    40.8 13867957      852.
#>  9 Afghanistan Asia       1992    41.7 16317921      649.
#> 10 Afghanistan Asia       1997    41.8 22227415      635.
#> # ... with 1,694 more rows
```

### Leer datos con delimitador arbitrario

Para archivos de texto con cualquier otro tipo de delimitador, usamos la función `read_delim()`, que además de solicitarnos la ubicación del archivo, nos pide especificar el delimitador que se usó para almacenar los datos.

Por ejemplo, inspeccionemos el archivo "gapminder_comas2.txt"

Vemos que es muy similar a los archivos anteriores, pero en este caso las columnas están separadas por punto y coma. 

Para leer este archivo, hacemos uso de `read_delim()` indicando como separador el caracter adecuado.


```r
datos_obtenidos_punto_y_coma <- read_delim("data/gapminder_comas2.txt", delim = ";")
#> Rows: 1704 Columns: 6
#> -- Column specification ------------------------------------
#> Delimiter: ";"
#> chr (2): country, continent
#> dbl (4): year, lifeExp, pop, gdpPercap
#> 
#> i Use `spec()` to retrieve the full column specification for this data.
#> i Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

## Importación de archivos excel

Para poder leer datos de excel, vamos a hacer uso del paquete `readxl`. 

Recuerda que para cargar cualquier paquete, especificas su nombre sin comillas dentro de un llamado a la funcion `library()`.


```r
library(readxl)
```

La función que nos permite leer archivos *.xlsx* es `read_xlsx()`. La usamos igual que en los casos anteriores.


```r
gapminder_excel <- read_xlsx("data/gapminder_excel.xlsx")
```


```r
gapminder_excel
#> # A tibble: 1,704 x 6
#>    country     continent  year lifeExp      pop gdpPercap
#>    <chr>       <chr>     <dbl>   <dbl>    <dbl>     <dbl>
#>  1 Afghanistan Asia       1952    28.8  8425333      779.
#>  2 Afghanistan Asia       1957    30.3  9240934      821.
#>  3 Afghanistan Asia       1962    32.0 10267083      853.
#>  4 Afghanistan Asia       1967    34.0 11537966      836.
#>  5 Afghanistan Asia       1972    36.1 13079460      740.
#>  6 Afghanistan Asia       1977    38.4 14880372      786.
#>  7 Afghanistan Asia       1982    39.9 12881816      978.
#>  8 Afghanistan Asia       1987    40.8 13867957      852.
#>  9 Afghanistan Asia       1992    41.7 16317921      649.
#> 10 Afghanistan Asia       1997    41.8 22227415      635.
#> # ... with 1,694 more rows
```

Alternativamente, podemos usar la función `read_excel()`, que nos da el mismo resultado.


```r
gapminder_excel2 <- read_excel("data/gapminder_excel.xlsx")
```


```r
gapminder_excel2
#> # A tibble: 1,704 x 6
#>    country     continent  year lifeExp      pop gdpPercap
#>    <chr>       <chr>     <dbl>   <dbl>    <dbl>     <dbl>
#>  1 Afghanistan Asia       1952    28.8  8425333      779.
#>  2 Afghanistan Asia       1957    30.3  9240934      821.
#>  3 Afghanistan Asia       1962    32.0 10267083      853.
#>  4 Afghanistan Asia       1967    34.0 11537966      836.
#>  5 Afghanistan Asia       1972    36.1 13079460      740.
#>  6 Afghanistan Asia       1977    38.4 14880372      786.
#>  7 Afghanistan Asia       1982    39.9 12881816      978.
#>  8 Afghanistan Asia       1987    40.8 13867957      852.
#>  9 Afghanistan Asia       1992    41.7 16317921      649.
#> 10 Afghanistan Asia       1997    41.8 22227415      635.
#> # ... with 1,694 more rows
```


## Importación de archivos SPSS

Para leer archivos SPSS cargamos el paquete `haven`.


```r
library(haven)
```

Como la extensión de los archivos es *.sav* podemos leerlos con la función `read_sav()`.


```r
universidades <- read_sav("data/09_UNIVERSIDADES_CARATULA.SAV")
```


```r
universidades
#> # A tibble: 122 x 8
#>    SELECT UC0DD_CD UC0DD_DPTO UC0PP_CD UC0PP_PROV UC0DI_CD
#>    <chr>  <chr>    <chr>      <chr>    <chr>      <chr>   
#>  1 U0001  16       LORETO     01       MAYNAS     13      
#>  2 U0002  14       LAMBAYEQUE 03       LAMBAYEQUE 01      
#>  3 U0003  15       LIMA       01       LIMA       40      
#>  4 U0004  15       LIMA       01       LIMA       13      
#>  5 U0005  15       LIMA       01       LIMA       35      
#>  6 U0006  15       LIMA       01       LIMA       40      
#>  7 U0007  15       LIMA       01       LIMA       21      
#>  8 U0008  23       TACNA      01       TACNA      01      
#>  9 U0009  10       HUANUCO    01       HUANUCO    01      
#> 10 U0010  15       LIMA       01       LIMA       13      
#> # ... with 112 more rows, and 2 more variables:
#> #   UC0DI_DIST <chr>, UC0P_OBS <chr>
```

Alternativamente, podemos usar la función `read_spss()`, que nos da el mismo resultado.


```r
universidades2 <- read_spss("data/09_UNIVERSIDADES_CARATULA.SAV")
```


```r
universidades2
#> # A tibble: 122 x 8
#>    SELECT UC0DD_CD UC0DD_DPTO UC0PP_CD UC0PP_PROV UC0DI_CD
#>    <chr>  <chr>    <chr>      <chr>    <chr>      <chr>   
#>  1 U0001  16       LORETO     01       MAYNAS     13      
#>  2 U0002  14       LAMBAYEQUE 03       LAMBAYEQUE 01      
#>  3 U0003  15       LIMA       01       LIMA       40      
#>  4 U0004  15       LIMA       01       LIMA       13      
#>  5 U0005  15       LIMA       01       LIMA       35      
#>  6 U0006  15       LIMA       01       LIMA       40      
#>  7 U0007  15       LIMA       01       LIMA       21      
#>  8 U0008  23       TACNA      01       TACNA      01      
#>  9 U0009  10       HUANUCO    01       HUANUCO    01      
#> 10 U0010  15       LIMA       01       LIMA       13      
#> # ... with 112 more rows, and 2 more variables:
#> #   UC0DI_DIST <chr>, UC0P_OBS <chr>
```

## Lectura de datos con más argumentos

No siempre los datos estarán de antemano en un formato que nos permita leerlos automáticamente. En algunos casos, será necesario especificar argumentos adicionales al momento de leerlos.

Intentar leer los datos de los archivos excel restantes.

## Solución

Usar los argumentos adicionales:

- `skip`
- `sheet`
- `col_names`


## Uso de datos de paquetes instalados

### Revisar datos disponibles

Cuando se carga un paquete, el usuario tiene acceso a los set de datos contenidos en él. Podemos obtener un listado de los set de datos contenidos en un paquete usando la función `datasets()` del paquete `vcdExtra`. Si no lo tenemos instalado debemos usar el siguiente código en la consola.


```r
install.packages("vcdExtra")
```

Para usar sus funciones usamos:


```r
library(vcdExtra)
```

Por ejemplo, para conocer los conjuntos de datos del paquete `ggplot2`:


```r
datasets("ggplot2")
#>              Item      class      dim
#> 1        diamonds data.frame 53940x10
#> 2       economics data.frame    574x6
#> 3  economics_long data.frame   2870x4
#> 4       faithfuld data.frame   5625x3
#> 5     luv_colours data.frame    657x4
#> 6         midwest data.frame   437x28
#> 7             mpg data.frame   234x11
#> 8          msleep data.frame    83x11
#> 9    presidential data.frame     11x4
#> 10          seals data.frame   1155x4
#> 11      txhousing data.frame   8602x9
#>                                                                Title
#> 1                           Prices of over 50,000 round cut diamonds
#> 2                                            US economic time series
#> 3                                            US economic time series
#> 4                           2d density estimate of Old Faithful data
#> 5                                            'colors()' in Luv space
#> 6                                               Midwest demographics
#> 7  Fuel economy data from 1999 to 2008 for 38 popular models of cars
#> 8       An updated and expanded version of the mammals sleep dataset
#> 9                    Terms of 11 presidents from Eisenhower to Obama
#> 10                                    Vector field of seal movements
#> 11                                               Housing sales in TX
```

Si ya hemos cargado el paquete, basta con llamar al conjunto de datos usando su nombre.


```r
library(ggplot2)
```

---


```r
diamonds
#> # A tibble: 53,940 x 10
#>    carat cut     color clarity depth table price     x     y
#>    <dbl> <ord>   <ord> <ord>   <dbl> <dbl> <int> <dbl> <dbl>
#>  1  0.23 Ideal   E     SI2      61.5    55   326  3.95  3.98
#>  2  0.21 Premium E     SI1      59.8    61   326  3.89  3.84
#>  3  0.23 Good    E     VS1      56.9    65   327  4.05  4.07
#>  4  0.29 Premium I     VS2      62.4    58   334  4.2   4.23
#>  5  0.31 Good    J     SI2      63.3    58   335  4.34  4.35
#>  6  0.24 Very G~ J     VVS2     62.8    57   336  3.94  3.96
#>  7  0.24 Very G~ I     VVS1     62.3    57   336  3.95  3.98
#>  8  0.26 Very G~ H     SI1      61.9    55   337  4.07  4.11
#>  9  0.22 Fair    E     VS2      65.1    61   337  3.87  3.78
#> 10  0.23 Very G~ H     VS1      59.4    61   338  4     4.05
#> # ... with 53,930 more rows, and 1 more variable: z <dbl>
```

---


```r
economics
#> # A tibble: 574 x 6
#>    date         pce    pop psavert uempmed unemploy
#>    <date>     <dbl>  <dbl>   <dbl>   <dbl>    <dbl>
#>  1 1967-07-01  507. 198712    12.6     4.5     2944
#>  2 1967-08-01  510. 198911    12.6     4.7     2945
#>  3 1967-09-01  516. 199113    11.9     4.6     2958
#>  4 1967-10-01  512. 199311    12.9     4.9     3143
#>  5 1967-11-01  517. 199498    12.8     4.7     3066
#>  6 1967-12-01  525. 199657    11.8     4.8     3018
#>  7 1968-01-01  531. 199808    11.7     5.1     2878
#>  8 1968-02-01  534. 199920    12.3     4.5     3001
#>  9 1968-03-01  544. 200056    11.7     4.1     2877
#> 10 1968-04-01  544  200208    12.3     4.6     2709
#> # ... with 564 more rows
```

---


```r
seals
#> # A tibble: 1,155 x 4
#>      lat  long delta_long delta_lat
#>    <dbl> <dbl>      <dbl>     <dbl>
#>  1  29.7 -173.     -0.915   0.143  
#>  2  30.7 -173.     -0.867   0.128  
#>  3  31.7 -173.     -0.819   0.113  
#>  4  32.7 -173.     -0.771   0.0980 
#>  5  33.7 -173.     -0.723   0.0828 
#>  6  34.7 -173.     -0.674   0.0675 
#>  7  35.7 -173.     -0.626   0.0522 
#>  8  36.7 -173.     -0.577   0.0369 
#>  9  37.7 -173.     -0.529   0.0216 
#> 10  38.7 -173.     -0.480   0.00635
#> # ... with 1,145 more rows
```

## Material extra

Para aprender más, puedes consultar: 

- R para ciencia de datos. [Capítulo Importación de datos](https://r4ds-en-espaniol.netlify.app/importaci%C3%B3n-de-datos.html)
- Importing data with RStudio [blog](https://support.rstudio.com/hc/en-us/articles/218611977-Importing-Data-with-RStudio)
- Different ways of importing data into R [blog](https://towardsdatascience.com/different-ways-of-importing-data-into-r-2d234e8e0dec)
