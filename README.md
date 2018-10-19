# django-backend

## Intro

### Command Line Cheat Sheet

#### Vagrant

Useful commands for managing your Vagrant box

###### Destroy Vagrant Box
```
vagrant destroy
```

###### Start Vagrant Box
```
vagrant up
```

###### Shutdown Vagrant Box
```
vagrant halt
```

###### Restart Vagrant Box
```
vagrant reload
```

###### Connect to Vagrant Box
```
vagrant ssh
```


#### Virtual Environments

To manage the Virtual Environments you must be connected to the Vagrant box.

###### Create Virtual Environment
```
mkvirtualenv <env-name> --python=python3
```

###### Remote Virtual Environment
```
rmvirtualenv <env-name>
```

###### Switch to Virtual Environment
```
workon <env-name>
```

###### Switch Off Virtual Environment
```
deactivate
```


#### Python PIP

Here are some useful commands for working with the PIP Python Package Manager.
Ensure you are connected to the Vagrant box and working on a Virtual Environment.

###### Installing Packages
```
pip install <package-name>
```

###### Saving Packages
```
pip freeze > <destination/requirements.txt>
```

###### Installing from Requirements File
```
pip install -r <source/requirements.txt>
```

###### Removing Packages
```
pip uninstall <package-name>
```

###### Updating Packages
```
pip install --upgrade <package-name>
```

#### Django Admin CLI

Ensure that you are working on the virtual environment and then Django python package is
installed.

###### Create Django Project
```
django-admin.py startproject <project_name>
```

#### Django Management CLI

Ensure you are working on the Virtual environment, Django is installed and you are switched to
the root of your Django project (where the **manage.py** file is).

###### Create Django App
```
python manage.py startapp <app_name>
```

###### Create Django Migrations
```
python manage.py makemigrations
```

###### Run Django Migrations
```
python manage.py migrate
```

###### Create Django Superuser
```
python manage.py createsuperuser
```

###### Resetting Superuser Password
```
python manage.py changepassword <email>
```

#### Git

Use Git from your local machine (don’t run from the Vagrant box).

###### Initialize Repository
```
git init
```

###### Add Files to Git
```
git add .
```

###### Commit new File
```
git commit -am “<commit message>”
```

###### View Previous Commits
```
git log
```

###### Checkout Previous Commit
```
git checkout <commit-id>
```

###### Switch back to Master
```
git checkout master
```

### Technologies used
- VirtualBox
- Vagrant
- Python
- Django
- Django REST Framework
- Atom
- Git
- ModHeader (Chrome Extension)

#### Categories

1.- Development Server (Vagrant > VirtualBox > Virtual Server - Application Code)

2.- Application Code (Python, Django, Django REST Framework)

3.- Tools (Atom, Git, ModHeader)

## Installation
- Vagrant - https://www.vagrantup.com/downloads.html
- VirtualBox - https://www.virtualbox.org/wiki/Downloads
- ModHeader - https://chrome.google.com/webstore/detail/modheader/idgpnmonknjnojddfkpgkljpfnnfcklj?hl=en
- Atom - https://atom.io/
- Git-SCM - https://git-scm.com/downloads

## Setting up your project

### Creating a workspace
- Create a "workspace" directory/folder
- Inside "workspace" folder, create a folder for the project called "profiles-rest-api"
- Atom > File > Add Project Folder > Select "workspace/profiles-rest-api"


### Creating a Git project
- Markdown Cheatsheet - https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet
- Github Python.gitignore - https://gist.github.com/LondonAppDev/66c3291e4f487ac92fcc96735e44c35e

## Creating a development server

### Creating a Vagrantfile

- Go to ~/workspace/profiles-rest-api in the terminal
- Initialize the vagrant project: 
```
vagrant init
```
You should receive a message like this:
```
A `Vagrantfile` has been placed in this directory. You are now
ready to `vagrant up` your first virtual environment! Please read
the comments in the Vagrantfile as well as documentation on
`vagrantup.com` for more information on using Vagrant.
```
And now there's a basic Vagrantfile in that folder

### Configuring our Vagrant box

*Vagrantfile Basic Template for Django Projects:*
```
# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://atlas.hashicorp.com/search.
  config.vm.box = "ubuntu/xenial64"

  config.vm.network "forwarded_port", host_ip: "127.0.0.1", guest: 8080, host: 8080

  config.vm.provision "shell", inline: <<-SHELL
    # Update and upgrade the server packages.
    sudo apt-get update
    sudo apt-get -y upgrade
    # Set Ubuntu Language
    sudo locale-gen en_GB.UTF-8
    # Install Python, SQLite and pip
    sudo apt-get install -y python3-dev sqlite python-pip
    # Upgrade pip to the latest version.
    sudo pip install --upgrade pip
    # Install and configure python virtualenvwrapper.
    sudo pip install virtualenvwrapper
    if ! grep -q VIRTUALENV_ALREADY_ADDED /home/vagrant/.bashrc; then
        echo "# VIRTUALENV_ALREADY_ADDED" >> /home/vagrant/.bashrc
        echo "WORKON_HOME=~/.virtualenvs" >> /home/vagrant/.bashrc
        echo "PROJECT_HOME=/vagrant" >> /home/vagrant/.bashrc
        echo "source /usr/local/bin/virtualenvwrapper.sh" >> /home/vagrant/.bashrc
    fi
  SHELL

end
```

### Running and connecting to our dev server
- Go to ~/workspace/profiles-rest-api in the terminal
- Start the Vagrant Server
```
vagrant up
```
- Connect to the Vagrant Server
```
vagrant ssh
```

### Running a Hello World script
- Disconnect to the Vagrant Server
```
exit
```
- Reconnect to the Vagrant Server

*start the existing image*
```
vagrant up
```
*connect to the Vagrant Server*
```
vagrant ssh
```

Vagrant synchronize the files on local to the folder "/vagrant"
```
cd /vagrant
```
List the files on that folder
```
ls -a
```
Your should see all the same files as your local

#### Hello World

Add a file to the folder `profiles-rest-api` on your local called `hello_world.py` with this line and save it
```
print("Hello World!")
```
Run the script on our server
Go to the terminal where you are inside the Vagrant Sever under `/vagrant` and run the script
```
python hello_world.py
```

## Creating a Django app

### Create Python Virtual Environment

```
cd workspace/profiles-rest-api/
```

```
vagrant up
```

```
vagrant ssh
```

```
mkvirtualenv profiles_api --python=python3
```

```
deactivate
```

- Virtual Environment (Docs) - https://python-guide.readthedocs.io/en/latest/dev/virtualenvs/

### Install required Python packages

```
workon profiles_api
```

```
pip install django==1.11
```

```
pip install djangorestframework==3.6.2
```


### Create a new Django project & app

Open `profiles-rest-api` in Atom
Right Click and add `New Folder` called `src`
Go to the terminal where you are connected to the Vagrant Server and working on the `profiles_api`

```
cd /vagrant/src
```

```
django-admin.py startproject profiles_project
```

```
cd profiles_project
```

```
python manage.py startapp profiles_api
```

### Enable our app in the Django settings file

ATOM:
profiles-rest-api > profiles_project > profiles_project > `settings.py`

Enable the following apps adding them to the list:
```
INSTALLED_APPS = [
... 
...
...
'rest_framework',
'rest_framework.authtoken',
'profiles_api',
]
```

### Saving our requirements

```
workon profiles_api
```

```
cd /vagrant/
```

```
pip freeze
```

```
pip freeze > requirements.txt
```

### Test and commit our changes

Make sure you are connected to our development server and you're workinng on the profiles API

```
cd /vagrant/src/profiles_project
```

```
python manage.py runserver 0.0.0.0:8080
```

Go to Chrome to `127.0.0.1:8080`

Open a separate terminal

```
cd workspace/profiles-rest-api
```

```
git add .
```

```
git commit -am "Added django project and app. Added requirements" 
```

## Setup the Database

### What are Django Models?

In Django we use models to describe the data that we need for our application.
Django uses these models to automatically set up and configure our database to store our data effectively.
Each model we describe in our Django models maps to a specific table on our database.
Django handles the interaction between out model and our database on our behalf so we don't need to write any sql statement or interact with the database directly we do it all using the Django models

- Django Models (Official Docs) - https://docs.djangoproject.com/en/1.11/topics/db/models/

### Create our user database model

*models.py*

```
# ... Imports Section ... #
from django.contrib.auth.models import AbstractBaseUser
from django.contrib.auth.models import PermissionsMixin
# ....................... #


class UserProfile(AbstractBaseUser, PermissionsMixin):
    """
    Represents a "user profile" inside out system. Stores all user account
    related data, such as 'email address' and 'name'.
    """

    email = models.EmailField(max_length=255, unique=True)
    name = models.CharField(max_length=255)
    is_active = models.BooleanField(default=True)
    is_staff = models.BooleanField(default=False)

    objects = UserProfileManager()

    USERNAME_FIELD = 'email'
    REQUIRED_FIELDS = ['name']

    def get_full_name(self):
        """Django uses this when it needs to get the user's full name."""

        return self.name

    def get_short_name(self):
        """Django uses this when it needs to get the users abbreviated name."""

        return self.name

    def __str__(self):
        """Django uses this when it needs to convert the object to text."""

        return self.email
```

- Django Model Fields (Official Docs) - https://docs.djangoproject.com/en/1.11/ref/models/fields/
- Substituting a custom User model (Official Docs) - https://docs.djangoproject.com/en/1.11/topics/auth/customizing/#auth-custom-user

### Add a user model manager

```
from django.contrib.auth.models import BaseUserManager
```

```
class UserProfileManager(BaseUserManager):
    """Helps Django work with out custom user model"""
    
    def create_user(self, email, name, password=None):
        """Creates a new user profile object"""
    
        if not email:
            raise ValueError('Users must have an email address')
        
        email = self.normalize_email(email)
        user = self.model(email=email, name=name)
    
        user.set_password(password)
        user.save(using=self._db)
    
        return user
        
    def create_superuser(self, email, name, password):
        """Creates and saves a new superuser with given details"""
        
        user = self.create_user(email, name, password)
        
        user.is_superuser = True
        user.is_staff = True
        
        user.save(using=self._db)
        
        return user
```

### Set our custom user model

Inside Atom:
profiles-rest-api > src > profiles_project > `setting.py`

Add the following last line to that file:
```
STATIC_URL = '/static/'

AUTH_USER_MODEL = 'profiles_api.UserProfile'
```

### Create migrations and sync DB

On the terminal go to `/workspace/profiles-rest-api`

```
vagrant up
```

```
vagrant ssh
```

```
workon profiles_api
```

```
cd /vagrant/src/profiles_project
```

```
python manage.py makemigrations
```

```
python manage.py migrate
```

## Setup Django Admin

### Creating a superuser

In the terminal go to the folder of the project

```
cd workspace/profiles-rest-api
```

```
vagrant up
```

```
vagrant ssh
```

```
workon profiles_api
```

```
cd /vagrant/src/profiles_project/
```

```
python manage.py createsuperuser
```

- Django Admin (Official Docs) - https://docs.djangoproject.com/en/1.11/ref/contrib/admin/

### Enable Django Admin

Open `profiles-rest-api` project on Atom

profiles-rest-api > src > profiles_project > profiles_api > `admin.py`

```
from django.contrib import admin

from . import models

# Register your model here.
admin.site.register(models.UserProfile)
```

### Test Django Admin

On the terminal inside the vagrant server, with `profiles_api` active
On the folder `/vagrant/src/profiles_project`

```
python manage.py runserver 0.0.0.0:8080
```

Now go to Chrome:

```
127.0.0.1:8080/admin
```

## Introduction to API Views

### What is an APIView?

Django Rest Framework offers a couple of helper classes we can use to create our API endpoints.

- APIView
- Viewset

Both ways are slightly different and offer their own benefits.

#### What are APIViews?

The API view is the most basic type of view we can use to build our API. It enables us to describe the logic which makes our API endpoint.

An API view allows us to define functions that match the standard HTTP methods
- GET, POST, PUT, PATCH, DELETE

Gives you the most control over the logic:
- Perfect for implementing complex logic
- Calling other APIs
- Working with local files

By allowing us to customize the function for each HTTP method on our API URL, API views give us the most control over our application logic. This is perfect in cases where you need to do something a little bit different from simply updating objects in the database

#### When to use APIViews?

Some examples of when to use an APIView:
- You need the full control over the logic
- Processing files and rendering a synchronous response
- You are calling other APIs/services
- Accessing local files or data

- APIView (Official Docs) - http://www.django-rest-framework.org/api-guide/views/

### Create first APIView

Open `profiles-rest-api` project in Atom:

profiles-rest-api > src > project_project > profiles_api > `views.py`



```
from django.shortcuts import render

from rest_framework.views import APIView
from rest_framework.response import Response

# Create your views here

class HelloApiView(APIView):
    """Test API View."""

    def get(self, request, format=None):
        """Returns a list of APIView features."""

        an_apiview= [
            'Uses HTTP methods as functions (get, post, patch, put, delete)',
            'Is similar to a traditional Django View',
            'Gives you the most control over your logic',
            'Is mapped manually to URLs',
        ]

        return Response({'message': 'Hello!', 'an_apiview': an_apiview})
```

### Configure view URL

Open `profiles-rest-api` project in Atom:

profiles-rest-api > src > project_project > profiles_api > profiles_project > `utils.py`

```
"""profiles_project URL Configuration
The `urlpatterns` list routes URLs to views. For more information please see:
    https://docs.djangoproject.com/en/1.11/topics/http/urls/
Examples:
Function views
    1. Add an import:  from my_app import views
    2. Add a URL to urlpatterns:  url(r'^$', views.home, name='home')
Class-based views
    1. Add an import:  from other_app.views import Home
    2. Add a URL to urlpatterns:  url(r'^$', Home.as_view(), name='home')
Including another URLconf
    1. Import the include() function: from django.conf.urls import url, include
    2. Add a URL to urlpatterns:  url(r'^blog/', include('blog.urls'))
"""
from django.conf.urls import url
from django.conf.urls import include
from django.contrib import admin

urlpatterns = [
    url(r'^admin/', admin.site.urls),
    url(r'^api/', include('profiles_api.urls')),
]
```

Create a new file under

`src\profiles_project\profiles_api\urls.py`

```
from django.conf.urls import url

from . import views


urlpatterns = [
    url(r'^hello-view/', views.HelloApiView.as_view()),
]
```

- URL Dispatcher (Official Docs) - https://docs.djangoproject.com/en/1.11/topics/http/urls/

### Testing our API View

In the terminal window that is running our profiles project vagrant server
Under `/vagrant/src/profiles_project` 
With `profiles_api` active

```
python manage.py runserver 0.0.0.0:8080
```

Go to Chrome 

```
127.0.0.1:8080/api/hello-view/
```


### Create a Serializer

In Atom inside `profiles-rest-api` project create a new file, `serializers.py`

profiles-rest-api > src > profiles_project > profiles_api > New file `serializers.py`

```
from rest_framework import serializers


class HelloSerializer(serializers.Serializer):
    """Serializes a name field for testing out APIView"""

    name = serializers.CharField(max_length=10)
```

- Serializers (Official Docs) - http://www.django-rest-framework.org/api-guide/serializers/
- Serializer Fields (Official Docs) - http://www.django-rest-framework.org/api-guide/fields/

### Add POST method to APIView

In Atom inside `profiles-rest-api` project

profiles-rest-api > src > profiles_project > profiles_api > `views.py`

```
from django.shortcuts import render

from rest_framework.views import APIView
from rest_framework.response import Response

from . import serializers
from rest_framework import status

# Create your views here.

class HelloApiView(APIView):
    """Test API View."""
    
    serializer_class = serializers.HelloSerializer

    def get(self, request, format=None):
        """Returns a list of APIView features."""

        an_apiview = [
            'Uses HTTP methods as function (get, post, patch, put, delete)',
            'It is similar to a traditional Django view',
            'Gives you the most control over your logic',
            'Is mapped manually to URLs'
        ]

        return Response({'message': 'Hello!', 'an_apiview': an_apiview})
      
    def post(self, request):
        """Create a hello message with our name."""

        serializer = serializers.HelloSerializer(data=request.data)

        if serializer.is_valid():
            name = serializer.data.get('name')
            message = 'Hello {0}!'.format(name)
            return Response({'message': message})
        else:
            return Response(
                serializer.errors, status=status.HTTP_400_BAD_REQUEST)
```

- Status Codes (Official Docs) - http://www.django-rest-framework.org/api-guide/status-codes/

### Test POST Function

In the terminal inside our Vagrant Server and under `/vagrant/src/profiles_project` with `profiles_api` active

```
python manage.py runserver 0.0.0.0:8080
```

Go to Chrome under:

```
127.0.0.1:8080/api/hello-view/
```

### Add PUT, PATCH and DELETE methods

```
def put(self, request, pk=None):
    """Handles updating an object."""
    
    return Response({'method': 'put'})
    
def patch(self, request, pk=None):
    """Patch request, only updates fields provided in the request""""
    
    return Response({'method': 'patch'})
    
def delete(self, request, pk=None):
    """Deletes an object"""
    
    return Response({'method': 'delete'})
```

### Test the PUT, PATCH and DELETE methods

In the terminal inside our Vagrant Server and under `/vagrant/src/profiles_project` with `profiles_api` active

```
python manage.py runserver 0.0.0.0:8080
```

Go to Chrome under:

```
127.0.0.1:8080/api/hello-view/
```

## Introduction to Viewsets

### What is a Viewset?

Django Rest Framework offers two classes that help us write the logic for our API

- APIView
- Viewset

**Viewsets**

Uses model operations for functions:
- List, Create, Retrieve, Update, Partial Update, Destroy

Takes care a lot of typical logic for you:
- Perfect for standard database operations
- Fastest way to make a database interface

**When to use Viewsets?**

Examples of when you might use a Viewset:
- You need a simple CRUD interface to your database
- You want a quick and simple API
- You need little to no customization on the logic
- You are working with standard data structures

### Create a simple Viewset

In Atom inside `profiles-rest-api` project

profiles-rest-api > src > profiles_project > profiles_api > `views.py`

```
from django.shortcuts import render

from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework import status
from rest_framework import viewsets

from . import serializers

# Create your views here.

class HelloApiView(APIView):
    """Test API View."""

    serializer_class = serializers.HelloSerializer

    def get(self, request, format=None):
        """Returns a list of APIView features."""

        an_apiview = [
            'Uses HTTP methods as function (get, post, patch, put, delete)',
            'It is similar to a traditional Django view',
            'Gives you the most control over your logic',
            'Is mapped manually to URLs'
        ]

        return Response({'message': 'Hello!', 'an_apiview': an_apiview})

    def post(self, request):
        """Create a hello message with our name."""

        serializer = serializers.HelloSerializer(data=request.data)

        if serializer.is_valid():
            name = serializer.data.get('name')
            message = 'Hello {0}'.format(name)
            return Response({'message': message})
        else:
            return Response(
                serializer.errors, status=status.HTTP_400_BAD_REQUEST)

    def put(self, request, pk=None):
        """Handles updating an object."""

        return Response({'method': 'put'})

    def patch(self, request, pk=None):
        """Patch request, only updates fields provided in the request."""

        return Response({'method': 'patch'})

    def delete(self, request, pk=None):
        """Deletes and object."""

        return Response({'method': 'delete'})
      
class HelloViewSet(viewsets.ViewSet):
    """Test API ViewSet"""

    def list(self, request):
        """Return a hello message."""

        a_viewset = [
            'Uses actions (list, create, retrieve, update, partial_update)',
            'Automatically maps to URLS using Routers',
            'Provides more functionality with less code'
        ]

        return Response({'message': 'Hello!', 'a_viewset': a_viewset})
```

### Add URL Router

In Atom inside `profiles-rest-api` project

profiles-rest-api > src > profiles_project > profiles_api > `urls.py`

```
from django.conf.urls import url
from django.conf.urls import include

from rest_framework.routers import DefaultRouter

from . import views

router = DefaultRouter()
router.register('hello-viewset', views.HelloViewSet, base_name='hello-viewset')

urlpatterns = [
    url(r'^hello-view/', views.HelloApiView.as_view()),
    url(r'', include(router.urls)),
]
```

### Testing our Viewset

In the terminal inside our Vagrant Server and under `/vagrant/src/profiles_project` with `profiles_api` active

```
python manage.py runserver 0.0.0.0:8080
```

Go to Chrome under:

```
127.0.0.1:8080/api/
```

### Add create, retrieve, update, partial_update and destroy functions

In Atom inside `profiles-rest-api` project

profiles-rest-api > src > profiles_project > profiles_api > `views.py`

```
from django.shortcuts import render

from rest_framework import viewsets
from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework import status

from . import serializers

# Create your views here.

class HelloApiView(APIView):
    """Test API View."""

    serializer_class = serializers.HelloSerializer

    def get(self, request, format=None):
        """Returns a list of APIView features."""

        an_apiview = [
            'Uses HTTP methods as function (get, post, patch, put, delete)',
            'It is similar to a traditional Django view',
            'Gives you the most control over your logic',
            'Is mapped manually to URLs'
        ]

        return Response({'message': 'Hello!', 'an_apiview': an_apiview})

    def post(self, request):
        """Create a hello message with our name."""

        serializer = serializers.HelloSerializer(data=request.data)

        if serializer.is_valid():
            name = serializer.data.get('name')
            message = 'Hello {0}'.format(name)
            return Response({'message': message})
        else:
            return Response(
                serializer.errors, status=status.HTTP_400_BAD_REQUEST)

    def put(self, request, pk=None):
        """Handles updating an object."""

        return Response({'method': 'put'})

    def patch(self, request, pk=None):
        """Patch request, only updates fields provided in the request."""

        return Response({'method': 'patch'})

    def delete(self, request, pk=None):
        """Deletes and object."""

        return Response({'method': 'delete'})


class HelloViewSet(viewsets.ViewSet):
    """Test API ViewSet."""

    serializer_class = serializers.HelloSerializer
    
    def list(self, request):
        """Return a hello message."""

        a_viewset = [
            'Uses actions (list, create, retrieve, update, partial_update)',
            'Automatically maps to URLs using Routers',
            'Provides more functionality with less code.'
        ]

        return Response({'message': 'Hello!', 'a_viewset': a_viewset})

    def create(self, request):
        """Create a new hello message."""

        serializer = serializers.HelloSerializer(data=request.data)

        if serializer.is_valid():
            name = serializer.data.get('name')
            message = 'Hello {0}'.format(name)
            return Response({'message': message})
        else:
            return Response(
                serializer.errors, status=status.HTTP_400_BAD_REQUEST)

    def retrieve(self, request, pk=None):
        """Handles getting an object by its ID."""

        return Response({'http_method': 'GET'})

    def update(self, request, pk=None):
        """Handles updating an object."""

        return Response({'http_method': 'PUT'})

    def partial_update(self, request, pk=None):
        """Handles updating part of an object."""

        return Response({'http_method': 'PATCH'})
        
    def destroy(self, request, pk=None):
        """Handles removing an object"""
        
        return Response('http_method': 'DELETE')
```

### Test Viewset

In the terminal inside our Vagrant Server and under `/vagrant/src/profiles_project` with `profiles_api` active

```
python manage.py runserver 0.0.0.0:8080
```

Go to Chrome under:

```
127.0.0.1:8080/api/hello-viewset/
```

```
127.0.0.1:8080/api/hello-viewset/1/
```

## Create Profiles API

### Plan our Profile API

**Basic Requirements**

- Create new profile
    - Validate profile data
- List existing profiles
    - Search for profiles
- View specific profile
- Update my profile of logged in user
    - Update name/email address
    - Change password
- Delete profile

**API URLs**

URLs for our API:
- `/api/profile/` - list all profiles
    - GET (list profiles)
    - POST (create profile)
- `/api/profile/<profile_id>` - manage specific profile
    - GET (view specific profile)
    - PUT / PATCH (update profile)
    - DELETE (remove profile)


### Create user profile serializer

In Atom inside `profiles-rest-api` project

profiles-rest-api > src > profiles_project > profiles_api > `serializers.py`

```
from rest_framework import serializers

from . import models


class HelloSerializer(serializers.Serializer):
    """Serializes a name field for testing our APIView."""

    name = serializers.CharField(max_length=10)


class UserProfileSerializer(serializers.ModelSerializer):
    """A serializer for our user profile objects."""

    class Meta:
        model = models.UserProfile
        fields = ('id', 'email', 'name', 'password')
        extra_kwargs = {'password': {'write_only': True}}

    def create(self, validated_data):
        """Create and return a new user."""

        user = models.UserProfile(
            email=validated_data['email'],
            name=validated_data['name']
        )

        user.set_password(validated_data['password'])
        user.save()

        return user
```

- ModelSerializer (Official Docs) - http://www.django-rest-framework.org/api-guide/serializers/#modelserializer

### Create profiles Viewset

In Atom inside `profiles-rest-api` project

profiles-rest-api > src > profiles_project > profiles_api > `views.py`

```
from django.shortcuts import render

from rest_framework import viewsets
from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework import status

from . import serializers
from . import models

# Create your views here.

class HelloApiView(APIView):
    """Test API View."""

    serializer_class = serializers.HelloSerializer

    def get(self, request, format=None):
        """Returns a list of APIView features."""

        an_apiview = [
            'Uses HTTP methods as function (get, post, patch, put, delete)',
            'It is similar to a traditional Django view',
            'Gives you the most control over your logic',
            'Is mapped manually to URLs'
        ]

        return Response({'message': 'Hello!', 'an_apiview': an_apiview})

    def post(self, request):
        """Create a hello message with our name."""

        serializer = serializers.HelloSerializer(data=request.data)

        if serializer.is_valid():
            name = serializer.data.get('name')
            message = 'Hello {0}'.format(name)
            return Response({'message': message})
        else:
            return Response(
                serializer.errors, status=status.HTTP_400_BAD_REQUEST)

    def put(self, request, pk=None):
        """Handles updating an object."""

        return Response({'method': 'put'})

    def patch(self, request, pk=None):
        """Patch request, only updates fields provided in the request."""

        return Response({'method': 'patch'})

    def delete(self, request, pk=None):
        """Deletes and object."""

        return Response({'method': 'delete'})


class HelloViewSet(viewsets.ViewSet):
    """Test API ViewSet."""

    serializer_class = serializers.HelloSerializer

    def list(self, request):
        """Return a hello message."""

        a_viewset = [
            'Uses actions (list, create, retrieve, update, partial_update)',
            'Automatically maps to URLs using Routers',
            'Provides more functionality with less code.'
        ]

        return Response({'message': 'Hello!', 'a_viewset': a_viewset})

    def create(self, request):
        """Create a new hello message."""

        serializer = serializers.HelloSerializer(data=request.data)

        if serializer.is_valid():
            name = serializer.data.get('name')
            message = 'Hello {0}'.format(name)
            return Response({'message': message})
        else:
            return Response(
                serializer.errors, status=status.HTTP_400_BAD_REQUEST)

    def retrieve(self, request, pk=None):
        """Handles getting an object by its ID."""

        return Response({'http_method': 'GET'})

    def update(self, request, pk=None):
        """Handles updating an object."""

        return Response({'http_method': 'PUT'})

    def partial_update(self, request, pk=None):
        """Handles updating part of an object."""

        return Response({'http_method': 'PATCH'})

    def destroy(self, request, pk=None):
        """Handles removing an object."""

        return Response({'http_method': 'DELETE'})


class UserProfileViewSet(viewsets.ModelViewSet):
    """Handles creating, creating and updating profiles."""

    serializer_class = serializers.UserProfileSerializer
    queryset = models.UserProfile.objects.all()
```

### Register profile Viewset with the URL router

In Atom inside `profiles-rest-api` project

profiles-rest-api > src > profiles_project > profiles_api > `urls.py`

```
from django.conf.urls import url
from django.conf.urls import include

from rest_framework.routers import DefaultRouter

from . import views

router = DefaultRouter()
router.register('hello-viewset', views.HelloViewSet, base_name='hello-viewset')
router.register('profile', views.UserProfileViewSet)

urlpatterns = [
    url(r'^hello-view/', views.HelloApiView.as_view()),
    url(r'', include(router.urls))
]
```

### Test creating a profile

In the terminal inside our Vagrant Server and under `/vagrant/src/profiles_project` with `profiles_api` active

```
python manage.py runserver 0.0.0.0:8080
```

Go to Chrome under:

```
127.0.0.1:8080/api/
```

```
127.0.0.1:8080/api/profile/
```

Create new user to test:
`Email:` test@test.com
`Name:` Test User
`Password:` Awesome1

Test updating a user:
```
127.0.0.1:8080/api/profile/2/
```

> Raw data
```
{
    "name": "Test User 2"
}
```


### Create permission class


In Atom inside `profiles-rest-api` project

Create a new file called `permisssions.py`

profiles-rest-api > src > profiles_project > profiles_api > `permisssions.py`

```
from rest_framework import permissions


class UpdateOwnProfile(permissions.BasePermission):
    """Allow users to edit their own profile."""

    def has_object_permission(self, request, view, obj):
        """Check user is trying to edit their own profile."""

        if request.method in permissions.SAFE_METHODS:
            return True

        return obj.id == request.user.id
```

- Permissions (Official Docs) - http://www.django-rest-framework.org/api-guide/permissions/

### Add authentication and permissions to Viewset

In Atom inside `profiles-rest-api` project

profiles-rest-api > src > profiles_project > profiles_api > `views.py`

```
from django.shortcuts import render

from rest_framework import viewsets
from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework import status
from rest_framework.authentication import TokenAuthentication

from . import serializers
from . import models
from . import permissions

# Create your views here.

class HelloApiView(APIView):
    """Test API View."""

    serializer_class = serializers.HelloSerializer

    def get(self, request, format=None):
        """Returns a list of APIView features."""

        an_apiview = [
            'Uses HTTP methods as function (get, post, patch, put, delete)',
            'It is similar to a traditional Django view',
            'Gives you the most control over your logic',
            'Is mapped manually to URLs'
        ]

        return Response({'message': 'Hello!', 'an_apiview': an_apiview})

    def post(self, request):
        """Create a hello message with our name."""

        serializer = serializers.HelloSerializer(data=request.data)

        if serializer.is_valid():
            name = serializer.data.get('name')
            message = 'Hello {0}'.format(name)
            return Response({'message': message})
        else:
            return Response(
                serializer.errors, status=status.HTTP_400_BAD_REQUEST)

    def put(self, request, pk=None):
        """Handles updating an object."""

        return Response({'method': 'put'})

    def patch(self, request, pk=None):
        """Patch request, only updates fields provided in the request."""

        return Response({'method': 'patch'})

    def delete(self, request, pk=None):
        """Deletes and object."""

        return Response({'method': 'delete'})


class HelloViewSet(viewsets.ViewSet):
    """Test API ViewSet."""

    serializer_class = serializers.HelloSerializer

    def list(self, request):
        """Return a hello message."""

        a_viewset = [
            'Uses actions (list, create, retrieve, update, partial_update)',
            'Automatically maps to URLs using Routers',
            'Provides more functionality with less code.'
        ]

        return Response({'message': 'Hello!', 'a_viewset': a_viewset})

    def create(self, request):
        """Create a new hello message."""

        serializer = serializers.HelloSerializer(data=request.data)

        if serializer.is_valid():
            name = serializer.data.get('name')
            message = 'Hello {0}'.format(name)
            return Response({'message': message})
        else:
            return Response(
                serializer.errors, status=status.HTTP_400_BAD_REQUEST)

    def retrieve(self, request, pk=None):
        """Handles getting an object by its ID."""

        return Response({'http_method': 'GET'})

    def update(self, request, pk=None):
        """Handles updating an object."""

        return Response({'http_method': 'PUT'})

    def partial_update(self, request, pk=None):
        """Handles updating part of an object."""

        return Response({'http_method': 'PATCH'})

    def destroy(self, request, pk=None):
        """Handles removing an object."""

        return Response({'http_method': 'DELETE'})


class UserProfileViewSet(viewsets.ModelViewSet):
    """Handles creating, creating and updating profiles."""

    serializer_class = serializers.UserProfileSerializer
    queryset = models.UserProfile.objects.all()
    authentication_classes = (TokenAuthentication,)
    permission_classes = (permissions.UpdateOwnProfile,)
```

### Test new permissions

In the terminal inside our Vagrant Server and under `/vagrant/src/profiles_project` with `profiles_api` active

```
python manage.py runserver 0.0.0.0:8080
```

Go to Chrome under:

```
127.0.0.1:8080/api/profile/
```

```
127.0.0.1:8080/api/profile/2/
```

### Add search profiles feature

In Atom inside `profiles-rest-api` project

profiles-rest-api > src > profiles_project > profiles_api > `views.py`

```
from django.shortcuts import render

from rest_framework import viewsets
from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework import status
from rest_framework.authentication import TokenAuthentication
from rest_framework import filters

from . import serializers
from . import models
from . import permissions

# Create your views here.

class HelloApiView(APIView):
    """Test API View."""

    serializer_class = serializers.HelloSerializer

    def get(self, request, format=None):
        """Returns a list of APIView features."""

        an_apiview = [
            'Uses HTTP methods as function (get, post, patch, put, delete)',
            'It is similar to a traditional Django view',
            'Gives you the most control over your logic',
            'Is mapped manually to URLs'
        ]

        return Response({'message': 'Hello!', 'an_apiview': an_apiview})

    def post(self, request):
        """Create a hello message with our name."""

        serializer = serializers.HelloSerializer(data=request.data)

        if serializer.is_valid():
            name = serializer.data.get('name')
            message = 'Hello {0}'.format(name)
            return Response({'message': message})
        else:
            return Response(
                serializer.errors, status=status.HTTP_400_BAD_REQUEST)

    def put(self, request, pk=None):
        """Handles updating an object."""

        return Response({'method': 'put'})

    def patch(self, request, pk=None):
        """Patch request, only updates fields provided in the request."""

        return Response({'method': 'patch'})

    def delete(self, request, pk=None):
        """Deletes and object."""

        return Response({'method': 'delete'})


class HelloViewSet(viewsets.ViewSet):
    """Test API ViewSet."""

    serializer_class = serializers.HelloSerializer

    def list(self, request):
        """Return a hello message."""

        a_viewset = [
            'Uses actions (list, create, retrieve, update, partial_update)',
            'Automatically maps to URLs using Routers',
            'Provides more functionality with less code.'
        ]

        return Response({'message': 'Hello!', 'a_viewset': a_viewset})

    def create(self, request):
        """Create a new hello message."""

        serializer = serializers.HelloSerializer(data=request.data)

        if serializer.is_valid():
            name = serializer.data.get('name')
            message = 'Hello {0}'.format(name)
            return Response({'message': message})
        else:
            return Response(
                serializer.errors, status=status.HTTP_400_BAD_REQUEST)

    def retrieve(self, request, pk=None):
        """Handles getting an object by its ID."""

        return Response({'http_method': 'GET'})

    def update(self, request, pk=None):
        """Handles updating an object."""

        return Response({'http_method': 'PUT'})

    def partial_update(self, request, pk=None):
        """Handles updating part of an object."""

        return Response({'http_method': 'PATCH'})

    def destroy(self, request, pk=None):
        """Handles removing an object."""

        return Response({'http_method': 'DELETE'})


class UserProfileViewSet(viewsets.ModelViewSet):
    """Handles creating, creating and updating profiles."""

    serializer_class = serializers.UserProfileSerializer
    queryset = models.UserProfile.objects.all()
    authentication_classes = (TokenAuthentication,)
    permission_classes = (permissions.UpdateOwnProfile,)
    filter_backends = (filters.SearchFilter,)
    search_fields = ('name', 'email',)
```

### Test searching profiles

In the terminal inside our Vagrant Server and under `/vagrant/src/profiles_project` with `profiles_api` active

```
python manage.py runserver 0.0.0.0:8080
```

Go to Chrome under:

```
127.0.0.1:8080/api/profile/
```

Create new user to test:

`Email:` brooke@test.com

`Name:` Brooke Rutherford

`Password:` Awesome1

-> User Profile List

-> Filters:

`Search:` Brooke 

-> Filters:

`Search:` test 

## Create login API

### Create login API viewset

Open Atom Project

profiles-rest-api > src > profiles_project > profiles_api > views.py

views.py

```
from django.shortcuts import render

from rest_framework import viewsets
from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework import status
from rest_framework.authentication import TokenAuthentication
from rest_framework import filters
from rest_framework.authtoken.serializers import AuthTokenSerializer
from rest_framework.authtoken.views import ObtainAuthToken

from . import serializers
from . import models
from . import permissions

# Create your views here.

class HelloApiView(APIView):
    """Test API View."""

    serializer_class = serializers.HelloSerializer

    def get(self, request, format=None):
        """Returns a list of APIView features."""

        an_apiview = [
            'Uses HTTP methods as function (get, post, patch, put, delete)',
            'It is similar to a traditional Django view',
            'Gives you the most control over your logic',
            'Is mapped manually to URLs'
        ]

        return Response({'message': 'Hello!', 'an_apiview': an_apiview})

    def post(self, request):
        """Create a hello message with our name."""

        serializer = serializers.HelloSerializer(data=request.data)

        if serializer.is_valid():
            name = serializer.data.get('name')
            message = 'Hello {0}'.format(name)
            return Response({'message': message})
        else:
            return Response(
                serializer.errors, status=status.HTTP_400_BAD_REQUEST)

    def put(self, request, pk=None):
        """Handles updating an object."""

        return Response({'method': 'put'})

    def patch(self, request, pk=None):
        """Patch request, only updates fields provided in the request."""

        return Response({'method': 'patch'})

    def delete(self, request, pk=None):
        """Deletes and object."""

        return Response({'method': 'delete'})


class HelloViewSet(viewsets.ViewSet):
    """Test API ViewSet."""

    serializer_class = serializers.HelloSerializer

    def list(self, request):
        """Return a hello message."""

        a_viewset = [
            'Uses actions (list, create, retrieve, update, partial_update)',
            'Automatically maps to URLs using Routers',
            'Provides more functionality with less code.'
        ]

        return Response({'message': 'Hello!', 'a_viewset': a_viewset})

    def create(self, request):
        """Create a new hello message."""

        serializer = serializers.HelloSerializer(data=request.data)

        if serializer.is_valid():
            name = serializer.data.get('name')
            message = 'Hello {0}'.format(name)
            return Response({'message': message})
        else:
            return Response(
                serializer.errors, status=status.HTTP_400_BAD_REQUEST)

    def retrieve(self, request, pk=None):
        """Handles getting an object by its ID."""

        return Response({'http_method': 'GET'})

    def update(self, request, pk=None):
        """Handles updating an object."""

        return Response({'http_method': 'PUT'})

    def partial_update(self, request, pk=None):
        """Handles updating part of an object."""

        return Response({'http_method': 'PATCH'})

    def destroy(self, request, pk=None):
        """Handles removing an object."""

        return Response({'http_method': 'DELETE'})


class UserProfileViewSet(viewsets.ModelViewSet):
    """Handles creating, creating and updating profiles."""

    serializer_class = serializers.UserProfileSerializer
    queryset = models.UserProfile.objects.all()
    authentication_classes = (TokenAuthentication,)
    permission_classes = (permissions.UpdateOwnProfile,)
    filter_backends = (filters.SearchFilter,)
    search_fields = ('name', 'email',)


class LoginViewSet(viewsets.ViewSet):
    """Checks email and password and returns an auth token."""

    serializer_class = AuthTokenSerializer

    def create(self, request):
        """Use the ObtainAuthToken APIView to validate and create a token."""

        return ObtainAuthToken().post(request)
```

profiles-rest-api > src > profiles_project > profiles_api > urls.py

urls.py

```
from django.conf.urls import url
from django.conf.urls import include

from rest_framework.routers import DefaultRouter

from . import views

router = DefaultRouter()
router.register('hello-viewset', views.HelloViewSet, base_name='hello-viewset')
router.register('profile', views.UserProfileViewSet)
router.register('login', views.LoginViewSet, base_name='login')

urlpatterns = [
    url(r'^hello-view/', views.HelloApiView.as_view()),
    url(r'', include(router.urls))
]
```

### Test login API 

Make sure your development server is running

On the terminal:
- Login to the Vagrant Server
- Work on the `profiles_api` virtual environment
- Change to the `/vagrant/src/profiles_project` location
- Run `python manage.py runserver 0.0.0.0:8080`

On Chrome:
- Go to `127.0.0.1:8080/api`
- We created a login API as a viewset, instead of as an API view because that way we can add it to this API root
- Click the URL here for the login API



### Set token header using ModHeader extension




## Create profile feed API

### Plan profile feed API

### Add new Model Item

models.py

```
from django.db import models
from django.contrib.auth.models import AbstractBaseUser
from django.contrib.auth.models import PermissionsMixin
from django.contrib.auth.models import BaseUserManager

# Create your models here.

class UserProfileManager(BaseUserManager):
    """Helps Django work with our custom user model."""

    def create_user(self, email, name, password=None):
        """Creates a new user profile object."""

        if not email:
            raise ValueError('Users must have an email address.')

        email = self.normalize_email(email)
        user = self.model(email=email, name=name)

        user.set_password(password)
        user.save(using=self._db)

        return user

    def create_superuser(self, email, name, password):
        """Creates and saves a new superuser with given details."""

        user = self.create_user(email, name, password)

        user.is_superuser = True
        user.is_staff = True

        user.save(using=self._db)

        return user


class UserProfile(AbstractBaseUser, PermissionsMixin):
    """Respents a "user profile" inside our system."""

    email = models.EmailField(max_length=255, unique=True)
    name = models.CharField(max_length=255)
    is_active = models.BooleanField(default=True)
    is_staff = models.BooleanField(default=False)

    objects = UserProfileManager()

    USERNAME_FIELD = 'email'
    REQUIRED_FIELDS = ['name']

    def get_full_name(self):
        """Used to get a users full name."""

        return self.name

    def get_short_name(self):
        """Used to get a users short name."""

        return self.name

    def __str__(self):
        """Django uses this when it needs to convert the object to a string"""

        return self.email


class ProfileFeedItem(models.Model):
    """Profile status update."""

    user_profile = models.ForeignKey('UserProfile', on_delete=models.CASCADE)
    status_text = models.CharField(max_length=255)
    created_on = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        """Return the model as a string."""

        return self.status_text
```

### Create and run model migration

### Add profile feed model to admin

admin.py

```

from django.contrib import admin

from . import models

# Register your models here.
admin.site.register(models.UserProfile)
admin.site.register(models.ProfileFeedItem)
```

### Create profile feed item serializer

serializers.py

```
from rest_framework import serializers

from . import models


class HelloSerializer(serializers.Serializer):
    """Serializes a name field for testing our APIView."""

    name = serializers.CharField(max_length=10)


class UserProfileSerializer(serializers.ModelSerializer):
    """A serializer for our user profile objects."""

    class Meta:
        model = models.UserProfile
        fields = ('id', 'email', 'name', 'password')
        extra_kwargs = {'password': {'write_only': True}}

    def create(self, validated_data):
        """Create and return a new user."""

        user = models.UserProfile(
            email=validated_data['email'],
            name=validated_data['name']
        )

        user.set_password(validated_data['password'])
        user.save()

        return user


class ProfileFeedItemSerializer(serializers.ModelSerializer):
    """A serializer for profile feed items."""

    class Meta:
        model = models.ProfileFeedItem
        fields = ('id', 'user_profile', 'status_text', 'create_on')
        extra_kwargs = {'user_profile': {'read_only': True}}

        
```

### Create Viewset for our profile feed Item

views.py

```
from django.shortcuts import render

from rest_framework import viewsets
from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework import status
from rest_framework.authentication import TokenAuthentication
from rest_framework import filters
from rest_framework.authtoken.serializers import AuthTokenSerializer
from rest_framework.authtoken.views import ObtainAuthToken

from . import serializers
from . import models
from . import permissions

# Create your views here.

class HelloApiView(APIView):
    """Test API View."""

    serializer_class = serializers.HelloSerializer

    def get(self, request, format=None):
        """Returns a list of APIView features."""

        an_apiview = [
            'Uses HTTP methods as function (get, post, patch, put, delete)',
            'It is similar to a traditional Django view',
            'Gives you the most control over your logic',
            'Is mapped manually to URLs'
        ]

        return Response({'message': 'Hello!', 'an_apiview': an_apiview})

    def post(self, request):
        """Create a hello message with our name."""

        serializer = serializers.HelloSerializer(data=request.data)

        if serializer.is_valid():
            name = serializer.data.get('name')
            message = 'Hello {0}'.format(name)
            return Response({'message': message})
        else:
            return Response(
                serializer.errors, status=status.HTTP_400_BAD_REQUEST)

    def put(self, request, pk=None):
        """Handles updating an object."""

        return Response({'method': 'put'})

    def patch(self, request, pk=None):
        """Patch request, only updates fields provided in the request."""

        return Response({'method': 'patch'})

    def delete(self, request, pk=None):
        """Deletes and object."""

        return Response({'method': 'delete'})


class HelloViewSet(viewsets.ViewSet):
    """Test API ViewSet."""

    serializer_class = serializers.HelloSerializer

    def list(self, request):
        """Return a hello message."""

        a_viewset = [
            'Uses actions (list, create, retrieve, update, partial_update)',
            'Automatically maps to URLs using Routers',
            'Provides more functionality with less code.'
        ]

        return Response({'message': 'Hello!', 'a_viewset': a_viewset})

    def create(self, request):
        """Create a new hello message."""

        serializer = serializers.HelloSerializer(data=request.data)

        if serializer.is_valid():
            name = serializer.data.get('name')
            message = 'Hello {0}'.format(name)
            return Response({'message': message})
        else:
            return Response(
                serializer.errors, status=status.HTTP_400_BAD_REQUEST)

    def retrieve(self, request, pk=None):
        """Handles getting an object by its ID."""

        return Response({'http_method': 'GET'})

    def update(self, request, pk=None):
        """Handles updating an object."""

        return Response({'http_method': 'PUT'})

    def partial_update(self, request, pk=None):
        """Handles updating part of an object."""

        return Response({'http_method': 'PATCH'})

    def destroy(self, request, pk=None):
        """Handles removing an object."""

        return Response({'http_method': 'DELETE'})


class UserProfileViewSet(viewsets.ModelViewSet):
    """Handles creating, creating and updating profiles."""

    serializer_class = serializers.UserProfileSerializer
    queryset = models.UserProfile.objects.all()
    authentication_classes = (TokenAuthentication,)
    permission_classes = (permissions.UpdateOwnProfile,)
    filter_backends = (filters.SearchFilter,)
    search_fields = ('name', 'email',)


class LoginViewSet(viewsets.ViewSet):
    """Checks email and password and returns an auth token."""

    serializer_class = AuthTokenSerializer

    def create(self, request):
        """Use the ObtainAuthToken APIView to validate and create a token."""

        return ObtainAuthToken().post(request)


class UserProfileFeedViewSet(viewsets.ModelViewSet):
    """Handles creating, reading and updating profile feed items."""

    authentication_classes = (TokenAuthentication,)
    serializer_class = serializers.ProfileFeedItemSerializer
    queryset = models.ProfileFeedItem.objects.all()

    def perform_create(self, serializer):
        """Sets the user profile to the logged in user."""

        serializer.save(user_profile=self.request.user)
```

urls.py

```
from django.conf.urls import url
from django.conf.urls import include

from rest_framework.routers import DefaultRouter

from . import views

router = DefaultRouter()
router.register('hello-viewset', views.HelloViewSet, base_name='hello-viewset')
router.register('profile', views.UserProfileViewSet)
router.register('login', views.LoginViewSet, base_name='login')
router.register('feed', views.UserProfileFeedViewSet)

urlpatterns = [
    url(r'^hello-view/', views.HelloApiView.as_view()),
    url(r'', include(router.urls))
]
```

### Test Feed API

### Add permissions for feed API

permissions.py

```

from rest_framework import permissions


class UpdateOwnProfile(permissions.BasePermission):
    """Allow users to edit their own profile."""

    def has_object_permission(self, request, view, obj):
        """Check user is trying to edit their own profile."""

        if request.method in permissions.SAFE_METHODS:
            return True

        return obj.id == request.user.id


class PostOwnStatus(permissions.BasePermission):
    """Allow users to update their own profile."""

    def has_object_permission(self, request, view, obj):
        """Check the user is trying to update their own status."""

        if request.method in permissions.SAFE_METHODS:
            return True

        return obj.user_profile.id == request.user.id
```

views.py

```
from django.shortcuts import render

from rest_framework import viewsets
from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework import status
from rest_framework.authentication import TokenAuthentication
from rest_framework import filters
from rest_framework.authtoken.serializers import AuthTokenSerializer
from rest_framework.authtoken.views import ObtainAuthToken
from rest_framework.permissions import IsAuthenticatedOrReadOnly

from . import serializers
from . import models
from . import permissions

# Create your views here.

class HelloApiView(APIView):
    """Test API View."""

    serializer_class = serializers.HelloSerializer

    def get(self, request, format=None):
        """Returns a list of APIView features."""

        an_apiview = [
            'Uses HTTP methods as function (get, post, patch, put, delete)',
            'It is similar to a traditional Django view',
            'Gives you the most control over your logic',
            'Is mapped manually to URLs'
        ]

        return Response({'message': 'Hello!', 'an_apiview': an_apiview})

    def post(self, request):
        """Create a hello message with our name."""

        serializer = serializers.HelloSerializer(data=request.data)

        if serializer.is_valid():
            name = serializer.data.get('name')
            message = 'Hello {0}'.format(name)
            return Response({'message': message})
        else:
            return Response(
                serializer.errors, status=status.HTTP_400_BAD_REQUEST)

    def put(self, request, pk=None):
        """Handles updating an object."""

        return Response({'method': 'put'})

    def patch(self, request, pk=None):
        """Patch request, only updates fields provided in the request."""

        return Response({'method': 'patch'})

    def delete(self, request, pk=None):
        """Deletes and object."""

        return Response({'method': 'delete'})


class HelloViewSet(viewsets.ViewSet):
    """Test API ViewSet."""

    serializer_class = serializers.HelloSerializer

    def list(self, request):
        """Return a hello message."""

        a_viewset = [
            'Uses actions (list, create, retrieve, update, partial_update)',
            'Automatically maps to URLs using Routers',
            'Provides more functionality with less code.'
        ]

        return Response({'message': 'Hello!', 'a_viewset': a_viewset})

    def create(self, request):
        """Create a new hello message."""

        serializer = serializers.HelloSerializer(data=request.data)

        if serializer.is_valid():
            name = serializer.data.get('name')
            message = 'Hello {0}'.format(name)
            return Response({'message': message})
        else:
            return Response(
                serializer.errors, status=status.HTTP_400_BAD_REQUEST)

    def retrieve(self, request, pk=None):
        """Handles getting an object by its ID."""

        return Response({'http_method': 'GET'})

    def update(self, request, pk=None):
        """Handles updating an object."""

        return Response({'http_method': 'PUT'})

    def partial_update(self, request, pk=None):
        """Handles updating part of an object."""

        return Response({'http_method': 'PATCH'})

    def destroy(self, request, pk=None):
        """Handles removing an object."""

        return Response({'http_method': 'DELETE'})


class UserProfileViewSet(viewsets.ModelViewSet):
    """Handles creating, creating and updating profiles."""

    serializer_class = serializers.UserProfileSerializer
    queryset = models.UserProfile.objects.all()
    authentication_classes = (TokenAuthentication,)
    permission_classes = (permissions.UpdateOwnProfile,)
    filter_backends = (filters.SearchFilter,)
    search_fields = ('name', 'email',)


class LoginViewSet(viewsets.ViewSet):
    """Checks email and password and returns an auth token."""

    serializer_class = AuthTokenSerializer

    def create(self, request):
        """Use the ObtainAuthToken APIView to validate and create a token."""

        return ObtainAuthToken().post(request)


class UserProfileFeedViewSet(viewsets.ModelViewSet):
    """Handles creating, reading and updating profile feed items."""

    authentication_classes = (TokenAuthentication,)
    serializer_class = serializers.ProfileFeedItemSerializer
    queryset = models.ProfileFeedItem.objects.all()
    permission_classes = (permissions.PostOwnStatus, IsAuthenticatedOrReadOnly)

    def perform_create(self, serializer):
        """Sets the user profile to the logged in user."""

        serializer.save(user_profile=self.request.user)
```

### Test feed API permissions

### Restrict viewing status updates to logged in user only

### Test new private feed



## Deploying our API to a server on AWS

### Introduction to deploying our app to AWS

- AWS Free Tier - https://aws.amazon.com/es/free/
- Github - https://github.com/

### Pushing our project to Github

### Add deployment script and configs to our project

- uWSGI official docs - https://uwsgi-docs.readthedocs.io/en/latest/

server_setup.sh

```
#!/usr/bin/env bash

set -e

# TODO: Set to URL of git repo.
PROJECT_GIT_URL='https://github.com/LondonAppDeveloper/byob-profiles-rest-api.git'

PROJECT_BASE_PATH='/usr/local/apps'
VIRTUALENV_BASE_PATH='/usr/local/virtualenvs'

# Set Ubuntu Language
locale-gen en_GB.UTF-8

# Install Python, SQLite and pip
echo "Installing dependencies..."
apt-get update
apt-get install -y python3-dev python3-venv sqlite python-pip supervisor nginx git

mkdir -p $PROJECT_BASE_PATH
git clone $PROJECT_GIT_URL $PROJECT_BASE_PATH/profiles-rest-api

mkdir -p $VIRTUALENV_BASE_PATH
python3 -m venv $VIRTUALENV_BASE_PATH/profiles_api

$VIRTUALENV_BASE_PATH/profiles_api/bin/pip install -r $PROJECT_BASE_PATH/profiles-rest-api/requirements.txt

# Run migrations
cd $PROJECT_BASE_PATH/profiles-rest-api/src

# Setup Supervisor to run our uwsgi process.
cp $PROJECT_BASE_PATH/profiles-rest-api/deploy/supervisor_profiles_api.conf /etc/supervisor/conf.d/profiles_api.conf
supervisorctl reread
supervisorctl update
supervisorctl restart profiles_api

# Setup nginx to make our application accessible.
cp $PROJECT_BASE_PATH/profiles-rest-api/deploy/nginx_profiles_api.conf /etc/nginx/sites-available/profiles_api.conf
rm /etc/nginx/sites-enabled/default
ln -s /etc/nginx/sites-available/profiles_api.conf /etc/nginx/sites-enabled/profiles_api.conf
systemctl restart nginx.service

echo "DONE! :)"
```

supervisor config

```
[program:profiles_api]
command = /usr/local/virtualenvs/profiles_api/bin/uwsgi --http :9000 --wsgi-file /usr/local/apps/profiles-rest-api/src/profiles_project/profiles_project/wsgi.py
directory = /usr/local/apps/profiles-rest-api/src/profiles_project/
user = root
autostart = true
autorestart = true
stdout_logfile = /var/log/profiles_api.log
stderr_logfile = /var/log/profiles_api_err.log
```

nginx config

```
server {
    listen 80 default_server;

    location /static/admin {
        alias /usr/local/virtualenvs/profiles_api/lib/python3.5/site-packages/django/contrib/admin/static/admin;
    }

    location /static/rest_framework {
        alias /usr/local/virtualenvs/profiles_api/lib/python3.5/site-packages/rest_framework/static/rest_framework;
    }

    location / {
        proxy_pass http://127.0.0.1:9000/;
        proxy_set_header    Host                $host;
        proxy_set_header    X-Real-IP           $remote_addr;
        proxy_set_header    X-Forwarded-For     $remote_addr;
        proxy_set_header    X-Forwarded-Proto   $scheme;
        proxy_redirect      off;

    }
}
```


### Create and launch an EC2 instance

- AWS Login Page - https://aws.amazon.com/es/console/

### Download and run server setup script

### Create superuser and test server

### Add domain to allowed list and push update

### Test site on server




## Summary

### Course outro

- Django (Official Docs) - https://docs.djangoproject.com/en/1.11/
- Django REST Framework (Official Docs) - https://www.django-rest-framework.org/


## References
- https://www.udemy.com/django-python
