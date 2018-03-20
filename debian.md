# Debian on ASUSTOR AS-6XXT

We will use the Debian netinst ISO (booted from a USB drive).

A `preseed` is needed to automate the installation up until we can SSH into the device (check your DHCP tables!).

```
wget https://cdimage.debian.org/debian-cd/current/amd64/iso-cd/debian-9.3.0-amd64-netinst.iso

mkdir /tmp/iso
mount -o loop debian-*-netinst.iso /tmp/iso

mkdir /tmp/debian-netinst
rsync -aHv --progress /tmp/iso/ /tmp/debian-netinst/

umount /tmp/iso
rmdir /tmp/iso
rm debian-*-netinst.iso

cd /tmp/debian-netinst

# Allow the NAS to boot GRUB.
cp efi/boot/bootx64.efi boot/asloader.efi

# TODO(mafredri): Add preseed
```

Remember to update `/boot/grub/grub.cfg`:

```
serial --unit=0 --speed=115200 --word=8 --parity=no --stop=1
terminal_input serial
terminal_output serial

set timeout=2

menuentry 'Network install' {
    linux    /install.amd/vmlinuz console=ttyS0,115200n8 auto=true file=/cdrom/preseed.cfg ---
    initrd   /install.amd/initrd.gz
}
menuentry 'Rescue' {
    linux    /install.amd/vmlinuz console=ttyS0,115200n8 rescue/enable=true ---
    initrd   /install.amd/initrd.gz
}
```

**NOTE:** `/cdrom/preseed.cfg` = `(usb)/preseed.cfg`!

Complete the installation via SSH:

```
ssh install@192.168.0.201
```

TODO :)

## Full disk encryption stuff, just personal notes really.

Moving FDE from one raid to another...

```
mdadm --create --verbose /dev/md1 --level=mirror --raid-devices=2 /dev/sdf1 missing
cryptsetup luksFormat -s 512 /dev/md1
cryptsetup luksHeaderBackup /dev/md1 --header-backup-file /boot/md1-luksHeader-2018-03-12.bak
cryptsetup luksOpen /dev/md1 md1_crypt

eval "$(blkid -o export /dev/md1)"
echo "md1_crypt UUID=$UUID none luks" >> /etc/crypttab
/usr/share/mdadm/mkconf | tee /etc/mdadm/mdadm.conf

VOLGROUP=MyHost-vg1

pvcreate /dev/mapper/md1_crypt
pvmove $VOLGROUP /dev/mapper/md0_crypt
vgreduce $VOLGROUP /dev/mapper/md0_crypt
pvremove /dev/mapper/md0_crypt

sfdisk --dump /dev/sdf | sfdisk /dev/sdc
mdadm --manage /dev/md1 --add /dev/sdc1

lvextend -r /dev/mapper/$VOLGROUP-root /dev/mapper/md1_crypt

vgchange -ay
```
