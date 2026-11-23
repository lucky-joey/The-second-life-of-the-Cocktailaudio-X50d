# OpenX50D Architecture

## System Architecture Diagram

```text
                                   OpenX50D
                         CocktailAudio X50D Architecture


                     ┌─────────────────────────────┐
                     │      NXP i.MX6 Dual         │
                     │    MCIMX6D5EYM10AD          │
                     │      Dual Cortex-A9         │
                     └─────────────┬───────────────┘
                                   │
          ┌────────────────────────┼────────────────────────┐
          │                        │                        │
          ▼                        ▼                        ▼

 ┌────────────────┐     ┌─────────────────┐     ┌─────────────────┐
 │  DDR3 RAM      │     │ Samsung eMMC    │     │ SPI NOR Flash   │
 │ ~1 GB          │     │ 8 GB            │     │ MX25L512xx (?)  │
 └────────────────┘     └────────┬────────┘     └────────┬────────┘
                                 │                       │
                                 ▼                       ▼

                        ┌─────────────────┐    ┌─────────────────┐
                        │ Linux 3.10.53   │    │ U-Boot 2014.04  │
                        │ zImage-N25      │    │ Nov 2020 Build  │
                        └────────┬────────┘    └────────┬────────┘
                                 │                      │
                                 └──────────┬───────────┘
                                            │

                              ┌─────────────▼─────────────┐
                              │      imx6q-n25.dtb       │
                              │      (not extracted)      │
                              └─────────────┬─────────────┘
                                            │

                                    ┌───────▼───────┐
                                    │    RootFS     │
                                    │  X20 Platform │
                                    └───────┬───────┘
                                            │
                     ┌──────────────────────┼──────────────────────┐
                     │                      │                      │
                     ▼                      ▼                      ▼

           ┌────────────────┐   ┌────────────────┐    ┌────────────────┐
           │ x20 Application│   │ Qt5 GUI Stack  │    │ RAAT / Roon    │
           │ 93 MB          │   │ QtSerialPort   │    │ DSD Support    │
           └────────┬───────┘   └────────────────┘    └────────────────┘
                    │
     ┌──────────────┼─────────────────────┬─────────────────┐
     │              │                     │                 │
     ▼              ▼                     ▼                 ▼

┌───────────┐  ┌──────────────┐  ┌────────────────┐  ┌─────────────┐
│ XMOS      │  │ DAB / FM     │  │ LVDS Display   │  │ HDMI Output │
│ DSD Audio │  │ UART Module  │  │ 800x480        │  │ 1280x720    │
└─────┬─────┘  └──────┬───────┘  └────────────────┘  └─────────────┘
      │               │
      ▼               ▼

  XMOS DFU      /dev/ttymxc1
  Internal      QKseRadioSerialPort
  XMOS FW       QPnpRadioSerialPort


UARTs discovered:

  ttymxc0  --> U-Boot + Linux Console (115200 Baud)
  ttymxc1  --> DAB/FM Radio Subsystem
```

## Boot Flow

```text
Power ON
    │
    ▼
SPI NOR Flash
    │
    ▼
U-Boot 2014.04
    │
    ▼
Load zImage-N25
    │
    ▼
Load imx6q-n25.dtb
    │
    ▼
Boot Linux 3.10.53
    │
    ▼
Mount RootFS (/dev/mmcblkXp3)
    │
    ▼
Start X20 Application
    │
    ├── XMOS Audio
    ├── DAB/FM Tuner
    ├── HDMI
    ├── LVDS Display
    └── Roon RAAT
```
