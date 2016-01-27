---
layout: post
title:  "Primeros pasos con Django"
date:   2015-02-18 16:13:00
categories: django
---
![Django logo]({{ site.url }}/assets/django-logo-positive.png)

### Capítulos

- **Capítulo 1: Primeros pasos con Django**
- [Capítulo 2: Modelos en Django](https://herchila.github.io/django/2016/01/24/modelos-en-django.html)

La idea de este post es que, en poco tiempo, puedan crear el primer proyecto en Django.

En mi caso usaré la distribución **Ubuntu**, si usas otra distribución pude que éstas indicaciones varíen un poco. Para los que recién empiezan, Django es un Framework para el desarrollo de aplicaciones Web basado en el lenguaje de programación Python. Django es usado por grandes empresas para sus proyectos web como la NASA, New York Times, Disqus, Instagram, Pinterest, etc.

## Primero lo primero

Lo recomendable es crear un ambiente de desarrollo antes de empezar cualquier proyecto. No importa el lenguaje.
**[En el post anterior](http://herchila.github.io/python/virtualenv/2015/02/14/armando-entorno-de-desarrollo-en-python-con-virtualenv.html "Armando entorno de desarrollo con virtualenv")** escribí sobre cómo armar un entorno de desarrollo. Antes de seguir con este post, te recomiendo que veas el otro.

## Instalando Django

Una vez **[instlado y configurado nuestro entorno de desarrollo](http://herchila.github.io/python/virtualenv/2015/02/14/armando-entorno-de-desarrollo-en-python-con-virtualenv.html "Armando entorno de desarrollo con virtualenv")**, nos aseguramos de tenerlo activado:

{% highlight bash %}
~$ source envs/mi_proyecto/bin/activate
(mi_proyecto) ~$
{% endhighlight %}

Chequeamos que nuestro entorno esté limpio:

{% highlight bash %}
(mi_proyecto) ~$ pip freeze         # no muestra nada
{% endhighlight %}

**¡Ahora si instalamos Django!**

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

Ingresamos a [http://127.0.0.1:8000/](http://127.0.0.1:8000/)

![Django It worked]({{ site.url }}/assets/django-it-worked.png)

¡Excelente! Acabas de crear tu primer sitio web y ejecutarlo en un servidor web! ¿No es genial?

**[Próximo capítulo: Modelos en Django](https://herchila.github.io/django/2016/01/24/modelos-en-django.html)**
