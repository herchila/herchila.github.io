---
layout: post
title:  "Jugando con la API"
date:   2016-01-26 17:10:00
categories: django
---

## Capítulos

- [Capítulo 1: Primeros pasos con Django](https://herchila.github.io/django/2015/02/18/primeros-pasos-con-django.html)
- [Capítulo 2: Modelos en Django](https://herchila.github.io/django/2016/01/24/modelos-en-django.html)
- **Capítulo 3: Jugando con la API**

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
p.created_date = timezone.now()
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

Guardamos los cambios y empezamos una nueva sesión en el shell corriendo `python manage.py shell` nuevamente:

{% highlight bash %}

{% endhighlight %}