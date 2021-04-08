# Creating a custom firmware for ESP-01 (512 kB)


https://docs.espressif.com/projects/esp-at/en/latest/Get_Started/index.html


Tested on Ubuntu 20.10 x86_64


1. install toolchain for target
1. install ESP-IDF
1. build ESP AT

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

Flashing a firmware

```shell
cd ~/Downloads/ESP8266-IDF-AT_V2.1.0.0/ESP8266-IDF-AT_V2.1.0.0/ESP8266-IDF-AT_V2.1/

export ESPTOOL_PORT=/dev/ttyUSB1
esptool.py flash_id

#esptool.py write_flash --flash_mode qio --flash_size 512KB  0x0 bootloader/bootloader.bin

esptool.py --chip auto --baud 115200 --before default_reset --after hard_reset write_flash -z @download.config

#esptool.py --chip auto --baud 115200  --before default_reset --after hard_reset write_flash -z --flash_mode qio --flash_freq 40m --flash_size 4MB ...
```


## ESP-IDF

https://docs.espressif.com/projects/esp-idf/en/v3.3.5/get-started/linux-setup.html

```

```




## setup toolchain for RTOS SDK

- https://docs.espressif.com/projects/esp8266-rtos-sdk/en/latest/get-started/index.html
- https://docs.espressif.com/projects/esp8266-rtos-sdk/en/latest/get-started/linux-setup.html

```shell
sudo apt-get install gcc git wget make libncurses-dev flex bison gperf python python-serial
```

### install toolchain (for ESP32)

```
mkdir -p ~/projects/hw/esp
cd ~/projects/hw/
wget ...
tar zxvf ...
# creates dir 'xtensa-lx106-elf'
# add this path + '/bin' to $PATH

sudo apt-get install python3 python3-pip python3-setuptools
```

### Get ESP-IDF

https://docs.espressif.com/projects/esp-idf/en/v3.3.5/get-started/index.html#get-started-get-esp-idf


```
mkdir -p ~/projects/hw/esp
cd ~/projects/hw/esp
git clone -b v3.3.5 --recursive https://github.com/espressif/esp-idf.git

```

Setup Path to ESP-IDF

```console
$ env | grep IDF
IDF_PATH=/home/dpc/projects/hw/esp/esp-idf
```


install the required python packages

```
cd $IDF_PATH
pip3 install --user -r $IDF_PATH/requirements.txt
```



Based on

- https://docs.espressif.com/projects/esp-at/en/latest/Compile_and_Develop/How_to_clone_project_and_compile_it.html


python3 dependencies


```shell
python3 -V # needs to be python 3.8 or greater
pip3 list --user
pip3 install --user pyyaml xlrd
```


ESP AT repository

```
mkdir -p ~/projects/hw/esp
cd ~/projects/hw/esp
git clone --recursive https://github.com/espressif/esp-at.git
cd esp-at
pip3 install --user -r esp-idf/requirements.txt
sudo apt install flex bison gperf
./build.py menuconfig

# make[4]: flex: No such file or directory

```
