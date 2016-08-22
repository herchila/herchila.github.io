---
layout: post
title:  "Returning JSON/JSONP from a Django view with a little decorator help"
date:   2016-08-21 23:32:00
categories: django
---

This is a decorator that helps creating a JSON response:

{% highlight bash %}
def json_response(func):
    """
    """
    def decorator(request, *args, **kwargs):
        objects = func(request, *args, **kwargs)
        if isinstance(objects, HttpResponse):
            return objects
        try:
            data = simplejson.dumps(objects)
            if 'callback' in request.REQUEST:
                data = '%s(%s);' % (request.REQUEST['callback'], data)
                return HttpResponse(data, "text/javascript")
        except:
            data = simplejson.dumps(str(objects))
        return HttpResponse(data, "application/json")
    return decorator
{% endhighlight %}

Usage:

`http://localhost/the_url/?callback=name`

The response will be in JSONP format!
