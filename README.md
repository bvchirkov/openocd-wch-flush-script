# OpenOCD Config for WCH RISC-V Microcontrollers (CH32V2xx)

This config extends the default configuration with features like:
- disabling MCU code protection before programming
- enabling protection again afterward

## Reasons

- MounRiver Studio versions below 2 cannot automatically disable flash protection.
- WCH-LinkUtility is not available on Linux-based operating systems.

## Requirements

- OpenOCD version with WCH RISC-V support
- WCH-Link adapter

## How to use

There are two ways to use this configuration.

### 1. As the Default Configuration

Replace the default OpenOCD configuration.

(If you're using **MounRiver Studio**, the path to the default configuration file is: `/MRS_Community/toolchain/OpenOCD/bin/`)

To access the custom lock and unlock functions, set up the IDE as shown in the image below.

![](https://github.com/bvchirkov/imgs/blob/main/MRS_RunConfigurations_Ext.png)

### 2. As an External Configuration

Create a bash-script for use without an IDE.

``` c
#!/bin/bash
OPENOCD_PATH=<path/to/wch/openocd/bin>
CFG_PATH=<path/to/configuration>
$OPENOCD_PATH/openocd \
  -f $CFG_PATH/wch-riscv.cfg \ # Choosing custom configuration file
  -c "init" \
  -c "unlock" \                # Releasing protection if active
  -c "program $1 verify" \     # Flashing firmware
  -c "lock" \                  # Enabling protection
  -c "reset" \
  -c "exit"
```

The script argument should be either a `hex` or `elf` firmware file.

## Reference

Original article on Habr: [Использование OpenOCD для установки/снятия запрета чтения памяти CH32V20x](https://habr.com/ru/articles/864344/)
