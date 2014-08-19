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
