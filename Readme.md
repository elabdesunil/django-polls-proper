# The first Django App again


- [The first Django App again](#the-first-django-app-again)
  - [Setup](#setup)
      - [Create virtual environment](#create-virtual-environment)
      - [Start the development server](#start-the-development-server)
    - [Create the project](#create-the-project)
      - [start project in same directory](#start-project-in-same-directory)
      - [start project in inside another folder in the dictory](#start-project-in-inside-another-folder-in-the-dictory)
        - [What are python `packages`?](#what-are-python-packages)
      - [Create the Polls App](#create-the-polls-app)
        - [Write the first view](#write-the-first-view)
          - [`views.py`](#viewspy)
        - [`urls.py`](#urlspy)
        - [About `path()` like `path(route, view, [kwargs, name])`](#about-path-like-pathroute-view-kwargs-name)
          - [`route`](#route)
          - [`view`](#view)
          - [`kwargs`](#kwargs)
          - [`name`](#name)


## Setup

#### Create virtual environment

```
python -m venv venv
```

To install django in the virtual environment
```
python -m pip install django
```

#### Start the development server
```
python manage.py runserver
```
To specify port number
```
python manage.py runserver 8080
```
To change the server's IP, for ex, to listen on all available public IPs
```
python manage.py runserver 0.0.0.0:8000
```
`\` in command prompt
to activate
```
venv\Scripts\activate
```
to deactivate
```
venv\Scripts\deactivate.bat // just deactivate in cmd
```

### Create the project

#### start project in same directory
```
django-admin startproject mysite
```
#### start project in inside another folder in the dictory
There is no need to create folder, reason is below shown by folder structure
```
django-admin startproject mysite .
```

The following get created
```
mysite/
    manage.py
    mysite/
        __init__.py
        settings.py
        urls.py
        asgi.py
        wsgi.py
```

- mysite/ root directory is a container for the project. it's name doesn't matter
- mysite/ inner directory is the actual Python package for the project. Refer to this by `mysite.urls`
- `mysite/__init__.py` - the directory containing this file is treated as packages
- `mysite/asgi.py` and `mysite/wsgi.py` - are entry points for ASGI-compatible and WSGI-compatible web servers.

##### What are python `packages`?
Packages are a way of structureing Python's module namespace by using "dotted module names". [More](https://docs.python.org/3/tutorial/modules.html#tut-packages)


#### Create the Polls App
Go to diretory containing `manage.py`
```
python manage.py startapp polls
```

##### Write the first view


###### `views.py`
```python
# polls/views.py

from django.http import HttpResponse

def index(request):
  return HttpResponse("Hello, world, You're at the index of poll")
```
to map the view, we need to map it to a URL - and for this we need a URLconf

##### `urls.py`
```py
# polls/urls.py

from django.urls import path
from . import views

urlpatterns = [
  path('', views.index, name='index')
]
```
import this urls file in `mysite/urls.py`
```py
# mysite/urls.py

from django.contrib import admin
from django.urls import include, path

urlpatterns = [
  path('polls/', include("polls.urls"))
  path("admin/", admin.site.urls), 
]
```
`include()` function allows referencing other URLconfs
  - when to use it? always use it when including other URL patterns. 
    - exept for `admin.site.urls`

To test visit: http://localhost:8000/polls/

##### About `path()` like `path(route, view, [kwargs, name])`

The `path()` function is passed four arguments, two required: `route` and `view` and two optional: `kwargs`, `name`.

###### `route` 
- route is the string that contains a URL pattern
- starts from the first patter in the urlpatters and works down the list
###### `view`
- specifies the view function with an `HttpRequest` object as the first argument
- and any 'captured' values from the route.

###### `kwargs`
- arbitrary keyword arguments can be passed in a dictionary to the target view.

###### `name`
- naming lets us refer to the URL elsewhere in Django, 
  - especially necessary for template files
- by touching one singe file, global changes to URL patters can be made