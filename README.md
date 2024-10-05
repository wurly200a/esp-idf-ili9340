# esp-idf-ili9340
SPI TFT and XPT2046 touch screen controller driver for esp-idf.

Please visit https://github.com/nopnop2002/esp-idf-ili9340 to see the original implementation

# Build

```
docker run --rm -it -v ${PWD}:/home/builder/esp-idf-ili9340 -w /home/builder/esp-idf-ili9340 wurly/builder_nuttx_esp32
get_idf
idf.py set-target esp32
```

```
idf.py menuconfig
```
 - TFT Configuration
  - SCREEN WIDTH -> 240
  - SCREEN HEIGHT -> 320
  - XPT2046 Touch Controller
   - (X) Enable Touch Controller using the same SPI bus as TFT

```
idf.py build
exit
```

# Write flashrom

```
python3 -m venv ./venv
. venv/bin/activate
pip3 install esptool==v4.7.0
deactivate
```

```
. venv/bin/activate
esptool.py --chip esp32 --port /dev/ttyUSB0 --baud 921600 --before default_reset erase_flash
esptool.py --chip esp32 --port /dev/ttyUSB0 -b 921600 --before default_reset --after hard_reset write_flash --flash_mode dio --flash_size 2MB --flash_freq 40m 0x1000 build/bootloader/bootloader.bin 0x8000 build/partition_table/partition-table.bin 0x10000 build/ili9340.bin 0x110000 build/storage0.bin
deactivate
```
