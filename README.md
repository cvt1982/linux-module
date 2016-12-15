# linux-module
Simple linux kernel module (hello world)

### Prerequisite
The module has only been tested on Ubuntu 16.04 LTS.
To begin run the following commands:

    sudo apt-get update
    sudo apt-get upgrade

Next we have to install a couple of packages:

    sudo apt-get install build-essential

Finally we need the correct set of linux kernel headers. It is important that we download kernel headers that match the current system.
To do this we can use the following command:

    apt-cache search linux-headers-$(uname -r)
    >> linux-headers-3.16.0-4-amd64 - Header files for Linux 3.16.0-4-amd64:
    sudo apt-get install linux-headers-3.16.0-4-amd64

We can check the installed headers by going to the following folder:

    cd /usr/src/linux-headers-3.16.0-4-amd64/

### Building sources
Pull det git spurces and run the provided `Makefile`

    git clone https://github.com/cvt1982/linux-module.git
    cd linux-module
    make

### Trying the kernel module
Load the newly builded kernel module `hello.ko` via the `insmod` command

    sudo insmod hello.ko

The module can be unloaded again with the `rmmod` command

    sudo rmmod hello.ko

We can check that the module has actually run by viewing the output in the `kern.log`

    cd /var/log
    cat kern.log

Here you should see the init and exit method outputs

    >> Apr 4 23:34:32 beaglebone kernel: [21613.495523] EBB: Hello world from the BBB LKM!
    >> Apr 4 23:35:17 beaglebone kernel: [21658.306647] EBB: Goodbye world from the BBB LKM!
    >> root@beaglebone:/var/log#

To use the paramenter `name` in the module is very simple. The `name` type is a char pointer and can point to strings.

    sudo insmod hello.ko name=Carsten

If we then unload the module again and output the `kern.log` we should now see the `world` change to `Carsten`

    >> Apr 5 00:02:20 beaglebone kernel: [23281.070193] EBB: Hello Carsten from the BBB LKM!
    >> Apr 5 00:08:18 beaglebone kernel: [23639.160009] EBB: Goodbye Carsten from the BBB LKM!

### Links
The sources herein is heavily based on the great work from Derek Molley's [homepage](http://derekmolloy.ie/).
Direct links to Derek's Linux Kernel Module tutorials are given below:

 * [http://derekmolloy.ie/category/general/linux/](http://derekmolloy.ie/category/general/linux/)
 * [http://derekmolloy.ie/writing-a-linux-kernel-module-part-1-introduction/](http://derekmolloy.ie/writing-a-linux-kernel-module-part-1-introduction/)
 * [http://derekmolloy.ie/writing-a-linux-kernel-module-part-2-a-character-device/](http://derekmolloy.ie/writing-a-linux-kernel-module-part-2-a-character-device/)
 * [http://derekmolloy.ie/kernel-gpio-programming-buttons-and-leds/](http://derekmolloy.ie/kernel-gpio-programming-buttons-and-leds/)