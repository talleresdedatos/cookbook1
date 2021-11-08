# Rmarkdown

## ¿Qué es Rmarkdown?

R Markdown nos provee de un marco de trabajo unificado para la ciencia de datos y el análisis de datos, combinando código, sus resultados, y los comentarios escritos por el autor. Los documentos R Markdown son totalmente reproducibles y soportan docenas de formatos de salida como PDFs, archivos Word, diapositivas, y más.

Uno de los objetivos de este taller es que los participantes aprendan a usar R Markdown y puedan encontrar la forma en que puede ayudarles a potenciar su trabajo.

Esta sección hace uso de la traducción al español del [capítulo R Markdown](https://r4ds-en-espaniol.netlify.app/r-markdown.html) de "R for Data Science".

Los archivos R Markdown están diseñados para ser usados de tres maneras:

-   Para comunicarte con los tomadores de decisiones, que quieren enfocarse en las conclusiones, no en el código detrás del análisis
-   Para colaborar con otros analistas de datos (incluyendo a tu futuro yo), quienes están interesados tanto en tus conclusiones como en la manera en que llegaste a ellas (el código)
-   Como un entorno en el que realizar ciencia de datos, como un cuaderno de trabajo moderno en el que puedes capturar no sólo lo que hiciste sino también en lo que estabas pensando.

Típicamente, un archivo R Markdown contiene tres tipos de contenido importantes

-   Un encabezado YAML (opcional) rodeado por --- (tres guiones seguidos)
-   Bloques de código rodeados de \`\`\` (acentos graves)
-   Texto mezclado con formato simple como \# encabezados, *cursivas* o **negritas**

Cuando abres un archivo .Rmd, se te muestra una interfaz de bloc de notas donde el código y sus resultados se intercalan. Puedes ejecutar cada bloque de código haciendo click en el botón "Run" (luce como un botón de *play* en la parte superior del bloque), o presionando Cmd/Ctrl + Shift + Enter. RStudio ejecuta el código y muestra los resultados seguidamente.

Para producir un reporte completo conteniendo todo el texto, código y resultados, haz click en "Knit" o presiona Cmd/Ctrl + Shift + K. Esto mostrará el reporte en un panel de Vista previa, y creará un archivo HTML que puedes compartir con otras personas.

Cuando haces knit el documento (knit significa tejer en inglés), R Markdown envía el .Rmd a knitr (<http://yihui.name/knitr/>) que ejecuta todos los bloques de código y crea un nuevo documento markdown (.md) que incluye el código y su output.

El archivo markdown generado por knitr es procesado entonces por pandoc (<http://pandoc.org/>) que es el responsable de crear el archivo terminado. La ventaja de este flujo de trabajo en dos pasos es que puedes crear un muy amplio rango de formatos de salida, que conocerás más adelante.
