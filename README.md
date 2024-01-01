The pdf contains steps to reprogram the agx orin and patch up the OS Image and dtb to connect to leopard GSML camera.

most of the steps were ok except installation of V4L. you will need to compile natively rather than apt install.

After completion, cloning of agx orin image.

https://docs.nvidia.com/jetson/archives/r35.4.1/DeveloperGuide/text/SD/FlashingSupport.html#basic-flashing-script-usage

Use the following commmand to clone the image
sudo ./flash.sh -r -k APP -G backup.img jetson-agx-orin-devkit mmcblk0p1

The backup will generate a sparse image(backup.img) and raw image(backup.img.raw)
Both image can be used for cloning.Raw image can be used to generate sparse image.

Copy backup.img to Linux_for_Tegra/bootloader/ system.img. It is advisable to backup the current system.img

Before cloning the image. Take note of the following:

If root file system of the source device for clone is resized during oem-config, the eMMC configuration file of destination device must be updated accordingly.
For example, with jetson-agx-orin-devkit, root file system (APP) is the final partition before secondary_gpt. If the APP is resized to maximum allowed size,
the allocation_attribute of APP partition in Linux_for_Tegra/bootloader/t186ref/cfg/flash_t234_qspi_sdmmc.xml must be updated from 0x8 to 0x808.

Use the following command to clone image:

sudo ./flash.sh -r jetson-agx-orin-devkit mmcblk0p1


