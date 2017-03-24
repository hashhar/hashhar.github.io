---
layout: post
title: "Arch Linux Step 1: Installation Guide"
date: 2017-03-15 13:29:06 +0530
categories: ArchLinux,setup,distro,linux
---

So now that you've decided to give Arch Linux a try, you'll be needing to install it (duh!). Arch Linux is pretty
different when it comes to installation. It does provide an ISO you can boot to but that's provided mainly for you to be
able to bootstrap your new system. I only provide some snippets of code to demonstrate what you should do but ideally
you **SHOULDN'T** copy paste the commands blindly. Arch won't be a good fit for you if you are not willing to learn. So
let's get started. I recommend you have an instance of the [Arch Wiki Installation Guide][] on hand to be able to dive
deeper if need be.

The sections below are structured similar to the [Arch Wiki Installation Guide][] so people already familiar with it
should feel at home.

## Acquiring The Installation Media

There are primarily two ways of getting some kind of installation medium although you can go for more specialised
configurations like PXE or installing from within another Linux machine. You can find those discussed in the wiki
[here][special installation scenarios].

- A bootable live-environment ISO
- A net-boot

You can find how to get them and use them [here][Arch Linux Download].

## Some Knowledge To Help Make The Process Easier

Arch Linux used to provide a 32-bit OS too but that has been removed in the March 2017 release going forward. You need
at least 500MB of RAM, 800MB of disk space and internet connectivity to complete the installation. There are ways to
bootstrap the system without internet connectivity. You can explore those on the wiki [here][offline installation of
packages] and [here][installing packages from a CD/DVD or USB stick]. I've also written a guide [here][offline arch
linux installation] if you'd want someone to hold your hand.

Once you boot into the live installation medium you can use a text mode browser ([ELinks][] in included in the ISO) to
browse the Arch Wiki. You can swicth between virtual consoles using the `Alt+arrow` shortcut. If you have a wired
connection and have DHCP working your internet connection should be good to go. Otherwise check out [this][network
configuration] article for wired networks and [this][wireless network configuration] article for wireless networks. By
default you will be dropped into [Zsh][] and have proper tab completion for most commands. As for editors you can use
[nano][], [vim][] or [vi][].

## Pre-installation

You should disable Secure Boot before proceeding to make sure we can install an unsigned bootloader. You can later write
hooks to self-sign kernel images on kernel updates and re-enable Secure Boot.

### Set the keyboard layout

The default layout that Arch Linux uses in the ISO is US. You can find available keymaps by running `ls
/usr/share/kbd/keymaps/**/*.gz`. You can change the active keyboard layout by running `loadkeys filename` where filename
is the one you got from running the above commands. If you have the time read the [`loadkeys` manpage][man:loadkeys] to
gain some knowledge. It's always a good idea to read the manpage of a command you've never encountered before. You can
similarly set console fonts using [`setfont`][man:setfont] and the fonts can be found under
`/usr/share/kbd/consolefonts/`.

### Verify the boot mode

Most likely you are booting in UEFI mode if you have a computer made after the 2008s. You can check if you are currently
booted using UEFI by seeing if `ls /sys/firmware/efi/efivars/` returns any files.

### Connect to the internet

If you have a wired connection and have DHCP working you should be good to go because the [dhcpcd][] daemon is enabled
on boot if a wired connection is detected. You can force it to try again by issuing `dhcpcd interface-name`. You can
find your intername using `ip link`. If the network is not working or you are using a wireless network, stop `dhcpcd`
using `systemctl stop dhcpcd` and follow the network configuration articles for [wired networks][network configuration]
and [wireless networks][wireless network configuration].

Check your internet connectivity using the `ping` command. Your [DNS][] may be the issue so you can try editing
`/etc/resolv.conf` to temporarily change them (they will be reset when you unplug the ethernet).

### Update the system clock

You can use [NTP][] to set time by using `timedatectl set-ntp true`. You can also use `timedatectl set-time time` to
manually set time if you can't access NTP.

#### RTC in localtime vs UTC

If you dual boot with Windows and your [Real Time Clock][RTC] is set to use local time you can do two things:

1. Tell Windows to [use UTC in the RTC][Windows UTC]. Then you can simply reboot and set the RTC to UTC time and
   continue with the process.
2. Continue using the RTC as local time. In that case you can achieve time-synchronization by running the following:

```bash
timedatectl set-timezone Timezone    # You can use tab completion
hwclock --hctosys --localtime        # This copies the time from your RTC to system time
timedatectl set-local-rtc true       # This tells the OS that you have RTC set to use localtime
# There are additional steps to take after chrooting in your new step which will be discussed in the relevant section.
```

There are some additional steps to take after the [Chroot][] step to ensure time synchronization is carried over the new
installation as well.

### Partition the disks

Use `fdisk -l` to identify the partitions or disks that you have on your system. At least a partition for the root
directory `/` and if using UEFI, an [EFI System Partition][] are required. Ideally you should have a separate home
partition and a swap partition if needed. Read the article on [Swap][] for the many possible ways you can set it up.

To modify partition tables you can use [`fdisk`][fdisk] (for MBR disks) or [`gdisk`][gdisk] (for GPT disks). I highly
recommend resizing your EFI partition to be atleast 250MB before proceeding.  If at all possible, grow it to 500MB. The
best way to resize it will be from either [GParted][] on Linux or [AOMEI Partition Assistant][] on Windows. However if
you are feeling lucky you can [parted][] too. My personal setup is:

```
Disk /dev/sda: 232.9 GiB, 250059350016 bytes, 488397168 sectors

Device         Start       End   Sectors  Size Type
/dev/sda1       2048    923647    921600  450M Windows recovery environment
/dev/sda2     923648   1959929   1036282  506M EFI System
/dev/sda3    1959930   1992697     32768   16M Microsoft reserved
/dev/sda4    1992704 316577791 314585088  150G Microsoft basic data
/dev/sda5  316577792 463403007 146825216   70G Linux filesystem

Disk /dev/sdb: 931.5 GiB, 1000204886016 bytes, 1953525168 sectors

Device          Start        End    Sectors  Size Type
/dev/sdb1        2048  524313411  524311364  250G Microsoft basic data
/dev/sdb2   524314624 1782620594 1258305971  600G Microsoft basic data
/dev/sdb3  1782622208 1929439231  146817024   70G Linux filesystem
/dev/sdb4  1929439232 1953523711   24084480 11.5G Linux swap
```

If you want to create any stacked block devices for [LVM][], [disk encryption][] or [RAID][] do it now before
proceeding.

### Format the partitions

After creating the partitions you need to format them with the appropriate filesystems. Format the EFI partition with
FAT32 using `mkfs.vfat /dev/sdaN`, the root partition using `mkfs.ext4 -L "Arch Linux" /dev/sdaN`. You should look into
[this][creating filesystems] for more details.

### Mount the partitions

[Mount][] the filesystem on root partition to `/mnt`. e.g. `mount /dev/sda2 /mnt`.  
Create mount points for any remaining partitions and mount them accordingly.

```bash
mkdir /mnt/boot
mount /dev/sda1 /mnt/boot
```

## Installation

### Select mirrors

Packages to be installed are downloaded from mirror servers, which are defined in `/etc/pacman.d/mirrorlist`. On the
live system, all mirrors are enabled, and sorted by their synchronization status and speed at the time the installation
image was created. The higher a mirror is placed in the list, the more priority it is given when downloading a package.
Edit the file accordingly, and move the geographically closest mirrors to the top of the list.

There is also a package [`reflector`][reflector mirrorlist] which can help you in generating the file.

### Install the base packages

Use the `pacstrap` script to install the [`base`][base package group] by issuing `pacstrap /mnt base`. You can append
other package groups or package names to the command to install them. The `base-devel` group is a good idea and
including some text based browser ([ELinks][]) is recommended if you need to access captive portals.

It is worth noting that the `base` group doesn't include everything the live environment offers. Specifically some
firmware. See [this][packages.both] for details.

## Configure the system

### Generate the fstab file

Use the command `genfstab` to generate a [fstab][] file. Also take a look at the article about [persistent block device
naming][] to find ways to make sure your `fstab` doesn't break when disks are reformatted or plugged in and out of the
system.

```bash
genfstab -U /mnt >> /mnt/etc/fstab # -U means UUIDs, while -L will use labels
```

Check the generated file under `/mnt/etc/fstab` and correct any errors.

### Chroot

Now you need to [change root][] into the new system so that all further commands we run will affect the newly installed
system instead of the live-environment.

```bash
arch-chroot /mnt
```

### Time zone

You can now set the timezone by linking the appropriate `tzfile` from `/usr/share/zoneinfo` as such:

```bash
ln -sf /usr/share/zoneinfo/Region/City /etc/localtime
```

You can now set the time using `hwclock`. If you have your RTC clock set to UTC and the time reported by the `date`
command is correct, you can simply run `hwclock --systohc` to generate `/etc/adjtime`.

Picking up from [previous instructions][RTC vs localtime], you have some additional steps to take if you have
your RTC clock set to use localtime. You can complete the time-synchronization by running the following:

```bash
hwclock --hctosys --localtime
```

Basically, the short guide is to use `--systohc` if system time is correct, `--hctosys` if hwclock is correct and append
`--localtime` or `--utc` according to your preferences.

### Locale

Now you need to set up [localizations][] for your system by uncommenting required locales in `/etc/locale.gen` and then
running:

```bash
locale-gen
```

It's a good idea to uncomment at least one UTF-8 locale, preferabbly `en_US.UTF-8 UTF-8`.

You now need to set the `LANG` variable in [`locale.conf`][man:locale.conf] accordingly. As an example:

```
# /etc/locale.conf
LANG=en_US.UTF-8
```

If you made any changes to the keyboard layout, you'll need to make the changes persistent in
[`vconsole.conf`][man:vconsole.conf] as:

```
# /etc/vconsole.conf
KEYMAP=de-latin1
```

### Hostname

Hostname can be set by using the [`hostname`][man:hostname] file:

```
# /etc/hostname
myhostname
```

You should also add a matching entry to your [`hosts`][man:hosts] file to allow local name resolution:

```
# /etc/hosts
127.0.1.1    myhostname.localdomain    myhostname
```

If you also want a pretty hostname (the machine name shown to other users on the network) like "Bob's Laptop", you can
use the [`machine-info`][man:machine-info] file:

```
# /etc/machine-info
PRETTY_HOSTNAME="Bob's Laptop"
ICON_NAME=computer-laptop
CHASSIS=laptop
DEPLOYMENT=production
```

## Network Configuration

The newly configured environment has no network connection activated by default.

<!-- Links -->
[Arch Wiki Installation Guide]: https://wiki.archlinux.org/index.php/Installation_guide
[special installation scenarios]: https://wiki.archlinux.org/index.php/Category:Getting_and_installing_Arch
[Arch Linux Download]: https://www.archlinux.org/download/
[offline installation of packages]: https://wiki.archlinux.org/index.php/Offline_installation_of_packages
[installing packages from a CD/DVD or USB stick]: https://wiki.archlinux.org/index.php/Pacman/Tips_and_tricks#Installing_packages_from_a_CD.2FDVD_or_USB_stick
[offline arch linux installation]: {{ site.url }}/offline-arch-linux-installation
[ELinks]: https://wiki.archlinux.org/index.php/ELinks#Usage
[network configuration]: https://wiki.archlinux.org/index.php/Network_configuration
[wireless network configuration]: https://wiki.archlinux.org/index.php/Wireless_network_configuration
[Zsh]: https://wiki.archlinux.org/index.php/Zsh
[nano]: https://wiki.archlinux.org/index.php/Nano#Usage
[vim]: https://wiki.archlinux.org/index.php/Vim#Usage
[vi]: https://en.wikipedia.org/wiki/vi
[man:loadkeys]: http://man7.org/linux/man-pages/man1/loadkeys.1.html
[man:setfont]: http://man7.org/linux/man-pages/man8/setfont.8.html
[dhcpcd]: https://wiki.archlinux.org/index.php/Dhcpcd
[DNS]: https://wiki.archlinux.org/index.php/Resolv.conf
[NTP]: https://en.wikipedia.org/wiki/Network_Time_Protocol
[RTC]: https://en.wikipedia.org/wiki/Real-time_clock
[Windows UTC]: https://wiki.archlinux.org/index.php/Time#UTC_in_Windows
[Chroot]: #chroot
[EFI System Partition]: https://wiki.archlinux.org/index.php/EFI_System_Partition
[Swap]: https://wiki.archlinux.org/index.php/Swap
[fdisk]: https://wiki.archlinux.org/index.php/Fdisk
[gdisk]: https://wiki.archlinux.org/index.php/Fdisk#gdisk
[GParted]: https://wiki.archlinux.org/index.php/GNU_Parted
[AOMEI Partition Assistant]: http://www.aomeitech.com/aomei-partition-assistant.html
[parted]: https://www.gnu.org/software/parted/manual/html_chapter/parted_2.html#SEC25
[LVM]: https://wiki.archlinux.org/index.php/LVM
[disk encryption]: https://wiki.archlinux.org/index.php/Disk_encryption
[RAID]: https://wiki.archlinux.org/index.php/RAID
[creating filesystems]: https://wiki.archlinux.org/index.php/File_systems#Create_a_file_system
[Mount]: https://wiki.archlinux.org/index.php/File_systems#Mount_a_filesystem
[reflector mirrorlist]: https://wiki.archlinux.org/index.php/Reflector
[base package group]: https://www.archlinux.org/groups/x86_64/base/
[packages.both]: https://projects.archlinux.org/archiso.git/tree/configs/releng/packages.both
[fstab]: https://wiki.archlinux.org/index.php/Fstab
[persistent block device naming]: https://wiki.archlinux.org/index.php/Persistent_block_device_naming#by-uuid
[change root]: https://wiki.archlinux.org/index.php/Change_root
[RTC vs localtime]: #RTC-in-localtime-vs-UTC
[localizations]: https://wiki.archlinux.org/index.php/Localization
[man:locale.conf]: http://man7.org/linux/man-pages/man5/locale.conf.5.html
[man:vconsole.conf]: http://man7.org/linux/man-pages/man5/vconsole.conf.5.html
