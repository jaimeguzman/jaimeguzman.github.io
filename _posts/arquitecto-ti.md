<div class="post">
<a name="6982586837393253403"></a>
<h3 class="post-title">
<a href="http://informaticodesorientado.blogspot.cl/2009/01/arquitectura-ti-que-es-y-que-hace-un.html">Arquitectura TI: Que es y que hace un arquitecto de TI?</a>
</h3>
<div class="post-header-line-1"></div>
<div class="post-body">
<p><span style="font-weight: bold;">Introducción:</span><br>Previamente publiqué un post sobre la <a href="http://informaticodesorientado.blogspot.com/2009/01/algo-sobre-frameworks-y-arquitectura.html">Arquitectura MVC</a>   pero... qué es realmente la arquitectura de software? cuál es el rol del arquitecto? Hay herramientas que faciliten la labor del arquitecto?<br><br><span style="font-weight: bold;">Definición</span>:<br>La arquitectura de software, llamada también la arquitectura lógica, es el diseño de mas alto nivel de la estructura de un sistema, aunque hay varias definiciones y no hay prácticamente ninguna que sea totalmente aceptada, aunque la más oficial es la definición oficial de Arquitectura del Software es la <a href="http://standards.ieee.org/reading/ieee/std_public/description/se/1471-2000_desc.html" target="_blank">IEEE Std 1471-2000</a> que dice: “La Arquitectura del Software es la organización fundamental de un sistema formada por sus componentes, las relaciones entre ellos y el contexto en el que se implantarán, y los principios que orientan su diseño y evolución”.<br><br>Otra definición es la de Hohmann, 2003: “Arquitectura de software es la suma de los módulos complejos, procesos y los datos del sistema, su estructura y exactas relaciones entre sí, cómo puede ser y se espera que sea, sus extensiones y qué tecnologías participan, deducir las capacidades exactas y flexibilidad del sistema desde el cual se puede formar un plan para la implementación o modificación del sistema.”<br><br><span style="font-weight: bold;">El Rol del arquitecto de software:</span><a onblur="try {parent.deselectBloggerImageGracefully();} catch(e) {}" href="http://2.bp.blogspot.com/_hfuwb5m9Tp0/SW4E2LVNeVI/AAAAAAAABO8/rcKViLb48yU/s1600-h/role-profile-for-software-architects-v1.png"><img alt="" border="0" id="BLOGGER_PHOTO_ID_5291171941008111954" src="http://2.bp.blogspot.com/_hfuwb5m9Tp0/SW4E2LVNeVI/AAAAAAAABO8/rcKViLb48yU/s400/role-profile-for-software-architects-v1.png" style="margin: 0pt 10px 10px 0pt; float: left; cursor: pointer; width: 400px; height: 300px;"></a><br>El arquitecto es el encargado de definir:<br><span style="font-weight: bold;">QUE?</span><br>La union de requerimientos del cliente, el conocimiento de su entorno, de sus necesidades, de sus espectativas de crecimiento, dan como resultado un producto intangible que será la solución que se va a proveer considerando un modelamiento del negocio conformado por el equipo de analistas que apoyan cada proyecto.<br>Acá se hace un paralelo con un drector de orquesta, que debe hacer que cada uno de los músicos interpreten la música de acuerdo a la partitura.<br><br><span style="font-weight: bold;"><br>COMO?</span><br>Una vez que está definido el qué se va a hacer, definidos ya el modelo de negocios, con los analistas que conforman el negocio, se confecciona la documentación técnica del proyecto que contiene análisis, modelos y diseño de la solución a los requerimientos del cliente. El arquitecto cumple su parte como organizador, facilitador de definiciones y como validador del modelamiento.<br><br><span style="font-weight: bold;">CON QUE?</span><br>Nuevamente el arquitecto entra en su rol de "director de orquesta" esta vez con otro equipo de trabajo, conformado por un jefe de proyectos (que tiene una metodología) y un experto técnico con un framework de trabajo bien definido y consolidado para definir cada una de las partes de la solución, esta vez se toma mas en detalle a la solución, se toman decisiones que deben estar de acuerdo a la tecnología que vaya ser utilizada por el equipo de desarrollo y se define presupuesto y cronograma del proyecto (a esta altura ya debiera estar bien definido el equipo de trabajo del proyecto). Lo anterior en teoría, porque según mi experiencia, el arquitecto no se entromete en temas de presupuesto ni de cronograma del proyecto dado que el líder técnico (jefe de proyectos) debe conformar su equipo y planificar las actividades de su equipo, aunque esto puede variar dependiendo de las empresas cuando el arquitecto también es el jefe de proyectos.<br><br><span style="font-weight: bold;">Productos resultantes de la ingeniría de software</span>:<br>No hay un consenso en cuanto al lenguaje y la forma en que el arquitecto entregue los elementos de ayuda a la toma de decisiones y que permitan la comunicación en un lenguaje común con todos los actores del proyecto. Acá, dependerá de la tendencia de la arquitectura escogida.<br>Uno de los modelos más usados y conocidos es el “4+1” de Philippe Kruchten, vinculado al Rational Unified Process (RUP), que define cuatro vistas diferentes:  </p><ul><li> Vista lógica: describe el modelo de objetos.  </li><li> Vista de proceso: muestra la concurrencia y sincronía de los procesos. </li><li> Vista física: muestra la ubicación del software en el hardware.  </li><li> Vista de desarrollo: describe la organización del entorno de desarrollo.  </li><li> Existe una quinta vista que consiste en una selección de casos de uso o de escenarios que los arquitectos pueden elaborar a partir de las cuatro vistas anteriores. </li></ul><span style="font-weight: bold;">Ejemplos de Arquitectura de Software</span>:<br>Para ilustrar y clarificar un poco lo anteior, se muestran dos diagramas de arquitectura en un entorno J2EE. Ambos diagramas están disponibles en <a href="http://java.sun.com/blueprints/guidelines/designing_enterprise_applications_2e/">Designing Enterprise Applications with the J2EE Platform, Second Edition</a> de SUN.<br><br>El primer diagrama consiste en una vista lógica que muestra los componentes y servicios típicos de un entorno J2EE.<br><a onblur="try {parent.deselectBloggerImageGracefully();} catch(e) {}" href="http://1.bp.blogspot.com/_hfuwb5m9Tp0/SW3_yDvFivI/AAAAAAAABOs/5L8lhCNGxnI/s1600-h/j2ee_envir.jpg"><img alt="" border="0" id="BLOGGER_PHOTO_ID_5291166372691544818" src="http://1.bp.blogspot.com/_hfuwb5m9Tp0/SW3_yDvFivI/AAAAAAAABOs/5L8lhCNGxnI/s400/j2ee_envir.jpg" style="margin: 0px auto 10px; display: block; text-align: center; cursor: pointer; width: 350px; height: 263px;"></a>Por otro lado, en el modelo MVC tenemos una vista de procesos que se relacionan entre las capas Modelo, Vista y Controlador en un entorno J2EE.<br><a onblur="try {parent.deselectBloggerImageGracefully();} catch(e) {}" href="http://2.bp.blogspot.com/_hfuwb5m9Tp0/SW4D4gTXvtI/AAAAAAAABO0/uMhIUTwv0wE/s1600-h/mvc_2.jpg"><img alt="" border="0" id="BLOGGER_PHOTO_ID_5291170881485651666" src="http://2.bp.blogspot.com/_hfuwb5m9Tp0/SW4D4gTXvtI/AAAAAAAABO0/uMhIUTwv0wE/s400/mvc_2.jpg" style="margin: 0px auto 10px; display: block; text-align: center; cursor: pointer; width: 386px; height: 275px;"></a><br><br>Hasta acá lo mas relevante con respecto a la arquitectura. En otros post posteriores seguramente mencionaré temas asociados como SOA (Arquitectura orientada a servicios), Usabilidad y Patrones y antipatrones de diseño.<br><br><span style="font-weight: bold;">Recursos:</span> <a href="http://msdn.microsoft.com/en-us/architecture/bb201656.aspx">The architecture journalist</a>; revista del MSDN (Microsoft Development Network) especialmente dedicada a temas de arquitectura. Hay varios números en español aunque atrasados con respecto a las publicaciones en inglés.<p></p>
<div style="clear: both;"></div>
</div>
<div class="post-footer">
<p class="post-footer-line post-footer-line-1"><span class="post-author">
Publicado por
Vicente
</span>
<span class="post-timestamp">
en
<a class="timestamp-link" href="http://informaticodesorientado.blogspot.cl/2009/01/arquitectura-ti-que-es-y-que-hace-un.html" title="permanent link">5:48</a>
</span>
<span class="post-comment-link">
</span>
<span class="post-icons">
<span class="item-control blog-admin pid-1407438901">
<a href="https://www.blogger.com/post-edit.g?blogID=1713956151778658720&amp;postID=6982586837393253403&amp;from=pencil" title="Editar entrada">
<span class="quick-edit-icon">&nbsp;</span>
</a>
</span>
</span>
</p>
<p class="post-footer-line post-footer-line-2"><span class="post-labels">
Etiquetas:
<a href="http://informaticodesorientado.blogspot.cl/search/label/Arquitectura" rel="tag">Arquitectura</a>,
<script src="/feeds/posts/default/-/Arquitectura?alt=json-in-script&amp;callback=related_results_labels&amp;max-results=5" type="text/javascript"></script>
<a href="http://informaticodesorientado.blogspot.cl/search/label/J2EE" rel="tag">J2EE</a>,
<script src="/feeds/posts/default/-/J2EE?alt=json-in-script&amp;callback=related_results_labels&amp;max-results=5" type="text/javascript"></script>
<a href="http://informaticodesorientado.blogspot.cl/search/label/MVC" rel="tag">MVC</a>,
<script src="/feeds/posts/default/-/MVC?alt=json-in-script&amp;callback=related_results_labels&amp;max-results=5" type="text/javascript"></script>
<a href="http://informaticodesorientado.blogspot.cl/search/label/RUP" rel="tag">RUP</a>
<script src="/feeds/posts/default/-/RUP?alt=json-in-script&amp;callback=related_results_labels&amp;max-results=5" type="text/javascript"></script>
</span>
</p>
<p class="post-footer-line post-footer-line-3"></p>
Artigos Relacionados:
<script type="text/javascript">

removeRelatedDuplicates();

printRelatedLabels();

</script><ul><li><a href="http://informaticodesorientado.blogspot.com/2009/01/arquitectura-ti-que-es-y-que-hace-un.html">Arquitectura TI: Que es y que hace un arquitecto de TI?</a></li><li><a href="http://informaticodesorientado.blogspot.com/2009/01/algo-sobre-frameworks-y-arquitectura.html">Algo sobre Frameworks y Arquitectura MVC y RIA (Rich Internet Applications)</a></li><li><a href="http://informaticodesorientado.blogspot.com/2012/04/video-tutorial-de-programacion-para.html">Video Tutorial de programación para Android</a></li><li><a href="http://informaticodesorientado.blogspot.com/2009/01/patrones-de-diseo-orientado-objetos.html">Patrones de diseño orientado a objetos</a></li></ul>
</div>
</div>
