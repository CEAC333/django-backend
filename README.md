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

### Installation
- Vagrant - https://www.vagrantup.com/downloads.html
- VirtualBox - https://www.virtualbox.org/wiki/Downloads
- ModHeader - https://chrome.google.com/webstore/detail/modheader/idgpnmonknjnojddfkpgkljpfnnfcklj?hl=en
- Atom - https://atom.io/
- Git-SCM - https://git-scm.com/downloads


## References
- https://www.udemy.com/django-python
