---
title: "Build Your Own OVMF Firmware for Qemu VM"
date: 2022-07-24T12:49:46-07:00
draft: true
---

# Overall Reference 
https://wiki.ubuntu.com/UEFI/EDK2

# My Environment
OS: Ubuntu 20.04

Kernel: 5.4

QEMU: 6.1

Guest OS Type: pc-q35-6.1

# Before You Start

- It does not seem to support zsh and oh-my-zsh, if you are using those as default shell like me, please first run `bash` and then rest of the commands in this guide.
- If your nasm version is less than 2.15.05 it may have some compatibility issue per [this](https://github.com/tianocore/edk2/blob/master/BaseTools/Bin/nasm_ext_dep.yaml). To install nasm_2.15.05 as a separate package on Ubuntu, the .deb file can be downloaded from [here](https://launchpad.net/ubuntu/+archive/primary/+files/nasm_2.15.05-1_amd64.deb). Install it via `sudo dpkg -i nasm_2.15.05-1_amd64.deb`
https://github.com/tianocore/edk2/blob/master/BaseTools/Bin/nasm_ext_dep.yaml


# Prepare the Build Environment
```
sudo apt install build-essential git uuid-dev iasl nasm ;
# check nasm version and do necessary steps in the Caution section above
git clone --depth 1 "https://github.com/tianocore/edk2.git" -b "edk2-stable202205" ;
cd edk2 ;
git submodule update --init --recursive ;
make -C BaseTools ;
. edksetup.sh #make sure to include the dot
```

# Make Changes for Config

Presuming you are running X64 just like me, the target.txt file needs to be changed a bit. See below.

$ vi Conf/target.txt

Replace it with

 ACTIVE_PLATFORM       = OvmfPkg/OvmfPkgX64.dsc 

Replace it with

 TOOL_CHAIN_TAG        = GCC5

Replace it with 'X64' for 64bit

 TARGET_ARCH           = X64 

# Build the Ovmf Firmware

Run:
```
build
```
When it's complete, the output is located in the `Build/OvmfX64/RELEASE_GCC5/FV/` folder.


## Build the firmware with Secure Boot support
If you wish to build OVMF with Secure Boot, you must follow the openssl installation instructions found in CryptoPkg/Library/OpensslLib/Patch-HOWTO.txt, and build with the SECURE_BOOT_ENABLE option:
`$ build -DSECURE_BOOT_ENABLE=TRUE`

# Post Build

Manually copy the output files:
```
sudo cp Build/OvmfX64/RELEASE_GCC5/FV/OVMF_CODE.fd  /usr/share/OVMF/OVMF_CODE.secboot.202207.fd ;
sudo cp Build/OvmfX64/RELEASE_GCC5/FV/OVMF_VARS.fd  /usr/share/OVMF/OVMF_VARS.secboot.202207.fd ;
```

Then, make necessary changes in the VM definition file. I'm using virsh for VM management, so I'm using the following command to edit my win10 VM. When it's complete, the <OS> section of the VM xml would look like this:

```xml
  <os>
    <type arch='x86_64' machine='pc-q35-6.1'>hvm</type>
    <loader readonly='yes' type='pflash'>/usr/share/OVMF/OVMF_CODE.secboot.202207.fd</loader>
    <nvram>/usr/share/OVMF/OVMF_VARS.secboot.202207.fd</nvram>
  </os>
```
