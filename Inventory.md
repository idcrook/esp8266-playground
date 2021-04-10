Inventory of Espressif-related gear
===================================

| name      | desc                       | flash  | mfg | chip         | MAC     |
|-----------|----------------------------|--------|-----|--------------|---------|
| ESP-01    | blue                       | 512 KB | c8  | ESP8266EX    | %:c4:4e |
| ESP-01    | blue                       | 512 KB | c8  | ESP8266EX    | %:c7:20 |
| ESP-01S   | black                      | 4 MB   | 68  | ESP8266EX    | %:7d:ee |
| Wifi i2c  | Pine64 module, with relay  | 1 MB   | e0  | ESP8266EX    | %:82:a2 |
| Wifi i2c  | Pine64 module, with relay  | 1 MB   | e0  | ESP8266EX    | %:31:17 |
| ESP-03    | soldered in hb0042 w/ OLED | 1 MB   | 85  | ESP8266EX    | %:29:75 |
| ESP32-CAM | incl. 2MP camera           | 4 MB   | 20  | ESP32-D0WDQ6 | %:eb:54 |

See [hb0042_portable_wifi_scanner](hb0042_portable_wifi_scanner#readme) for pointers on board ESP-03 is soldered into.

See [ESP32-CAM README](README-ESP32-CAM.md) for additional details on this ESP32-based module

useful commands

```shell
esptool.py --port /dev/ttyUSB1 --no-stub --after no_reset flash_id
esptool.py --port /dev/ttyUSB1 --no-stub --after no_reset chip_id
esptool.py --port /dev/ttyUSB2 --no-stub --after no_reset read_mac
```

The `--after no_reset` only has an effect if RTS pin is available/routed.

Related sockets

| name                 | desc                  | qty | link                                               |
|----------------------|-----------------------|-----|----------------------------------------------------|
| Breadboard Adapter   | nRF24L01+ and ESP8266 | 7   | https://www.addicore.com/bb-adtr-p/ad-bb-adtr.htm  |
| ESP-01S DS18B20 v1.0 | Temp. sensor incl. VR | 1   | https://github.com/IOT-MCU/ESP-01-01S-DS18B20-v1.0 |

USB to UART

| name                    | desc                                   | qty | chipset | other     | link                                                             |
|-------------------------|----------------------------------------|-----|---------|-----------|------------------------------------------------------------------|
| PMPROG01                | PINE A64 USB SERIAL CONSOLE/PROGRAMMER | 1   | cp210x  |           | https://pine64.com/product/pine64-usb-serial-console-programmer/ |
| USB to TTL Serial Cable | Console Cable for Raspberry Pi         | 1   | PL2303  | 5 Vcc     | https://www.adafruit.com/product/954                             |
| FTDI TTL-232R-3V3       | USB to UART cable (3.3V UART)          | 2   | FT232RL | 5 Vcc     | https://ftdichip.com/products/ttl-232r-3v3/                      |
| FTDI USB to UART        | USB to UART cable (3.3V UART)          | 1   | FT232RL | 5/3.3 Vcc | https://www.instructables.com/HackerBox-0043-Falkens-Maze/       |
|                         |                                        |     |         |           |                                                                  |

**ESP-01S DS18B20 v1.0**

-	Temperature sensor: DS18B20
-	Operating voltage: DC 3.7V-12V (support 3.7v lithium battery power supply)
-	Measuring range: -55 ° C to + 125 ° C
-	Accuracy: ± 0.5 ° C

Insert ESP-01 / ESP-01S into the 2 * 4 pin (in the direction of the arrow) (so that it overhangs PCB)
