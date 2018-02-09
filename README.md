Boilerplate Apache + Django Virtual Machines with optional project boilerplate
and support for isolated project frontend/backend, for Red Hat and Debian based
environments.

This repo provides an easy way to get started on a new Django backend that has an
isolated frontend. By simply bringing up a new virtual machine via `vagrant up`,
it is possible to have a Red Hat or Debian based environment set up with an
Apache + Django configuration, along with a default project already up and running,
served at `localhost:8080`. The created project will already have Django's
`STATIC_ROOT` and `STATIC_URL` variables configured, and `requirements.txt`/`package.json`
files present. With all this out of the way, you can get to business. All configuration
is stateful thanks to Ansible, so the next time you run `vagrant provision`, your
project will be exactly how you left it, the provisioner will just update
the database, static files, and installed requirements on the machine to match your project.

You can also import an existing Django project into this environment! Simply make
sure you put the root of the backend (with `manage.py`) directly in the
`server/app` folder. The frontend of the project (with `package.json`) should go in
the `./client` folder. A `requirements.txt`/`package.json` will be created automatically
if they didn't already exist in your project.
For now, only SQLite will work for importing.

Requirements:

* Vagrant 1.8.7+
* VirtualBox 5.1.x

# Get Started:

* Install [Vagrant 1.8+](https://www.vagrantup.com/).
* Clone this repository:
`git clone https://github.com/gscoppino/vagrant-simple-apache-djangoREST --depth=1`
* (optional) Place an existing Django backend in the `server/app` directory
of your project folder. The `manage.py` script for the project should be in the root,
eg. `server/app/manage.py`.
* (optional) Place an existing frontend in the `client` directory of your project folder. The
`package.json` file for the project should in the root, eg. `client/package.json`.
* Run `vagrant up` from your project folder.

# Basic Usage:

* `vagrant up` :  When you are ready to spin up the server. If this is the first run,
a backend and frontend will be created, if they did not already exist.
* `vagrant provision` : can be used to bring the virtual machine to a new
desired state without starting from scratch. If changing a configuration file
(such as the Apache config, or your `requirements.txt`), just run
`vagrant provision` to get the machine to the state it needs to be in.

# Initial Environment

* A new backend called `app` will automatically be created for you in
`server/app` if the provisioner didn't find an existing project, and a new
frontend will be created in `client` if the provisioner didn't find an existing
project. Otherwise, the only differences to note will be that a `requirements.txt`
was created for your backend, a `package.json` was created for the frontend,
(if they didn't already have these files), and the `STATIC_URL`/`STATIC_ROOT` variables
in your `settings.py` will have been changed to match the environment.
* The project will have been set up and running on `localhost:8080` (note that
  for now, if your existing project is using a database other than **SQLite**,
  the migration task will have failed since the database engine will not exist
  on the VM. Planning to set up dynamic installing
  of the database engine in the very near future).

# Making Changes to the Apache Configuration

The Apache VirtualHost configuration can be found in
`server/provision/roles/apache/files`. If you customize it (for example, to
change the path to `wsgi.py` if you imported your own project or changed the
project name), make sure to run `vagrant provision`, which will copy it
into the virtual machine.

# Working with the virtual machine from outside the VM

Helper scripts for common tasks live in `server/utils` directory.

# Django 101

## Project Configuration

Stored in `server/app/app/settings.py`.

### Project Admin Configuration ###

Create a superuser using `server/utils/django-admin createsuperuser`.
Alternatively, if you wish to do this from within the VM:
* SSH into the VM: `vagrant ssh`.
* Activate the virtualenv: `source /home/vagrant/project_env/bin/activate`.
* Change directory to `/vagrant/server/app`.
* run `python manage.py createsuperuser`.

### Project Database Configuration ####

`DATABASES['default']`: Provide information for your database
management system here.

### Project Static Files Configuration ####

`STATIC_URL`: The directory Django will look in for static files within `INSTALLED_APPS` when using `./manage.py runserver` or `./manage.py collectstatic`.

`STATIC_ROOT`: Where Django will dump static files it finds via `./manage.py collectstatic`.
