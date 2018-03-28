# initramfs

## Fix occasional *slow CPU*

This is a super dirty work-around for occasional "slow CPU" during boot.

Reason is unknown, but CPU slows to a crawl. This would happen always when I tried any type of RAM upgrade, the only thing that worked was replacing the internal 1GB module with a 2GB module.

After I got my 4GB module, sometimes when I `reboot`, the system is sloooow. Well, a reboot (again) fixes it, so voilá.

```console
$ cat /etc/initramfs-tools/scripts/init-top/reboot_on_slow_cpu
#!/bin/sh

PREREQ=""

prereqs() {
	echo "$PREREQ"
}

case $1 in
prereqs)
	prereqs
	exit 0
	;;
esac

bootlimit=12
boottime="$(cat /proc/uptime | cut -d. -f1)"
if [ "$boottime" -gt "$bootlimit" ]; then
	echo "!!! ERROR: SLOW CPU DETECTED !!!"
	echo "Boot took: $boottime (max $bootlimit)"
	echo "Immediate magic reboot imminent..."
	sleep 1
	echo b > /proc/sysrq-trigger
fi

exit 0
```

Add the script, `chmod +x /etc/initramfs-tools/scripts/init-top/reboot_on_slow_cpu` and `update-initramfs -u`.
