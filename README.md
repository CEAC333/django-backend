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

### What are Django Models?

## References
- https://www.udemy.com/django-python