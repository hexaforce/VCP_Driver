# VCP_Driver
Last modified January 29, 2021

Release Notes 
--------------


This driver was tested on Linux 5.4.0 kernel (Ubuntu 18.04.5 LTS) only
but it should work with other versions of Linux kernel as well.
Linux kernel also has a default cp210x driver which is maintained at www.kernel.org.
It is recommended to use the driver there that matches your specific kernel version.

The bundle contains:
* cp210x.c
* Makefile
* cp210x_gpio_example.c
* cp210x_gpio_example_gpiolib.c
* build.sh
* CP210x_VCP_Linux_4.x_Release_Notes.txt


Build instrutions:

Ubuntu:
1. make ( your cp210x driver )
2. cp cp210x.ko to /lib/modules/<kernel-version>/kernel/drivers/usb/serial
3. insmod /lib/modules/<kernel-version/kernel/drivers/usb/serial/usbserial.ko
4. insmod cp210x.ko

RedHat:
1. yum update kernel*  //need to update the kernel first otherwise your header won't match
2. yum install kernel-devel kernel-headers  //get the devel and header packages.
3. reboot  //your build link should be fixed after your system come back
4. make ( your cp210x driver )  // should be able to build successfully at this point
5. cp cp210x.ko to /lib/modules/<kernel-version>/kernel/drivers/usb/serial
6. insmod /lib/modules/<kernel-version/kernel/drivers/usb/serial/usbserial.ko
6. insmod cp210x.ko
7. sudo chmod 666 /dev/ttyUSB0
8. sudo chmod 666 /dev/ttyUSB1


GPIO example:
This shows how to use the two IOCTLs to set GPIO state.
Note: By default, this example assumes you are working with a CP2108 device.
If you want to work with other devices such as CP2102N, etc, 
then please comment out the line "#define CP2108" in cp210x_gpio_example.c

Build instructions:
1. g++ cp210x_gpio_example.c -o cp210x_gpio_example
2. ./cp210x_gpio_example


GPIO example using gpiolib:

Since Linux version 4.8 the GPIO sysfs interface is deprecated.
Now we have a new API based on character devices to access GPIO lines from user space.
This shows how to use library and a set of tools provided by the libgpiod project 
to read and write all the gpio of cp210x device at the same time.

Buil instructions:
1. g++ cp210x_gpio_example_gpiolib.c -lgpiod -o cp210x_gpio_example_gpiolib
2. Run "./program_name argv[1] argv[2]"
    - Where:
        argv[1] is the file path of gpiochip. example: '/dev/gpiochip0'
        argv[2] is value that is writed to gpio:
            0 : Low level, turn on led on cp210x ek board
            1 : high level, turn off led on cp210x ek board


CP210x Linux VCP Driver Revision History
----------------------------------------

January 29, 2021

    - Add VID/PID for WSDA-200-USB.
    - Support software flow control.
    - Fix bug RTS pin doesn't change to high level when Linux goes to suspended mode.
    - Fix error 32 when open using pyserial with hardware flow control enabled.
    - Support gpio interface(gpiolib) for CP2108.
    - Add gpio example using gpiolib: cp210x_gpio_example_gpiolib.c

July 12, 2019

    - Support for the CP2102N