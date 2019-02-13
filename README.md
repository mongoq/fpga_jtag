# fpga jtag over wlan

**ESP201 - JTAG over WLAN**

Follow instructions on https://github.com/emard/wifi_jtag to prepare ESP201

TODO: Create Altera SVF-File - https://www.intel.com/content/www/us/en/programmable/support/support-resources/knowledge-base/solutions/rd07222008_677.html

---

![ESP201a](https://www.mikrocontroller.net/attachment/307865/Flashing-The-ESP8266-ESP201-Module-Board-With-TTL-UART.jpg)

---

![ESP201b](https://www.mikrocontroller.net/attachment/307864/esp8266_esp_201_module_pinout_diagram_cheat_sheet_by_adlerweb-d9iwmqp.jpg
)

---


Second: JTAG to TB276 Board pinout

![ESP201b](https://github.com/emard/wifi_jtag/raw/master/pic/altera10pin_xilinx14pin.jpg)

Pinout Mapping ESP201 to JTAG Pins: https://github.com/emard/wifi_jtag (!!!) 

---

**JTAG over Raspberry Pi**

https://github.com/synthetos/PiOCD/wiki/Using-a-Raspberry-Pi-as-a-JTAG-Dongle
https://sourceforge.net/p/urjtag/discussion/682993/thread/d31f1840/
https://www.mikrocontroller.net/articles/Raspberry_Pi_als_Universalprogrammer

---

Generate .svf file: https://www.intel.com/content/www/us/en/programmable/support/support-resources/knowledge-base/solutions/rd07222008_677.html

Flash this: https://github.com/emard/wifi_jtag/blob/master/arduino/jtag_wifi_serial.ino

http://www.nxlab.fer.hr/fpgarduino/linux_bitstreams.html ?!?!?

https://github.com/emard/LibXSVF-ESP

---

Get this to work with WLAN flashing - https://github.com/mfischer/Altera-Makefile

---

OpenOCD Installation:

install libftdi for usb blaster (apt-get install ...)

git clone http://repo.or.cz/r/openocd.git ?! https://github.com/ntfreak/openocd

./bootstrap

./configure --enable-usb-blaster --enable-remote-bitbang

make

make install

---

In der Datei *openocd.cfg*: (TODO: IP/jtag.lan)?

interface remote_bitbang

remote_bitbang_port 3335

remote_bitbang_host jtag.lan

jtag newtap tb276 tap -expected-id 0x020f10dd -irlen 10

init

scan_chain

svf -tap tb276.tap project.svf

shutdown

---

Precompiled binary is available in bin/

Default SSID is "jtag" and password "12345678" 

---

TO convert sof to svf (TODO) - https://www.intel.co.jp/content/dam/altera-www/global/ja_JP/pdfs/literature/manual/tclscriptrefmnl.pdf

---

http://www.freddiechopin.info/en/download/category/4-openocd (Windows OpenOCD)

---

Precompiled Binaries at /bin ...

esptool -vv -cd ck -cb 115200 -cp /dev/ttyUSB0 -ca 0x00000 -cf jtag_wifi_serial.cpp.bin

---

https://oliverlorenz.com/blog/esp8266-firmware-unter-ubuntu-aufspielen/

--> sudo esptool.py --port /dev/ttyUSB3 write_flash 0x00000 jtag_wifi_serial.cpp.bin

---

Flash Vorgang

zuerst: "Unverfänglicher Verbindungstest"

```
sudo esptool.py -p /dev/ttyUSB3 chip_id

esptool.py v1.3

Connecting....

Chip ID: 0x001c1495
```

Dann:

````
sudo esptool.py -p /dev/ttyUSB3 erase_flash 

esptool.py v1.3

Connecting....

Running Cesanta flasher stub...

Erasing flash (this may take a while)...

Erase took 2.8 seconds

````

Und zum Flashen schließlich:

````
sudo esptool.py -p /dev/ttyUSB3 write_flash 0x00000 jtag_wifi_serial.cpp.bin

esptool.py v1.3

Connecting....

Auto-detected Flash size: 8m

Running Cesanta flasher stub...

Flash params set to 0x0020

Wrote 303104 bytes at 0x0 in 26.3 seconds (92.3 kbit/s)...

Leaving...
````
GND-Verbindung zu IO-0 trennen (s.o.) ... sonst weiterhin im Flash Modus ... also nach neu Booten sollte AP mit jtag zu finden sein ...

---

http://androegg.de/?p=242 --> Stromversorgung nicht über USB-TTL-Wandler
 
