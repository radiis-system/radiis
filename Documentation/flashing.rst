.. _flashing-label:

Programming
==================================

As RadIIS contains two microcontroller (STM32L476 and ESP32) the programming via USB is a two step process. Programming of the ESP32 is managed by the STM32 as this controller provides the USB interface.  

For all steps/methods RadIIS needs to be connected via USB to your (linux) computer. You need to be able to send SCPI commands to RadIIS (see :ref:`SCPI-label`).

Simple method (flashRadIIS.py)
-----------

.. TODO: Implement/Document the flashRadIIS.py

Use the provided flashRadIIS.py utility.


Direct programming STM32
-----------

1) Make sure the SCPI communication is working e.g. by requesting the device ID via ``*IDN?`` or check for connection via ``SYS:ACK?``
2) Reset the STM32 and enable the internal bootloader via ``SYStem:BOOTLoader``
3) Flash the firmware via dfu-util: ``dfu-util -a 0 -s 0x08000000:leave -D .pioenvs/RadIIS_STM/firmware.bin``


Direct programming ESP32
-----------

1) Make sure the SCPI communication is working e.g. by requesting the device ID via ``*IDN?`` or check for connection via ``SYS:ACK?``
2) Reset the ESP32, enable its bootloader and make route the USB communication to the ESP via the single command: ``SYStem:ESP:FLASHMode``
3) Flash the firmware via platform-io: ``pio run --target upload``

Internally platform-io uses esptool which can by also used to upload the firmware. 

The internal command is:

``python3 esptool.py --chip esp32 --port "/dev/ttyACM0" --baud 115200 --before default_reset --after hard_reset write_flash -z --flash_mode dio --flash_freq 40m --flash_size detect 0x1000 /home/<USER>/.platformio/packages/framework-arduinoespressif32/tools/sdk/bin/bootloader_dio_40m.bin 0x8000 <PROJECTDIR>/.pio/build/esp32dev/partitions.bin 0xe000 /home/<USER>/.platformio/packages/framework-arduinoespressif32/tools/partitions/boot_app0.bin 0x10000 <PROJECTDIR>/.pio/build/esp32dev/firmware.bin``

See https://docs.espressif.com/projects/esptool/en/latest/esp32/esptool/flashing-firmware.html for further details.
