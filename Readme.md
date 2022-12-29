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
  - [\[Part 3\] Views](#part-3-views)
      - [Writing more views](#writing-more-views)
      - [How does this requesting work?](#how-does-this-requesting-work)
      - [The Proper View](#the-proper-view)
        - [Each view is responsible for two things: `HttpResponse` \& `Http404`](#each-view-is-responsible-for-two-things-httpresponse--http404)
        - [Range of choices offered by the view:](#range-of-choices-offered-by-the-view)
        - [Add templates to `polls`](#add-templates-to-polls)
        - [Info: Here is the directory `DjangoTemplates` wants: `polls/templates/polls/index.html`. to access just do `polls/index.html`. \[important\]](#info-here-is-the-directory-djangotemplates-wants-pollstemplatespollsindexhtml-to-access-just-do-pollsindexhtml-important)
        - [Info: Instead of using `django.template.render`, we can use use `django.shortcuts.render` to render the template](#info-instead-of-using-djangotemplaterender-we-can-use-use-djangoshortcutsrender-to-render-the-template)
        - [Info: `render()` syntax: `render(request, template_file, context)`](#info-render-syntax-renderrequest-template_file-context)
      - [Raising a 404 error](#raising-a-404-error)
        - [Info: to manually put an error message, use `Http404`](#info-to-manually-put-an-error-message-use-http404)
        - [Info: this shortcut function `get_objects_or_404` can get or raise error for us](#info-this-shortcut-function-get_objects_or_404-can-get-or-raise-error-for-us)
      - [Remove hard coded urls](#remove-hard-coded-urls)
        - [Info: url should look like: `{% url 'detail' question.id %}`, where is 'detail' is the name passed to `path()` in `polls.urls`](#info-url-should-look-like--url-detail-questionid--where-is-detail-is-the-name-passed-to-path-in-pollsurls)
      - [How to change url of polls `detail` view?](#how-to-change-url-of-polls-detail-view)
    - [Namespacing URL names](#namespacing-url-names)
        - [Info: to avoid confusion: set `app_name` (for ex. `poll`) in `polls/urls.py` and access by `{% url 'polls:detail' question.id %}` in `polls/index.html`](#info-to-avoid-confusion-set-app_name-for-ex-poll-in-pollsurlspy-and-access-by--url-pollsdetail-questionid--in-pollsindexhtml)
  - [\[Part 4\] - Form Processing and Generic Views](#part-4---form-processing-and-generic-views)
    - [Replace the dummy contents from `polls/views.py vote()` by](#replace-the-dummy-contents-from-pollsviewspy-vote-by)
        - [Info: always return a `HttpResponseRedirect` after succesfully dealing with POST data - solution to the `Back` button problem 😉](#info-always-return-a-httpresponseredirect-after-succesfully-dealing-with-post-data---solution-to-the-back-button-problem-)
    - [Update the `polls/views results()` method](#update-the-pollsviews-results-method)
    - [Finally add the template file `results.html` accordingly](#finally-add-the-template-file-resultshtml-accordingly)
    - [Use Generic views: less code is better then](#use-generic-views-less-code-is-better-then)
        - [Info: use generic views where possible from the start, but understand what to do if we need more flexibility](#info-use-generic-views-where-possible-from-the-start-but-understand-what-to-do-if-we-need-more-flexibility)
      - [Amend URLconf](#amend-urlconf)
      - [Amend Views](#amend-views)
        - [Info: A weird problem: we need to add `queryset = Question.objects.all()` to the `ResultsView` to avoid error that looks like](#info-a-weird-problem-we-need-to-add-queryset--questionobjectsall-to-the-resultsview-to-avoid-error-that-looks-like)
  - [\[Part 5\] Setting up automated tests for the app](#part-5-setting-up-automated-tests-for-the-app)
    - [Automated Tests](#automated-tests)
    - [Why Tests](#why-tests)
      - [Save time](#save-time)
      - [They often prevent problems from occurring](#they-often-prevent-problems-from-occurring)
      - [More attractive](#more-attractive)
      - [Teamwork](#teamwork)
    - [Testing Strategies](#testing-strategies)
    - [Writing our first test](#writing-our-first-test)
      - [Identify a bug](#identify-a-bug)
      - [Create the test](#create-the-test)
      - [Running tests](#running-tests)
      - [Fixing the bug](#fixing-the-bug)
      - [More comprehensive tests](#more-comprehensive-tests)
    - [Test a View](#test-a-view)


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
```python
# polls/urls.py

from django.urls import path
from . import views

urlpatterns = [
  path('', views.index, name='index')
]
```
import this urls file in `mysite/urls.py`
```python
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

```python
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
```python
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
```python
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

```python
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
```python

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

```python
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

## [Part 3] Views
A view is a "type" of web page in your Django application that generally serves as specific functions and has a specific template.

A typical blog application could have:
- 1. Blog homepage - displays the latest few entries
- 2. Entry "detail" page - permalink page for a single entry
- 3. Year-based archive page - displays all months with entries in the given year
- 4. Month-based archive page - displays all days with entries in the given month
- 5. Day-based archive page - displays all entries in a given day
- 6. Comment action - handles posting comments to a given entry

Features that will be implemented in our app:
- 1. Question "index" page
- 2. Question "detail" page
- 3. Question "results" page
- 4. Vote action

A django url patter looks like `/newsarchive/<year>/<month>/`
- `URLconf`  maps URL patterns to views


#### Writing more views


```python
# polls/views.py

def detail(request, question_id):
  return HttpResponse("Your're looking at question %s." % question_id)

def results(request, question_id):
  response = "You're looking at the results of question %s."
  return HttpResponse(response % question_id)


def vote(request, question_id):
  return HttpResponse("You're voting on question %s." % question_id)

```

connect the views

```python
# polls/urls.py

from django.urls import path
from . import views

urlpatterns = [
  # ex: /polls/
  path('', views.index, name='index'),

  # ex: /polls/5/
  path('<int:question_id>/', views.detail, name="detail"),

  # ex: /polls/5/results/
  path('<int:question_id>/results/', views.results, name="results"),

  # ex: /polls/5/vote/
  path('<int:question_id>/vote/', views.vote, name='vote')
]
```

#### How does this requesting work?

- when we visit - say, `polls/1/`, Django will load mysite.urls that is pointed by the ROOT_URLCONF setting
- it finds `urlpatterns`, and traverse through it to find
  - `polls/`, which gets stripped off
  - remaining `1/` is sent to the `polls.urls` URLconf for further processing
- `<int:question_id>/` is found, which calls detail()
  - it looks like:
    `detail(request=<HttpRequest object>, question_id=1)`


New github feature `Ctrl+shift+k` to pull of command pallet, switch organization and search


#### The Proper View

##### Each view is responsible for two things: `HttpResponse` & `Http404`
1. Return an HttpResponse object containing the content for the requested page
2. Raise an exception such as Http404
These are the only two things django requires view to create.

##### Range of choices offered by the view:
1. Abilty to access database or not
2. Template system from Django itself or from a third-party or not
3. Generate a PDF file, output XML, create a ZIP file on the fly or not

```python
# polls/views.py

from django.http import HttpResponse
from .models import Question

def index(request):
  latest_question_list = Question.objects.order_by('-pub_date')[:5]
  output = ', '.join([q.question_text for q in latest_question_list])
  return HttpResponse(output)
```

##### Add templates to `polls`

- how the templates are loaded, is determined by `TEMPLATES`.
- `DjangoTemplates` backend accesses all `APP_DIRS` to look for templates.
  - By convention, `DjangoTemplates` looks for a `templates` subdirectory in each of the `INSTALLED_APPS`

##### Info: Here is the directory `DjangoTemplates` wants: `polls/templates/polls/index.html`. to access just do `polls/index.html`. [important]

```html
<!--polls/templates/polls/index.html-->
{% if latest_question_list %}
  <ul>
    {% for question in latest_question_list %}
    <li><a href="/polls/{{ question.id }}/" >
      {{ question.question_text }}
    </a></li>
    {% endfor %}
  </ul>
{% else %}
  <p>No polls are available</p>
{% endif %}
```
Add the template to `views.py`
```python
from django.http import HttpResponse
from django.template import loader

from .models import Question

def index(request):
  latest_question_list = Question.objects.order_by('-pub_date')[:5]
  template = loader.get_template("polls/index.html")
  context = {
    "latest_question_list": latest_question_list,
  }
  return HttpResponse(template.render(context, request))
```

##### Info: Instead of using `django.template.render`, we can use use `django.shortcuts.render` to render the template
##### Info: `render()` syntax: `render(request, template_file, context)`
The shortcut exists because this is very common.
Here's how:
```python
# polls/views.py

from django.shortcuts import render
from .models import Question

def index(request):
  latest_question_list = Question.objects.order_by('-pub_date')[:5]
  context = {'latest_question_list': latest_question_list}
  return render (request, 'polls/index.html', context)
```

#### Raising a 404 error

```python
# polls/views.py

# import ...
from django.http import Http404
# ...
def detail(request, question_id):
  try:
    question = Question.objects.get(pk=question_id)
  except Question.DoesNotExist:
    raise Http404("Question does not exist")
  return render(request, "poll/detail.html", {"question": question})
```
temporary put the following in templates
```html
<!--polls/templates/polls/detail.html-->
{{ question }}
```
##### Info: to manually put an error message, use `Http404`

##### Info: this shortcut function `get_objects_or_404` can get or raise error for us
So modify `details` method as:
```python
# polls/views.py

from django.shortcuts import get_object_or_404, render
from .models import Question
# ...
def detail(request, question_id):
  question = get_object_or_404(Question, pk=question_id)
  return render(request, 'polls/detail.html', {'question': question})
```

modify `/polls/detail.html`
```html
<!--polls/templates/polls/detail.html-->

<h1>{{ question.question_text }}</h1>
<ul>
  {% for choice in question.choice_set.all %}
    <li>{{ choice.choice_text }}</li>
  {% endfor %}
</ul>
```
- the template system uses dot-lookup syntax to access variable attributes
  - in the example `{{ question.question_text }}`
    - django does a dictionary lookup which fails
    - so it loops up attribute
    - should last fail, it would try `list-index` lookup

#### Remove hard coded urls
##### Info: url should look like: `{% url 'detail' question.id %}`, where is 'detail' is the name passed to `path()` in `polls.urls`

change the following:
```html
<li><a href="/polls/{{ question.id }}">{{ question.question_text }}</a></li>
```
To:
```html
<li><a href="{% url 'detail' question.id %}">{{ question.question_text }}</a></li>
```

#### How to change url of polls `detail` view?

change
```python
# the 'name' value as called by the {% url %} template tag
path('<int:question_id>/', views.detail, name='detail'),
```
to
```python
# added the word 'specifics'
path('specifics/<int:question_id>/', views.detail, name='detail'),
```

### Namespacing URL names
##### Info: to avoid confusion: set `app_name` (for ex. `poll`) in `polls/urls.py` and access by `{% url 'polls:detail' question.id %}` in `polls/index.html`

Lets do this insertion in `polls/urls.py`

```python
# polls/urls.py
# ...
app_name = 'polls'
# ...
```
change:
```html
<!--polls/templates/polls/index.html-->
<li><a href="{% url 'detail' question.id %}">{{ question.question_text }}</a></li>
```
to
```html
<!--polls/templates/polls/index.html-->
<li><a href="{% url 'polls:detail' question.id %}">{{ question.question_text }}</a></li>
```

## [Part 4] - Form Processing and Generic Views

Add a form to the `detail.html`
```html
<!--polls/templates/polls/detail.html-->
<form action="{% url 'polls:vote' question.id %}" method="post">
{% csrf_token %}
<fieldset>
    <legend><h1>{{ question.question_text }}</h1></legend>
    {% if error_message %}<p><strong>{{ error_message }}</strong></p>{% endif %}
    {% for choice in question.choice_set.all %}
        <input type="radio" name="choice" id="choice{{ forloop.counter }}" value="{{ choice.id }}">
        <label for="choice{{ forloop.counter }}">{{ choice.choice_text }}</label><br>
    {% endfor %}
</fieldset>
<input type="submit" value="Vote">
</form>
```

- `value` of each button is choice's ID. Than `name` of each radio button is `choice`. 
- the form's `action` is set to `{% url 'polls:vote' question.id %}`
- `forloop.counter` countes how many times for tag has gone through its loop
- cross Site Request Forgeries is taken care of by `{% csrf_token %}`.


### Replace the dummy contents from `polls/views.py vote()` by

```python
# ...

def vote(request, question_id):
    question = get_object_or_404(Question, pk=question_id)
    try:
        selected_choice = question.choice_set.get(pk=request.POST['choice'])
    except (KeyError, Choice.DoesNotExist):
        # Redisplay the question voting form.
        return render(request, 'polls/detail.html', {
            'question': question,
            'error_message': "You didn't select a choice.",
        })
    else:
        selected_choice.votes += 1
        selected_choice.save()
        # Always return an HttpResponseRedirect after successfully dealing
        # with POST data. This prevents data from being posted twice if a
        # user hits the Back button.
        return HttpResponseRedirect(reverse('polls:results', args=(question.id,)))
```
- request.POST
  - is a dictionary-like object that lets us access submitted data by key name
  - `request.POST['choice']` returns the ID of the selected choice as string
  - return is always a string
- `HttpResponseRedirect` takes a sigle argument, the url to redirect to
- `reverse()`
  - this functions helps avoid having to hard-code a url
  - takes two arguments
    - name of the view that we want to pass control to
    - variable portion of the URL pattern that points to the view
  - return looks like: `/polls/2/results/`
- `request` is a [HttpRequest](https://docs.djangoproject.com/en/4.1/ref/request-response/#django.http.HttpRequest) object
##### Info: always return a `HttpResponseRedirect` after succesfully dealing with POST data - solution to the `Back` button problem 😉

### Update the `polls/views results()` method

```python
# ...

def results(request, question_id):
  question = get_object_or_404(Question, pk=question_id)
  return render(request, 'polls/results.html', {'question': question })
```
Note: this is almost the same as `detail()` with the difference being the template name.

### Finally add the template file `results.html` accordingly

```html
<!--polls/templates/polls/results.html-->
<h1>{{ question.question_text}}</h1>

<ul>
{% for choice in question.choice_set.all %}
  <li>{{ choice.choice_text }} -- {{ choice.votes }} vote{{ choice.votes|pluralize }}</li>
{% endfor %}
</ul>

<a href=" {% url 'polls:detail' question.id %} ">Vote again? </a>
```
Note: there is an issue with the `vote()` function. If two users vote at the same time a problem might occur called [race condition](https://docs.djangoproject.com/en/4.1/ref/models/expressions/#avoiding-race-conditions-using-f)

### Use Generic views: less code is better then
steps taken here:
- 1. convert the URLconf
- 2. Delete some of the old, unneeded views
- 3. Introduce new views based on Django's generic views

##### Info: use generic views where possible from the start, but understand what to do if we need more flexibility

#### Amend URLconf

```python
# polls/urls.py

from django.urls import path
from . import views

app_name = 'polls'

urlpatterns = [
  path('', views.IndexView.as_view(), name="index"),
  path('<int:pk>/', views.DetailView.as_view(), name="detail"),
  path("<int:pk>/results/", views.ResultsView.as_view(), name="results"),
  path('<int:question_id>/vote/', views.vote, name='vote')
]
```
Note: the name of the matched pattern in the path strings of the second and third patterns has changed from `<question_id>` to `<pk>`.

#### Amend Views

replace the old `index`, `detail`, and `results` views
```python
# polls/views

from django.http import HttpResponseRedirect
from django.shortcuts import get_object_or_404, render
from django.urls import reverse
from django.views import generic

from .models import Choice, Question

class IndexView(generic.ListView):
  template_name = 'polls/index.html'
  context_object_name = 'latest_question_list'

  def get_queryset(self):
    return Question.objects.order_by('-pub_date')[:5]

class DetailView(generic.DetailView):
  model = Question
  template_name = 'polls/detail.html'

class ResultsView(generic.DetailView):
  modal = Question
  template_name = "polls/results.html"
  queryset = Question.objects.all()

# ...
```
Here
- those with `_name` are either template urls or the names by which they will be referred to in the template files
- Each generic view needs to know what model it will be acting upon.
  - `model` = `model_name`
- `DetailView` generic view expects the primary key value to be captured form the URL  to be called `pk`, so `question_id` have been changed to `pk`
- `DetailView` generic view uses a template called `<app name>/<modelname>_detail.html`.
  - `template_name` attribute defines a specific name instead
- `ListView` uses a default template called `<app name>/<model name>_list.html` template
  - we use `template_name` to tell `ListView` to use our existing `polls/index.html` template
- context
  - `DetailView` generic view can use model `Question` to generate `question` variable automatically
  - `ListView` would generate `question_list` automatically, but we want to override it
    - `context_object_name` attribute  is used to given another variable instead
    - `get_queryset()` helps generate the new list

##### Info: A weird problem: we need to add `queryset = Question.objects.all()` to the `ResultsView` to avoid error that looks like
```
C:\Python310\lib\site-packages\django\views\generic\detail.py", line 72, in get_queryset raise ImproperlyConfigured( django.core.exceptions.ImproperlyConfigured: ResultsView is missing a QuerySet. Define ResultsView.model, ResultsView.queryset, or override ResultsView.get_queryset().
```
Extra info: `git diff starting-commit-sha ending-commit-sha myPatch. patch` can be used to create a git patch file

## [Part 5] Setting up automated tests for the app

### Automated Tests

- Tests are routines that check the operation of the code
- it can be appplied to
  - tiny details
  - overall operation of the software
### Why Tests
#### Save time
- saves time for large applications

#### They often prevent problems from occurring
- it identifies the problem from inside and points us to it

#### More attractive
- code without tests is broken by design

#### Teamwork
- one programmer may not break another's codes if the tests are established beforehand

### Testing Strategies
- 1. `test-driven development` - some write tests before writing their code
  - formalizes the problem in a Python test case
- 2. write tests as each functionality is implemented

### Writing our first test

#### Identify a bug
there is the bug in the code function: `was_published_recently()` method
- it return true both when the date is within last 24 hour or in the future as well

#### Create the test
```python
# polls/tests.py

import datetime

from django.test import TestCase
from django.utils import timezone

from .models import Question


class QuestionModelTests(TestCase):

    def test_was_published_recently_with_future_question(self):
        """
        was_published_recently() returns False for questions whose pub_date
        is in the future.
        """
        time = timezone.now() + datetime.timedelta(days=30)
        future_question = Question(pub_date=time)
        self.assertIs(future_question.was_published_recently(), False)

```

#### Running tests

```
python manage.py test polls
```
- `manage.py test polls` looks for tests in the polls application
- `django.test.TestCase` is found
- a special database is created for the purpose of testing
- it looks for test methods - ones that begin with `test`
- a `Question` instance with a future `pub_date` is created
- `self.assertIs` checks to see if the value returned is false

#### Fixing the bug

modify `was_published_recently`:
```python
# polls/models.py

def was_published_recently(self):
  now = timezone.now()
  return now - datetime.timedelta(days=1) <= self.pub_date <= now

```
Bug is fixed, and with the test setup, we might be informed if there is any change that creates error in this function.

#### More comprehensive tests

```python

#..
# class
  def test_was_published_recently_with_old_question(self):
        """
        was_published_recently() returns False for questions whose pub_date
        is older than 1 day.
        """
        time = timezone.now() - datetime.timedelta(days=1, seconds=1)
        old_question = Question(pub_date=time)
        self.assertIs(old_question.was_published_recently(), False)

    def test_was_published_recently_with_recent_question(self):
        """
        was_published_recently() returns True for questions whose pub_date
        is within the last day.
        """
        time = timezone.now() - datetime.timedelta(hours=23, minutes=59, seconds=59)
        recent_question = Question(pub_date=time)
        self.assertIs(recent_question.was_published_recently(), True)

```
now we have 3 tests that confirm that `Question.was_published_recently()` returns sensible values for past, recent, and future questions.

### Test a View