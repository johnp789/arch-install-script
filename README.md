# Arch Linux Installation Script
These scripts use QEMU to install Arch Linux on a physical disk.  
Edit `launch_iso` and `install_script_settings` to use them.  

## Summary

* Two target physical storage devices are required:
    * A boot disk, typically a small USB flash drive
    * A main drive onto which everything else will be installed, 
    typically an SSD or hard disk
* Boot the Arch Linux installation ISO in a QEMU virtual machine
* Give the VM access to the two physical drives
* Format the drives:
    * Boot disk:
        * MBR partition table
        * Single partition
        * Ext4 filesystem
        * Contents of /boot are placed here
    * Main disk:
        * GPT partition table
        * BIOS boot partition
        * Remaining space in a second partition:
            * LUKS encrypted
            * LVM physical volume
            * 50 GB logical volume for /
* The encryption key for the LUKS partition is written to the USB drive
* Grub and initramfs are configured to supply the key from the USB for
unattended boot
* Unmount and remove the USB drive after booting for security, if
desired, but remember that the USB drive is needed for rebooting
* Users are created
    * Added to sudoers
    * SSH public keys added
* All password login is disabled, that is, SSH login with public key is
the only login method allowed
* Bare-bones set of packages are installed
* Locale and timezone are configured
* Hostname and network are configured

## Recommendations

* Make a backup of the contents of the USB drive---without the
`encryption.txt` file on that drive, the contents of the main drive are
unrecoverable
* Keep the package list for pacstrap minimal, and do other package
installation later with Ansible
