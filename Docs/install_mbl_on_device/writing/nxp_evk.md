# NXP 8M Mini EVK devices

<span class="warnings">UNFINISHED. DO NOT USE.</span>

<span class="tips">This process currently uses a micro-SD card. A future release will use the on-board EMMC.</span>

1. Check the current block storage devices on your PC:

    ```
    lsblk
    ```

    A list of devices displays. For example:

    ```
    NAME          MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
    sdb             8:16   0   1.8T  0 disk
    └─sdb1          8:17   0   1.8T  0 part /mnt/2tb-disk
    sr0            11:0    1  1024M  0 rom  
    sda             8:0    0 238.5G  0 disk
    ├─sda2          8:2    0   238G  0 part
    │ ├─vg00-swap 253:1    0   7.5G  0 lvm  [SWAP]
    │ └─vg00-root 253:0    0 230.6G  0 lvm  /
    └─sda1          8:1    0   476M  0 part /boot
    ```

   You'll need to refer to this output in the following steps, so save it for reference.

1. Connect a micro-SD card to your PC. You can now see new storage devices:


    ```
    lsblk
    ```

    In this example, the device is listed as `sdc` (the partitions on the device are also shown):
<!--this doesn't match, because it's PICO-->

    ```
    NAME          MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
    sdb             8:16   0   1.8T  0 disk
    └─sdb1          8:17   0   1.8T  0 part /mnt/2tb-disk
    sr0            11:0    1  1024M  0 rom  
    sdc             8:32   1  14.5G  0 disk
    └─sdc1          8:33   1  14.5G  0 part /media/jjohnson/141E-8726
    sda             8:0    0 238.5G  0 disk
    ├─sda2          8:2    0   238G  0 part
    │ ├─vg00-swap 253:1    0   7.5G  0 lvm  [SWAP]
    │ └─vg00-root 253:0    0 230.6G  0 lvm  /
    └─sda1          8:1    0   476M  0 part 
    ```

    <span class="notes">In the commands below, replace `/dev/sdX` with the device file name for the SD card _without_ a number at the end.</span>

1. Ensure none of the micro-SD card's partitions are mounted (replace `/dev/sdX` as explained above):

    ```
    sudo umount /dev/sdX*
    ```

1. Write the disk image to the SD card device - not a partition on it (replace `/dev/sdX` as explained above):

    ```
    sudo bmaptool copy --nobmap /path/to/artifacts/machine/nxpimx-mbl/images/mbl-image-development/images/mbl-image-development-nxpimx-mbl.wic.gz /dev/sdX
    ```

    This action may take some time.

    <span class="tips">**Tip**: Use `--nobmap` for the initial flashing, to ensure your flash is set up correctly. On all subsequent flashings, you can use the `--bmap` option with a bmap file to speed up the process: `sudo bmaptool copy --bmap /path/to/artifacts/machine/nxpimx-mbl/images/mbl-image-development/images/mbl-image-development-nxpimx-mbl.wic.bmap /path/to/artifacts/machine/nxpimx-mbl/images/mbl-image-development/images/mbl-image-development-nxpimx-mbl.wic.gz /dev/sdX`</span>

1. When `bmaptool` has finished, eject the device:

    ```
    sudo eject /dev/sdX
    ```

1. To let the device boot from the SD-card, set up the device's DIP switches as explained on the device.

1. Detach the micro-SD card from your PC, and plug it into the NXP 8M Mini EVK.

1. connect the micro-USB port to your PC.

1. Power on the device.

1. From your PC, connect to the device's serial console (over your micro-USB port):

    ```
    minicom -D /dev/ttyUSB0
    ```

    <span class="notes">**Note**: If you have also connected the USB-C cable to your PC, you may need `/dev/ttyUSB1`</span><!--instead or as well?-->

    Use the following settings:

    * Baud rate: 115200.
    * Encoding: [8N1](https://en.wikipedia.org/wiki/8-N-1).
    * No hardware flow control (enabled by default).

1. Connect the NXP 8M Mini EVK USB-C **power** socket to the kit's power supply.

1. Connect the kit's power supply to a wall plug.

    The device now boots into MBL.

1. To log in to MBL, wait for a login prompt, and then enter the username `root`. You will not be prompted for a password.