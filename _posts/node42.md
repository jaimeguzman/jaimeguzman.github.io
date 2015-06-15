Concepto de MapReduce {.blog-post-title}
---------------------

El concepto MapReduce lo introdujo Google en 2004 en el paper
“MapReduce: Simplified Data Processing on Large Clusters”.

El objetivo principal de **MapReduce** era permitir la computación
paralela sobre grandes colecciones de datos permitiendo abstraerse de
los grandes problemas de la computación distribuida.

MapReduce consta de 2 fases: Map y Reduce. Las
funciones **Map** y **Reduce** se aplican sobre pares de datos (clave,
valor).

· **Map** toma como entrada un par (clave,valor) y devuelve una lista de
pares (clave2,valor2)

Esta operación se realiza en paralelo para cada par de datos de entrada.

Luego el framework MapReduce (como Hadoop MapReduce) agrupa todos los
pares generados con la misma clave de todas las listas, creando una
lista por cada una de las claves generadas.

· **Reduce** se realiza en paralelo tomando como entrada cada lista de
las obtenidas en el Map y produciendo una colección de valores.

[![](http://unpocodejava.files.wordpress.com/2013/05/image0011.png?w=780)](http://unpocodejava.files.wordpress.com/2013/05/image0011.png)

El ejemplo más simple es el **WordCount** (el Hello World del MapReduce)
sobre un fichero o ficheros de texto.

Hadoop divide estos ficheros en bloques

[![](http://unpocodejava.files.wordpress.com/2013/05/image0085.jpg?w=780)](http://unpocodejava.files.wordpress.com/2013/05/image0085.jpg)

**MAP:**

y cada bloque se lo pasa a una tarea Map (Map Task),

Para esta entrada (línea de fichero de texto en el que el key es el
número de línea y value la línea:

![](http://unpocodejava.files.wordpress.com/2013/05/image0092.jpg?w=780)

en pseudocódigo tendríamos:

[![](http://unpocodejava.files.wordpress.com/2013/05/image0103.jpg?w=780)](http://unpocodejava.files.wordpress.com/2013/05/image0103.jpg)

Que generaría:

![](http://unpocodejava.files.wordpress.com/2013/05/image011.jpg?w=780)

Tras esto la función intermedia del framework agrupa los datos con la
misma clave, de tal forma que se obtienen tantos pares (clave,
listaValores) como tareas Reduce se ejecutarán.

**REDUCE:**

En nuestro caso cada tarea Reduce suma los valores de entrada y genera
una única salida con la palabra y el número de estas

En nuestro ejemplo le llegará:

[![](http://unpocodejava.files.wordpress.com/2013/05/image0122.jpg?w=780)](http://unpocodejava.files.wordpress.com/2013/05/image0122.jpg)

Tendremos nuestra función reduce:

[![](http://unpocodejava.files.wordpress.com/2013/05/image0131.jpg?w=780)](http://unpocodejava.files.wordpress.com/2013/05/image0131.jpg)

Que finalmente generará:

[![](http://unpocodejava.files.wordpress.com/2013/05/image0143.jpg?w=780)](http://unpocodejava.files.wordpress.com/2013/05/image0143.jpg)

El proceso completo:

[![](http://unpocodejava.files.wordpress.com/2013/05/image015.png?w=780)](http://unpocodejava.files.wordpress.com/2013/05/image015.png)

 

fuente: [http://unpocodejava.wordpress.com/2013/05/22/explicando-mapreduce/](http://unpocodejava.wordpress.com/2013/05/22/explicando-mapreduce/)

 

Enviado por jguzman el Sáb, 05/31/2014 - 15:31

-   [blog de
    jguzman](/es/blog/1 "Leer últimas entradas al blog de jguzman.")

