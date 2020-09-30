# Dual Boot Arch Linux with Windows



## Windows Checklist

1. Disable Windows Fast Startup (found in power options).
2. Disable Secure boot in BIOS Menu.
3. Don't Hibernate Windows.
4. Partition Windows Drive (for linux).



## In the Arch Live

1. **Check internet connection**

    ```
    ip a
    ```
or, 
    ```
    ip addr
    ```

    * `enp` - Ethernet
    * `wlp` / `wlan` - WIFI
    * `NO CARRIER` - Hardware missing

2. **To connect to WIFI**

    * `wifi-menu` is not included in the live iso.
    * We need to use the `iwctl` command.
    * The following are the steps:

        ```bash
        > iwctl     # To start
        > device list   # to list the devices - Note the Name of device
        > station `Name` scan   # To scan for wifi
        > station `Name` get-networks   # To get the list of WIFIs detected
        > station `Name` connect `Network-Name`     # Connect to Network-Name
        # provide the password
        > exit      # To exit
        ```

    * Check if we have internet using Point 1.

3. **Update the `mirrorlist`**
   
    * Install `reflector` package

        ```
        # Install
        > pacman -Syy reflector

        > reflector -c `CoutnryName` -a 6 --sort rate --save /etc/pacman.d/mirrorlist
        > pacman -Syyy  # Re-sync the server
        ```

4. **Sync the Network Time Protocol**

    ```
    timedatectl set-ntp true
    ```

5. **Format the disk**

    * Use `fdisk` or `cfdisk`
    * **Don't create another partition table**
    * For `cfdisk`

        ```
        > n     # To create a new partition
        > +500M     # Size of 500M (note the + sign)
        > t     # To input the type of partition
        > p     # show current partitions again
        > w     # To write the changes
        ```

    * We need:
        * To create a partition of type `Linux Partition` for root.
        * To create another partition for home (type `Linux Partition`). This is required if we are formatting to `ext4`.
    * Check using the `lsblk` command.

    * Format the partition(s)

        ```
        > mkfs.fat -F32 /dev/`deviceName`   # Not required for dual boot
        > mkfs.ext4 /dev/`deviceName`   # For ext4
        > mkfs.btrfs /dev/`deviceName`  # For btrfs
        ```

    * Finally we are supposed to have
        1. Windows Recovery Environment - 529M
        2. EFI system - 100M
        3. Microsoft Reserved - 16M
        4. C Drive / Microsoft basic data - ?
        5. Linux Partition for root - ?
        6. Linux Partition for home - ? (Optional)

6. **Create sub-volume (for `btrfs`)**

    * Mount the partition for `root`

        ```
        mount /dev/`deviceName` /mnt
        ```

    * Create the `root` sub-volume

        ```
        btrfs su cr /mnt/@root
        ```

    * Create the `home` sub-volume

        ```
        btrfs su cr /mnt/@home
        ```

    * Create the `var` sub-volume

        ```
        btrfs su cr /mnt/@var
        ```

    * Create the `srv` sub-volume

        ```
        btrfs su cr /mnt/@srv
        ```

    * Create the `opt` sub-volume

        ```
        btrfs su cr /mnt/@opt
        ```

    * Create the `tmp` sub-volume

        ```
        btrfs su cr /mnt/@tmp
        ```

    * Create the `swap` sub-volume

        ```
        btrfs su cr /mnt/@swap
        ```

    * Create the `snapshots` sub-volume

        ```
        btrfs su cr /mnt/@.snapshots
        ```

    * Unmount

        ```
        umount /mnt
        ```

7. **Mount the sub volumes with various options (for `btrfs`)**

    * Mount the root sub-volume

        ```
        mount -o noatime,compress=lzo,space_cache,subvol=@root /dev/`deviceName` /mnt
        ```

    * Make the required directories

        ```
        mkdir /mnt/{boot,home,var,srv,opt,tmp,swap,.snapshots}
        ```

    * Mount the sub-volumes

        ```
        mount -o noatime,compress=lzo,space_cache,subvol=@home /dev/`deviceName` /mnt/home
        mount -o noatime,compress=lzo,space_cache,subvol=@srv /dev/`deviceName` /mnt/srv
        mount -o noatime,compress=lzo,space_cache,subvol=@tmp /dev/`deviceName` /mnt/tmp
        mount -o noatime,compress=lzo,space_cache,subvol=@opt /dev/`deviceName` /mnt/opt
        mount -o noatime,compress=lzo,space_cache,subvol=@.snapshots /dev/`deviceName` /mnt/.snapshots
        ```

    * Mount the following with different options

        ```
        mount -o nodatacow,subvol=@swap /dev/`deviceName` /mnt/swap
        mount -o nodatacow,subvol=@var /dev/`deviceName` /mnt/var
        ```

    * Mount the boot partition

        ```
        mount /dev/`EFI partition` /mnt/boot
        ```
        
        * For `btrfs` we need to mount to `/mnt/boot` rather than `/mnt/boot/efi`

    * Check using the `lsblk`. Note the MOUNTPOINT will be the last one (`/mnt/var`) for the `deviceName` partition.

8. **Mount partitions (for `ext4`)**

    * Mount the root partition

        ```
        mount /dev/`deviceName` /mnt
        ```

    * Create required directories

        ```
        mkdir /mnt/{home,boot}
        ```

    * Mount the partitions

        ```
        mount /dev/`homePartition` /mnt/home
        mount /dev/'EFI partition' /mnt/boot
        ```

    * Verify with `lsblk`.

9. **Install the base packages**

    ```
    pacstrap /mnt base linux linux-lts linux-firmware neovim intel-ucode btrfs-progs
    ```

10. **Generate the File System Table**

    ```
    genfstab -U /mnt >> /mnt/etc/fstab
    ```

    * Verify the `fstab` file

        ```
        cat /mnt/etc/fstab
        ```

11. **`chroot` inside the installation**

    ```
    arch-chroot /mnt
    ```

12. **Create the `swapfile`**

    * **For `btrfs`**

        ```
        truncate -s 0 /swap/swapfile
        chattr +C /swap/swapfile
        btrfs property set /swap/swapfile compression none
        dd if=/dev/zero of=/swap/swapfile bs=1G count=2 status=progress
        chmod 600 /swap/swapfile
        mkswap /swap/swapfile
        swapon /swap/swapfile
        ```

    * Put the `swapfile` into the `fstab` file. Add the following line `fstab` file

        ```
        /swap/swapfile none swap defaults 0 0
        ```

    * **For `ext4`**

        ```
        dd if=/dev/zero of=/swapfile bs=1G count=2 status=progress
        chmod 600 /swapfile
        mkswap /swapfile
        swapon /swapfile
        ```

    * Put the `swapfile` into the `fstab` file. Add the following line `fstab` file

        ```
        /swapfile none swap defaults 0 0
        ```

13. **Add time zone**

    * Find time zones

        ```
        timedatectl list-timezones | grep `CityName`
        ```

    * Add that

        ```
        ln -sf /usr/share/zoneinfo/`Continent`/`city` /etc/localtime
        ```

    * Sync hardware clock to system clock

        ```
        hwclock --systohc
        ```

14. **Create the `locale`**

    * Edit the `/etc/locale.gen` file, by un-commenting `en_US.UTF-8 UTF-8`.
    * Generate locale

        ```
        locale-gen
        ```

    * Put the locales in `/etc/locale.conf` file. Also put some country specific settings.

        ```
        LANG=en_US.UTF-8
        LC_ADDRESS=en_IN
        LC_IDENTIFICATION=en_IN
        LC_MEASUREMENT=en_IN
        LC_MONETARY=en_IN
        LC_NAME=en_IN
        LC_NUMERIC=en_IN
        LC_PAPER=en_IN
        LC_TELEPHONE=en_IN
        LC_TIME=en_IN
        ```

15. **Hostname**

    * In the file `/etc/hostname` write the hostname of your choice (say 'archPC').
    * In the `/etc/hosts` add the following

        ```
        127.0.0.1   localhost
        ::1         localhost
        127.0.1.1   archPC.localdomain  archPC
        ```

16. **Install some useful packages**

    ```
    grub
    grub-btrfs
    efibootmgr
    dosfstools
    os-prober
    mtools
    linux-headers
    linux-lts-headers
    base-devel
    networkmanager
    network-manager-applet
    wpa_supplicant
    wireless_tools
    netctl
    dialog
    wifi-menu
    git
    reflector
    bluez
    bluez-utils
    cups
    xdg-utils
    xdg-user-dirs
    ```

    * Enable `NetworkManager`

        ```
        systemctl enable NetworkManager
        ```

    * Enable bluetooth

        ```
        systemctl enable bluetooth
        ```

    * Enable cups

        ```
        systemctl enable org.cups.cupsd
        ```

    * Install graphics drivers

        ```
        mesa
        xf86-video-intel
        xf86-video-amdgpu
        ```

17. **Create Initial ramdisk**

    * Add `btrfs` module in file `/etc/mkinitcpio.conf`

        ```
        # Change MODULES=() to
        MODULES=(btrfs)
        ```

    * Now generate the ramdisk
    
        ```
        mkinitcpio -p linux
        mkinitcpio -p linux-lts
        ```

18. **Users**

    * Add a password for the root user.

        ```
        > passwd
        # Enter password after prompt
        ```

    * Create a user

        ```
        useradd -m -g users -G wheel <username>
        ```

    * Set password for user

        ```
        passwd <username>
        ```

    * Allow users in the 'wheel' group to use sudo (Make sure that `sudo` is installed)

        ```
        EDITOR=neovim visudo
        ```

        * Un-comment

        ```
        %wheel ALL=(ALL) ALL
        ```

19. **Install Grub**

    * Packages have been installed already.
    * Install grub

        ```
        grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=grub_uefi --recheck
        ```

    * Temporarily mount the windows partition

        ```
        mkdir /mnt/windows10
        mount /dev/`CDrive` /mnt/windows10
        ```

    * Generate the grub configuration file

        ```
        grub-mkconfig -o /boot/grub/grub.cfg
        ```

20. **Exit**

    * Exit the system

        ```
        > exit
        ```

    * Unmount all

        ```
        umount -a
        ```

    * reboot



## After reboot

1. Install display server

    ```
    pacman -S xorg xorg-xinit
    ```

2. Install display manager

    ```
    pacman -S sddm
    ```

    * Enable that

        ```
        sudo systemctl enable sddm
        ```

3. Install KDE plasma

    ```
    sudo pacman -S plasma 
    ```

4. Install the following KDE apps

    ```
    konsole
    dolphin
    firefox
    kate
    breeze-gtk
    kde-gtk-config
    kdeplasma-addons
    plasma-nm   # Should be installed with plasma
    plasma-pa   # Should be installed with plasma
    ```

5. The follwing are additional KDE applications 

    ```
    ark
    libreoffice-fresh
    okular
    kinfocenter
    kwalletmanager
    kompare     # or kdiff3
    kfind
    ktorrent    # or qbittorrent
    gwenview
    kipi-plugins
    spectacle
    kcolorchooser
    kruler
    vlc
    speedcrunch
    redshift 
    ```

---

The above was created from 

1. [Arch Linux BTRFS Install](https://www.youtube.com/watch?v=7ituCCKXmMM)
2. [How to dual boot Arch Linux and Windows 10 on UEFI (full install and removal)](https://www.youtube.com/watch?v=QU0KRTgRJZU)
3. [How to Install Arch Linux](https://wiki.learnlinux.tv/index.php/How_to_Install_Arch_Linux)
