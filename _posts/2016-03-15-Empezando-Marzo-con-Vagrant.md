---
layout: post
title: "(es) Empezando el 2016 con Vagrant"
date: 2016-03-15 16:25:06 -0700
comments: true
---


# (es) Jugando con Vagrant y empezando a escribir algo !



Basicamente *vagrant* ha sido un de las herramientas que he utilizado por un largo tiempo 
para no ensuciar mi computador, es facil poder usar y crear cualquier ambiente de desarrollo, 
pero en muchas empresas y sistemas legacy de mi pais aún no se adopta. Dockers por otro lado ha llamado la atención constantemente aun cuando funcionan y se usan para distintos tipos de escenarios, ambos pueden crear un ambiente homologado de desarrollo. 

Este post es directamente para vagrant ya que dentro de las ventajas para crear ambientes de desarrollo local, es que es multiplaforma.

Se puede utilizar como agente virtualizador: Virtualbox o Vmware, en mi caso he dejado vmware solo para cuando quiero utilizar un ambiente completo de un sistema operativo y virtualbox para correr Dockers Machine y maquinas de Vagrant. Este último ocupa cajas para ir encapsulando lo escencial del sistema operativo y al igual que github se encuentra en repositorio general donde podemos encontrar un sin fin e inumberables distribuciones de boxes para cualquier fin que queramos.

Basandome en un post de *@greyfocus*, que ha sido uno de los que mas me ha gustado para por ejemplo crear un ambiente local para usar Jekyll para este blog.

PD: He traducido dentro de lo razonable, cualquier correción es bienvenida


## Configuración

Para instalar se requieren hacer lo siguientes pasos para instalar Jekyll, esto puede ser 
en windows/mac/osx, usando Vagrant:


1. Instalar Vagrant
2. Instalar VirtualBox
3. Instalar el plugin de vagrant: `vagrant plugin install vagrant-vbguest`




4. Clonar este repositorio el repo de [https://github.com/greyfocus](https://github.com/greyfocus), que tiene toda la magia:

```
  git clone https://github.com/greyfocus/vagrant-jekyll.git
```

## Comenzando la magia de vagrant

  ```
  vagrant up
  ```


## Trabajando con vagrant

El concepto es igual que tener un ambiente en tu sistema operativo local. Se usa el editor
favorito o IDE o lo que desees para complicarte la vida para modificar los archivos `.md` o 
simplemente `vi o vim`. En el archivo `Vagrantile` del repositorio que hemos clonado 
tiene una receta de como empezar y además un llamado a un archivo shell para hacer `provision`.
Esta receta lo que hace es que permite de manera declarativa indicar a Vagrant la boxes que usar
en este caso un `ubuntu`, la carpeta y el puerto al cual `forwardear`. Por defecto vagrant crea una carpeta compartida desde donde se ejecutra el `vagrant up` y el `guest` en la ruta `/vagrant`

Cuando ya esta listo ...y no hay que ser impacientes porque la demora no es exclusivamente porque tu computador sea una tortuga, mas bien porque en la "receta" muchas veces se instalan librerias ó se actualiza. En fin para conectars a la máquina Vagrant por consola `ssh`:

```vagrant ssh```


En nuestro caso particular, como estamos usando _jekyll_ ejecutamos `build` y `server` que son para hacer correr jekyll.

```
  jekyll build 
  
  jekyll serve --host 0.0.0.0

```
Como no configuramos ninguna IP, podemos ver corriendo nuestro maquina vagrant jekyll en localhost, osea: http://127.0.0.1:4000/ 


Happy vagrant up!

