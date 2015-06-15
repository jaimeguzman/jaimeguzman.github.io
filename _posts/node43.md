MapReduce for dummies {.blog-post-title}
---------------------

Supongamos que tenemos que hace un programa que cuente las palabras de
esta frase:

![](https://unpocodejava.files.wordpress.com/2012/07/image00119.png?w=780 "image00119")

(sin incluir a, the,…)

Una solución tradicional consistiría es ir recorriendo secuencialmente
las palabras de nuestra frase y añadir a un HashMap como clave la
palabra y como valor el número de ocurrencias:

[![](https://unpocodejava.files.wordpress.com/2012/07/image0028.png?w=780 "image0028")](https://unpocodejava.files.wordpress.com/2012/07/image0028.png)

Pero imaginemos ahora que tenemos que contar todas las palabras de una
enciclopedia!!!, en este caso el procesamiento secuencial sería muy poco
óptimo y tardaría mucho.

Podríamos dividir la frase en 2 y hacer el mismo procesamiento sobre
cada una de estas partes almacenando las palabras en un
HashMap **(Map)**

[![](https://unpocodejava.files.wordpress.com/2012/07/image0034.png?w=780 "image0034")](https://unpocodejava.files.wordpress.com/2012/07/image0034.png)

Cuando hubiésemos contado las palabras de cada parte tendríamos que
combinarlos mapas de las dos sentencias en un Mapa único (**Reduce):**

[![](https://unpocodejava.files.wordpress.com/2012/07/image0047.png?w=780 "image0047")](https://unpocodejava.files.wordpress.com/2012/07/image0047.png)

Si ahora imaginamos que cada parte del texto la tenemos almacenada en
bloques HDFS y mapeamos las tareas en cada uno de los nodos entonces
podremos usar el poder de computación de cada nodo:

[![](https://unpocodejava.files.wordpress.com/2012/07/image0055.png?w=780 "image0055")](https://unpocodejava.files.wordpress.com/2012/07/image0055.png)

Cada nodo puede correr tareas (Map o Reduce)

Cada nodo puede tener almacenados Data Stores para múltiples ficheros,
en cada nodo pueden correr varias tareas.

Para controlar las tareas MapReduce existen dos procesos (Hadoop):

· **JobTracker** : es el servicio que pas alas tareas MapReduce a los
nodos específicos del cluster (idealmente cada nodo tundra los datos a
procesar)

· **TaskTracker: **es el proceso que arranca y sigue last areas
MapReduce en el cluster. Comunica con el JobTracker para la asignación
de tareas y el reporting de resultados

fuente: [https://unpocodejava.wordpress.com/2012/07/30/mapreduce-for-dummies/](https://unpocodejava.wordpress.com/2012/07/30/mapreduce-for-dummies/)

 

 

Enviado por jguzman el Sáb, 05/31/2014 - 15:36

-   [blog de
    jguzman](/es/blog/1 "Leer últimas entradas al blog de jguzman.")

