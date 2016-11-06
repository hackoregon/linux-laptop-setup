# Making a Ubuntu server for Hack Oregon in VirtualBox

1. You will need to be connected to the internet. Install VirtualBox if you haven't already.
2. Download the Ubuntu 16.04.1 LTS server ISO file from http://releases.ubuntu.com/16.04.1/ubuntu-16.04.1-server-amd64.iso
3. Open VirtualBox. Create a virtual machine.
    * Call it "Hack Oregon Base".
    * Set the operating system to Ubuntu 64.
    * Set the memory to 1024 MiB.
    * Create a 32 GB dynamically allocated ***VMDK*** disk. If you use the default VirtualBox VDI disk the Vagrant packaging step will have to spend energy converting it to VMDK.
4. Open the 'Settings' wizard.
5. Under 'System', change the pointing device to 'PS/2 mouse'.
6. Under 'Audio', uncheck 'Enable Audio'.
7. Under 'USB', uncheck 'Enable USB Controller'.
8. Now go to the 'Storage' settings.
    * Click on the empty CD drive and point it to the Ubuntu server ISO file you downloaded.
    * Click on the SATA controller. Make sure "Use Host I/O Cache" is cleared. I had mysterious installation failures with this option set!
9. Close the Settings wizard.
10. Start the machine. When it comes up you'll be in a text-based installer. In most cases, you'll be able to accept the defaults.
11. Accept all the defaults till you get to the host name screen. Call the machine 'hackoregon-base'.
12. Set the user full name to 'Hack Oregon'.
13. Set the username to 'hack'.
14. Set the password to 'ORturkeyeggs'.
15. Do not encrypt your home directory.
16. Accept the default time zone.
16. Now comes the tricky part - partitioning. 
    * Select 'Guided - use entire disk'.
17. The installer will start again. Accept the defaults until you come to 'Software Selection'. Select both 'standard system utilities' and 'OpenSSH server'.
18. The installer will start again. Accept the defaults until the install is complete. Then select "Continue" to reboot the system.
