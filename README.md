Setting up a Linux Laptop for Hack Oregon Projects
================
M. Edward (Ed) Borasky <znmeb@znmeb.net>

Bugs? Feature requests? Unclear documentation?
----------------------------------------------

File an issue at <https://github.com/hackoregon/linux-laptop-setup/issues/new>.

Design goals
------------

1.  Run at native speed on a machine that might be too small to host a Docker container, a Vagrant box or a full virtual machine guest.
2.  Support 32-bit machines to the extent that Linux and the applications do.
3.  Provide Docker, Vagrant / VirtualBox and Virtual Machine Manager hosting on systems with the hardware capability.
4.  Modular - you only install what you need to get your tasks done.
5.  Use the same software versions that other Hack Oregon projects are using whenever possible.
6.  The "server" subset must run in an Ubuntu 16.04 LTS system, including a Vagrant box.

Which version of Linux should you use?
--------------------------------------

I've designed these scripts on Linux Mint 18 "Sarah" with the Cinnamon desktop (<https://www.linuxmint.com/download.php>). Except for virtual machine hosting, they should work on any of the Linux Mint "Sarah" desktops and any Ubuntu 16.04 LTS "Xenial Xerus" desktop, 32 or 64 bits. File an issue at <https://github.com/hackoregon/linux-laptop-setup/issues/new> if you find problems on a Ubuntu 16.04 or Linux Mint 18 system.

If you have some other version of Linux already installed, open an issue at <https://github.com/hackoregon/linux-laptop-setup/issues/new>. It will only take me a half-day or thereabouts to port them to any recent Linux distro.

Why Linux Mint 18 / Ubuntu 16.04 LTS?
-------------------------------------

1.  Long-term support: five years, starting in 2016!
2.  Safety in numbers: Linux Mint is the most popular community desktop distro, and Ubuntu is the most popular community server distro.
3.  Third-party support: the first distros third parties test on are Ubuntu LTS and RHEL / CentOS. If they have lots of resources, the next priority is SUSE Linux Enterprise. The rest of them - Fedora, Debian, openSUSE, etc. - are all fine distros, but unless you have a good reason to use them, you're essentially doing free QA for a vendor / community.

Getting started
---------------

You'll need wall power and a reliable internet connection. Coffee shop WiFi can be problematic. I don't have an accurate RAM requirement defined yet but I know the `r-platform` script crashes if you don't have at least 1536 MiB in a 64-bit Ubuntu 16.04 Vagrant box. See <https://github.com/hackoregon/linux-laptop-setup/issues/11> and <https://github.com/tidyverse/readr/issues/544> for the gory details. My goal is for everything except virtual machine hosting to run in a 1 GB machine.

1.  Download and unpack <https://github.com/hackoregon/linux-laptop-setup/archive/master.zip>. This will create a directory `linux-laptop-setup-master`.
2.  Open a terminal and `cd` into the directory.
3.  About `sudo`: `sudo` (super-user do) is a Linux utility that allows you to perform adminstrative tasks like installing software by temporarily operating as the `root` super-user. If you see the prompt `[sudo] password for <your username>:`, enter *your* password.
4.  Type `./1core`. This will install a few core utilities, including `git` and `vim`. It will also install the Python utility `virtualenvwrapper`, which allows isolating Python applications in named virtual environments.
5.  Log out and back in again. This sets the environment variables you need for the Python virtual environments.
6.  The scripts are modular - you only need to install what you're going to use. By task:
    -   Git Large File Storage: we used this last year for Crop Compass. Note that GitHub charges money for both storage and download bandwidth for this, so be careful! If you need it, type `./git-lfs`.
    -   Data science: Install the `jupyter` package by typing `./jupyter`. This will create a Jupyter notebook Python 3 virtualenv. To run it, type

            workon jupyter
            jupyter notebook

        A browser will open with the Jupyter notebook web app.

        Note that I've included the IPyParallel package (<http://ipyparallel.readthedocs.io/en/latest/intro.html>). If you've got a machine with multiple cores you'll be able to run larger problems with this.

        To run a Jupyter notebook server, type `./serve-jupyter`. This will open a Jupyter notebook server listening on 0.0.0.0:8888. Type `ifconfig` to see what your machine's IP address is, then browse to that IP address port 8888 from another machine.
    -   Database / SQL / GIS: Install PostgreSQL and PostGIS by typing `./postgres`. This will install PostgreSQL and PostGIS and create a PostgreSQL super-user with the same user ID as your Linux user ID.

        Note that as installed, the PostgreSQL service is only accessible inside the workstation / laptop. If you need to expose it to a local area network, you'll need to do some configuration.
    -   QGIS and PgAdmin3 GUI tools: Type `./qgis-pgadmin3` to install PgAdmin3, the QGIS (Quantum GIS) desktop GUI and server.
    -   VirtualBox and Vagrant: If you want to host VirtualBox guests or Vagrant boxes, type `./vbox-vagrant`. During the installation you'll be asked to accept the VirtualBox Extension Pack's Personal Use and Evaluation License (PUEL). You will need to log out and back in again after the install to join the `vboxusers` group.

Advanced tools
--------------

1.  The R platform: type `./r-platform` to install the R platform. This includes:

    -   R
    -   Java
    -   The `tidyverse` data wrangling, modeling and visualization packages
    -   The `Shiny` interactive application development package
    -   The `flexdashboard`, `bookdown`, `tufte` and `rticles` authoring packages, and
    -   The `devtools` and `roxygen2` package development tools.

2.  RStudio: type `./rstudio-desktop` to install the RStudio Desktop. Type `./rstudio-server` to install the RStudio Server. You don't need both but it won't hurt anything if you have both of them.

    RStudio Server listens on port 8787. It's enabled and running by default. Use `ipconfig` to determine the IP address and browse to port 8787 from another machine. You'll log in with your regular Linux username and password.
3.  Virtual Machine Manager: The "native" Linux virtual machine hosting software is called Virtual Machine Manager. To install it, type `./virt-manager`. You will need to log out and back in again to join the `libvirtd` group. You will have a menu item added to start it.
4.  Docker hosting: If you want to run (or build) Docker images, install the Docker hosting with `./docker-hosting`. You will need to log out and back in again to join the `docker` group.

Bugs? Feature requests? Unclear documentation?
----------------------------------------------

File an issue at <https://github.com/hackoregon/linux-laptop-setup/issues/new>.

Todo
----

1.  Instructions for connecting QGIS to the PostGIS database.
2.  Front-end tools: I'm not a front-end person so I have no idea what we'll need there. If there's something you want, file an issue and I'll add it.
3.  Django: The last I heard we'll be using Django for some projects. I am working through <https://www.amazon.com/Mastering-Django-Core-Complete-Guide-ebook/dp/B01KR6F4Z2> and can easily add install scripts if anyone wants them.
4.  Anaconda: If we decide we're using it, I'll add an installer script, although it's easy to install from the web.
