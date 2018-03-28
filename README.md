# ASUSTOR AS-6XXT

This repo contains my notes and debugging data from turning an ASUSTOR AS-604T into a Debian machine (almost fully featured).

It's an unordered mess.

There be dragons.

Proceed at your own peril.

## Progress

Pretty much everything I need is working.

### What works

* Heat sensors (CPU, Motherboard)
* Fan speed adjustment
    * Via kernel module `it87-wdt` (`pwm1`)
* Toggling LEDs
    * Via kernel module `gpio-it87` (see [gpio](gpio.md) for pins)
* Adjust LED brightness
    * Via kernel module `it87-wdt` (`pwm3`)
* LCD (working on a separate project for this)
    * Display text
    * Read button input
    * Sleep

### What doesn't

* Turning of the HDD leds (via [Marvell 88SE9215](datasheets/marvell-storage-88se9215-datasheet-2016-01.pdf))
    * Don't know how to access the Marvell chip
* System buzzer (via [ITE IT8728F](datasheets/IT8728F-ITE.pdf))
* Blinking leds
    * This could be achieved relatively easily with some modifications to e.g. [tomtastic/qnap-gpio](https://github.com/tomtastic/qnap-gpio) which uses a similar ITE chip

## My setup

If you're interested.

* Boot via external USB drive
    * Contains `/boot/asloader.efi`
    * I re-purposed the 4th parititon of internal flash for `/boot`, it used to contain the stock App Central package bundle
* Debian with full disk encryption (raid-1)
    * Unlock via SSH
* A second raid-1 (scratch disk)
* RAM: 1x 4GB (ADATA AD3S1333W4G9-R)
    * Replaced the internal 1GB RAM module
    * Upgrading RAM on the system was problematic (slowed system to a crawl)
        * See [initramfs](initramfs.md) for more info

## Why did I start this?

Well, this had happened by itself:

<img width="350px" src="resources/very-sad-northbridge.jpg">

And the AS-6XXT series runs a kernel that's TOO... DAMN... OLD. It's 3.12.20 if you must know.

So, I fixed the heatsink and got to work.
