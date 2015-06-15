SSH sin contraseña y de forma segura usando pareja de claves RSA {.blog-post-title}
----------------------------------------------------------------

Todos sabemos que ssh es un protocolo muy seguro, ya que todo la
información que circula por la conexión entre cliente y servidor está
encriptada.

Cuando hacemos una conexión a un servidor ssh lo hacemos así:\
  

Código: [Seleccionar]

ssh [usuario@direcion.ip.com](mailto:usuario@direcion.ip.com)\
El servidor y el cliente ssh negocian el protocolo ssh que se usará (por
seguridad debe ser la versión 2)

Y después generan un acuerdo de cifrado para el canal, de manera que ya
todo lo que circule por él, esté cifrado.

La conexión continua..

Una vez abierto un canal seguro, el servidor nos pedirá contraseña para
acceder y tras introducirla el servidor la comprobará.

Si la clave es correcta obtendremos acceso al servidor ssh.

Aquí se presentan dos inconvenientes.

1.- El password, aunque encriptado, viaja por la red cada vez que nos
conectamos y esto puede ser peligroso si alguien estuviese capturando
(snifando) datos de nuestra red y fuese capaz de generar fuerza bruta
contra nuestros datos para sacar el pass.

2.- Tener que meter una y otra vez la clave cuando iniciamos sesión es
'cansino' y además, si tenemos 20 servidores y los que acceder....
Tenemos que tener 20 claves diferentes, ya que si usamos una sola para
todo compromete la seguridad de los 20 pcs si perdemos la clave o
alguien nos la roba.

Para todo esto hay una solución, usar un par de claves RSA.

 

Explicación sobre pareja de claves RSA

**¿Que es RSA?**

RSA es un sistema de encriptado que usa un algoritmo imposible de
solucionar a día de hoy. Se creé que con la llegada de sistemas
cuánticos y su rapidez se puedan llegar a solucionar.

**¿Por qué una pareja?**\
Esto mucha gente no lo entiende, y lo voy a explicar muy claro.

Es una pareja de claves porque se crean a pares y de forma que una no
sirve sin la otra, una clave es privada y la otra pública.

Clave privada:\
Se usa para desencriptar y es la que debe estar solo presente en la
máquina cliente y protegerla.

Clave  pública:\
Se usa para encriptar y se puede repartir sin miedo a tus amigos.\
Esta clave solo encripta y no puede usarse para desencriptar ni lo que
se haya encriptado con ella.\
Solo la clave privada asociada a ella puede desencriptar lo que se haya
encriptado con la clave pública.

**¿Y como funciona?**\
El proceso es el mismo pero con una diferencia...\
  

Código: [Seleccionar]

ssh [usuario@direcion.ip.com](mailto:usuario@direcion.ip.com)\
El servidor y el cliente ssh negocian el protocolo ssh que se usará (por
seguridad debe ser la versión 2)\
Y después generan un acuerdo de cifrado para el canal, de manera que ya
todo lo que circule por él, esté cifrado.\
La conexión continua..

Hasta aquí todo igual, pero ahora comienza la diferencia.....:

El cliente decide usar usar RSA para identificarse, así que le manda al
servidor información sobre su clave pública..

El servidor busca en su base da datos en busca de la clave pública del
cliente, cuando la encuentra le manda un desafío, si si, como lo oyes.
El servidor manda un paquete llamado (challenge) que contiene un número
aleatorio cifrado con la clave pública del cliente.

El cliente recibe el paquete incriptado y usa la clave privada para
desencriptar el mensaje, una vez desencriptado se lo devuelve al
server... y le dice "¡Eh mira, lo he averiguado, así que soy yo!".

El servidor comprueba que el número devuelto es igual que el que mandó
encriptado y por tanto el usuario es el que dice ser, ya que ninguna
otra persona puede desencriptar el paquete sin no tiene la clave privada
asociada a la pública. Así que se permite la sesión.

Yo creo que explicado así se entiende a la perfección.

¿Os habéis fijado que en ningún momento se mandan claves por la red?

Ahora podrías dar a todos tus amigos tu cláve pública y así poder entrar
en sus sistemas sin pedir contraseña ni tener que usar una contraseña
para cada sitio.

Como podréis imaginar, si alguien os copia la clave privada de vuestro
ordenador, podrá acceder a cualquier sistema en el que estén la clave
pública..... que peligro ¿no?

Tranquilos, generaremos una clave RSA que esté encriptada con clave, de
manera que si alguien os la quita se quedará con la cara partida porque
hace falta una clave para poder usarla... jejeje.

 

Preparación del Servidor para permitir autenticación por RSA

Accedemos a nuestro servidor:\
  

Código: [Seleccionar]

ssh [usuario@servidor.com](mailto:usuario@servidor.com)\
Tendremos que meter la clave como siempre.

Una vez dentro tendremos que editar el archivo */etc/ssh/sshd\_config*\
  

Código: [Seleccionar]

sudo nano /etc/ssh/sshd\_config\
Buscamos las siguientes líneas y nos aseguramos de que estén así:\
  

Citar

> PubkeyAuthentication yes\
>  AuthorizedKeysFile      %h/.ssh/authorized\_keys

 

La primara opción permite el uso de clave pública para que un cliente se
identifique.\
La segunda especifíca donde se guarda la relación de claves públicas
autorizadas.

Una vez modificado lo que sea necesario presionamos Ctrl+o para guardar,
presionamos "enter" para confirmar y luego Ctrl+x para salir.

Comprobamos que existe la carpeta .ssh dentro del directorio home, el
cual debe de contener el archivo authorized\_keys\
  

Código: [Seleccionar]

ls -al  \~\
Si no existe lo crearemos\
  

Código: [Seleccionar]

mkdir \~/.ssh\
 touch \~/.ssh/authorized\_keys\
En ese archivo se guardan las claves públicas de los clientes
autorizados.\
Necesitamos reiniciar el demonio ssh y normalmente eso no supone una
caida del cliente que está conectado a ssh... así que:\
  

Código: [Seleccionar]

sudo service ssh restart\
Ahora ya podemos salir del ssh:\
  

Código: [Seleccionar]

exit

 

Configuración del Cliente para usar RSA

El cliente de la versión actual de ssh está configurado para usar en
primera instancia automáticamente una privada RSA para identificarse,
así que solo tenemos que crearla.

 

Creación de parejas de claves

Ahora llega el momento de crear nuestra pareja de claves, es importante
comprender que podemos crear la pareja de claves en cualquier pc y
llevarlas a donde queramos.... pero en este caso las crearemos en el
cliente.

Primero comprobamos que existe la carpeta .ssh dentro del directorio
home, el cual es necesario ya que es ahí donde se generarán la pareja de
claves.\
  

Código: [Seleccionar]

ls -al  \~\
Si no existe lo crearemos\
  

Código: [Seleccionar]

mkdir \~/.ssh\
Ahora generamos la pareja de claves RSA\
  

Código: [Seleccionar]

ssh-keygen -t rsa\
Nota: ssh-keygen genera por defecto claves RSA pero por si las moscas lo
hemos especificado.

Creará una clave de 1028 Bits, muy segura, aunque se pueden crear
mayores claves con menor rendimiento... claro está.

Tras ejecutar el comando nos preguntará donde generar la clave, le
dejamos la ruta por defecto \~/.ssh/id\_rsa

Seguidamente nos pedirá una clave de paso, yo uso la misma que para mi
sesión, pero podéis usar cualquier clave que sea segura, que contenga
numeros, símbolos y letras en minúscula y mayúscula.

Una vez terminado tendréis una clave privada (id\_rsa) y otra pública
(ud\_rsa.pub) en el directorio .ssh de vuestra carpeta personal.

Ahora moveremos la llave pública al servidor usando scp, que pertenese
al paquete de ssh y permite copiar contenido usando ssh de forma
segura:\
  

Código: [Seleccionar]

scp \~/.ssh/id\_rsa.pub
[usuario@servidor.com](mailto:usuario@servidor.com):/tmp/\
Y metemos la clave para mandar la llave pública a la carpeta temporal
del server.

 

Añadiendo la clave pública al servidor ssh

Tras copiar la clave pública en el server hay que añadirlo al archivo
authorized\_keys\
Accedemos al servidor ssh:\
  

Código: [Seleccionar]

ssh [usuario@servidor.com](mailto:usuario@servidor.com)\
Y ejecutamos:\
  

Código: [Seleccionar]

cat /tmp/id\_rsa.pub \>\> \~/.ssh/authorized\_keys\
Listo, ya podemos salir del server y al entrar no usaremos clave para
iniciar sesión, y ojo, sigue leyendo.\
  

Código: [Seleccionar]

exit

 

Usar ssh-agent para evitar la frase de paso

Si ya has intentado acceder pos ssh para probar la conexión te habrás
percatado de que te pide contraseña.... ehhh! jeje para para, no me
llames mentirosooooo jejee

Dije que no te pediría clave para iniciar sesión, y de hecho, no lo
hace, lo que pasa es que tu clave privada está cifrada ¿Recuerdas que al
crearla pusiste una clave para protegerla?

Usaremos ssh-agent, que se instala automáticamente al instalar ssh y que
se ejecuta al inicio de cada sesión de manera automática.

Usaremos ssh-add para añadir la clave a ssh-agent de manera que esté
vinculada a la sesión y no te pida más la pass para desencriptar la
clave privada.... ya se encargará ssh de pedírsela a ssh-agent.\
  

Código: [Seleccionar]

ssh-add \~/.ssh/id\_rsa\
Eso nos pedirá la frase de paso que usamos para crearla, y la añadirá a
la base de datos.

Prueba ahora y verás...

Ya puedes usar tu clave privada sin temor, y compartir la pública para
poder acceder a otros servidores o pc's sin tener que memorizar gran
cantidad de claves.

Nota: Tras reiniciar o cerrar la sesión actual en tu sistema linux, se
te volverá a pedir la clave de paso, pero puedes hacer que no te la
vuelva a pedir pinchando en la opción "Desbloquear al iniciar sesión"
.... o algo así XD

Bien, para terminar os recomiendo que impidáis el acceso mediante clave
al server ssh.

¿Para qué?

Pues para evitar ataque de fuerza bruta y que intenten sacar el pass.

Accedemos a nuestro servidor:\
  

Código: [Seleccionar]

ssh [usuario@servidor.com](mailto:usuario@servidor.com)\
Ya no tendríamos que meter la clave... jejeje\
Una vez dentro tendremos que editar nuevamente el archivo
/etc/ssh/sshd\_config\
  

Código: [Seleccionar]

sudo nano /etc/ssh/sshd\_config\
Buscamos las siguientes líneas y nos aseguramos de que estén así:\
  

Código: [Seleccionar]

\#PasswordAuthentication yes\
Y la dejamos así:\
  

Código: [Seleccionar]

PasswordAuthentication no\
Ahora reinicias el servicio ssh\
  

Código: [Seleccionar]

sudo service ssh restart\
Listo, ya no os pedirá contraseña normal y exigirá identificación por
RSA.

Enviado por jguzman el Sáb, 06/14/2014 - 15:50

-   [blog de
    jguzman](/es/blog/1 "Leer últimas entradas al blog de jguzman.")

