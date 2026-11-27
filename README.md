# MStar Collect FPGA

English | [简体中文](README.zh-CN.md)

An open-source multi-channel data acquisition reference design for Anlogic PH1 FPGAs, built with the Tang Dynasty IDE.

## Overview

This project targets an 8-channel AD data acquisition system. Sampled data is buffered in DDR through DMA and streamed to a host PC over gigabit Ethernet (RGMII) using UDP. Key features:

- **8-channel AD acquisition**: ADC drivers for CM2268 and similar chips
- **DDR buffering**: acquisition data cached in DDR via AXI DMA
- **Gigabit Ethernet output**: UDP/IP stack over RGMII
- **Trigger control**: external trigger input and output
- **AXI Lite register interface**: host-configurable acquisition parameters

## Hardware

[![Data acquisition board](hdl/collect_picture.png)](https://magicstar.taobao.com/)

> Photo not loading? [View the full-size image on Gitee](https://gitee.com/mstar_ypc/mstar-collect-fpga/raw/master/hdl/collect_picture.png).

## Related Projects

- [MStar Oscilloscope](https://gitee.com/mstar_ypc/mstar-oscilloscope) — companion host-side waveform viewer for this FPGA design
- Gitee main repository: [https://gitee.com/mstar_ypc/mstar-collect-fpga](https://gitee.com/mstar_ypc/mstar-collect-fpga)

## Repository Structure

```
tand_udp/
├── hdl/
│   ├── mstar_udp_top.v          # Top-level module
│   ├── ad_driver/               # ADC chip drivers
│   │   ├── adchip_ctrl.v
│   │   ├── cm2268_drv.v
│   │   └── driver_cm2268.v
│   ├── collect/                 # Acquisition logic
│   │   ├── mstar_collect.v
│   │   ├── Mstar_collect_ctl.v
│   │   ├── mstar_collectdma.v
│   │   ├── ad_collect_trig_ctrl.v
│   │   ├── ad_dma_axictrl.v
│   │   ├── get_extreflg.v
│   │   ├── onedmafifo_con.v
│   │   └── triggel.v
│   ├── axi_dma/                 # AXI DMA read/write masters
│   │   ├── axi_dma_example_axi_read_master.sv
│   │   ├── axi_dma_example_axi_write_master.sv
│   │   └── axi_dma_example_counter.sv
│   ├── axi_io/                  # AXI Lite register interface
│   │   ├── mstar_axifulltopaxi.v
│   │   └── mstar_axilite_io.v
│   ├── udpip/                   # Ethernet bridge IP (encrypted)
│   │   └── mstar_ethernet_bridge.ipr.v
│   ├── sync/                    # Clock-domain crossing and reset sync
│   │   ├── sync_block.v
│   │   ├── reset_sync.v
│   │   ├── resetn_sync.v
│   │   └── get_singer_edge.v
│   └── constraint/              # Constraint files
│       ├── mstar_udp_adc.adc    # Pin constraints
│       └── mstar_udp_sdc.sdc    # Timing constraints
├── al_ip/                       # Anlogic IP cores
│   ├── MIS_DDR/                 # DDR controller
│   ├── AXIfull_Matrix/          # AXI Full interconnect
│   ├── AXI_lite_Matrix/         # AXI Lite interconnect
│   ├── AXI_Clock_Converter_ddr/ # AXI clock-domain converter
│   ├── axis_dmadat_fifo/        # DMA data FIFO
│   ├── axis_mac_rxfifo/         # MAC receive FIFO
│   ├── axis_macdat_fifo/        # MAC data FIFO
│   ├── fifo_axis_parkenumber/   # Packet counter FIFO
│   ├── fifo_axis_rullcmd/       # Command FIFO
│   └── sysPLL/                  # System PLL
├── chip_debugger/               # Debug configuration
├── tand_udp.al                  # Tang Dynasty project file
└── tand_udp_devicesetting.cfg   # Device settings
```

## Target Device

- **FPGA**: Anlogic PH1P35MDG324-3
- **IDE**: Tang Dynasty ≥ 6.2

## Build

1. Install the Tang Dynasty IDE (≥ 6.2)
2. Open the `tand_udp.al` project file
3. Confirm the target device is set to PH1P35MDG324-3
4. Run synthesis → place & route → generate bitstream

## Encrypted IP Notice

`hdl/udpip/mstar_ethernet_bridge.ipr.v` is a pre-synthesized encrypted netlist implementing the Ethernet bridge (MAC layer, UDP/IP stack, AXI bus interface, etc.). The IP is distributed in encrypted form: the interface is documented and usable, while the RTL source is not disclosed.

## Contact

- Company: Hangzhou Magicstar Technology Co., Ltd.
- Email: yupc123@qq.com
- Phone: +86-13867435883
- Taobao: https://magicstar.taobao.com/

## License

Licensed under GPL-3.0-or-later. See the [LICENSE](LICENSE) file for details.

## Changelog

### v1.0 (2026-10-15)
- Initial open-source release
- Added license headers (copyright + SPDX identifier) to all RTL sources
- Added photo of the acquisition board