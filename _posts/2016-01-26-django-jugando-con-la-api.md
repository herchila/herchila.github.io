---
layout: post
title:  "Django: Jugando con la API"
date:   2016-01-26 17:10:00
categories: django
---

### Capítulos

- [Capítulo 1 - Django: Primeros pasos](https://herchila.github.io/django/2015/02/18/django-primeros-pasos.html)
- [Capítulo 2 - Django: Modelos](https://herchila.github.io/django/2016/01/24/django-modelos.html)
- **Capítulo 3 - Django: Jugando con la API**

Django nos provee una API completa que nos permite acceder a los modelos y jugar:

{% highlight bash %}
(mi_proyecto)~/proyectos/mi_proyecto: $ python manage.py shell
{% endhighlight %}

Usamos esto en lugar de simplemente tipear "python" porque `manage.py` setea la variable de entorno `DJANGO_SETTINGS_MODULE`, que le da a Django el import path al archivo `mysite/settings.py`.

Una vez en el shell, exploramos la [API de base de datos](https://docs.djangoproject.com/en/1.8/topics/db/queries):

{% highlight bash %}
from blog.models import Post

Post.objects.all()
[]                                  # No hay registros en la tabla Post.

from django.utils import timezone
user = User.objects.all().first()   # de todos los usuarios, pido el primero.
p = Post()
p.author = user
p.title = 'Titulo del primer artículo'
p.text = 'Texto del primer artículo de mi blog.'
p.published_date = timezone.now()

p.save()    # Ahora guardamos
p.id        # Accedemos al ID
1

p.title     # Accedemos al título
'Titulo del primer artículo'

Post.objects.all()      # Muestra todos los posts de la base de datos
[<Post: Post object>]
{% endhighlight %}

`[<Post: Post object>]` es, definitivamente, una representación poco útil de este objeto. Arreglemos esto editando los modelos (en el archivo `blog/models.py`) y agregando el método `__str__()` a `Post`:

{% highlight bash %}
blog/models.py
from django.db import models

class Post(models.Model):
    # ...
    def __str__(self):              # __unicode__ on Python 2
        return self.title
{% endhighlight %}

Es importante agregar el método __str__() a nuestros modelos porque es la representación usada por Django en la interfaz de administración autogenerada.

El método `publish` agrega la fecha y hora actual y luego guarda el registro. Si queremos diferenciar artículos publicados de los que aun no se publicaron, usamos la función `publish`.

{% highlight bash %}
p = Post.objects.get(pk=1)
p.publish()
{% endhighlight %}

Agregamos un método más a modo de ejemplo:

{% highlight bash %}
blog/models.py

import datetime

from django.db import models
from django.utils import timezone


class Post(models.Model):
    # ...
    def was_published_recently(self):
        return self.published_date >= timezone.now() - datetime.timedelta(days=1)
{% endhighlight %}

Guardamos los cambios y empezamos una nueva sesión en el shell corriendo `python manage.py shell` nuevamente:

{% highlight bash %}
Post.objects.all()
[<Post: Titulo del primer artículo>]       # Nos aseguramos que el __str__() que agregamos funcione.

Post.objects.filter(id=1)
[<Post: Titulo del primer artículo>]
Post.objects.filter(title__startswith='Titulo')
[<Post: Titulo del primer artículo>]

Post.objects.get(id=2)
Traceback (most recent call last):
    ...
DoesNotExist: Post matching query does not exist.

Post.objects.get(pk=1)      # Django provee un atajo para acceder de manera exacta a traves de la clave primaria (PK)
<Post: Titulo del primer artículo>

p = Post.objects.get(pk=1)
p.was_published_recently()          # Probamos el metodo que creamos
True
{% endhighlight %}

## Acceder a objetos relacionados

Con el manejador de relaciones de Django (related manager), podemos acceder a otros objetos que esten relacionados. Veamos un ejemplo:

Vamos a mostrar como listamos todos los artículos de un usuario en particular.

Primero vamos a crear una instancia de un usuario que exista en la base de datos:

{% highlight bash %}
from django.contrib.auth.models import User
juan = User.objects.get(pk=1)
{% endhighlight %}

Luego procedemos a crear un artículo referenciándolo al usuario Juan:

{% highlight bash %}
Post.objects.create(author=juan, title='Primer post de Juan', text='Este es el primer post que Juan escribe.')    # Primero creamos artículos para el autor Juan
{% endhighlight %}

Probemos que esto funcionó:

{% highlight bash %}
Post.objects.filter(author=juan)
[<Post: Primer post de Juan>]
{% endhighlight %}

Otra forma de crear artículos para un usuario en particular:

{% highlight bash %}
juan.post_set.all()
[<Post: Primer post de Juan>]

juan.post_set.create(author=juan, title='Segundo post de Juan', text='Este es el segundo post que Juan escribe.')

u = juan.post_set.create(author=juan, title='Tercer post de Juan', text='Este es el tercer post que Juan escribe.')
u.post
[<Post: Tercer post de Juan>]

juan.post_set.all()     # Muestra todos los artículos de Juan
[<Post: Primer post de Juan>, <Post: Segundo post de Juan>, <Post: Tercer post de Juan>]

juan.post_set.count()       # Muestra el total de artículos de Juan
3
{% endhighlight %}

Para más información sobre relaciones en modelos, ver ver la [documentacion de objetos relacionados](https://docs.djangoproject.com/en/1.8/ref/models/relations).

Para el detalle completo de la API de base de datos, ver [Database API reference](https://docs.djangoproject.com/en/1.8/topics/db/queries).
