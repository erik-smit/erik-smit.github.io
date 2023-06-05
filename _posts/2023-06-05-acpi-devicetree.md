# ACPI SSDT Overlays

Here are some notes from my playing with ACPI SSDT Overlays.  
* In the ARM world people load things with [DeviceTree overlay](https://docs.kernel.org/devicetree/overlay-notes.html).
* The equivalent in x64 is [SSDT Overlay](https://www.kernel.org/doc/html/next/admin-guide/acpi/ssdt-overlays.html).
* The kernel has some more [docs](https://kernel.org/doc/html/v6.3/driver-api/gpio/board.html) about this equivalence. 
* I needed to manually `modprobe acpi_configfs` for ` /sys/kernel/config/acpi` to appear.
* Intel has some decent examples in [meta-acpi](https://github.com/westeri/meta-acpi), like [this one](https://github.com/westeri/meta-acpi/blob/master/recipes-bsp/acpi-tables/samples/minnowboard/buttons.asl).
* Contrary to other drivres I've tried, [spi-gpio](https://github.com/torvalds/linux/blob/master/drivers/spi/spi-gpio.c#L330-L351) doesn't support SSD Overlays, because it has the devicetree behind a CONFIG_OF.
* The path to the GPIO controller on my Netgear RN516 is `\\_SB.PCI0.LPCB`.
* (re-)Loading the overlay becomes active immediately and should triggering modules loading.
* `/sys/bus/acpi/devices/PRP0001\:00/modalias` shows the modalias, `of:NspigTCspi-gpio` for instance, and this is matched with "/lib/modules/\`uname -r\`/modules.alias".
* `MODULE_DEVICE_TABLE(of, spi_gpio_dt_ids);` is a macro that ends up in filling the `lib/modules/modules.alias`.
