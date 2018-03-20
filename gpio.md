GPIO ports of the LEDs (on the it87 chip):

```
20 power red led
34 usb led
45 panel1 power
47 status led
77 lan led
80 power blue led
83 lcd power
```

Contents of `/proc/asled`:

```
######################################################
LED 0 Mode = 0x00000001
LED 0 Flag = 0x00000001
LED 0 Status = 0x00000000
LED 0 pActivity = 0xFFFFFFFF81C2E880
LED 0 HardwareType = 0x00000001
LED 0 Gpio = 0x00020000
LED 0 ActivityMsecs = 0x00000032
LED 0 BlinkMsecs = 0x00000032
######################################################
LED 1 Mode = 0x00000000
LED 1 Flag = 0x00000000
LED 1 Status = 0x00000000
LED 1 pActivity = 0x00000000
LED 1 HardwareType = 0x00000001
LED 1 Gpio = 0x00080000
LED 1 ActivityMsecs = 0x00000032
LED 1 BlinkMsecs = 0x00000032
######################################################
LED 2 Mode = 0x00000001
LED 2 Flag = 0x00000001
LED 2 Status = 0x00000000
LED 2 pActivity = 0xFFFFFFFF81C2E8A0
LED 2 HardwareType = 0x00000001
LED 2 Gpio = 0x00200000
LED 2 ActivityMsecs = 0x00000032
LED 2 BlinkMsecs = 0x00000032
######################################################
LED 3 Mode = 0x00000000
LED 3 Flag = 0x00000000
LED 3 Status = 0x00000000
LED 3 pActivity = 0x00000000
LED 3 HardwareType = 0x00000001
LED 3 Gpio = 0x00400000
LED 3 ActivityMsecs = 0x00000032
LED 3 BlinkMsecs = 0x00000032
######################################################
LED 4 Mode = 0x00000001
LED 4 Flag = 0x00000001
LED 4 Status = 0x00000000
LED 4 pActivity = 0xFFFFFFFF81C2E8C0
LED 4 HardwareType = 0x00000001
LED 4 Gpio = 0x00000020
LED 4 ActivityMsecs = 0x00000032
LED 4 BlinkMsecs = 0x00000032
######################################################
LED 5 Mode = 0x00000000
LED 5 Flag = 0x00000000
LED 5 Status = 0x00000000
LED 5 pActivity = 0x00000000
LED 5 HardwareType = 0x00000001
LED 5 Gpio = 0x00000040
LED 5 ActivityMsecs = 0x00000032
LED 5 BlinkMsecs = 0x00000032
######################################################
LED 6 Mode = 0x00000001
LED 6 Flag = 0x00000001
LED 6 Status = 0x00000000
LED 6 pActivity = 0xFFFFFFFF81C2E8E0
LED 6 HardwareType = 0x00000001
LED 6 Gpio = 0x00000080
LED 6 ActivityMsecs = 0x00000032
LED 6 BlinkMsecs = 0x00000032
######################################################
LED 7 Mode = 0x00000000
LED 7 Flag = 0x00000000
LED 7 Status = 0x00000000
LED 7 pActivity = 0x00000000
LED 7 HardwareType = 0x00000001
LED 7 Gpio = 0x00010000
LED 7 ActivityMsecs = 0x00000032
LED 7 BlinkMsecs = 0x00000032
```
