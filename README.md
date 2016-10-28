Leostick bootloader v2.1-duckie
===============================

Patched with shorter boot timeout to be more useful as a rubber duckie.


Instructions
------------

Those instructions are for macOS.

Download ArduinoISP project from https://code.google.com/archive/p/mega-isp/downloads and apply this patch:
```
diff --git a/ArduinoISP.pde b/ArduinoISP.pde
index 899a4cc..b1f411f 100644
--- a/ArduinoISP.pde
+++ b/ArduinoISP.pde
@@ -99,7 +99,7 @@ void heartbeat() {
   if (hbval < 32) hbdelta = -hbdelta;
   hbval += hbdelta;
   analogWrite(LED_HB, hbval);
-  delay(40);
+  delay(20);
 }
```
otherwise avrdude will report either of those messages and programming will fail
```
avrdude: stk500_recv(): programmer is not responding
avrdude: stk500_cmd(): programmer is out of sync
```

Connect Arduino UNO and upload this sketch.

With Leostick wired as shown [in the official tutorial](http://www.freetronics.com.au/pages/using-arduino-compatible-boards-as-external-programmers), quit Arduino IDE and reset Arduino with the onboard switch.

Detect what port Arduino is connected on with `ls /dev/cu.usbmodem*` (eg `/dev/cu.usbmodem371`).

`$HEX_FILE` is file to built bootloader, such as `Caterina-LeoStick.hex`.

Flash Leostick bootloader with this command:

```
avrdude -cavrisp -b19200 -v -patmega32u4
	-P $PORT\
	-e \
	-Ulock:w:0x3F:m \
	-Uefuse:w:0xcb:m \
	-Uhfuse:w:0xd8:m \
	-Ulfuse:w:0xff:m \
	-Uflash:w:$HEX_FILE:i \
	-Ulock:w:0x2F:m
```
