# Booting a custom Linux distro

Beware, you will not be able to inspect the boot process without connecting to the serial port on the motherboard!

The HDMI port will **not** produce output until you have successfully booted into Linux, if even then ;).

**NOTE:** Do not use any of the internal flash partitions for `asloader.efi`! They are first in the boot order, meaning you cannot control the boot process via external USB if they are booted first.

**NOTE:** It seems ASUSTOR didn't add SCSI support to their EFI firmware, this means it's impossible to boot from harddisks, and removable media must be used.

## Disable ASUSTOR custom bootloader (grub)

To mount the internal flash memory, we must first unlock the device:

```console
$ toolbox diskman -lock /dev/sda 0
```

The disk has 4 partitions, the first (`sda1`) contains the boot loader.

```console
$ lsblk -f
NAME                 FSTYPE      LABEL UUID                                   MOUNTPOINT
sda
├─sda1               vfat              7666-5203
├─sda2               ext4              ac587fb5-5b84-437d-9c9f-5526cfc8c27e
├─sda3               ext4              d60b4845-fe4f-4184-9e34-41ce268533e1
└─sda4               ext4              8a91e48b-5b12-41b5-b5c0-a3393fa755ea
```

It can be disabled via rename, you also might want to back up the contents.

```console
$ mkdir /mnt/boot
$ mount /dev/sda1 /mnt/boot
$ mv /mnt/boot/boot/asloader.efi /mnt/boot/boot/asloader.efi.disable
$ umount /mnt/boot
```

**Done!**

## Now...

Now you can plug in a USB stick (vfat) with a grub EFI bootloader located at `(stick)/boot/asloader.efi`.

You might want to add kernel parameters:

```
console=tty0 console=ttyS0,115200n8
```

And for loading the it87 kernel module:

```
acpi_enforce_resources=lax
```

ASUSTOR also uses:

```
mem=4G snd_hda_instel.bdl_pos_adj=8
```

Do you need it? `¯\_(ツ)_/¯`...

## Installing GRUB

This was in Ubuntu, I used something like:

```shell
export BOOT_DRIVE=/root/usb

mkdir $BOOT_DRIVE
mount /dev/sda2 $BOOT_DRIVE # External USB!

grub-install --target=x86_64-efi \
        --no-uefi-secure-boot \
        --efi-directory=/root/usb \
        --boot-directory=/root/usb/boot \
        --bootloader-id=asloader \
        --removable --recheck

grub-install --target=x86_64-efi \
        --no-uefi-secure-boot \
        --efi-directory=$BOOT_DRIVE \
        --boot-directory=$BOOT_DRIVE/boot \
        --removable --recheck

# Firmware looks for /boot/asloader.efi
cp $BOOT_DRIVE/EFI/BOOT/BOOTX64.EFI $BOOT_DRIVE/boot/asloader.efi
```

You can probalby omit a bunch of stuff there...

My `lsblk` looked something like this:

```console
$ lsblk -f
NAME                 FSTYPE      LABEL UUID                                   MOUNTPOINT
sda
├─sda1               vfat        EFI   67E3-17ED
└─sda2               vfat        LACIE 7970-07F1                              /root/usb
sdb
├─sdb1               vfat              7666-5203
├─sdb2               ext4              ac587fb5-5b84-437d-9c9f-5526cfc8c27e
├─sdb3               ext4              d60b4845-fe4f-4184-9e34-41ce268533e1
└─sdb4               ext4              8a91e48b-5b12-41b5-b5c0-a3393fa755ea
```

As for a GRUB entry?

```
serial --unit=0 --speed=115200 --word=8 --parity=no --stop=1
terminal_input serial
terminal_output serial

set timeout=2

menuentry 'Debian GNU/Linux' {
	set root='hd1,gpt2'
	echo	'Loading Linux 4.9.0-6-amd64 ...'
	linux	/vmlinuz-4.9.0-6-amd64 root=/dev/mapper/hostname_vg1-root ro console=tty0 console=ttyS0,115200n8 acpi_enforce_resources=lax
	echo	'Loading initial ramdisk ...'
	initrd	/initrd.img-4.9.0-6-amd64
}
```

## PS

* If you want to return to ADM, never touch the internal flash memory!
  * Just rename the `asloader.efi.disabled` back.
