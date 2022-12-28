# The first Django App again


- [The first Django App again](#the-first-django-app-again)
  - [\[Part 1\] Setup](#part-1-setup)
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
  - [Part 2 - Database and model setting up](#part-2---database-and-model-setting-up)
    - [Database setup](#database-setup)
      - [`INSTALLED_APPS`](#installed_apps)
      - [Create tables in the database using `migrate`](#create-tables-in-the-database-using-migrate)
        - [\[Optional\] check out the tables that got created command-line client](#optional-check-out-the-tables-that-got-created-command-line-client)
    - [Setting up Models](#setting-up-models)
      - [Add models `Question` and `Choice`](#add-models-question-and-choice)
      - [Activate models](#activate-models)
        - [make migrations - that tells Django that some changes have been made to it](#make-migrations---that-tells-django-that-some-changes-have-been-made-to-it)
          - [Info: `polls/migrations/0001_initial.py` is human-readable and modifiable](#info-pollsmigrations0001_initialpy-is-human-readable-and-modifiable)
          - [Checkout the SQL query using `sqlmigrate`](#checkout-the-sql-query-using-sqlmigrate)
        - [check if there is any problem before running migrations using `python manage.py check`](#check-if-there-is-any-problem-before-running-migrations-using-python-managepy-check)
          - [Migrate](#migrate)
          - [Info: 3 step to make model changes: change `models.py`, run `makemigrations` and `migrate`](#info-3-step-to-make-model-changes-change-modelspy-run-makemigrations-and-migrate)
          - [Why do we need to make and apply migrations separately?](#why-do-we-need-to-make-and-apply-migrations-separately)
      - [Playing with the API](#playing-with-the-api)
        - [Info: Add `__Str__()` to all models to replace Django's automatically-generated admin](#info-add-__str__-to-all-models-to-replace-djangos-automatically-generated-admin)
        - [add a custom method `was_published_recently` to model `Question`](#add-a-custom-method-was_published_recently-to-model-question)
        - [Info: we can pass to `Model.objects.somefunct` something like `pub_date__year` to access year or functions that are part of its data\_type, `pub_date` for ex](#info-we-can-pass-to-modelobjectssomefunct-something-like-pub_date__year-to-access-year-or-functions-that-are-part-of-its-data_type-pub_date-for-ex)
        - [Info: we can access by `[Question Object]q.choice_set`, key being `_set`, because Django creates a set to hold the "other side" of a ForeignKey relation, which can be access by `choice_set`](#info-we-can-access-by-question-objectqchoice_set-key-being-_set-because-django-creates-a-set-to-hold-the-other-side-of-a-foreignkey-relation-which-can-be-access-by-choice_set)
    - [Django Admin](#django-admin)
        - [Info: if you set `LANGUAGE_CODE`, the admin interface can be translated to available languages](#info-if-you-set-language_code-the-admin-interface-can-be-translated-to-available-languages)
      - [Make the poll app modifiable in admin](#make-the-poll-app-modifiable-in-admin)
        - [Info: To make `Question` model modifiable in django admin add to `polls/admin.py` \`admin.site.register(Questions) after necessary imports](#info-to-make-question-model-modifiable-in-django-admin-add-to-pollsadminpy-adminsiteregisterquestions-after-necessary-imports)
      - [Some notes on Djnago Admin dashboard](#some-notes-on-djnago-admin-dashboard)


## [Part 1] Setup

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



## Part 2 - Database and model setting up

### Database setup
- python comes with SQLite support and is included in Django
- to use another daztabse, install appropriate database bindings and change the following in `DATABASES 'default'`:
  - `ENGINE` - `django.db.backends.sqlite`, `django.db.backends.mysql`, or `django.db.backends.oracle`
  - `NAME` - default is `db.sqlite3`

- if not using SQLite as the database, additional settings such as `USER`, `PASSWORD`, and `HOST` must be added.
```
IF using a database besides SQLite, create a database before moving to next steps
- Do `CREATE DATABASE database_name;` within the database's prompt
```

- set up `TIME_ZONE` when in settings.py/

#### `INSTALLED_APPS`
- `django.contrib.admin` - the admin site. you'll use it shortly
- `django.contrib.auth` -  An authentication system
- `django.contrib.contenttypes` - a framework for content types
- `django.contrib.sessions` - a session framework
- `django.contrib.messages` - A messaging framework
- `django.contrib.staticfiles` - A framework for managing static files


#### Create tables in the database using `migrate`
- the following looks at the `installed_apps` and creates any necessary database tables.
```
python manage.py migrate
```

##### [Optional] check out the tables that got created command-line client
- `\dt` (PostgreSQL),
- `SHOW_TABLES;` (MariaDB, MySQL)
- `.tables` (SQLite)
- `SELECT TABLE_NAME FROM USER_TABLES;` (Oracle)


### Setting up Models
- models
  - 1. database layout
  - 2. additional metadata
  - philosophy: in python, a model is to be a single, difinitive source of information about the data.

#### Add models `Question` and `Choice`

```py
# polls/models.py

from django.db import models

class Question(models.Model):
  question_text = models.CharField(max_length=200)
  pub_date = models.DateTimeField('date published') # defines human readable name, maybe for documentation purposes

class Choice (models.Model):
  question = models.ForeignKey(Question, on_delete=models.CASCADE)
  choice_text = models.CharField(max_length=200)
  votes = models.IntegerField(default=0)

```
Explanation:
- `django.db.models.Model` as a subclass
- `Field` - each field is represeneted by it
  - ex. `CharField`, `DateTimeField`
  - the name of the fields are `questions_text`, `pub_date`, which will also be the column name in the database
  - required arguments
    - `CharField` for example require `max_length`
  - optional arguments
    - `default`  to set a default value
      - used in `votes`
      - `ForeignKey` - in the example above, we told django that `Choice` is related to a single `Question`
- all database relationships are supported:
  - many-to-many relationships
  - many-to-one relationships
  - one-to-one relationships

#### Activate models

after the completion of the last [section](#add-models-question-and-choice), we can:
1. create a database schema (`CREATE TABLE`)
2. create a Python database-access API for accessing Question and Choice objects

Note: django apps are pluggable which means: it can be used in multiple projects and can be distributed 

include the app in the project by adding to the `INSTALLED_APPS`:
```py
# mysite/settings.py

INSTALLED_APPS = [
  'polls.apps.PollsConfig',
  'django.contrib.admin',
  # ...,
]
```

##### make migrations - that tells Django that some changes have been made to it
Now, make migrations
```
python manage.py makemigrations polls
```

###### Info: `polls/migrations/0001_initial.py` is human-readable and modifiable

###### Checkout the SQL query using `sqlmigrate`
do 
```
python manage.py sqlmigrate polls 001
``` 
to view how the migration looks like in SQL query

output could look like:
```sql
BEGIN;
--
-- Create model Question
--
CREATE TABLE "polls_question" (
    "id" bigint NOT NULL PRIMARY KEY GENERATED BY DEFAULT AS IDENTITY,
    "question_text" varchar(200) NOT NULL,
    "pub_date" timestamp with time zone NOT NULL
);
--
-- Create model Choice
--
CREATE TABLE "polls_choice" (
    "id" bigint NOT NULL PRIMARY KEY GENERATED BY DEFAULT AS IDENTITY,
    "choice_text" varchar(200) NOT NULL,
    "votes" integer NOT NULL,
    "question_id" bigint NOT NULL
);
ALTER TABLE "polls_choice"
  ADD CONSTRAINT "polls_choice_question_id_c5b4b260_fk_polls_question_id"
    FOREIGN KEY ("question_id")
    REFERENCES "polls_question" ("id")
    DEFERRABLE INITIALLY DEFERRED;
CREATE INDEX "polls_choice_question_id_c5b4b260" ON "polls_choice" ("question_id");

COMMIT;
```

- table names are generated by combining 
  - name of the app
  - lowercase name of the model - `question` and `choice` which can be modified
- primary keys are added automatically
- foreign key field name, appears with suffix `_id`, which can be modified, ex `question_id`
- `sqlmigrate`
  - doesn't actually run the migration but prints how it looks like
    - can be used to diagnose anything missing or errors before commiting migrations
    - or when database administrators require scripts for making the changes

##### check if there is any problem before running migrations using `python manage.py check`
then run `python manage.py migrate`


###### Migrate
- takes all the migrations that haven't been applied and runs them against the database to sync
- lets us change database and table, make new ones without the need to delete your database or tables
  - lets live upgrade with no data loss

###### Info: 3 step to make model changes: change `models.py`, run `makemigrations` and `migrate`

###### Why do we need to make and apply migrations separately?
- because the migrations are commited to version control system and are shipped with the app
  - easier
  - shareable
  - production ready

#### Playing with the API

```
python manage.py shell
```

```python
>>> from polls.models import Choice, Question  # Import the model classes we just wrote.

# No questions are in the system yet.
>>> Question.objects.all()
<QuerySet []>

# Create a new Question.
# Support for time zones is enabled in the default settings file, so
# Django expects a datetime with tzinfo for pub_date. Use timezone.now()
# instead of datetime.datetime.now() and it will do the right thing.
>>> from django.utils import timezone
>>> q = Question(question_text="What's new?", pub_date=timezone.now())

# Save the object into the database. You have to call save() explicitly.
>>> q.save()

# Now it has an ID.
>>> q.id
1

# Access model field values via Python attributes.
>>> q.question_text
"What's new?"
>>> q.pub_date
datetime.datetime(2012, 2, 26, 13, 0, 0, 775217, tzinfo=datetime.timezone.utc)

# Change values by changing the attributes, then calling save().
>>> q.question_text = "What's up?"
>>> q.save()

# objects.all() displays all the questions in the database.
>>> Question.objects.all()
<QuerySet [<Question: Question object (1)>]>
```

`<QuerySet [<Question: Question object (1)>]>` is not very helpful, so lets add `__str__` to the model classes to display another output

##### Info: Add `__Str__()` to all models to replace Django's automatically-generated admin

changes made:
```py
# polls/models.py

from django.db import models

class Question(models.Model):
    # ...
    def __str__(self):
        return self.question_text

class Choice(models.Model):
    # ...
    def __str__(self):
        return self.choice_text
```

##### add a custom method `was_published_recently` to model `Question`

```py
# polls/models.py

import datetime

from django.db import models
from django.utils import timezone


class Question(models.Model):

  # ..

  def was_published_recently(self):
    return self.pub_date >= timezone.now() - datetime.timedelta(days=1)
```

run `python manage.py shell`
```py

>>> from polls.models import Choice, Question

# Make sure our __str__() addition worked.
>>> Question.objects.all()
<QuerySet [<Question: What's up?>]>

# Django provides a rich database lookup API that's entirely driven by
# keyword arguments.
>>> Question.objects.filter(id=1)
<QuerySet [<Question: What's up?>]>
>>> Question.objects.filter(question_text__startswith='What')
<QuerySet [<Question: What's up?>]>

# Get the question that was published this year.
>>> from django.utils import timezone
>>> current_year = timezone.now().year
>>> Question.objects.get(pub_date__year=current_year)
<Question: What's up?>

# Request an ID that doesn't exist, this will raise an exception.
>>> Question.objects.get(id=2)
Traceback (most recent call last):
    ...
DoesNotExist: Question matching query does not exist.

# Lookup by a primary key is the most common case, so Django provides a
# shortcut for primary-key exact lookups.
# The following is identical to Question.objects.get(id=1).
>>> Question.objects.get(pk=1)
<Question: What's up?>

# Make sure our custom method worked.
>>> q = Question.objects.get(pk=1)
>>> q.was_published_recently()
True

# Give the Question a couple of Choices. The create call constructs a new
# Choice object, does the INSERT statement, adds the choice to the set
# of available choices and returns the new Choice object. Django creates
# a set to hold the "other side" of a ForeignKey relation
# (e.g. a question's choice) which can be accessed via the API.
>>> q = Question.objects.get(pk=1)

# Display any choices from the related object set -- none so far.
>>> q.choice_set.all()
<QuerySet []>

# Create three choices.
>>> q.choice_set.create(choice_text='Not much', votes=0)
<Choice: Not much>
>>> q.choice_set.create(choice_text='The sky', votes=0)
<Choice: The sky>
>>> c = q.choice_set.create(choice_text='Just hacking again', votes=0)

# Choice objects have API access to their related Question objects.
>>> c.question
<Question: What's up?>

# And vice versa: Question objects get access to Choice objects.
>>> q.choice_set.all()
<QuerySet [<Choice: Not much>, <Choice: The sky>, <Choice: Just hacking again>]>
>>> q.choice_set.count()
3

# The API automatically follows relationships as far as you need.
# Use double underscores to separate relationships.
# This works as many levels deep as you want; there's no limit.
# Find all Choices for any question whose pub_date is in this year
# (reusing the 'current_year' variable we created above).
>>> Choice.objects.filter(question__pub_date__year=current_year)
<QuerySet [<Choice: Not much>, <Choice: The sky>, <Choice: Just hacking again>]>

# Let's delete one of the choices. Use delete() for that.
>>> c = q.choice_set.filter(choice_text__startswith='Just hacking')
>>> c.delete()
```

##### Info: we can pass to `Model.objects.somefunct` something like `pub_date__year` to access year or functions that are part of its data_type, `pub_date` for ex

##### Info: we can access by `[Question Object]q.choice_set`, key being `_set`, because Django creates a set to hold the "other side" of a ForeignKey relation, which can be access by `choice_set`

The API automatically follow relationships as far as you need. 
- Use double underscores to separate relationships
- This works as many levels deep as you want; there's no limit
`Choice.objects.filter(question__pub_date__year=current_year)`

### Django Admin 

Philosophy
- creation of admin interfaces for models is automated
- admin isn't intended to be used by site visitors. It's for site managers.


Create an admin user

```
python mange.py createsuperuers
```

Start development server

```
python manage.py runserver
```

##### Info: if you set `LANGUAGE_CODE`, the admin interface can be translated to available languages

After setting up the adming user, and accessing it the `poll` will not have displayed.


#### Make the poll app modifiable in admin

```py
# polls/admin.py

from django.contrib import admin

from .models import Question

admin.site.register(Question)
```

##### Info: To make `Question` model modifiable in django admin add to `polls/admin.py` `admin.site.register(Questions) after necessary imports


#### Some notes on Djnago Admin dashboard
- form is automatically generatted based on Question model
- each `DateTimeField` gets free Javascript shortcuts:
  - Dates get a "Today" shortcut and calendar popup
  - times get a "Now" shortcut and a convenient poput that lists commonly entered times

