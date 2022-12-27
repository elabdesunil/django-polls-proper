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

```python
# polls/views.py

from django.htpp import HttpResponse

def index(request):
  return HttpResponse("Hello, world, You're at the index of poll")
```