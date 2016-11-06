# Making a Ubuntu server for Hack Oregon in VirtualBox

## Creating a virtual machine
1. You will need to be connected to the internet. Install VirtualBox if you haven't already.
2. Download the Ubuntu 16.04.1 LTS server ISO file from http://releases.ubuntu.com/16.04.1/ubuntu-16.04.1-server-amd64.iso
3. Open VirtualBox. Create a virtual machine.
    * Call it "Hack Oregon Base".
    * Set the operating system to Ubuntu 64.
    * Set the memory to 1024 MiB.
    * Create a 32 GB dynamically allocated ***VMDK*** disk. If you use the default VirtualBox VDI disk the Vagrant packaging step will have to spend energy converting it to VMDK.
4. Open the 'Settings' wizard.
    * Under 'System', change the pointing device to 'PS/2 mouse'.
    * Under 'Audio', uncheck 'Enable Audio'.
    * Under 'USB', uncheck 'Enable USB Controller'.
5. Now go to the 'Storage' settings.
    * Click on the empty CD drive and point it to the Ubuntu server ISO file you downloaded.
    * Click on the SATA controller. Make sure "Use Host I/O Cache" is ***cleared***. I had mysterious installation failures with this option set!
6. Now go to the 'Network' settings.
    * Select 'Advanced'. Then press the 'Port Forwarding' button.
    * Create a rule with 'SSH' as the name, 'TCP' as the protocol, 127.0.0.1 as the Host IP, 2222 as the host port and 22 as the guest port. Leave the guest IP blank.
    * Create a rule with 'Jupyter' as the name, 'TCP' as the protocol, 127.0.0.1 as the Host IP, 7777 as the host port and 8888 as the guest port. Leave the guest IP blank.
6. Close the Settings wizard.
7. Start the machine. When it comes up you'll be in a text-based installer. In most cases, you'll be able to accept the defaults.
8. Accept all the defaults till you get to the host name screen.
    * Call the machine 'hackoregon-base'.
    * Set the user full name to 'Hack Oregon'.
    * Set the username to 'hack'.
    * Set the password to 'ORturkeyeggs'.
    * Do not encrypt your home directory.
    * Accept the default time zone.
9. When you come to the "Partition disks" step, select 'Guided - use entire disk'.
    * There's only one disk to select, so select it.
    * When it asks to write the changes to disk, say 'Yes'.
10. The installer will start again. Accept the defaults until you come to 'Software selection'. Select ***both*** 'standard system utilities' and 'OpenSSH server'.
11. The installer will start again. Accept the defaults until the install is complete. Then select "Continue" to reboot the system.
12. When the system comes up, log in as 'hack'.
13. Type `ssh-keygen` to generate secure shell keys. Answer all the prompts with `Enter`.
14. Type `sudo shutdown -h now` to power off the virtual machine. You'll need to enter the password.
15. Go to the VirtualBox "Snapshots" page and take a snapshot, so you can get back to this state.

## Installing the services
You can use the virtual machine as it is with the "hack" account and password. You have to install the services you'll be using and forward the ports in the VMware GUI, but you have a working server at this point.

1. Boot the machine up and log in as "hack", password "ORturkeyeggs".
2. Type `git clone https://github.com/hackoregon/linux-laptop-setup`.
3. Type `cd linux-laptop-setup`.
4. Type `git checkout <working branch>`.
5. Type `./1core`. This will install the core packages for all users.
6. Type `./database-gis-services-upstream`. This installs PostgreSQL and PostGIS for all users on the machine. It will add 'hack' as a database super-user.
7. Type `./data-science-services`. This will install Miniconda and the data science environment for the 'hack' user.
8. Type `ssh-keygen` to generate keys for secure shell. Answer all the prompts with `Enter`.
9. Shut down the system with `sudo shutdown -h now`.
10. In the machine's network settings, set port forwarding: port 22 -> 2222 and port 8888 -> 8888.

## Preparing the virtual machine as a Vagrant box
5. Type `cd vagrant-box-creation`.
6. Type `./vagrantize`.
7. When 'vagrantize' is done, shut the machine down again with `sudo shutdown -h now`. Take another snapshot so you can get back to this state.

## Packaging the Vagrant box
This is a single-step operation, but it takes a long time. On my machine it's about 30 minutes!. Open a terminal on a Linux host in `linux-laptop-setup/vagrant-box-creation`, type `./package` and wait.

The final Vagrant box file will be moved to `~/Downloads`. It's about 705 megabytes as of this writing.

## Testing the box
1. `cd linux-laptop-setup/vagrant-box-execution`
2. `./add-init-up`. This will
    * Remove any existing 'hackoregon' box.
    * Add the new one from `~/Downloads`.
    * Remove any existing Vagrantfile.
    * Do a 'vagrant init' and a 'vagrant up --provision'.
