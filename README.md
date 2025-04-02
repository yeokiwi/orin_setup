# Reprogramming Guide

# USBIPD 


For WSL,

to list the usb for AGX Orin:

usbipd list

look for 0955:7023 device

to bind the usb for sharing:

usbipd bind -busid 1-1

To attach to WSL

usbipd attach --wsl --auto-attach --busid 1-1

if you encountered the following error

"usbipd: warning: A firewall appears to be blocking the connection; ensure TCP port 3240 is allowed."

you will need to Create or edit %UserProfile%.wslconfig and add the following:

[wsl2]

networkingMode=mirrored

The .wslconfig file does not exist by default. It must be created and stored in your %UserProfile% directory to apply these configuration settings.

Used to configure settings globally across all installed Linux distributions running as the WSL 2 version.

Can be used only for distributions run by WSL 2. Distributions running as WSL 1 will not be affected by this configuration as they are not running as a virtual machine.

To get to your %UserProfile% directory, in PowerShell, use cd ~ to access your home directory (which is typically your user profile, C:\Users\<UserName>) or you can open Windows File Explorer and enter %UserProfile% in the address bar. The directory path should look something like: C:\Users\<UserName>\.wslconfig.


# Reprogramming
The pdf contains steps to reprogram the agx orin and patch up the OS Image and dtb to connect to leopard GSML camera.

most of the steps were ok except installation of V4L. you will need to compile natively rather than apt install.

After completion, cloning of agx orin image.

https://docs.nvidia.com/jetson/archives/r35.4.1/DeveloperGuide/text/SD/FlashingSupport.html#basic-flashing-script-usage

For flash.sh, you will need to install the prerequisites

sudo tools/l4t_flash_prerequisites.sh

Use the following commmand to clone the image from AGX Orin

sudo ./flash.sh -r -k APP -G backup.img jetson-agx-orin-devkit mmcblk0p1

The backup will generate a sparse image(backup.img) and raw image(backup.img.raw)

Both image can be used for cloning.Raw image can be used to generate sparse image.

Copy backup.img to Linux_for_Tegra/bootloader/ system.img. It is advisable to backup the current system.img

Before cloning the image. Take note of the following:

If root file system of the source device for clone is resized during oem-config, the eMMC configuration file of destination device must be updated accordingly.
For example, with jetson-agx-orin-devkit, root file system (APP) is the final partition before secondary_gpt. If the APP is resized to maximum allowed size,
the allocation_attribute of APP partition in Linux_for_Tegra/bootloader/t186ref/cfg/flash_t234_qspi_sdmmc.xml must be updated from 0x8 to 0x808.

Use the following command to clone image to AGX Orin:

sudo ./flash.sh -r jetson-agx-orin-devkit mmcblk0p1

https://learn.microsoft.com/en-us/windows/wsl/connect-usb

Attach a USB device if you are using WSL for flashing

look for USB device with the VID:PID as 0955:7023

Since the orin will reset during flashing, the usb will be detached. So, for wsl flashing, it is necessary to set to auto-attach for usb

Eg. usbipd attach --wsl --auto-attach --bus-id 1-10

# Recompile Nvidia kernel OS guide

Download the following archives of the correct jetson version:

https://developer.nvidia.com/embedded/jetson-linux-archive

1. Driver Package (BSP) https://developer.nvidia.com/downloads/embedded/l4t/r35_release_v5.0/release/jetson_linux_r35.5.0_aarch64.tbz2
2. Sample Root Filesystem https://developer.nvidia.com/downloads/embedded/l4t/r35_release_v5.0/release/tegra_linux_sample-root-filesystem_r35.5.0_aarch64.tbz2
3. Driver Package (BSP) Sources https://developer.nvidia.com/downloads/embedded/l4t/r35_release_v5.0/sources/public_sources.tbz2
4. Bootlin Toolchain gcc 9.3 https://developer.nvidia.com/embedded/jetson-linux/bootlin-toolchain-gcc-93

Steps to recompile the kernel by nvidia scripts:

https://docs.nvidia.com/jetson/archives/r35.3.1/DeveloperGuide/text/SD/Kernel/KernelCustomization.html

Using typical linux recompilation steps:

https://chipnbits.github.io/content/projects/RLUnicycle/rtkernel/rtpatch.html

# How to build Jetson Orin offline

Download the following packages:

1. Driver Package (BSP) https://developer.nvidia.com/downloads/embedded/l4t/r35_release_v5.0/release/jetson_linux_r35.5.0_aarch64.tbz2

Create a new directory for the os and bootloader: mkdir r355_os,  cd r355_os

Uncompress jetson_linux_r35.5.0_aarch64.tbz2 : sudo tar ../jetson_linux_r35.5.0_aarch64.tbz2

After uncompressing, it should have a directory called Linux_for_Tegra.

2. Sample Root Filesystem https://developer.nvidia.com/downloads/embedded/l4t/r35_release_v5.0/release/tegra_linux_sample-root-filesystem_r35.5.0_aarch64.tbz2

cd Linux_for_Tegra, cd rootfs

Uncompress tegra_linux_sample-root-filesystem_r35.5.0_aarch64.tbz2: sudo tar ../../../tegra_linux_sample-root-filesystem_r35.5.0_aarch64.tbz2

change directory to Linux_for_Tegra : cd ..

perform sudo ./apply_binaries.sh

# additional libraries

1. sudo apt install nvidia-jetpack

2. sudo apt install qt5-default

3. sudo apt install libssl-dev

4. Download cmake.

5. ./bootstrap --qt-gui

6. 
