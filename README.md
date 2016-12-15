# linux-module
Simple linux kernel module (hello world)

### Prerequisite
The module has only been tested on Ubuntu 16.04 LTS.
To begin run the following commands:

    sudo apt-get update
    sudo apt-get upgrade

Next we have to install a couple of packages:

    sudo apt-get install build-essential git

Finally we need the correct set of linux kernel headers. It is important that we download kernel headers that match the current system.
To do this we can use the following command:

    apt-cache search linux-headers-$(uname -r)
    >> linux-headers-4.4.0-53-generic - Linux kernel headers for version 4.4.0 on 64 bit x86 SMP
    sudo apt-get install linux-headers-4.4.0-53-generic

We can check the installed headers by going to the following folder:

    cd /usr/src/linux-headers-4.4.0-53-generic/

### Building sources
Pull det git sources and run the provided `Makefile`

    git clone https://github.com/cvt1982/linux-module.git
    cd linux-module
    make

Output should be something like

    carsten@carsten-VirtualBox:~/Desktop/github/linux-module/helloworld$ make
    make -C /lib/modules/4.4.0-53-generic/build/ M=/home/carsten/Desktop/github/linux-module/helloworld modules
    make[1]: Entering directory '/usr/src/linux-headers-4.4.0-53-generic'
      CC [M]  /home/carsten/Desktop/github/linux-module/helloworld/hello.o
      Building modules, stage 2.
      MODPOST 1 modules
      CC      /home/carsten/Desktop/github/linux-module/helloworld/hello.mod.o
      LD [M]  /home/carsten/Desktop/github/linux-module/helloworld/hello.ko
    make[1]: Leaving directory '/usr/src/linux-headers-4.4.0-53-generic'


### Trying the kernel module
Load the newly builded kernel module `hello.ko` via the `insmod` command

    sudo insmod hello.ko

The module can be unloaded again with the `rmmod` command

    sudo rmmod hello.ko

We can check that the module has actually run by viewing the output in the `kern.log`

    cat /var/log/kern.log

Here you should see the init and exit method outputs

    >> Dec 15 20:18:30 carsten-VirtualBox kernel: [  551.523561] EBB: Hello world from the BBB LKM!
    >> Dec 15 20:18:39 carsten-VirtualBox kernel: [  559.921211] EBB: Goodbye world from the BBB LKM!


To use the paramenter `name` in the module is very simple. The `name` type is a char pointer and can point to strings.

    sudo insmod hello.ko name=Carsten

If we then unload the module again and output the `kern.log` we should now see the `world` change to `Carsten`

    >> Dec 15 20:19:57 carsten-VirtualBox kernel: [  637.678870] EBB: Hello Carsten from the BBB LKM!
    >> Dec 15 20:20:00 carsten-VirtualBox kernel: [  640.705158] EBB: Goodbye Carsten from the BBB LKM!

### Links
The sources herein is heavily based on the great work from Derek Molley's [homepage](http://derekmolloy.ie/).
Direct links to Derek's Linux Kernel Module tutorials are given below:

 * [http://derekmolloy.ie/category/general/linux/](http://derekmolloy.ie/category/general/linux/)
 * [http://derekmolloy.ie/writing-a-linux-kernel-module-part-1-introduction/](http://derekmolloy.ie/writing-a-linux-kernel-module-part-1-introduction/)
 * [http://derekmolloy.ie/writing-a-linux-kernel-module-part-2-a-character-device/](http://derekmolloy.ie/writing-a-linux-kernel-module-part-2-a-character-device/)
 * [http://derekmolloy.ie/kernel-gpio-programming-buttons-and-leds/](http://derekmolloy.ie/kernel-gpio-programming-buttons-and-leds/)