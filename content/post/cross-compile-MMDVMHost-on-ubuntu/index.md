---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Cross Compile MMDVMHost on Ubuntu"
subtitle: "Using raspberry-pi toolchain and then some"
summary: "A quick \"How-to\" guide to compiling raspberry-pi programs on Ubuntu 20.04 using pi toolchain. This post takes Pi Zero W (BCM2708 chip) as an example but the approach would be applicable to other Pi systems."
profile: false
authors:
  - david-xiao
categories:
  - Raspberry-Pi
tags:
  - raspberrypi
  - cross-compile
  - linux
  - c++
  - cmake
date: 2020-05-25
lastmod: 2020-05-25
featured: false
draft: false



# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  placement: 1
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

A little teaser here: Raspberry Pi is not really ediable :-)

So I have a [Raspberry Pi Zero W](https://www.raspberrypi.org/blog/raspberry-pi-zero-w-joins-family/) for about one year now.

It runs a few software. One of them is called Pi-Star. It's an open source toolkit for digial voice over [amateur radio](https://en.wikipedia.org/wiki/Amateur_radio). Find more detail about Pi-Star [here](https://www.pistar.uk).

Amateur radio is one of my hobbies. Figuring out how [digital voice modes](https://en.wikipedia.org/wiki/List_of_amateur_radio_modes#Digital_voice) work in the amateur radio world took some time for me but it was rewarding. At the end of the day, listening to hams talking about their passions from all over the world on my little handheld radio feels amazing.

Pi-Star works very well in my setup, so when I found out Pi-Star does not display my public IP address on OLED, I decided to write some code for it.

Here's what it looks like.

{{< figure src="pistar-inetip.jpg" title="OLED display showing my Public IP" >}}

---

## 1. Download the Toolchain with Extra Libs and Headers

I use Ubuntu 20.04 on my homelab as cross compiling platform, but any recent Linux distro should work.

My toolchain is a fork from the original toolchain with extra libs and headers for compiling MMDVMHost.

    $ cd ~/code
    $ git clone https://github.com/davxiao/tools.git

## 2. Code is the Easy Part <3

Pi-Star consists of a few components including a PHP frontend and a few programs as backend for data exchange over various digital voice networks.

MMDVMHost is the program that interfaces to the digital voice modem (MMDVM) on one side, and a suitable network on the other. It's written in standard C++ with dependencies to external libs such as [ArduiPi_OLED](https://github.com/hallard/ArduiPi_OLED).

For my purpose, I added some code in `CNetworkinfo` class and `COLED` class. If you don't know much about C++, no worries, just download all source code from my github repo.

Download my repo:

    $ cd ~/code
    $ git clone https://github.com/davxiao/MMDVMHost.git

## 3. Prep for the Cross Compilation

In `MMDVMHost/cmake/CrossCompile.cmake`, you wanted to update toolchain paths so that CMake will be able to generate correct Makefile afterwards.

When it's done, run:

    $ cd MMDVMHost/cmake
    $ cmake ../ -DCMAKE_TOOLCHAIN_FILE=./CrossCompile.cmake

If you see warnings like this, try delete `CMakeCache.txt` and run the cmake command again.

```text
CMake Warning:
  Manually-specified variables were not used by the project:
    CMAKE_TOOLCHAIN_FILE
```

## 4. Last Step

When Cmake is done, a `Makefile` was generated under the same directory. Run the following in the same directory:

    $ make ;

When the complication is complete, you should see `MMDVMHost` in the `cmake/` directory. You may wish to run `file ./MMDVMHost` to confirm the target platform is ARM as opposed to amd64. Here's my output:

```text
 ./MMDVMHost: ELF 32-bit LSB executable, ARM, EABI5 version 1 (SYSV), 
 dynamically linked, interpreter /lib/ld-linux-armhf.so.3, for GNU/Linux 2.6.32,
 with debug_info, not stripped
```

Congrats! You've built MMDVMHost using cross compilcation.

## Deployment

You can skip this section if you have set up your own deployment pipeline. 

On my homelab, I mount the same Samba share folder on both the Ubuntu and the Pi, then just copy the MMDVMHost over. Before replacing the MMDVMHost, you need to confirm the SD card is mounted in R/W mode and MMDVMHost service is stopped. Make a backup of the original MMDVMHost is also a good idea.

    $ rpi-rw ; 
    $ sudo systemctl stop mmdvmhost ; 
    $ sudo systemctl stop mmdvmhost.timer ;
    $ sudo cp ~/nas-dir/MMDVMHost.build /usr/local/bin/MMDVMHost ;
    $ sudo systemctl start mmdvmhost ;
    $ sudo systemctl start mmdvmhost.timer ;

{{% alert note %}}

If you experience `mount error(115): Operation now in progress` when mounting CIFS on Pi, it might be caused by the iptable rules set by Pi-Star.

To troubleshooting the issue, run the following commands on Pi-Star and see if mount works.

No worries, the following changes do not persist between restarts.

```text
    sudo iptables -P INPUT ACCEPT; 
    sudo iptables -P FORWARD ACCEPT; 
    sudo iptables -P OUTPUT ACCEPT; 
    sudo iptables -F; 
    sudo iptables -X ; 
    sudo iptables -nvL ;
```

{{% /alert %}}
