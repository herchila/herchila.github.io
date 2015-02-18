---
layout: post
title:  "Armando entorno de desarrollo en Python con virtualenv"
date:   2015-02-14 20:15:00
categories: python virtualenv
---
# Armando entorno de desarrollo en Python con virtualenv

### ¿Qué es virtualenv?

virtualenv, es una herramienta para crear entornos virtuales (aislados) en Python.

### Por qué crear entornos virtuales en Python

Cuando se está desarrollando con Python, es común utilizar diferentes versiones de un mismo paquete. Por ejemplo, estas trabajando con la versión 2.7 de Python y querés empezar a migrar tu proyecto a la versión 3.4 de Python. La solución a esto consiste en crear entornos virtuales (virtualenvs), uno para cada versión de Python. Esto permite trabajar en un espacio completamente independiente entre los entornos y de los paquetes instalados globalmente en el sistema.

Esto quiere decir que podes instalar Python 2.7 en un entorno virtual y Python 3.4 en otro entorno diferente o de forma global sin problema.

### Instalación

Abrimos la terminal (o consola) y escribimos lo siguiente:

{% highlight bash %}
~$ sudo apt-get update
{% endhighlight %}

Luego instalamos Pip:

> Pip es el **gestor de paquetes de Python**, al igual que aptitude o apt-get puedes instalar paquetes con el comando pip install nombre_paquete, buscar paquetes con el comando pip search nombre_paquete o listar los paquetes instalados con el comando pip freeze.

{% highlight bash %}
~$ sudo apt-get install python-pip
{% endhighlight %}

Una vez instalado Pip, podemos instalar Virtualenv:

{% highlight bash %}
~$ sudo pip install virtualenv
{% endhighlight %}

### Cómo crear un Python virtualenv

Primero creamos la carpeta envs en el directorio que quieras (ej, en la carpeta Documents):

{% highlight bash %}
$ mkdir envs
$ cd envs
{% endhighlight %}

Luego creamos el entorno ejecutando el siguiente comando:

{% highlight bash %}
~/envs$ virtualenv mi_proyecto
{% endhighlight %}

Y como resultado crea la siguiente estructura:

{% highlight bash %}
envs
  ├── mi_proyecto
        └── bin
        └── include
        └── lib
{% endhighlight %}

En el directorio **bin/** se encuentran los ejecutables necesarios para interactuar con el virtualenv. En **include/** se encuentran algunos archivos de cabecera de C (archivos .h) necesarios para compilar algunas librerías de Python. Y finalmente en **lib/** se encuentra una copia de Python así como un directorio llamado **site-packages/** en el cual se aloja el código fuente de los paquetes Python instalados en el virtualenv.

### Cómo activar un Python virtualenv

{% highlight bash %}
~$ cd envs
~/envs$ cd mi_proyecto
~/envs/mi_proyecto$ source bin/activate
(mi_proyecto)~/envs/mi_proyecto$
{% endhighlight %}

### Cómo desactivar un Python virtualenv

{% highlight bash %}
(mi_proyecto) ~$ deactivate
~$
{% endhighlight %}

¿No fue sencillo?
Si tienes alguna duda o algo para mejorar, escribe un comentario y aprendemos todos.