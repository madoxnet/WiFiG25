WiFiG25
=======

WiFiG25

#Preparation
##Development Environment
1. Install VirtualBox
2. Install Ubuntu 12.04 (32bit)
3. Update the system
4. Install VirtualBox Guesst Editions
5. Install packages

```
sudo apt-get install build-essential git subversion mercurial bison flex gettext texinfo unzip libncurses5-dev
sudo apt-get install gnome-panel
sudo apt-get install mtd-utils u-boot-tools 
sudo apt-get install meld geany
```

##Install Prerequisites

##Install Toolchain
```
wget http://sourcery.mentor.com/public/gnu_toolchain/arm-none-linux-gnueabi/arm-2011.09-70-arm-none-linux-gnueabi-i686-pc-linux-gnu.tar.bz2

sudo mkdir /opt/codesourcery/
sudo tar xvjf arm-2011.09-70-arm-none-linux-gnueabi-i686-pc-linux-gnu.tar.bz2 -C /opt/codesourcery/

echo "export PATH=/opt/codesourcery/arm-2011.09/bin:$PATH" >> ~/.bashrc
```

#Linux
##Clone the git repository
```
git clone git://github.com/linux4sam/linux-at91.git -b linux-3.15.0-at91
```

```
export ARCH=arm 
export CROSS_COMPILE=arm-none-linux-gnueabi-
make distclean
make at91_dt_defconfig
make dtbs
make menuconfig
make uImage
```

#Buildroot

#Manual Loading
This is probably only required for the early releases of the board where the recovery u-boot was not implemented
##Using USB
###Rootfs
```
fatload usb 0 0x22000000 rootfs.ubi
nand erase 0x800000 0xf800000
nand write.trimffs 0x22000000 0x800000 0x4000000
```
###DTB
```
fatload usb 0 0x21000000 at91-wifig25.dtb
nand erase 0x180000 0x20000
nand write 0x21000000 0x180000 0x20000
```
###Kernel
```
fatload usb 0 0x22000000 uImage
nand erase 0x200000 0x600000
nand write 0x22000000 0x200000 0x600000
```
###Manually boot a DTB and Kernel without saving to NAND
```
fatload usb 0 0x21000000 at91-wifig25.dtb
fatload usb 0 0x22000000 uImage.test
bootm 0x22000000 - 0x21000000
```

#Examples
##GPIO
##PWM
###Exporting PWM
```
echo 0 > /sys/class/pwm/pwmchip0/export
echo 1 > /sys/class/pwm/pwmchip0/export
echo 2 > /sys/class/pwm/pwmchip0/export
echo 3 > /sys/class/pwm/pwmchip0/export
```
###Exporting TCB-PWM
```
echo 0 > /sys/class/pwm/pwmchip4/export
echo 1 > /sys/class/pwm/pwmchip4/export
```
###Using PWM
```
echo 1000000 > /sys/class/pwm/pwmchip0/pwm0/period 
echo 500000 > /sys/class/pwm/pwmchip0/pwm0/duty_cycle
echo 1 > /sys/class/pwm/pwmchip0/pwm0/enable
```
###Running a servo
```
echo 20000000 > /sys/class/pwm/pwmchip4/pwm0/period 
echo 1500000 > /sys/class/pwm/pwmchip4/pwm0/duty_cycle
echo 1 > /sys/class/pwm/pwmchip4/pwm0/enable
```
```
echo 500000 > /sys/class/pwm/pwmchip4/pwm0/duty_cycle
```
```
echo 2500000 > /sys/class/pwm/pwmchip4/pwm0/duty_cycle
```
##I2C
```
//Example code for BH1750FVL Light Sensor Modules
//
//The modules are based off the ROHM BH1750FVL Ambient Light Sensor IC's and are 
//available from eBay for less than $3 delivered worldwide.
//
//This example code does not handle errors etc, search for proper code handling
//examples online.

#include <stdio.h>
#include <linux/i2c-dev.h>
#include <sys/ioctl.h>
#include <fcntl.h>

#define SENSOR_ADDRESS 0x23

int main()
{
  int fd;
  char data[2];
  float light;
  char command = 0x20;  //One Medium Resolution Read
  
  fd = open( "/dev/i2c-0", O_RDWR );
  ioctl( fd, I2C_SLAVE, SENSOR_ADDRESS );
  write( fd, &command, 1 );
  read( fd, data, 2 );
  light = (float) ( ( data[0] << 8 ) | data[1] ) / 1.2;
  printf( "%04f\n", light );
}
```
```
arm-none-linux-gnueabi-gcc lightread.c -o lightread
```
##SPI
##ADC
##1-Wire
