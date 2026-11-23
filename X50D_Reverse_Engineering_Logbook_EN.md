# CocktailAudio X50D Reverse Engineering Logbook

## Project Overview
Reverse engineering of the CocktailAudio X50D firmware and hardware platform.

---

# Hardware Findings

## CPU
Confirmed:
- NXP / Freescale i.MX6 Dual
- Part: MCIMX6D5EYM10AD
- Dual Cortex-A9 @ 1 GHz

## Memory
Confirmed:
- Samsung K4B4G1646E-BCNB DDR3
- Hynix H5TQ4G63AFR DDR3
- Total RAM very likely 1 GB

## eMMC Storage
Confirmed:
- Samsung KLM8G1GEMD-B031
- 8 GB eMMC

## SPI NOR Flash
Likely:
- Macronix MX25L512xx family
- Used for U-Boot and boot infrastructure

---

# Firmware Analysis

## Firmware Package
Firmware:
- X50D-CA-1.0.0.r1721.pkg

Container:
- ROMFS filesystem
- Internal package name: X20_PKG

## Root Filesystem
Successfully extracted.

Contains:
- /boot
- /etc
- /usr
- /var
- /home/root/bin
- complete Linux root filesystem

---

# Linux Platform

## Kernel
Found:
- zImage-3.10.53-1.1.0_ga+g4742c51
- vmlinux-3.10.53-1.1.0_ga+g4742c51

System banner:
- Cocktail Audio Linux 3.14.52

Likely based on Freescale BSP 3.14.28.

## Freescale/NXP Drivers Found
Examples:
- mxc_hdmi
- mxc_lcdif
- mxc_sdc_fb
- mxc-ehci
- gpio-mxc

Confirms:
- i.MX6 platform
- HDMI support
- LCD/LVDS support
- USB host support

---

# U-Boot Analysis

## Version
Confirmed:
- U-Boot 2014.04
- Build date: Nov 12 2020

## Boot Variables
Found inside u-boot.imx:

```text
image=zImage-N25
fdt_file=imx6q-n25.dtb
console=ttymxc0
baudrate=115200
```

## Boot Sequence

```text
SPI NOR
  -> U-Boot
      -> zImage-N25
      -> imx6q-n25.dtb
      -> Linux
      -> RootFS on eMMC
```

---

# Device Tree Findings

The actual DTB file was not extracted.

However U-Boot explicitly references:

```text
imx6q-n25.dtb
```

Confirmed board identifier:

```text
N25
```

---

# UART Findings

## UART0
Confirmed from U-Boot and kernel:

```text
console=ttymxc0,115200
```

Purpose:
- Kernel console
- U-Boot console
- Debug console

## UART1
Found in application:

```text
/dev/ttymxc1
```

Likely used by:
- FM/DAB subsystem
- Auxiliary controller

---

# Display Subsystem

Confirmed from U-Boot:

```text
video=mxcfb0:dev=ldb,LDB-XGA,800x480M@51
```

Display:
- 800 x 480
- LVDS/LDB connection

HDMI:

```text
video=mxcfb1:dev=hdmi,1280x720M@60
```

Default HDMI mode:
- 1280x720 @ 60 Hz

---

# Main Application

Binary:

```text
/home/root/bin/x20
```

Properties:
- ARM ELF
- Qt5 based
- Contains debug information
- Not stripped

Internal source path:

```text
/home/dev/IMX6_src_n25/x20_gp
```

---

# XMOS Audio Processor

Confirmed references:

```text
XMOS DSD Audio
blInternalXmos
xmosdfu
```

Capabilities found:
- DSD support
- XMOS firmware update support

---

# SPDIF

Application contains:

```text
AUD_SPDIF
AUD_SPDIF_OUTPUT
```

Confirms active SPDIF control in software.

---

# FM / DAB Subsystem

Classes found:

```text
QAbstractFmDabSerialPort
QKseRadioSerialPort
QPnpRadioSerialPort
```

Functions found:

```text
ReqTuneSeek
SetDabChannel
ReqTuneDabScan
ReqChangeFreq
```

Conclusion:
- Separate FM/DAB subsystem
- Communicates via serial port
- Controlled by Qt SerialPort framework

---

# Roon / RAAT

Found inside x20:

```text
raat_plugin_transport
supports_native_dsd
max_dsd_rate = 256
```

Detected support:
- PCM up to 192 kHz / 24 bit
- DSD64
- DSD128
- DSD256

---

# GPIO Findings

Micom script:

```text
GPIO104
```

Used through:

```text
/sys/class/gpio/export
```

Application also contains GPIO helper classes.

---

# Current System Architecture

```text
i.MX6 Dual
│
├── 1 GB DDR3
├── Samsung eMMC 8 GB
├── SPI NOR (MX25L family)
├── LVDS Display (800x480)
├── HDMI Output
├── XMOS Audio Processor
├── FM/DAB UART Module
└── Linux + Qt5 Application (x20)
```

---

# Open Items

Still missing:
- Physical extraction of imx6q-n25.dtb
- Exact XMOS chip identification
- UART pin location on PCB
- Full eMMC partition map
- Verification of SPI flash contents

# Assessment

The X50D appears to be a fairly standard NXP i.MX6 embedded Linux platform.

No evidence of Secure Boot, firmware encryption, or platform lockdown has been found so far.

An open firmware or community Linux project appears technically feasible.
