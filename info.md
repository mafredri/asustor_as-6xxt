## Boot

* Boots from internal flash memory (A-DATA)
* Uses modified GRUB bootloader
* Kernel params
    * Enable the serial console (for debugging)
        * console=tty0
        * console=ttyS0,115200n8
    * Allow it87 modules to load properly
        * acpi_enforce_resources=lax
    * Used by ASUSTOR
        * mem=4G snd_hda_instel.bdl_pos_adj=8

## Linux

* Modules
    * `/etc/modprobe.d/blacklist-asustor.conf`
        * gma500_gfx
    * `/etc/modules`
        * it87
        * it87_wdt
        * gpio-it87

### LEDs

* Mostly it87 GPIOs
    * The it87 chip has two led blink settings, how to access?
        * The `qnap-leds` could be forked (kernel module)
    * Brightness controlled via FAN PWM (pwm3)
* HDD leds are controlled via the Marvell chip, dunno how to access them

### LCD (LCM)

* `/dev/ttyS1` (it87 uart)
    * Controls LCD
    * Button events
