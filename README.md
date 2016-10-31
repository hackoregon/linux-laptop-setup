Setting up a Linux Laptop for Hack Oregon Projects
================
M. Edward (Ed) Borasky <znmeb@znmeb.net>

Bugs? Feature requests? Unclear documentation?
----------------------------------------------

File an issue at <https://github.com/hackoregon/linux-laptop-setup/issues/new>.

Design goals
------------

1.  Run at native speed on bare metal, without the overhead of running as a guest in a virtual machine or container.
2.  Work on smaller / older machines that are too small to host a Docker container, a Vagrant box or a full virtual machine guest. This includes 32-bit machines to the extent that the applications allow.
3.  Provide Docker, Vagrant / VirtualBox and Virtual Machine Manager hosting on systems with the hardware capability.
4.  Modularity - you only install what you need to get your tasks done.
5.  Use the same software versions that other Hack Oregon projects are using whenever possible.

Which version of Linux should you use?
--------------------------------------

I've designed these scripts on Linux Mint 18 "Sarah" with the Cinnamon desktop (<https://www.linuxmint.com/download.php>). I test them on a Sarah 64-bit Cinnamon system, a Sarah 32-bit KDE system and an Ubuntu 16.04.1 32-bit system with the default "Unity" desktop.

Except for virtual machine hosting, they should work on any of the Linux Mint "Sarah" desktops and any Ubuntu 16.04.1 LTS "Xenial Xerus" desktop, 32 or 64 bits. File an issue at <https://github.com/hackoregon/linux-laptop-setup/issues/new> if you find problems on an Ubuntu 16.04.1 or Linux Mint 18 system.

If you have some other version of Linux already installed, open an issue at <https://github.com/hackoregon/linux-laptop-setup/issues/new>. It will only take me a half-day or thereabouts to port them to any recent Fedora, openSUSE or Debian system.

Why Linux Mint 18 / Ubuntu 16.04.1 LTS?
---------------------------------------

1.  Long-term support: five years, starting in 2016!
2.  Safety in numbers: Linux Mint is the most popular community desktop distro, and Ubuntu LTS is the most popular community server distro.
3.  Third-party support: the first distros third parties test on are Ubuntu LTS and RHEL / CentOS. If they have oodles of resources, the next priority is the commercial SUSE Linux Enterprise. The rest of them - Fedora, Debian, openSUSE, Arch, Gentoo, etc. - are all fine distros, but unless you have an institutional reason to use them, you're essentially doing unpaid QA for a vendor / community.

A bit about hardware
--------------------

There are essentially three kinds of Intel / AMD based PCs:

1.  32-bit only: These are usually older machines, although some Atom-based machines sold today will only run 32-bit software. These machines should be capable of running the Python data science / machine learning tools, the database and GIS tools, the R platform and RStudio. They will not run Docker, Vagrant / VirtualBox or Virtual Machine Manager.
2.  64-bit: These machines will run either 32-bit or 64-bit software. They will run everything a 32-bit machine can run and they will run Docker. They will not run Vagrant / VirtualBox or Virtual Machine Manager.
3.  64-bit plus virtualization assists (64-bit VA): these are the most recent machines. The virtualization assists are sometimes disabled in the BIOS / firmware. You will need to enable the assists if they are disabled. They will run everything in this collection.

Getting started
---------------

You'll need wall power and a reliable internet connection. Coffee shop WiFi can be problematic. I don't have an accurate RAM requirement defined yet but I know the `r-platform` script crashes if you don't have at least 1536 MiB. See <https://github.com/hackoregon/linux-laptop-setup/issues/11> and <https://github.com/tidyverse/readr/issues/544> for the gory details. My goal is for everything except virtual machine hosting to run in a 1 GiB machine.

1.  Download and unpack <https://github.com/hackoregon/linux-laptop-setup/archive/master.zip>. This will create a directory `linux-laptop-setup-master`.
2.  Open a terminal and `cd` into the directory.
3.  About `sudo`: `sudo` (super-user do) is a Linux utility that allows you to perform adminstrative tasks like installing software by temporarily operating as the `root` super-user. If you see the prompt `[sudo] password for <your username>:`, enter *your* password.
4.  Type `./1core`. This will
    -   update all packages to the latest version,
    -   install a few core utilities, including `git` and `vim`, and
    -   install the Python utility `virtualenvwrapper`, which allows isolating Python applications in named virtual environments.

5.  Log out and back in again. This sets the environment variables you need for the Python virtual environments.
6.  The scripts are modular - you only need to install what you're going to use. By task:
    -   Python data science / machine learning: Install the `jupyter` package by typing `./jupyter`. This will create a Jupyter notebook Python 3 virtualenv. I've included the [`scikit-learn`](http://scikit-learn.org/stable/) machine learning toolkit.

        To run the notebook, type

            workon jupyter
            jupyter notebook

        A browser will open with the Jupyter notebook web app. When you're finished, close the browser and type `CTL-C` in the terminal window. You'll see

            Serving notebooks from local directory: /home/znmeb/Projects/linux-laptop-setup
            0 active kernels 
            The Jupyter Notebook is running at: http://localhost:8888/
            Shutdown this notebook server (y/[n])?

        Enter "y" to shut down Jupyter.
    -   Database / SQL / GIS: Install PostgreSQL and PostGIS by typing `./postgres`. This will install PostgreSQL and PostGIS and create a PostgreSQL super-user with the same user ID as your Linux user ID.

        Note that as installed, the PostgreSQL service is only accessible inside the workstation / laptop. If you need to expose it to a local area network, you'll need to do some configuration.
    -   QGIS and PgAdmin3 GUI tools: Type `./qgis-pgadmin3` to install PgAdmin3, the QGIS (Quantum GIS) desktop GUI, and the QGIS map server.
    -   VirtualBox and Vagrant hosting (64-bit VA bare metal only): If you want to host VirtualBox guests or Vagrant boxes, type `./vbox-vagrant`. During the installation you'll be asked to accept the VirtualBox Extension Pack's Personal Use and Evaluation License (PUEL). You will need to log out and back in again after the install to join the `vboxusers` group. In theory this works on a 32-bit host but I don't recommend it.
    -   Git Large File Storage: we used this last year for Crop Compass. Note that GitHub charges money for both storage and download bandwidth for this, so be careful! If you need it, type `./git-lfs`.

Advanced tools
--------------

1.  The R platform: type `./r-platform` to install the R platform. This includes:

    -   R
    -   Java
    -   The `tidyverse` data wrangling, modeling and visualization packages
    -   The `Shiny` interactive application development package
    -   The `flexdashboard`, `bookdown`, `tufte` and `rticles` authoring packages, and
    -   The `devtools` and `roxygen2` package development tools.

    This takes a long time to install. You will probably have to watch it, because if the Linux package install takes long enough, it will pause wanting a `sudo` password entry.

2.  RStudio: type `./rstudio-desktop` to install the RStudio Desktop.
3.  Virtual Machine Manager (64-bit VA bare metal only): The "native" Linux virtual machine hosting software is called Virtual Machine Manager. To install it, type `./virt-manager`. You will need to log out and back in again to join the `libvirtd` group. You will have a menu item added to start it.
4.  Docker hosting (64-bit or 64-bit VA, will run inside a guest machine): If you want to run (or build) Docker images, install the Docker hosting with `./docker-hosting`. You will need to log out and back in again to join the `docker` group.

Bugs? Feature requests? Unclear documentation?
----------------------------------------------

File an issue at <https://github.com/hackoregon/linux-laptop-setup/issues/new>.

Todo
----

1.  Instructions for connecting QGIS to the PostGIS database. Also, make sure QGIS versions are the same on Linux and Windows.
2.  Front-end tools: I'm not a front-end person so I have no idea what we'll need there. If there's something you want, file an issue and I'll add it.
3.  Django: The last I heard we'll be using Django for some projects. I am working through <https://www.amazon.com/Mastering-Django-Core-Complete-Guide-ebook/dp/B01KR6F4Z2> and can easily add install scripts if anyone wants them.
