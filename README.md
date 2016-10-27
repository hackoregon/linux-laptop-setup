# linux-laptop-setup

## Scripts for setting up a Linux laptop for working on Hack Oregon projects
These scripts were designed to work on Linux Mint 18 "Sarah", which is based on Ubuntu 16.04LTS "Xenial Xerus". Both are supported for five years.

The scripts should work on any of the Ubuntu 16.04 desktops, although I haven't tested them. There are problems with the newer Ubuntu 16.10, and it's only supported for nine months, so don't use it.

The scripts here are modular, depending on the tasks you want to perform. If you aren't doing Docker hosting, for example, you don't need to run `docker-hosting`. If you're running a Linux distro other than Linux Mint / Ubuntu 16.04, let me know and I'll build a virtual machine and port the scripts.

* 1core: `./1core` installs the core command line tools. Everyone needs this.
* docker-hosting: `docker-hosting` installs Docker hosting
* postgres: `./postgres` installs PostgreSQL and creates a user / database with your Linux ID as the name.
* gis: `./gis` run the `postgres` script, then installs PostGIS, QGIS and GRASS
* jupyter: `./jupyter` installs a Jupyter virtualenv
* R-platform: `./R-platform` installs R, RStudio Desktop and Server and the "tidyverse". This takes a long time; it's installing the entire LaTeX toolchain - "texlive-full" - which takes up about 4 GB.
* dropbox: `./dropbox` installs the DropBox sync packages
* virt-manager: `./virt-manager` installs the native Linux Virtual Machine Manager virtualization platform
* virtualbox: `./virtualbox` installs the VirtualBox platform
* vagrant: `./vagrant` installs the Vagrant platform
