---
layout: post
title:  Hard disk replacement on Linux
categories:
- blog
---

Sometimes it comes to replace the hard disk in your laptop, because of lack of space or upgrade to SSD. To do this you will need:

* new 2,5" disk of the same or bigger capacity then the old one
* external USB enclosure for 2,5" disk
* USB stick with live system, like [Ubuntu 14.04.4 LTS](http://releases.ubuntu.com/14.04.4/ubuntu-14.04.4-desktop-amd64.iso).

And the following steps will be necessary:

0. Identify (and write down) the name of your current "old" hard disk calling `lsblk` in terminal. You should see the disk names, partitions on them and their sizes. If this does not work, you can try also:

         dmesg | grep 'logical blocks'

1. Switch off the computer. :wink:
2. Remove old disk from your laptop and insert it into external enclosure.
3. Insert new disk into computer.
4. Insert USB stick and power on computer. If the system does not start you may need to set the USB drive to be your primary startup choice in BIOS.
5. Plug in the external "old" disk (and do not mount anything).
6. Identify again the names of your drives in the system (as in the first step). Lets asume your new disk (being inserted into computer) is named `sda`, and the old one (in the case box) is named `sdb`.

    > Note, that if the new disk is named just like the old one in the first step (which is very probable) then the entries in `/etc/fstab` and bootloader configuration can remain untouched.

7. Clone the old disk to the new one via:

        sudo dd if=/dev/sdb of=/dev/sda

    or, if you have `pv` on your computer:

        sudo pv < /dev/sdb > /dev/sda

    The second option will display nice progress bar and is generally faster. These commands will clone all partitions and also MBR.

    > Note, that if your new disk is exactly of the same size and shape as the old one then there's
    > actually nothing more to do. Just reboot (and don't forget to remove USB stick).

8. Remove the external "old" disk. This is a safe copy of your system. You will experiment with the disk in the computer now.
8. Resize partitions on your new disk. I recommend a graphical interfaces to parted, like `gparted` or `partitionmanager`. The easiest and fastest way would be to enlarge the last partition to the end of the available space or just creation of new partition in the free space. What you will do (move, resize, create) depends on your current configuration and your goals. However if you resize  don't forget to call after that:

        sudo e2fsck -f /dev/sdaX
        sudo resize2fs /dev/sdaX

9. Modify (or not, see note above) `/etc/fstab`. To modify you need to mount your root partition, for example:

        mkdir /mnt/newdisk
        sudo mount /dev/sda2 /mnt/newdisk

    and edit as root `/mnt/newdisk/etc/fstab`

10. Modify (or not, see note above) your bootloader. If the names of the old and ne disks are different, or you changed configuration of your partitions, then you probably need to reconfogure bootloader. Mount your boot partition, for example:

        sudo -i
        mount /dev/sda1 /mnt/newdisk/boot

    and chroot into your system on new disk:

        mount -t proc proc /mnt/newdisk/proc
        mount --rbind /sys /mnt/newdisk/sys   
        mount --make-rslave /mnt/newdisk/sys          
        mount --rbind /dev /mnt/newdisk/dev
        mount --make-rslave /mnt/newdisk/dev
        chroot /mnt/newdisk                   
        source /etc/profile
        export PS1="(chroot) $PS1"

    Now you are able to install and configure bootloader. If you are using `grub`:

        grub-install

    and edit `/boot/grub/grub.conf`. If you are using `grub2`, then it should be enough to call:

        grub2-install
        grub2-mkconfig

11. Reboot and, hopefully, you're done. :smile:
