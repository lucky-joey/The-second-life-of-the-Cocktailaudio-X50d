# Storage Layout

## Overview

Based on firmware extraction, U-Boot environment analysis, and application binaries, the CocktailAudio X50D uses an eMMC-based Linux storage architecture with a separate bootloader.

---

## Confirmed Components

### SPI NOR Flash

Probable device:

```text
MX25L512xx (Macronix)
```

Likely stores:

```text
U-Boot
Environment variables
Recovery configuration
```

Evidence:

```text
u-boot.imx.n25.202011121649
```

found in firmware update package.

---

### eMMC

Confirmed device:

```text
Samsung KLM8G1GEMD-B031
```

Estimated capacity:

```text
8 GB
```

Stores:

```text
Kernel
Device Tree
Root filesystem
Application binaries
Configuration
Media database
```

---

# Boot Process

Recovered from U-Boot:

```text
image=zImage-N25
fdt_file=imx6q-n25.dtb
mmcdev=1
mmcpart=1
```

Boot chain:

```text
SPI NOR
   |
   +--> U-Boot 2014.04
            |
            +--> eMMC Device 1
                     |
                     +--> zImage-N25
                     +--> imx6q-n25.dtb
                     +--> Linux 3.10.53
                     +--> RootFS
```

---

# Known Partitions

## U-Boot References

```text
mmcpart=1
```

Boot partition:

```text
mmc device 1
partition 1
```

contains:

```text
zImage-N25
imx6q-n25.dtb
boot.scr (optional)
```

---

## Root Filesystem

Recovered from U-Boot:

```text
mmcroot=/dev/mmcblk2p3 rootwait rw
```

Indicates:

```text
Partition 3
contains Linux root filesystem
```

---

## Additional References Found

Application strings contain:

```text
/dev/mmcblk3p1
/dev/mmcblk3p2
/dev/mmcblk3p3
```

This suggests:

- different kernel numbering between releases, or
- multiple MMC devices present, or
- historical migration between BSP versions.

Status:

```text
Not yet verified on running hardware.
```

---

# Proposed Partition Map

Current best reconstruction:

```text
eMMC
│
├── p1  FAT Boot Partition
│      ├── zImage-N25
│      ├── imx6q-n25.dtb
│      └── boot scripts
│
├── p2  System/Data Partition
│      └── unknown
│
└── p3  Linux RootFS
       ├── /boot
       ├── /etc
       ├── /usr
       ├── /home
       ├── x20
       └── application data
```

---

# RootFS Contents

Recovered from firmware:

```text
/app
/bin
/boot
/dev
/etc
/home
/lib
/media
/mnt
/opt
/proc
/run
/sbin
/sys
/tmp
/usr
/var
```

Special files:

```text
version
mmcblk3p3
```

---

# Firmware Package Layout

Package:

```text
X50D-CA-1.0.0.r1721.pkg
```

Format:

```text
ROMFS
```

Internal package name:

```text
X20_PKG
```

Recovered artifacts:

```text
Linux RootFS
Kernel image
vmlinux image
U-Boot image
GUI resources
Audio resources
Database files
```

---

# Outstanding Questions

## Missing Device Tree

Known:

```text
imx6q-n25.dtb
```

Status:

```text
Filename confirmed
File not yet extracted
```

## Exact Partition Table

Need verification from:

```text
lsblk
fdisk -l
cat /proc/cmdline
```

on running hardware.

## SPI NOR Usage

Still needs confirmation whether:

```text
Kernel boots from SPI NOR
```

or

```text
Only U-Boot resides in SPI NOR
```

---

# Confidence Levels

| Item | Confidence |
|--------|------------|
| eMMC stores RootFS | High |
| eMMC stores Kernel | High |
| eMMC stores DTB | High |
| SPI NOR contains U-Boot | High |
| RootFS on p3 | High |
| Boot partition on p1 | High |
| Exact partition numbering | Medium |
| Full partition table | Pending |
