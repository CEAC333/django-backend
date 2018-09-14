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



### Add PUT, PATCH and DELETE methods



### Test the PUT, PATCH and DELETE methods



## References
- https://www.udemy.com/django-python
