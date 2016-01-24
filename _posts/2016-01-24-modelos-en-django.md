---
layout: post
title:  "Modelos en Django"
date:   2016-01-24 19:00:00
categories: django
---

Lo que vamos a crear ahora es algo que va a guardar todos los posts de un blog.

## Empecemos a crear un un modelo en Django

Un modelo en Django es un tipo especial de objeto que se guarda en la `base de datos`. Ahí es donde se almacenará la información sobre usuarios, posts del blog, etc. Utilizaremos una base de datos SQLite para almacenar nuestros datos. Este es el adaptador de base de datos predeterminada en Django.

## Creando la aplicación

Ahora vamos a crear una aplicación dentro del proyecto, de esta manera mantenemos un orden. Para crear una aplicación necesitamos ejecutar el siguiente comando en la consola:

{% highlight bash %}
(mi_proyecto)~/proyectos/mi_proyecto: $ python manage.py startapp blog
{% endhighlight %}

Notarás que se crea un nuevo directorio `blog` que contiene los siguientes archivos:

{% highlight bash %}
proyectos
  ├── mi_proyecto
     ├── manage.py
     └── mi_proyecto
             └── __init__.py
             └── settings.py
             └── urls.py
             └── wsgi.py
    └── blog
        ├── migrations
            | __init__.py
        ├── __init__.py
        ├── admin.py
        ├── models.py
        ├── tests.py
        └── views.py
{% endhighlight %}

Después de crear una aplicación también hay que avisarle a Django que tiene que usarla:

{% highlight bash %}
INSTALLED_APPS = (
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'blog',
)
{% endhighlight %}

## Creando el modelo Post

En el archivo `blog/models.py` definimos todos los objetos llamados **Models** - este es un lugar en el cual definiremos nuestro modelo post.

Vamos abrir `blog/models.py`, quitamos todo y escribimos un código como este:

{% highlight bash %}
from django.db import models
from django.contrib.auth.models import User
from django.utils import timezone

class Post(models.Model):
    author = models.ForeignKey(User)
    title = models.CharField(max_length=200)
    text = models.TextField()
    created_date = models.DateTimeField(
            default=timezone.now)
    published_date = models.DateTimeField(
            blank=True, null=True)

    def publish(self):
        self.published_date = timezone.now()
        self.save()

    def __str__(self):
        return self.title
{% endhighlight %}

Todas las líneas que comienzan con `from` o `import` son líneas para añadir algo de otros archivos. Así que en vez de copiar y pegar las mismas cosas en cada archivo, podemos incluir algunas partes con `from... import ...`.

`class Post(models.Model):` esta línea define nuestro modelo (es un objeto).

- `class` es una palabra reservada que indica que estamos definiendo un objeto.
- `Post` es el nombre de nuestro modelo. Podemos darle un nombre diferente (pero debemos evitar espacios en blanco y caracteres especiales). Una clase siempre comienza con su primera letra en mayúscula.
- `models.Model` significa que Post es un modelo de Django, así Django sabe que debe guardarlo en la base de datos.

Ahora definimos las propiedades: title, text, created_date, published_date y author. Para hacer eso tenemos que definir un tipo de dato (¿es texto? ¿un número? ¿una fecha? ¿una relación con otro objeto - es decir, un usuario?).

- `models.CharField` - esto es como defines un texto con un número limitado de caracteres.
- `models.TextField` - esto es para textos largos sin un límite. Será ideal para el contenido de un post.
- `models.DateTimeField` - esto es fecha y hora.
- `modelos.ForeignKey` - este es un vínculo con otro modelo.

Si queres saber más sobre los tipos de datos de los Modelos y cómo definir cosas diferentes de las que recién definimos, te recomiendo que eches un vistazo a la documentación de Django: [https://docs.djangoproject.com/en/1.8/ref/models/fields/#field-types](https://docs.djangoproject.com/en/1.8/ref/models/fields/#field-types)

## Crear tablas para los modelos en tu base de datos

El último paso es añadir nuestro nuevo modelo a nuestra base de datos. Primero tenemos que hacer que Django sepa que tenemos algunos cambios en nuestro modelo:

{% highlight bash %}
(mi_proyecto)~/proyectos/mi_proyecto: $ python manage.py makemigrations blog
Migrations for 'blog':
  0001_initial.py:
  - Create model Post
{% endhighlight %}

Django preparará un archivo de migración que tenemos que aplicar ahora a nuestra base de datos escribiendo `python manage.py migrate blog`:

{% highlight bash %}
(mi_proyecto)~/proyectos/mi_proyecto: $ python manage.py migrate blog
Operations to perform:
  Apply all migrations: blog
Running migrations:
  Applying blog.0001_initial... OK
{% endhighlight %}

¡Buenísimo! Nuestro modelo de Post está ahora en nuestra base de datos. Sería bueno verlo, ¿no? Andá al siguiente capítulo.