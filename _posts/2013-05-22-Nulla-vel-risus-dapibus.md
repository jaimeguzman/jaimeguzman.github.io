---
layout: post
title: Ejemplo de sintaxis para SCP
date: "2013-12-22 16:25:06 -0700"
comments: true
published: true
---


**SCP** permite que los archivos sean copiados a, desde o entre
diferentes **hosts**. Utiliza ssh para la transferencia de datos y
proporciona la misma autenticación y el mismo nivel de seguridad que
**ssh**.

**Ejemplos:**

#### **1)** Copiar un archivo "lala.txt" **desde un host remoto a tu localhost (remote -\> local)**

    $ scp tu_nombredecuentassh@remotehost.cl:lala.txt /some/local/directory

####  

#### **2)** Copiar el archivo "lala.txt" **desde un localhost a un host remoto (remote \<- local) .**

       $ scp lala.txt tu_nombredecuentassh@remotehost.cl:/some/remote/directory

####  

#### **3) **Copiar el directorio **"foo"**  desde **localhost a una capeta "bar" en un host remoto. ( "bar" remote \<- "foo" localhost )**

    $ scp -r foo tu_nombredecuentassh@remotehost.cl:/some/remote/directory/bar

####  

#### **4)**Copiar un archivo **"lala.txt"** desde un host remoto1** "rh1.cl"** a otro host to remoto  **"rh2.cl"**.**( remote1 -\> remote2 )**

               $ scp tu_nombredecuentassh@rh1.cl:/some/remote/directory/lala.txt \

                tu_nombredecuentassh@rh2.cl:/some/remote/directory/

####  

#### **5)** Copiar los archivos  "foo.txt" y "bar.txt" desde tu máquina local a tu carpeta "home" de tu host remote.  **(remote \ <- localhost: "foo.txt", "bar.txt"  )**

     
        scp foo.txt bar.txt tu_nombredecuentassh@remotehost.cl:~

####  

#### **6)**Copiar el archivo "lala.txt" desde localhost a un host remoto usando el  puerto 2264

    $ scp -P 2264 lala.txt tu_nombredecuentassh@remotehost.cl:/some/remote/directory

####  

#### **7)**Copiar multiples archivo en una máquina remota a  tu directorio actual en tu localhost.

    $ scp tu_nombredecuentassh@remotehost.cl:/some/remote/directory/\{a,b,c\} .

 
    $ scp tu_nombredecuentassh@remotehost.cl:~/\{foo.txt,bar.txt\} .

###  

### **scp** Performance

Por defecto **scp** utiliza el cifrado Triple-DES para cifrar los datos
que se envían. Usando el sistema de cifrado **Blowfish** se ha
demostrado que aumenta la velocidad. Esto se puede hacer mediante el uso
de la opción **-c blowfish** en el mismo terminal.

 

    $ scp -c blowfish some_file tu_nombredecuentassh@remotehost.cl:~

Se sugiere que la opción **-C** para la compresión también debe ser
utilizado para aumentar la velocidad. El efecto de la compresión, sin
embargo, aumentará significativamente la velocidad sólo si su conexión
es muy lenta. De lo contrario, sólo puede ser la adición de una carga
adicional en la CPU. Un ejemplo de la utilización de blowfish y
compresión:

    $ scp -c blowfish -C local_file tu_nombredecuentassh@remotehost.cl:~

### **Fuente: [http://www.hypexr.org/linux\_scp\_help.php](http://www.hypexr.org/linux_scp_help.php)**




