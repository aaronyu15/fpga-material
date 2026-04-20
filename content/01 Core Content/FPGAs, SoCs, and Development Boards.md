---
modified: 2026-04-20T00:18:35-04:00
date: 2025-04-10
---
This page lists and explains the various FPGA chips and development boards available, categorised based on the producer (Xilinx, Altera, 3rd party), how much it costs, and the ease for beginners in terms of available tutorials/intention.

>[!info] Terminology
> - **FPGA** - Field Programmable Gate Array. Typically this refers to the FPGA chip itself without connecting it to a board. You wouldn't typically buy this outright unless you have a board design for it.
> - **SOC** - System On Chip. This refers to a chip that has both an FPGA and a processor core built into it. This allows software to be designed for the processor (the so-called Processing System or PS) while hardware can be designed for the FPGA (the so-called Programmable Logic or PL). Again, you wouldn't typically buy this outright unless you have a board design for it.
> - **SOM** - System On Module. A term to refer to small embedded boards (credit card sized) that include an SOC along with other supporting circuits. Usually these lack connectors like USB, UART, or Ethernet to communicate to the device to help develop it. There are some starter kits available, but this category is more for use in production environments.
> - **Development Boards** - Larger boards than SOMs, and typically have an SOC along with a lot of other things like DDR memory, JTAG, UART, Ethernet, HDMI, DisplayPort, PCIe, mezzanine interfaces, etc, to help develop applications. The starter kits for these typically target university courses and above, so they're a good starting point.

All these systems are purposed for industry development in areas like automotive, defence, space, embedded vision, robotics, and others. However, since FPGAs are repurposable, many can also be used for other endeavours like custom microprocessor design, making hardware accelerators for specific applications, research, etc. 

---

# Xilinx Family

The Xilinx family of SoCs and development boards is extensive. These are typically used with Xilinx Vivado/Vitis for hardware and software design.

## FPGA
Xilinx FPGAs contain only the FPGA chip themselves, so there is no attached board, processor, etc. In other words, unless you have a board to put it on to even begin programming it, these aren't bought by themselves. However, development boards and SOMs can contain one of these FPGA chips, and knowing what chip it is can give some insight into what it is targeted for. There are 4 FPGA "families" that Xilinx offers. The family names are only for categorising the application focus, the real FPGA chips are further sub variants for each family, sold as different "series".

| Family Name | Focus                                                            |
| :-----------: | ---------------------------------------------------------------- |
| Spartan (S)     | Low cost with higher I/O count to connect to programmable logic. |
| Artix (A)       | Cost efficient with good performance for general applications.   |
| Kintex (K)      | Mid-range cost and power for high performance applications.      |
| Virtex (V)      | High cost for even higher performance applications.              |

The available series for each family are shown.

|   Series    |   Spartan    | Artix |    Kintex    | Virtex | Notes                                              |
|:-----------:|:------------:|:-----:|:------------:|:------:| -------------------------------------------------- |
|  6 Series   | Discontinued |   -   | Discontinued |   -    | -                                                  |
|  7 Series   |      ✅      |  ✅   |      ✅      |   ✅   | Original mainline family.                          |
| UltraScale  |      -       |   -   |      ✅      |   ✅   | New routing, improved fabric, better transceivers. |
| UltraScale+ |      ✅      |  ✅   |      ✅      |   ✅   | FinFET process, more capabilites.                  |

Within each series, a family might have multiple different variants where the only difference is in the number of logic cells, DSP slices, memory, I/O, or some other resource. Essentially, it's the same chip but with different amounts of inbuilt logic. Specific chips will have part numbers that can identify what family and series the chip is from. 

An example part number might look like this: `XC7K160T`

Where `XC = Xilinx Chip`, `7` refers to the 7 series, `K` refers to Kintex, and `160` is the approximate number of logic cells available (in thousands). The `T`, if present, refers to the availability of high speed transceivers, which lets you know if the FPGA supports high-speed protocols like PCIe, HDMI, DisplayPort, etc. If there is no `T`, the chip is used with only general-purpose I/O.

An example part number for the UltraScale+ series might look like this: `XCVU7P`.

Where `XC = Xilinx Chip`, `V` refers to Virtex, `U` refers to the UltraScale series, `7` is an indication of device size, and `P` refers to the UltraScale+ series. If `U` is present without `P`, the part is just an UltraScale chip.


>[!info] Although there is no hard-logic processors like there are in SoCs, Xilinx FPGAs can be programmed with the MicroBlaze Processor IP to perform similar features, although this does take up resources in the programmable logic.

Within each series, there are also "automotive-grade", "defence-grade", and "space-grade" classes, however those are more targeted towards specific applications so they won't be discussed here.

## SOC

Xilinx SOCs are a single chip that contains both an FPGA ([[PL]]) and integrated processor(s) ([[PS]]). Note that these "FPGAs" are not the same as the ones in the FPGA section above.  There are 2 main "families" of SOCs from Xilinx. Unlike the FPGAs, the SOCs have differing numbers of "series".

| Family Name | Focus                                                            |
| :-----------: | ---------------------------------------------------------------- |
| Zynq (Z)        | Embedded systems with integrated ARM processors and FPGA fabric. |
| Versal (V)      | High-performance computing with a mix of CPUs, FPGAs, DSPs, AI Engines, etc. |

The available series for each family are shown.

|         Series         | Description                                                                                                          |   Example Part Names   |
|:----------------------:| -------------------------------------------------------------------------------------------------------------------- |:----------------------:|
|     Zynq 7000 SoC      | Lowest cost. Contains a single-core (7000S) or dual-core (7000) ARM Cortex-A9 alongside FPGA fabric.                 |    XC7Z007S, XC7Z010     |
| Zynq UltraScale+ MPSoC | Mid-range cost. Contains dual-core (CG), quad-core + GPU (EG), or quad-core + GPU + video codec (EV).                |  XCZU1CG, XCZU1EG, XCZU4EV   |
| Zynq UltraScale+ RFSoC | Very high cost. Used for Radio Frequency (RF) applications so it has special ADC/DAC blocks alongside the ARM cores. | XCZU21DR, XCZU39DR, XCZU42DR |
|  Versal Prime Series   | General-purpose applications (no AI engines).                                                                        |         XCVM1102         |
| Versal Premium Series  | High-speed networking, PCIe Gen5, 600 Gb/s Ethernet.                                                                 |         XCVP1002         |
|    Versal RF Series    | Used for RF applications so it has special ADC/DAC blocks alongside all the other cores.                             |         XCVR1602         |
| Versal AI Edge Series  | Designed for edge AI (low power, real-time inference).                                                               |         XCVE2002         |
| Versal AI Core Series  | Contains AI engines for ML/AI purposes.                                                                              |         XCVC1502         |
|   Versal HBM Series    | Has integrated High Bandwidth Memory (HBM2) for memory intensive applications.                                       |         XCVH1522         |

>[!info] The Versal Prime, Premium, and AI Edge series all have Gen 2 variants, but those are still relatively new. Presumably the others will get Gen 2 variants as well at some point.


Everything in the Versal or Zynq UltraScale+ RFSoC categories typically cost a fortune ($10,000+ USD), and so it's unlikely for you to ever own a personal one if you're a hobbyist. They are not typically sold in development boards/evaluation kits either, aside from a select few developed by AMD. Though there are a few partner SOMs that seem to be in development with unlisted prices.

For all intents and purposes, most FPGA users will want to focus on the Zynq 7000 and UltraScale+ MPSoC categories since they are more common and more affordable.


## System On Module

A System on Module (SOM) takes one of the FPGA or SOC chips and places it on a small board for development or integration into a production system. The number of added components can vary greatly, but typically things like DDR memory, Flash memory, microSD card slots, or a programmable interface like JTAG are included. While some SOMs come equipped with enough to use like an FPGA learning board, many are meant for use in production environments so be wary of that when looking to purchase one for personal use.

SOMs produced by Xilinx are called the "Kria" family, while boards produced by SOM partners have their own naming schemes based on the partner. All the available SOMs can be searched for on the official [AMD page](https://www.amd.com/en/search/adaptive-socs-and-fpgas/boards.html#f-amd_product_board_type=SOMs). Most SOMs use the Zynq UltraScale+ MPSoC or Zynq 7000, with very few using the Spartan/Artix/Kintex/Virtex lineup.

However, most of the time you'll want a development board if you're trying to buy an FPGA/SOC for personal use.

## Development Boards

Development boards are self-explanatory in name. Most boards are equipped with enough ports, memory, communication protocols, and cores to be versatile for any application. Xilinx produces their own development boards alongside their partners. Costs range from several hundred dollars to several thousand depending on the board and FPGA/SOC being used. 

After reading the above information regarding FPGAs and SOCs, a comprehensive list of development boards can be found [here](https://www.fpgadeveloper.com/list-of-fpga-dev-boards-by-device/). Note that some of the links may point to SOMs, which may not be as developer friendly.

The tables below summarize some board options along with relative prices in USD. 

>[!important] To see if your Vivado license can be used with a certain device, check [UG973](https://docs.amd.com/r/en-US/ug973-vivado-release-notes-install-license/Supported-Devices).

Prices with a \* mean there are alternative options available, usually with more LUTs and resources.


### 🔰 Beginner / Inexpensive Boards

>[!faq] If you want a beginner FPGA board, sticking to something like the Basys3, Nexys A7 would be fine. If you want to delve more with SOCs, the ZUBoard-1CG, Ultra96-v2, or PYNQ-Z1 would be good. If you want both flavours, the ZUBoard-1CG or Blackboard may be good options.

| Board Name                                                                                                  | Device Name            | Type | Device            | Vendor      | Price | Use Case                              |
| ----------------------------------------------------------------------------------------------------------- | ---------------------- | ---- | ----------------- | ----------- | ----- | ------------------------------------- |
| [Basys 3](https://digilent.com/shop/basys-3-artix-7-fpga-trainer-board-recommended-for-introductory-users/) | Artix-7                | FPGA | XC7A35T-1CPG236C  | Digilent    | $165  | Learning FPGA Basics.                 |
| [Nexys A7](https://digilent.com/shop/nexys-a7-fpga-trainer-board-recommended-for-ece-curriculum/)           | Artix-7                | FPGA | XC7A100T-1CSG324C | Digilent    | $349  | Learning FPGA Basics.                 |
| [Arty S7](https://digilent.com/shop/arty-s7-spartan-7-fpga-development-board/)                              | Spartan-7              | FPGA | XC7S25-CSGA324    | Digilent    | $119* | Educational/Starter Kit.              |
| [Arty A7](https://digilent.com/shop/arty-a7-100t-artix-7-fpga-development-board/)                           | Artix-7                | FPGA | XC7A100TCSG324-1  | Digilent    | $299  | Educational/Starter Kit.              |
| [Arty Z7](https://digilent.com/shop/arty-z7-zynq-7000-soc-development-board/)                               | Zynq-7000              | SOC  | XC7Z010-1CLG400C  | Digilent    | $249* | Educational/Starter Kit.              |
| [Cora Z7](https://digilent.com/shop/cora-z7-zynq-7000-single-core-for-arm-fpga-soc-development/)            | Zynq-7000              | SOC  | XC7Z007S-1CLG400C | Digilent    | $149  | Learn Zynq-based designs.             |
| [PYNQ-Z1](https://digilent.com/shop/pynq-z1-python-productivity-for-zynq-7000-arm-fpga-soc/)                | Zynq-7000              | SOC  | XC7Z020-1CLG400C  | Digilent    | $299  | Learn Zynq-based designs.             |
| [Zedboard](https://digilent.com/shop/zedboard-zynq-7000-arm-fpga-soc-development-board/)                    | Zynq-7000              | SOC  | XC7Z020-CLG484    | Digilent    | $589  | Educational.                          |
| [Zybo Z7](https://digilent.com/shop/zybo-z7-zynq-7000-arm-fpga-soc-development-board/)                      | Zynq-7000              | SOC  | XC7Z010-1CLG400C  | Digilent    | $299* | Educational.                          |
| [Cmod A7](https://digilent.com/shop/cmod-a7-35t-breadboardable-artix-7-fpga-module/)                        | Artix-7                | FPGA | XC7A35            | Digilent    | $99   | Breadboard/prototype boards.          |
| [Cmod S7](https://digilent.com/shop/cmod-s7-breadboardable-spartan-7-fpga-module/)                          | Spartan-7              | FPGA | XC7S25-1CSGA225C  | Digilent    | $99   | Breadboard/prototype boards.          |
| [ZUBoard-1CG](https://www.avnet.com/wps/portal/us/products/avnet-boards/avnet-board-families/zuboard-1cg)   | Zynq UltraScale+ MPSoC | SOC  | XCZU1CG-1SBVA484E | Avnet       | $159  | Learn Zynq UltraScale+ based designs. |
| [Ultra96-v2](https://www.avnet.com/wps/portal/us/products/avnet-boards/avnet-board-families/ultra96-v2)     | Zynq UltraScale+ MPSoC | SOC  | XCZU3EG-1SBVA484E | Avnet       | $299  | Learn Zynq UltraScale+ based designs. |
| [Blackboard](https://www.realdigital.org/hardware/blackboard)                                               | Zynq-7000              | SOC  | XC7Z007S          | RealDigital | $184  | Learn FPGA/Microcontroller Basics.    |
| [Boolean Board](https://www.realdigital.org/hardware/boolean)                                               | Spartan-7              | FPGA | XC7S50-CSGA324    | RealDigital | $92   | Learn FPGA Basics.                    |




### ⚙️ Intermediate / Mid-range Boards


| Board Name                                                                                                   | Device Name            | Type | Device                        | Vendor      | Price | Use Case                                                           |
| ------------------------------------------------------------------------------------------------------------ | ---------------------- | ---- | ----------------------------- | ----------- | ----- | ------------------------------------------------------------------ |
| [Nexys Video](https://digilent.com/shop/nexys-video-artix-7-fpga-trainer-board-for-multimedia-applications/) | Artix-7                | FPGA | XC7A200T-1SBG484C             | Digilent    | $549  | Multimedia video and audio processing trainer board.               |
| [Eclypse Z7](https://digilent.com/shop/eclypse-z7/)                                                          | Zynq-7000              | SOC  | XC7Z020-1CLG484C              | Digilent    | $499  | Developing embedded measurement systems.                           |
| [RFSoC 4x2](https://www.realdigital.org/hardware/rfsoc-4x2)                                                  | Zynq UltraScale+ RFSoC | SOC  | ZU48DR                        | RealDigital | $2149 | Radio Frequency applications such as Software Defined Radio (SDR). |
| [Kria KV260](https://www.amd.com/en/products/system-on-modules/kria/k26/kv260-vision-starter-kit.html)       | Zynq UltraScale+ MPSoC | SOC  | XCK26 (Kria Zynq UltraScale+) | AMD         | $249  | Vision starter kit.                                                |
| [Kria KR260](https://www.amd.com/en/products/system-on-modules/kria/k26/kr260-robotics-starter-kit.html)     | Zynq UltraScale+ MPSoC | SOC  | XCK26 (Kria Zynq UltraScale+) | AMD         | $349  | Robotics starter kit.                                              |
| [SP701](https://www.amd.com/en/products/adaptive-socs-and-fpgas/evaluation-boards/sp701.html)                | Spartan-7              | FPGA | XC7S100                       | AMD         | $836  | Spartan-7 evaluation kit for general purpose applications.         |
| [AC701](https://www.amd.com/en/products/adaptive-socs-and-fpgas/evaluation-boards/ek-a7-ac701-g.html)        | Artix-7                | FPGA | XC7A200T-2FBG676C             | AMD         | $1678 | Artix-7 evaluation kit for general purpose applications.           |
| [ZC702](https://www.amd.com/en/products/adaptive-socs-and-fpgas/evaluation-boards/ek-z7-zc702-g.html)        | Zynq-7000              | SOC  | XC7Z020-CLG484-1              | AMD         | $1160 | Mid-range general purpose.                                         |
| [ZCU102](https://www.amd.com/en/products/adaptive-socs-and-fpgas/evaluation-boards/ek-u1-zcu102-g.html)      | Zynq UltraScale+ MPSoC | SOC  | XCZU9EG-2FFVB1156             | AMD         | $3234 | Mid-range general purpose.                                         |
| [ZCU104](https://www.amd.com/en/products/adaptive-socs-and-fpgas/evaluation-boards/zcu104.html)              | Zynq UltraScale+ MPSoC | SOC  | XCZU7EV-2FFVC1156             | AMD         | $1678 | Mid-range general purpose.                                         |
| [ZCU106](https://www.amd.com/en/products/adaptive-socs-and-fpgas/evaluation-boards/zcu106.html)              | Zynq UltraScale+ MPSoC | SOC  | XCZU7EV-2FFVC1156             | AMD         | $3234 | Mid-range general purpose.                                         |




### 💼 Advanced / More Expensive Boards 

| Board Name                                                                                              | Device Name           | Type | Device                | Vendor   | Price  | Use Case                                                   |
| ------------------------------------------------------------------------------------------------------- | --------------------- | ---- | --------------------- | -------- | ------ | ---------------------------------------------------------- |
| [Genesys ZU](https://digilent.com/shop/genesys-zu-zynq-ultrascale-mpsoc-development-board/)             | Zynq-UltraScale+      | SOC  | XCZU3EG-SFVC784-1-E   | Digilent | $1249* | Multimedia, automotive, surveillance, and embedded vision. |
| [Genesys 2](https://digilent.com/shop/genesys-2-kintex-7-fpga-development-board/)                       | Kintex-7              | FPGA | XC7K325T-2FFG900C     | Digilent | $1099  | Data and video processing, general purpose.                |
| [VC709](https://www.amd.com/en/products/adaptive-socs-and-fpgas/evaluation-boards/dk-v7-vc709-g.html)   | Virtex-7              | FPGA | XC7VX690T-2FFG1761C   | AMD      | $8094  | High performance general purpose applications.             |
| [KCU105](https://www.amd.com/en/products/adaptive-socs-and-fpgas/evaluation-boards/kcu105.html)         | Kintex UltraScale     | FPGA | XCKU040-2FFVA1156E    | AMD      | $6495  | Data center / DSP application prototyping.                 |
| [KCU116](https://www.amd.com/en/products/adaptive-socs-and-fpgas/evaluation-boards/ek-u1-kcu116-g.html) | Kintex UltraScale+    | FPGA | XCKU5P-2FFVB676E      | AMD      | $6495  | High end data transfer applications.                       |
| [VCU108](https://www.amd.com/en/products/adaptive-socs-and-fpgas/evaluation-boards/ek-u1-vcu108-g.html) | Virtex UltraScale     | FPGA | CVU095-2FFVA2104E     | AMD      | $11995 | High end applications.                                     |
| [VCU118](https://www.amd.com/en/products/adaptive-socs-and-fpgas/evaluation-boards/vcu118.html)         | Virtex UltraScale+    | FPGA | XCVU9P-L2FLGA2104E    | AMD      | $14995 | High end applications.                                     |
| [VCU129](https://www.amd.com/en/products/adaptive-socs-and-fpgas/evaluation-boards/vcu129.html)         | Virtex UltraScale+    | FPGA | XCVU29P-L2FSGA2577E   | AMD      | $20995 | High end applications.                                     |
| [VEK280](https://www.amd.com/en/products/adaptive-socs-and-fpgas/evaluation-boards/vek280.html)         | Versal AI Edge Series | SOC  | XCVE2802-2MSEVSVH1760 | AMD      | $6995  | Edge AI prototyping.                                       |
| [VHK158](https://www.amd.com/en/products/adaptive-socs-and-fpgas/evaluation-boards/vhk158.html)         | Versal HBM Series     | SOC  | XCVH1582-2MSEVSVA3697 | AMD      | $14995 | Memory intensive applications.                             |
| [VMK180](https://www.amd.com/en/products/adaptive-socs-and-fpgas/evaluation-boards/vmk180.html)         | Versal Prime Series   | SOC  | XCVM1802-2MSEVSVA2197 | AMD      | $9345  | High end applications.                                     |
| [VPK120](https://www.amd.com/en/products/adaptive-socs-and-fpgas/evaluation-boards/vpk120.html)         | Versal Premium Series | SOC  | XCVP1202-2MSEVSVA2785 | AMD      | $11994 | High end applications.                                     |
| [VPK180](https://www.amd.com/en/products/adaptive-socs-and-fpgas/evaluation-boards/vpk180.html)         | Versal Premium Series | SOC  | XCVP1802-2MSELSVC4072 | AMD      | $17995 | High end applications.                                     |
| [VCK190](https://www.amd.com/en/products/adaptive-socs-and-fpgas/evaluation-boards/vck190.html)         | Versal AI Core Series | SOC  | XCVC1902-2MSEVSVA2197 | AMD      | $13195 | AI applications.                                                           |

## Alveo Family
Xilinx also offers the Alveo product line, which are *technically* SoC devices since they are based on FPGA/SoC families such as Versal and Virtex, but AMD markets them as data center accelerators and not FPGA development boards or solutions. This note is included since they are still FPGAs, but like Versal, it is unlikely the layman will ever get to use one.


# Intel Family



# Other FPGAs / Development Boards