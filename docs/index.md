--- 
title: "Cookbook de análisis de datos básico"
author: "Samuel Calderon"
date: "2022-04-21"
site: bookdown::bookdown_site
documentclass: book
bibliography: [book.bib, packages.bib]
# url: your book url like https://bookdown.org/yihui/bookdown
# cover-image: path to the social sharing image like images/cover.jpg
description: |
  This is a minimal example of using the bookdown package to write a book.
  The HTML output format for this example is bookdown::bs4_book,
  set in the _output.yml file.
biblio-style: apalike
csl: chicago-fullnote-bibliography.csl
---

# Presentación {#presentacion .unnumbered}

Este *cookbook* busca ser una fuente de consulta rápida para personas que inician el proceso de aprendizaje de R usando RStudio. La idea es tener un conjunto suficientemente grande de ejemplos para que usuarios novatos tengan una idea de lo que es posible hacer con R y no se preocupen por memorizar nombres de funciones sino soluciones a problemas concretos.

## Lo que contiene el libro {#sobre-el-contenido .unnumbered}

Debido a que este libro aspira a ser un material de consulta rápida contendrá sobretodo problemas planteados y una solución al respecto. 

## Lo que NO contiene el libro {.unnumbered}

El libro no abordará detalles teóricos sobre el código presentado. Asimismo, no se profundizará en temas de estadística o programación.

## ¿Para quién es este libro? {#para-quien .unnumbered}

Recomiendo este libro a tres tipos de personas:

1. Personas que interactúan con R por primera vez, posiblemente en el contexto de una clase de estadística
2. Docentes que enseñan estadística usando R
3. Personas que quieren usar R en algún proyecto y necesitan refrescar su memoria sobre lo que es posible hacer.

Para las personas del tipo 1 y 2, recomiendo que el uso del libro se vea acompañado de los videos correspondientes al curso "Análisis exploratorio de datos con R" disponible en Youtube. En el caso de los docentes, recomiendo ver el vídeo "Un problema al enseñar estadística con un software".

## Autor {-}

En elaboración...

Samuel Enrique Calderon Serrano:

-   Politólogo de la Universidad Antonio Ruiz de Montoya.

-   Actualmente trabaja en la Superintendencia Nacional de Educación Universitaria - SUNEDU como miembro del Equipo Técnico Normativo de la Dirección de Licenciamiento.

-   Miembro de la organización [DecideBien](https://github.com/DecideBienpe). Colabora ocasionalmente en iniciativas de código abierto.

-   Proviene de Lima, Perú.

-   Otros canales:

    -   Web: [www.samuelenrique.com](https://www.samuelenrique.com)
    -   Twitter: [\@samuel\_\_case](https://twitter.com/samuel__case)
    -   Github: [/calderonsamuel](https://github.com/calderonsamuel)
    
## Contenido del curso {-}

-   Público objetivo:

    -   Estudiantes o egresados de carreras de ciencias sociales, periodismo o educación con interés en aprender herramientas de análisis y visualización de datos.

-   Aprendizajes esperados:

    -   Elementos básicos del análisis de datos usando R a través de RStudio

        -   Importación de datos
        -   Limpieza y ordenamiento de datos
        -   Análisis exploratorio de datos ordenados (tidy data)

    -   Elaboración de reportes de análisis de datos usando R Markdown



## Software requerido {-}

Para la presente edición del taller es necesario contar con el siguiente software instalado:

1.  R programming language (versión 4.0.0 o superior)
2.  RStudio IDE (versión 1.4.0 o superior)

También se necesitan los siguientes paquetes de R:

1.  tidyverse (colección de paquetes)
2.  rmarkdown
