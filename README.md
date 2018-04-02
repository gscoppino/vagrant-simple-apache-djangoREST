# Purpose

Provide a configuration managed Virtual Machine for a Django server application
that communicates with a client Single Page Application (SPA) over a REST API.

# Problem Statement

This project attempts to mitigate the following concerns relevant to team-based
web application development.

* Ensuring that the local development environment of the web application is as
close to the production environment as possible (which can make it easier to
plan and execute testing / deployment processes).

* Mimizing the number of development dependencies that a developer needs to
install locally on their development machine (which can reduce the chance of
development issues that occur due to machine-specific misconfiguration).

* Ensuring that all developers are able to bring their local development
environments to a common state with regards to application dependencies and
fixture data (which ensures that all issues with the application can be
understood and reproduced by anyone with access to the project).

# Overview

The virtual machine is configured and managed using Vagrant. Vagrant uses
VirtualBox as the provider for the virtual machine.

The configuration managed Virtual Machine provides the following development
environment:

* Operating System: Ubuntu 14.04 OR CentOS 7.4
* Web Server: Apache
* Server Programming Environment: Python 2.x, Django
* Client Programming Environment: NodeJS 8.x

# Dependencies

* [VirtualBox 5.1.x](https://www.virtualbox.org/)
* [Vagrant 2.0.x](https://www.vagrantup.com/)

Software versions other than ones specified may work, but it is not recommended
to deviate from these requirements. Where a flexible version range is provided,
consider using the lowest software version in said range if you encounter
issues with a new(er) software version.

# Getting Started

## Start the Environment

* Ensure that all of the dependencies are installed and operational. Consult
the documentation provided by the publishers of these software for details on
how to install and verify proper operation.
* Download the environment and pick either the `Ubuntu` or `CentOS` derivative.
* From a Command Line Interface (CLI), execute the command `vagrant up` in the
root directory of the environment. This command will build a virtual machine
and start the application.
* Open a web browser and load the web address `localhost:8080` to confirm the
application is operational.
* From a CLI, execute the command `vagrant halt`. This command will shut down
the virtual machine.

The environment will have automatically created a new Django project in
`./server/project` if one did not previously exist, with empty `fixtures.json`
and `requirements.txt` files. The `settings.py` will also have the
`STATIC_ROOT` and `STATIC_URL` variables correctly configured. The
environment will also have automatically created a `package.json` file in
`./client/` if one did not previously exist.


## Manage the Environment

After the environment has been shut down, running `vagrant up` to restart
the environment will not perform any further configuration changes. At times,
though, it may be necessary or desired to run configuration steps again. For
example, if performing a checkout of another developers branch where they
updated a dependency, it would be ideal that the same dependency version is
employed on your local machine. Another example is if it is desired to reset
the database to some default set of pre-configured data. There are two methods
to go about re-running configuration:

1. Running the command `vagrant destroy` will delete the virtual machine that
was created using `vagrant up`. Running `vagrant up` again will create a new
virtual machine and run configuration steps.

2. Running the command `vagrant provision` while a virtual machine that exists
is running will run configuration steps without recreating the virtual machine.
This is much quicker and recommended for most cases.

## SSH Into the Environment

At times, it may be necessary to enter into the environment, such as to make
migrations for changes to Django models. Secure Shell (SSH) can be utilized for
this purpose. Running the command `vagrant ssh` while a virtual machine that
exists is running will enter the environment. However, this approach is not
recommended since the Python environment configuration will not be set in the
shell. Instead, running the shell script `./server/utils/shell.sh` will perform
the SSH connection and ensure that the Python environment configuration is set
in the resulting shell.

# Advanced Usage

## Importing an Existing Project

An existing Django project or client SPA can be imported into this environment.
This is due to the fact that the environment will not attempt to start a new
client or Django project if one already exists. To integrate the existing
component, ensure the following:

* The existing component should be capable of running on the software version(s)
present in the environment. If it does not, either modify the component to do so
or modify the configuration to match the required software version(s) of the
software.

* If the existing component is a server application, it should be placed such
that the `manage.py` script is located in `./server/project`.

* If the existing component is a client SPA, it should be placed such that the
`package.json` is location in `./client`.

## Making Changes to the Configuration

The VM is configured using [Vagrant](https://vagrantup.com/). Consult the
documentation for information on how to modify the configuration of the VM.

The environment inside the VM is configured using
[Ansible](https://www.ansible.com/). Consult the Ansible documentation for
information on how to modify the configuration of the environment.