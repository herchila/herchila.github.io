---
layout: post
title:  "Primeros pasos con Django"
date:   2015-02-14 20:03:00
categories: django, virtualenv
---
La idea de este post es que, en poco tiempo, puedan crear una aplicación web con Django.

## Armando entorno de desarrollo con virtualenv

**¿Qué es virtualenv?**

virtualenv, es una herramienta para crear entornos virtuales (aislados) en Python.

**Por qué crear entornos virtuales en Python**

Cuando se está desarrollando con Python, es común utilizar diferentes versiones de un mismo paquete. Por ejemplo, estas trabajando con la versión 2.7 de Python y querés empezar a migrar tu proyecto a la versión 3.4 de Python. La solución a esto consiste en crear entornos virtuales (virtualenvs), uno para cada versión de Python. Esto permite trabajar en un espacio completamente independiente entre los entornos y de los paquetes instalados globalmente en el sistema.

Esto quiere decir que podes instalar Python 2.7 en un entorno virtual y Python 3.4 en otro entorno diferente o de forma global sin problema.

### Instalación

> La instalación de todo lo necesario para llevar a cabo este tutorial, se hará sobre Ubuntu/Debian

Abrimos la terminal (o consola) y escribimos lo siguiente:

{% highlight bash %}
~$ sudo apt-get update
{% endhighlight %}

Luego instalamos PIP:

{% highlight bash %}
~$ sudo apt-get install python-pip
{% endhighlight %}

Una vez instalado PIP, podemos instalar Virtualenv:

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

### Cómo activar un Python virtualenv

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

## Instalando Django

Primero hay que asegurarse de tener activado el entorno de desarrollo (virtualenv):

{% highlight bash %}
~$ source envs/mi_proyecto/bin/activate
(mi_proyecto) ~$
{% endhighlight %}

Chequeamos que nuestro entorno esté limpio:

{% highlight bash %}
(mi_proyecto) ~$ pip freeze         # no muestra nada
{% endhighlight %}

Ahora si instalamos Django:

{% highlight bash %}
(mi_proyecto) ~$ pip install Django
{% endhighlight %}

Ejecutamos nuevamente `pip freeze`:

{% highlight bash %}
(mi_proyecto) ~$ pip freeze
Django==1.7.4
argparse==1.2.1
wsgiref==0.1.2
{% endhighlight %}

Listo! Ya tenemos Django instalado en el entorno virtual “miproyecto” :)

## Creando el primer proyecto con Django

El primer paso es crear un nuevo proyecto Django. Básicamente, quiere decir que vamos a correr algunos scripts propios de Django que nos ayudará en la creación de la estructura de archivos de Django.
Los nombres de algunos archivos y directorios son muy importantes para Django. No deberías cambiar los nombres, tampoco moverlos a otro lugar.

**Bueno, arrancamos!**

Primero creamos la carpeta “proyectos” (en cualquier otro lado fuera de la carpeta “envs”) y entramos a la misma para crear el proyecto. Esto lo hacemos de la siguiente manera **(recordá que siempre tenemos que tener activo el entorno virtual)**: 

{% highlight bash %}
~$ source envs/mi_proyecto/bin/activate
(mi_proyecto)~$ mkdir proyectos
(mi_proyecto)~$ cd proyectos
(mi_proyecto)~/proyectos$ mkdir mi_proyecto
(mi_proyecto)~/proyectos$ cd mi_proyecto
(mi_proyecto)~/proyectos/mi_proyecto$ django-admin.py startproject mi_proyecto .
{% endhighlight %}

> No te olvides del punto final (.), esto indica que vas a crear el proyecto en la carpeta actual.

Al finalizar va a crear una carpeta con el nombre que le pusimos al proyecto quedando de la siguiente forma:

{% highlight bash %}
proyectos
  ├── mi_proyecto
     ├── manage.py
     └── mi_proyecto
             └── settings.py
             └── urls.py
             └── wsgi.py
             └── __init__.py
{% endhighlight %}

### Especificar dependencias con PIP

{% highlight bash %}
(mi_proyecto)~/proyectos/mi_proyecto$ pip freeze > requirements.txt
{% endhighlight %}

Esto produce un archivo de requerimientos en el formato que pip puede entender, el archivo de requerimientos detalla las dependencias que tiene un ambiente, su formato acepta tanto orígenes del pypi, paquetes en formato tar.gz repositorios en git, mercurial y svn. Comúnmente nos encontraremos de un requirements.txt en muchos proyectos.

Si ejecutamos `cat requirements.txt` vamos a ver el contenido del archivo requirements.txt

{% highlight bash %}
Django==1.7.4
argparse==1.2.1
wsgiref==0.1.2
{% endhighlight %}

### Levantar el servidor y ver nuestro proyecto en el navegador

En este punto ya tenemos la base de nuestro proyecto y ya podemos levantar el servidor web ejecutando:

{% highlight bash %}
(mi_proyecto)~/proyectos/mi_proyecto: $ python manage.py runserver
Validating models...

0 errors found
February 09, 2015 - 17:59:46
Django version 1.7.4, using settings 'mi_proyecto.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C.
{% endhighlight %}

Ingresamos a http://127.0.0.1:8000/

![Django It worked]({{ site.url }}/assets/django-it-worked.png)

¡Excelente! Acabas de crear tu primer sitio web y ejecutarlo mediante un servidor web! ¿No es genial?

**En el próximo post voy hablar sobre los Modelos y cómo crear nuestra primer aplicación.**
