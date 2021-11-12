### Questions ### 
#### Q1 ####
Jason Liu, I worked on this project alone. 
#### Q2 ####
* Steps to reproduce results. 
##### Setting up Environment #####
* Get ISO from Ubuntu website, version obtained as of this writing: ubuntu-20.04.3-desktop-amd64.iso
* Create VM on VMware Workstation Beta with the desired specs. 
* Install necessary packages to create kernel in working environemnt, terminal commands are as followed:
   * sudo apt-get update
   * sudo apt-get dist-upgrade
   * sudo apt-get install fakeroot build-essential crash kexec-tools makedumpfile kernel-wedge
   * sudo apt-get build-dep linux
      * at this step I received an error message about including deb-src in the uri for source.list 
      * use 'sudo nano /etc/apt/sources.list' and uncomment the deb-src lines
      * run sudo-apt update
      * re run sudo apt-get build-dep linux
    * sudo apt-get install git-core libncurses5 libncurses5-dev libelf-dev asciidoc binutils-dev
    * sudo apt-get install libssl-dev
    * sudo apt install flex bison
    * sudo apt install zstd
    * src for the previous commands: https://askubuntu.com/questions/718381/how-to-compile-and-install-custom-mainline-kernel/718662#718662
* At this point all the necessary packages have been installed 
* Using git clone, clone the linux fork into the VM
* use uname -a to figure out the version of the config// mine was 5.11.0-40-generic
* use command: cp /boot/config-[version]
* run: make oldconfig
    * hold enter
* run: make prepare
* After this we can begin making the modules 
* I allocated 8 cpu cores, and read that it was optimal to do number of cpus -1 
* run: make -j 7 modules
* run: make -j 7
* run: sudo make INSTALL_MOD_STRIP=1 modules_install 
* run: sudo make install
* run: sudo reboot
##### Changing the cmpe283-1.c file #####
The changes to this file included:
* creating structures and loading structures with values from the intel-sdm, more info can be referenced from this file located under the cmpe283 directory
* defining the msr values to be loaded 
* adding function calls to iterate through the structures. 
#### Loading the module ####
* After rebooting the VM
* use the make file to create the module 
* verify the cmpe283-1.ko exists
* run: sudo insmod cmpe283-1.ko
* run: lsmod | grep cmpe283 // this verifys that the module has been loaded in
* run: dmesg // this shows the output of the module from the system messages. 
* module outputs are available in the cmpe283 subdirectory
