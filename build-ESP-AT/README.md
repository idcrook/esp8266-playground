Creating a custom firmware for ESP-01 (512 kB)
==============================================

https://docs.espressif.com/projects/esp-at/en/latest/Get_Started/index.html

Tested on **Ubuntu 20.10 x86_64**

1.	install toolchain for target
2.	install ESP-IDF / ESP8266_RTOS_SDK
3.	fetch and build ESP AT

Or

1. Download firmware from https://www.espressif.com/en/support/download/at

useful utils
------------

terminal emulator (for USB to serial)

```shell
sudo apt install minicom
sudo usermod -aG dialout $USER
sudo adduser $USER dialout
```

esptool (for flashing)

```shell
pip3 install --user esptool
```

### install toolchain (for 8266)

```
mkdir -p ~/projects/hw/esp
cd ~/projects/hw/
wget https://dl.espressif.com/dl/xtensa-lx106-elf-gcc8_4_0-esp-2020r3-linux-amd64.tar.gz
cd esp

tar xvf ../xtensa-lx106-elf-gcc8_4_0-esp-2020r3-linux-amd64.tar.gz
# creates dir 'xtensa-lx106-elf'
# add this path + '/bin' to $PATH

env | grep xtensa

sudo apt-get install python3 python3-pip python3-setuptools
```

ESP8266_RTOS_SDK
----------------

https://docs.espressif.com/projects/esp8266-rtos-sdk/en/latest/get-started/index.html#setup-toolchain

```
mkdir -p ~/projects/hw/esp
cd ~/projects/hw/esp

git clone --recursive https://github.com/espressif/ESP8266_RTOS_SDK.git

cd ESP8266_RTOS_SDK
```

Setup Path to SDK (in shell profile)

```console
$ env | grep IDF
IDF_PATH=/home/dpc/projects/hw/esp/esp-idf
```

Install the required python packages

```
cd $IDF_PATH
pip3 install --user -r $IDF_PATH/requirements.txt
```

### start a project

```shell
cd ~/projects/hw/esp
cp -r $IDF_PATH/examples/get-started/hello_world .
cd hello_world
make menuconfig
# set serial flash port
make flash
make flash ESPPORT=/dev/ttyUSB1 ESPBAUD=115200
make monitor
# reset ESP
```

Exiting the monitor: <kbd>\<Ctrl\>-]</kbd>

setup toolchain for RTOS SDK
----------------------------

-	https://docs.espressif.com/projects/esp8266-rtos-sdk/en/latest/get-started/index.html
-	https://docs.espressif.com/projects/esp8266-rtos-sdk/en/latest/get-started/linux-setup.html

```shell
sudo apt-get install gcc git wget make libncurses-dev flex bison gperf
```

Based on

-	https://github.com/espressif/esp-at/blob/master/docs/en/Compile_and_Develop/How_to_clone_project_and_compile_it.rst#esp8266-platform

explicit python dependencies

```shell
python3 -V # needs to be python 3.8 or greater
pip3 list --user
pip3 install --user pyyaml xlrd
```

ESP AT repository

```
mkdir -p ~/projects/hw/esp
git clone --recursive https://github.com/espressif/esp-at.git
cd esp-at
rm -rf build sdkconfig sdkconfig.old
# rm -rf esp-idf
sudo apt install flex bison gperf
./build.py menuconfig
./build.py build
#./build.py flash
```

output gives flash command

```
esptool.py -p (PORT) -b 460800 --after hard_reset write_flash --flash_mode dio --flash_size 4MB --flash_freq 80m 0x0 build/bootloader/bootloader.bin 0x8000 build/partition_table/partition-table.bin 0x9000 build/ota_data_initial.bin 0xf000 build/phy_init_data.bin 0x18000 build/at_customize.bin 0x19000 build/customized_partitions/factory_param.bin 0x1A000 build/customized_partitions/client_cert.bin 0x1B000 build/customized_partitions/client_key.bin 0x1C000 build/customized_partitions/client_ca.bin 0x1D000 build/customized_partitions/mqtt_cert.bin 0x1E000 build/customized_partitions/mqtt_key.bin 0x1F000 build/customized_partitions/mqtt_ca.bin 0x20000 build/esp-at.bin
```

put that into a file to run ranually (e.g. `download.config`\)

```
cd ~/projects/hw/esp/esp-at
export ESPTOOL_PORT=/dev/ttyUSB1
export ESPTOOL_BAUD=460800
esptool.py --chip auto --before default_reset --after hard_reset write_flash @download.config
```

in a window for serial connection


```shell
minicom -D /dev/ttyUSB1 -b 115200
minicom -D /dev/ttyUSB1 --noinit -b 115200
```

reconfigure

https://docs.espressif.com/projects/esp-at/en/latest/Compile_and_Develop/How_to_set_AT_port_pin.html#esp8266-at

```console
$ grep 115200,1,3 /home/dpc/projects/hw/esp/esp-at/components/customized_partitions/raw_data/factory_param/factory_param_data.csv
PLATFORM_ESP8266,WROOM-02-N,TX:1 RX:3,0xfcfc,3,0,78,0,1,13,CN,115200,1,3,-1,-1,-1,-1
$ rm sdkconfig sdkconfig.old
$ rm -rf build esp-idf
$ ./build.py menuconfig
```

Select `ESP8266` and `WROOM-02-N` so that ESP-01 will choose right AT UART pins.

also, can go with 2MB

```diff
diff --git a/components/customized_partitions/raw_data/factory_param/factory_param_data.csv b/components/customized_partitions/raw_data/factory_param/fa>
index 905275d..2a88525 100644
--- a/components/customized_partitions/raw_data/factory_param/factory_param_data.csv
+++ b/components/customized_partitions/raw_data/factory_param/factory_param_data.csv
@@ -7,7 +7,7 @@ PLATFORM_ESP32,MINI-1,ESP32-U4WDH chip inside,0xfcfc,3,0,78,1,1,13,CN,115200,22,
 PLATFORM_ESP32,ESP32-D2WD,"2MB flash, No OTA",0xfcfc,3,0,78,1,1,13,CN,115200,22,19,15,14,-1,-1
 PLATFORM_ESP8266,WROOM-02,TX:15 RX:13,0xfcfc,3,0,78,0,1,13,CN,115200,15,13,3,1,-1,-1
 PLATFORM_ESP8266,WROOM-5V2L,5V UART level,0xfcfc,3,0,78,0,1,13,CN,115200,15,13,3,1,5,-1
-PLATFORM_ESP8266,ESP8266_1MB,No OTA,0xfcfc,3,0,78,0,1,13,CN,115200,15,13,3,1,-1,-1
+PLATFORM_ESP8266,ESP8266_1MB,No OTA,0xfcfc,3,0,78,0,1,13,CN,115200,1,3,-1,-1,-1,-1^M
 PLATFORM_ESP8266,WROOM-02-N,TX:1 RX:3,0xfcfc,3,0,78,0,1,13,CN,115200,1,3,-1,-1,-1,-1
 PLATFORM_ESP8266,WROOM-S2,,0xfcfc,3,0,78,0,1,13,CN,115200,15,13,3,1,-1,-1
 PLATFORM_ESP32S2,WROOM,,0xfcfc,3,0,78,1,1,13,CN,115200,17,21,20,19,-1,-1
```


```
./build.py menuconfig
./build.py build
./build.py flash
./build.py monitor
```

AT commands test

```shell
sudo apt install picocom
picocom --baud 115200 --omap crcrlf /dev/ttyUSB1

```
Exiting `picocom`: <kbd>C-a C-x</kbd>


```shell
# DOES NOT WORK # stty -F /dev/ttyUSB1 -icrnl
screen /dev/ttyUSB1 115200
```

Exiting `picocom`: <kbd>C-a :quit</kbd>

# AT Commands


```
AT+GMR
AT+RST
AT+CWMODE?
AT+CWMODE=1
AT+CWLAP
AT+CIPSTAMAC?
AT+CWSTATE?
```
