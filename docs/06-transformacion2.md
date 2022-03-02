# Transformación de datos

La sección de transformación de datos está basada en los ejemplos presentados en el Cheat Sheet de [Data Transformation with dplyr](https://raw.githubusercontent.com/rstudio/cheatsheets/main/data-transformation.pdf).

En este capítulo se trabaja con funciones del paquete `dplyr`, haciendo uso de *pipes* y asumiendo que el conjunto de datos está ordenado (tidy data).

Los conjuntos de datos ordenados cumplen tres principios:

1. Cada variable tiene su propia columna
2. Cada caso u observación tiene su propia fila
3. Cada valor tiene su propia *celda*

Para trabajar con el paquete dplyr usa una de las siguientes llamadas:


```r
library(dplyr) # para cargar solo dplyr
```



```r
library(tidyverse) # para cargar dplyr y toda la colección del tidyverse
#> -- Attaching packages ------------------- tidyverse 1.3.1 --
#> v ggplot2 3.3.5     v purrr   0.3.4
#> v tibble  3.1.5     v dplyr   1.0.7
#> v tidyr   1.1.4     v stringr 1.4.0
#> v readr   2.1.0     v forcats 0.5.1
#> -- Conflicts ---------------------- tidyverse_conflicts() --
#> x dplyr::filter() masks stats::filter()
#> x dplyr::lag()    masks stats::lag()
```

Para estos ejemplos, se ha aplicado el conjunto de datos `mtcars` ha sido convertido a *tibble*.


```r
mtcars <- as_tibble(mtcars)
```


## Pipes 

El operador *pipe* viene integrado en el paquete `dplyr`. Busca simplificar la manera en que se componen las funciones que trabajan con datos ordenados. Evita la creación de objetos intermedios y permite leer una operación como una historia contada en lenguaje natural.

En la práctica, convierte el objeto a la izquieda del `%>%` en el primer argumento de la función a la derecha.

En lugar de usar `f(x)` puedo usar `x %>% f()` para referirme a la misma operación. De este modo, puedo componer una operación más compleja encadenando *pipes*.


```r
x %>% 
    f() %>% 
    g() %>% 
    h() 
```

Se puede traducir en lenguaje natural a:

1. Tomo el objeto `x`
2. *Luego*, le aplico la función `f()`
3. *Luego*, al resultado le aplico la función `g()`
4. *Luego*, al resultado le aplico la función `h()`

A partir de la versión 4.1 de R, se introdujo un operador *pipe* nativo, que no necesita cargar ningún paquete previamente para ser usado. Reescribiendo la operación anterior con ese operador se vería así:


```r
x |> 
    f() |> 
    g() |> 
    h() 
```

Debido a que el *pipe* de `dplyr` es aún el de uso más extendido, este cookbook seguirá usándolo para los ejemplos.


## Resumir observaciones

Aplicar funciones de resumen a columnas crea un nuevo conjunto de datos que contiene estadísticas de resumen. Las funciones de resumen toman un vector como *input* y retornan un único valor.

Puedes ver las funciones de resumen más comunes en **Referenciar capítulo de funciones de resumen**.

### Obtener tabla con una medida de resumen de una variable

Usar `summarise()`.

A partir de la siguiente tabla:


```r
mtcars
#> # A tibble: 32 x 11
#>      mpg   cyl  disp    hp  drat    wt  qsec    vs    am
#>    <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl>
#>  1  21       6  160    110  3.9   2.62  16.5     0     1
#>  2  21       6  160    110  3.9   2.88  17.0     0     1
#>  3  22.8     4  108     93  3.85  2.32  18.6     1     1
#>  4  21.4     6  258    110  3.08  3.22  19.4     1     0
#>  5  18.7     8  360    175  3.15  3.44  17.0     0     0
#>  6  18.1     6  225    105  2.76  3.46  20.2     1     0
#>  7  14.3     8  360    245  3.21  3.57  15.8     0     0
#>  8  24.4     4  147.    62  3.69  3.19  20       1     0
#>  9  22.8     4  141.    95  3.92  3.15  22.9     1     0
#> 10  19.2     6  168.   123  3.92  3.44  18.3     1     0
#> # ... with 22 more rows, and 2 more variables: gear <dbl>,
#> #   carb <dbl>
```
Puede obtenerse el promedio de **mpg**.


```r
mtcars %>% 
    summarise(promedio = mean(mpg))
#> # A tibble: 1 x 1
#>   promedio
#>      <dbl>
#> 1     20.1
```

### Obtener tabla con múltiples medidas de resumen de una variable

Usar `summarise()`.

A partir de la siguiente tabla:


```r
mtcars
#> # A tibble: 32 x 11
#>      mpg   cyl  disp    hp  drat    wt  qsec    vs    am
#>    <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl>
#>  1  21       6  160    110  3.9   2.62  16.5     0     1
#>  2  21       6  160    110  3.9   2.88  17.0     0     1
#>  3  22.8     4  108     93  3.85  2.32  18.6     1     1
#>  4  21.4     6  258    110  3.08  3.22  19.4     1     0
#>  5  18.7     8  360    175  3.15  3.44  17.0     0     0
#>  6  18.1     6  225    105  2.76  3.46  20.2     1     0
#>  7  14.3     8  360    245  3.21  3.57  15.8     0     0
#>  8  24.4     4  147.    62  3.69  3.19  20       1     0
#>  9  22.8     4  141.    95  3.92  3.15  22.9     1     0
#> 10  19.2     6  168.   123  3.92  3.44  18.3     1     0
#> # ... with 22 more rows, and 2 more variables: gear <dbl>,
#> #   carb <dbl>
```

Puede obtenerse el promedio, mediana, rango intercuartil, etc, de **mpg**.


```r
mtcars %>% 
    summarise(promedio = mean(mpg),
              mediana = median(mpg),
              rango_iq = IQR(mpg))
#> # A tibble: 1 x 3
#>   promedio mediana rango_iq
#>      <dbl>   <dbl>    <dbl>
#> 1     20.1    19.2     7.38
```


### Obtener tabla con múltiples medidas de resumen de múltiples variable

Usar `summarise()`.

A partir de la siguiente tabla:


```r
mtcars
#> # A tibble: 32 x 11
#>      mpg   cyl  disp    hp  drat    wt  qsec    vs    am
#>    <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl>
#>  1  21       6  160    110  3.9   2.62  16.5     0     1
#>  2  21       6  160    110  3.9   2.88  17.0     0     1
#>  3  22.8     4  108     93  3.85  2.32  18.6     1     1
#>  4  21.4     6  258    110  3.08  3.22  19.4     1     0
#>  5  18.7     8  360    175  3.15  3.44  17.0     0     0
#>  6  18.1     6  225    105  2.76  3.46  20.2     1     0
#>  7  14.3     8  360    245  3.21  3.57  15.8     0     0
#>  8  24.4     4  147.    62  3.69  3.19  20       1     0
#>  9  22.8     4  141.    95  3.92  3.15  22.9     1     0
#> 10  19.2     6  168.   123  3.92  3.44  18.3     1     0
#> # ... with 22 more rows, and 2 more variables: gear <dbl>,
#> #   carb <dbl>
```

Puede obtenerse el promedio y mediana  de **mpg** y **hp**.


```r
mtcars %>% 
    summarise(promedio_mpg = mean(mpg),
              mediana_mpg = median(mpg),
              promedio_hp = mean(hp),
              mediana_hp = median(hp))
#> # A tibble: 1 x 4
#>   promedio_mpg mediana_mpg promedio_hp mediana_hp
#>          <dbl>       <dbl>       <dbl>      <dbl>
#> 1         20.1        19.2        147.        123
```

### Obtener recuento de observaciones de una tabla

Usar `count()`.

A partir de la siguiente tabla:


```r
mtcars
#> # A tibble: 32 x 11
#>      mpg   cyl  disp    hp  drat    wt  qsec    vs    am
#>    <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl>
#>  1  21       6  160    110  3.9   2.62  16.5     0     1
#>  2  21       6  160    110  3.9   2.88  17.0     0     1
#>  3  22.8     4  108     93  3.85  2.32  18.6     1     1
#>  4  21.4     6  258    110  3.08  3.22  19.4     1     0
#>  5  18.7     8  360    175  3.15  3.44  17.0     0     0
#>  6  18.1     6  225    105  2.76  3.46  20.2     1     0
#>  7  14.3     8  360    245  3.21  3.57  15.8     0     0
#>  8  24.4     4  147.    62  3.69  3.19  20       1     0
#>  9  22.8     4  141.    95  3.92  3.15  22.9     1     0
#> 10  19.2     6  168.   123  3.92  3.44  18.3     1     0
#> # ... with 22 more rows, and 2 more variables: gear <dbl>,
#> #   carb <dbl>
```

Se puede obtener el recuento de observaciones presentes en la tabla


```r
mtcars %>% 
    count()
#> # A tibble: 1 x 1
#>       n
#>   <int>
#> 1    32
```

### Obtener recuento de observaciones de una variable

Usar `count()`.

A partir de la siguiente tabla:


```r
mtcars
#> # A tibble: 32 x 11
#>      mpg   cyl  disp    hp  drat    wt  qsec    vs    am
#>    <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl>
#>  1  21       6  160    110  3.9   2.62  16.5     0     1
#>  2  21       6  160    110  3.9   2.88  17.0     0     1
#>  3  22.8     4  108     93  3.85  2.32  18.6     1     1
#>  4  21.4     6  258    110  3.08  3.22  19.4     1     0
#>  5  18.7     8  360    175  3.15  3.44  17.0     0     0
#>  6  18.1     6  225    105  2.76  3.46  20.2     1     0
#>  7  14.3     8  360    245  3.21  3.57  15.8     0     0
#>  8  24.4     4  147.    62  3.69  3.19  20       1     0
#>  9  22.8     4  141.    95  3.92  3.15  22.9     1     0
#> 10  19.2     6  168.   123  3.92  3.44  18.3     1     0
#> # ... with 22 more rows, and 2 more variables: gear <dbl>,
#> #   carb <dbl>
```

Se puede obtener el recuento de observaciones para cada valor de **cyl** presente en la tabla


```r
mtcars %>% 
    count(cyl)
#> # A tibble: 3 x 2
#>     cyl     n
#>   <dbl> <int>
#> 1     4    11
#> 2     6     7
#> 3     8    14
```

### Obtener recuento de observaciones de combinaciones de variables

Usar `count()`.

A partir de la siguiente tabla:


```r
mtcars
#> # A tibble: 32 x 11
#>      mpg   cyl  disp    hp  drat    wt  qsec    vs    am
#>    <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl>
#>  1  21       6  160    110  3.9   2.62  16.5     0     1
#>  2  21       6  160    110  3.9   2.88  17.0     0     1
#>  3  22.8     4  108     93  3.85  2.32  18.6     1     1
#>  4  21.4     6  258    110  3.08  3.22  19.4     1     0
#>  5  18.7     8  360    175  3.15  3.44  17.0     0     0
#>  6  18.1     6  225    105  2.76  3.46  20.2     1     0
#>  7  14.3     8  360    245  3.21  3.57  15.8     0     0
#>  8  24.4     4  147.    62  3.69  3.19  20       1     0
#>  9  22.8     4  141.    95  3.92  3.15  22.9     1     0
#> 10  19.2     6  168.   123  3.92  3.44  18.3     1     0
#> # ... with 22 more rows, and 2 more variables: gear <dbl>,
#> #   carb <dbl>
```

Se puede obtener el recuento de observaciones combinación de **cyl** y **vs** presente en la tabla


```r
mtcars %>% 
    count(cyl, vs)
#> # A tibble: 5 x 3
#>     cyl    vs     n
#>   <dbl> <dbl> <int>
#> 1     4     0     1
#> 2     4     1    10
#> 3     6     0     3
#> 4     6     1     4
#> 5     8     0    14
```

## Agrupar casos 

Para crear un conjunto de datos "agrupados" por las columnas especificadas. Las funciones verbo de `dplyr` trabajarán cada grupo por separado y combinarán los resultados.

### Obtener tabla con una medida de resumen para cada categoría de una variable

Usar un encadenamiento de `group_by()` y `summarise()`.

A partir de la siguiente tabla:


```r
mtcars
#> # A tibble: 32 x 11
#>      mpg   cyl  disp    hp  drat    wt  qsec    vs    am
#>    <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl>
#>  1  21       6  160    110  3.9   2.62  16.5     0     1
#>  2  21       6  160    110  3.9   2.88  17.0     0     1
#>  3  22.8     4  108     93  3.85  2.32  18.6     1     1
#>  4  21.4     6  258    110  3.08  3.22  19.4     1     0
#>  5  18.7     8  360    175  3.15  3.44  17.0     0     0
#>  6  18.1     6  225    105  2.76  3.46  20.2     1     0
#>  7  14.3     8  360    245  3.21  3.57  15.8     0     0
#>  8  24.4     4  147.    62  3.69  3.19  20       1     0
#>  9  22.8     4  141.    95  3.92  3.15  22.9     1     0
#> 10  19.2     6  168.   123  3.92  3.44  18.3     1     0
#> # ... with 22 more rows, and 2 more variables: gear <dbl>,
#> #   carb <dbl>
```

Se puede obtener el promedio de **mpg** para cada valor de **cyl** presente en la tabla


```r
mtcars %>% 
    group_by(cyl) %>% 
    summarise(promedio_mpg = mean(mpg))
#> # A tibble: 3 x 2
#>     cyl promedio_mpg
#>   <dbl>        <dbl>
#> 1     4         26.7
#> 2     6         19.7
#> 3     8         15.1
```

### Obtener tabla con múltiples medidas de resumen para cada categoría

Usar un encadenamiento de `group_by()` y `summarise()`.

A partir de la siguiente tabla:


```r
mtcars
#> # A tibble: 32 x 11
#>      mpg   cyl  disp    hp  drat    wt  qsec    vs    am
#>    <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl>
#>  1  21       6  160    110  3.9   2.62  16.5     0     1
#>  2  21       6  160    110  3.9   2.88  17.0     0     1
#>  3  22.8     4  108     93  3.85  2.32  18.6     1     1
#>  4  21.4     6  258    110  3.08  3.22  19.4     1     0
#>  5  18.7     8  360    175  3.15  3.44  17.0     0     0
#>  6  18.1     6  225    105  2.76  3.46  20.2     1     0
#>  7  14.3     8  360    245  3.21  3.57  15.8     0     0
#>  8  24.4     4  147.    62  3.69  3.19  20       1     0
#>  9  22.8     4  141.    95  3.92  3.15  22.9     1     0
#> 10  19.2     6  168.   123  3.92  3.44  18.3     1     0
#> # ... with 22 more rows, and 2 more variables: gear <dbl>,
#> #   carb <dbl>
```

Se puede obtener el promedio de **mpg** y **hp** para cada valor de **cyl** presente en la tabla


```r
mtcars %>% 
    group_by(cyl) %>% 
    summarise(promedio_mpg = mean(mpg),
              promedio_hp = mean(hp))
#> # A tibble: 3 x 3
#>     cyl promedio_mpg promedio_hp
#>   <dbl>        <dbl>       <dbl>
#> 1     4         26.7        82.6
#> 2     6         19.7       122. 
#> 3     8         15.1       209.
```

### Obtener tabla con una medida de resumen para combinaciones de categorías de más de una variable

Usar un encadenamiento de `group_by()` y `summarise()`.

A partir de la siguiente tabla:


```r
mtcars
#> # A tibble: 32 x 11
#>      mpg   cyl  disp    hp  drat    wt  qsec    vs    am
#>    <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl>
#>  1  21       6  160    110  3.9   2.62  16.5     0     1
#>  2  21       6  160    110  3.9   2.88  17.0     0     1
#>  3  22.8     4  108     93  3.85  2.32  18.6     1     1
#>  4  21.4     6  258    110  3.08  3.22  19.4     1     0
#>  5  18.7     8  360    175  3.15  3.44  17.0     0     0
#>  6  18.1     6  225    105  2.76  3.46  20.2     1     0
#>  7  14.3     8  360    245  3.21  3.57  15.8     0     0
#>  8  24.4     4  147.    62  3.69  3.19  20       1     0
#>  9  22.8     4  141.    95  3.92  3.15  22.9     1     0
#> 10  19.2     6  168.   123  3.92  3.44  18.3     1     0
#> # ... with 22 more rows, and 2 more variables: gear <dbl>,
#> #   carb <dbl>
```

Se puede obtener el promedio de **mpg** para cada combinación de los valores de **cyl** y **vs** presente en la tabla


```r
mtcars %>% 
    group_by(cyl, vs) %>% 
    summarise(promedio_mpg = mean(mpg))
#> `summarise()` has grouped output by 'cyl'. You can override using the `.groups` argument.
#> # A tibble: 5 x 3
#> # Groups:   cyl [3]
#>     cyl    vs promedio_mpg
#>   <dbl> <dbl>        <dbl>
#> 1     4     0         26  
#> 2     4     1         26.7
#> 3     6     0         20.6
#> 4     6     1         19.1
#> 5     8     0         15.1
```

### Desagrupar tablas de resumen

Usar `ungroup()`.

La siguiente tabla es el resultado de una agrupación y un cálculo de resumen. Como se usó más de una variable de agrupación, se mantiene agrupada.


```r
mtcars %>% 
    group_by(cyl, vs) %>% 
    summarise(promedio_mpg = mean(mpg))
#> `summarise()` has grouped output by 'cyl'. You can override using the `.groups` argument.
#> # A tibble: 5 x 3
#> # Groups:   cyl [3]
#>     cyl    vs promedio_mpg
#>   <dbl> <dbl>        <dbl>
#> 1     4     0         26  
#> 2     4     1         26.7
#> 3     6     0         20.6
#> 4     6     1         19.1
#> 5     8     0         15.1
```

Es posible desagruparla de la siguiente manera:


```r
mtcars %>% 
    group_by(cyl, vs) %>% 
    summarise(promedio_mpg = mean(mpg)) %>% 
    ungroup()
#> `summarise()` has grouped output by 'cyl'. You can override using the `.groups` argument.
#> # A tibble: 5 x 3
#>     cyl    vs promedio_mpg
#>   <dbl> <dbl>        <dbl>
#> 1     4     0         26  
#> 2     4     1         26.7
#> 3     6     0         20.6
#> 4     6     1         19.1
#> 5     8     0         15.1
```

## Transformar observaciones

### Filtrar observaciones que cumplen un criterio

Usar `filter()`. Dentro de la función debe usarse una operación lógica.

A partir de la siguiente tabla:


```r
mtcars
#> # A tibble: 32 x 11
#>      mpg   cyl  disp    hp  drat    wt  qsec    vs    am
#>    <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl>
#>  1  21       6  160    110  3.9   2.62  16.5     0     1
#>  2  21       6  160    110  3.9   2.88  17.0     0     1
#>  3  22.8     4  108     93  3.85  2.32  18.6     1     1
#>  4  21.4     6  258    110  3.08  3.22  19.4     1     0
#>  5  18.7     8  360    175  3.15  3.44  17.0     0     0
#>  6  18.1     6  225    105  2.76  3.46  20.2     1     0
#>  7  14.3     8  360    245  3.21  3.57  15.8     0     0
#>  8  24.4     4  147.    62  3.69  3.19  20       1     0
#>  9  22.8     4  141.    95  3.92  3.15  22.9     1     0
#> 10  19.2     6  168.   123  3.92  3.44  18.3     1     0
#> # ... with 22 more rows, and 2 more variables: gear <dbl>,
#> #   carb <dbl>
```

Se puede filtrar las observaciones cuyo valor de **mpg** sea mayor a 20.


```r
mtcars %>% 
    filter(mpg > 20)
#> # A tibble: 14 x 11
#>      mpg   cyl  disp    hp  drat    wt  qsec    vs    am
#>    <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl>
#>  1  21       6 160     110  3.9   2.62  16.5     0     1
#>  2  21       6 160     110  3.9   2.88  17.0     0     1
#>  3  22.8     4 108      93  3.85  2.32  18.6     1     1
#>  4  21.4     6 258     110  3.08  3.22  19.4     1     0
#>  5  24.4     4 147.     62  3.69  3.19  20       1     0
#>  6  22.8     4 141.     95  3.92  3.15  22.9     1     0
#>  7  32.4     4  78.7    66  4.08  2.2   19.5     1     1
#>  8  30.4     4  75.7    52  4.93  1.62  18.5     1     1
#>  9  33.9     4  71.1    65  4.22  1.84  19.9     1     1
#> 10  21.5     4 120.     97  3.7   2.46  20.0     1     0
#> 11  27.3     4  79      66  4.08  1.94  18.9     1     1
#> 12  26       4 120.     91  4.43  2.14  16.7     0     1
#> 13  30.4     4  95.1   113  3.77  1.51  16.9     1     1
#> 14  21.4     4 121     109  4.11  2.78  18.6     1     1
#> # ... with 2 more variables: gear <dbl>, carb <dbl>
```

Para más ejemplos de operaciones lógicas revisa la sección **intertar referencia**.

### Filtrar observaciones que cumplen más de un criterio

Usar `filter()`. Dentro de la función debe usarse una operación lógica.

A partir de la siguiente tabla:


```r
mtcars
#> # A tibble: 32 x 11
#>      mpg   cyl  disp    hp  drat    wt  qsec    vs    am
#>    <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl>
#>  1  21       6  160    110  3.9   2.62  16.5     0     1
#>  2  21       6  160    110  3.9   2.88  17.0     0     1
#>  3  22.8     4  108     93  3.85  2.32  18.6     1     1
#>  4  21.4     6  258    110  3.08  3.22  19.4     1     0
#>  5  18.7     8  360    175  3.15  3.44  17.0     0     0
#>  6  18.1     6  225    105  2.76  3.46  20.2     1     0
#>  7  14.3     8  360    245  3.21  3.57  15.8     0     0
#>  8  24.4     4  147.    62  3.69  3.19  20       1     0
#>  9  22.8     4  141.    95  3.92  3.15  22.9     1     0
#> 10  19.2     6  168.   123  3.92  3.44  18.3     1     0
#> # ... with 22 more rows, and 2 more variables: gear <dbl>,
#> #   carb <dbl>
```

Se puede filtrar las observaciones cuyo valor de **mpg** sea mayor a 20 y menor que 25. La coma (,) dentro de `filter()` cumple la función de un operador Y lógico (&).


```r
mtcars %>% 
    filter(mpg > 20, mpg < 25)
#> # A tibble: 8 x 11
#>     mpg   cyl  disp    hp  drat    wt  qsec    vs    am
#>   <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl>
#> 1  21       6  160    110  3.9   2.62  16.5     0     1
#> 2  21       6  160    110  3.9   2.88  17.0     0     1
#> 3  22.8     4  108     93  3.85  2.32  18.6     1     1
#> 4  21.4     6  258    110  3.08  3.22  19.4     1     0
#> 5  24.4     4  147.    62  3.69  3.19  20       1     0
#> 6  22.8     4  141.    95  3.92  3.15  22.9     1     0
#> 7  21.5     4  120.    97  3.7   2.46  20.0     1     0
#> 8  21.4     4  121    109  4.11  2.78  18.6     1     1
#> # ... with 2 more variables: gear <dbl>, carb <dbl>
```

Para más ejemplos de operaciones lógicas revisa la sección **intertar referencia**.

### Filtrar observaciones que cumplen un criterio u otro

Usar `filter()`. Dentro de la función debe usarse una operación O lógica.

A partir de la siguiente tabla:


```r
mtcars
#> # A tibble: 32 x 11
#>      mpg   cyl  disp    hp  drat    wt  qsec    vs    am
#>    <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl>
#>  1  21       6  160    110  3.9   2.62  16.5     0     1
#>  2  21       6  160    110  3.9   2.88  17.0     0     1
#>  3  22.8     4  108     93  3.85  2.32  18.6     1     1
#>  4  21.4     6  258    110  3.08  3.22  19.4     1     0
#>  5  18.7     8  360    175  3.15  3.44  17.0     0     0
#>  6  18.1     6  225    105  2.76  3.46  20.2     1     0
#>  7  14.3     8  360    245  3.21  3.57  15.8     0     0
#>  8  24.4     4  147.    62  3.69  3.19  20       1     0
#>  9  22.8     4  141.    95  3.92  3.15  22.9     1     0
#> 10  19.2     6  168.   123  3.92  3.44  18.3     1     0
#> # ... with 22 more rows, and 2 more variables: gear <dbl>,
#> #   carb <dbl>
```

Se puede filtrar las observaciones cuyo valor de **mpg** sea menor a 15 o mayor a 30.


```r
mtcars %>% 
    filter(mpg < 15 | mpg > 30)
#> # A tibble: 9 x 11
#>     mpg   cyl  disp    hp  drat    wt  qsec    vs    am
#>   <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl>
#> 1  14.3     8 360     245  3.21  3.57  15.8     0     0
#> 2  10.4     8 472     205  2.93  5.25  18.0     0     0
#> 3  10.4     8 460     215  3     5.42  17.8     0     0
#> 4  14.7     8 440     230  3.23  5.34  17.4     0     0
#> 5  32.4     4  78.7    66  4.08  2.2   19.5     1     1
#> 6  30.4     4  75.7    52  4.93  1.62  18.5     1     1
#> 7  33.9     4  71.1    65  4.22  1.84  19.9     1     1
#> 8  13.3     8 350     245  3.73  3.84  15.4     0     0
#> 9  30.4     4  95.1   113  3.77  1.51  16.9     1     1
#> # ... with 2 more variables: gear <dbl>, carb <dbl>
```

Para más ejemplos de operaciones lógicas revisa la sección **intertar referencia**.
