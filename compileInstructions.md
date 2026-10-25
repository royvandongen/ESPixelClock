This document describes how to compile modified firmware for the ESPixelClock and upload it to the clock over WiFi.

The firmware is built with [PlatformIO](https://platformio.org/) (PlatformIO Core 6.2.0 or newer) on arduino-esp32 core 3.x. `platformio.ini` in the repository root pins the platform and all library versions.

## Build

From the repository root:

```
pio run              # firmware -> .pio/build/espixelclock/firmware.bin
pio run -t buildfs   # web files in examples/ESPixelClock/data -> .pio/build/espixelclock/spiffs.bin
```

Check the `Flash:` line of the build output. The firmware must fit in the 1280 KB OTA app slot.

## Upload over WiFi

Open `http://<clock-ip>/update` in a browser:

- Select **Firmware** and upload `firmware.bin`.
- Select **Filesystem** and upload `spiffs.bin` only when the web files changed.

## Notes

- OTA cannot change the partition table. `partitions.csv` matches the layout the clock shipped with; do not change it.
- SPIFFS file names are limited to 31 characters, including the leading `/` and folders.
- The prebuilt original firmware is in `examples/ESPixelClock/compiled/`. Upload `ESPixelClock.ino.bin` through the same page to go back to it.
- If the clock is connected over USB, `pio run -t upload` and `pio run -t uploadfs` flash it directly.
