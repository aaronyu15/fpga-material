
# General Terms
## Vivado Related
### Project Mode vs Non-Project Mode
In Vivado, a design "project" can be managed in either Project Mode or Non-Project Mode. In Project Mode, Vivado automatically manages the design structure including what design files to include, constraints to use, and the storage of simulation/synthesis/implementation results. In Non-Project Mode, you handle this overhead yourself, and only use the Vivado tools to perform simulation/synthesis/implementation. In other words, you'd have to specify things like source files by yourself at runtime. In Non-Project Mode, no files are written to the disk, while Project Mode maintains a directory structure on disk to keep track of sources, results, and settings. See UG888 or UG892 for more details.

### Pblock
Placement Block. This is a floorplanning constraint that defines a region/area of the FPGA where specified modules can be placed by the placement tool.

### Waivers
Waivers are used to suppress things like DRC messages.


# Acronyms 

## Vivado Related
### APU
Application Processing Unit - One of the processors on the Zynq SoC. Specifically, this refers to the ARM Cortex-A9 core(s) for Zynq-7000 SoCs, and ARM Cortex-A53 core(s) for Zynq UltraScale+ SoCs.

### RPU
Realtime Processing Unit - One of the processors on the Zynq UltraScale+ MPSoC. Specifically, this refers to the ARM Cortex-R5F core(s).

### BSP    
Board Support Package - Support code for a hardware platform/board that helps it initialize the correct applications at power up. Can contain specific information related to operating system boot loaders and device drivers. The 'Standalone' BSP can be used to provide basic processor features.

### Domain 
Essentially this is the "Operating System" that is used to run applications. The "domain" can come from the Standalone BSP, a Linux OS, third-party methods such as FreeRTOS, or the "device tree generator". The domain must be configured to run on one of the cores (APU or RPU).

### Application   
The program that runs on the PS side built using Vitis.

### Platform
The

### TCF
Target Communication Framework.

### SDT
System Device Tree.

### PS
Processing System.

### PL
Programmable Logic.

### AXI
Advanced eXtensible Interface.

### HPM
High-Performance Master.

### LPD
Low Power Domain.

### FPD
Full Power Domain.

### OCM
On-Chip Memory.

### OOC
Out-Of-Context. This is a "design flow" style where modules can be synthesized/implemented "out-of-context", which treats them as standalone modules separate from the top level design. This is useful for modular designs where finished modules don't have to be re-synthesized, speeding up parts of the development process. 

### XCI
Xilinx IP Configuration files. 

### DCP
Design Checkpoint file. 

### AXI4
Advanced eXtensible Interface 4, also called AMBA AXI4 since it is part of the AMBA (Advanced Microcontroller Bus Architecture) communication protocol family from AMD. AXI4 is meant for "high speed systems/data transfers".

### XDC
Xilinx Design Constraints file. This is used to define "constraints" like clock timing and pin mapping for the design. Aside from syntax, this is analogous to the other industry standard, SDC.

### SDC
Synopsys Design Constraints file. This is used to define "constraints" like clock timing and pin mapping for the design. Aside from syntax, this is analogous to the one used by Xilinx, XDC.

### MMCM
Mixed Mode Clock Manager. This is a primitive module made by AMD that is used to generate different clocks with configurable phase and frequencies given an input clock. This makes use of PLLs but includes more control logic.

### STA
Static Timing Analysis.

### SDF
Standard Delay Format file.

### IDR
Intelligent Design Runs. This is a "design flow" that can be used to help close timing on a design.

### ILA
Integrated Logic Analyzer.

### BEL
Basic Element of Logic. This is the "lowest-level building block" in the FPGA logic fabric, and usually refers to a primitive element like a LUT, FF, or multiplexer.

### LOC
Location. This is a property that is used to define what site/pin an element is placed.

### CLB
Configurable Logic Block.


### MGT
Multi-Gigabit Transceiver.

### XST
Xilinx Synthesis Technology. This *was* the name for the backend synthesizer tool used by Vivado, which is now replaced by Vivado Synthesis. Either way, this refers to the tool used to create the post-synthesis gate-level netlist.

### TDP
Targeted Design Platform. Basically the development board or FPGA part you are targetting.

### XPS
Xilinx Platform Studio. This was the tool for handling IP for embedded processor designs, which has now been replaced by AMD Vivado IP integrator.

### XPM
Xilinx Parameterized Macros. These are simple "parameterized IPs" that can be instantiated without extra work by directly instantiating the relevant modules in your code. Available XPMs can be found in the Language Templates window of the in-built Text Editor.

### CMB
Clock Modifying Blocks. These are blocks which "modify" (like frequency multiplication, division, phase shift, etc) an input clock and produce an output clock. In the 7 series family, the CMBs are MMCM/PLL, BUFR, and PHASER primitives. In the UltraScale family, the CMBs are MMCM/PLL, BUFG_GT/BUFGCE_DIV, GT_COMMON/GT_CHANNEL/IBUFDS_GTE3, BITSLICE_CONTROL/RX_BITSLICE, and ISERDESE3.

### RPM
Relatively Placed Macro.


# Filetypes

| Extension  | Definition                                                                                                                                                                                                                                                                                                                   |
| :--------: | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|    .xpr    | Xilinx Project File. This holds the general project information when you create a new project in Vivado.                                                                                                                                                                                                                     |
|    .tcl    | Tcl (Tool Command Language AKA "tickle") scripts. These hold tcl commands that can be used to run parts/all of the Vivado process.                                                                                                                                                                                           |
|    .log    | Log files. By default when you use Vivado, a `vivado.log` file is created which records the tcl commands that are run during the Vivado session, and the printed messages as a result of those commands.                                                                                                                     |
|    .jou    | Journal files. By default when you use Vivado, a `vivado.jou` file is created which records the commands during the session. This can be used as a starting point to create new tcl scripts.                                                                                                                                 |
|    .xci    | Xilinx Core Instance file. This contains customization information for generated IPs.                                                                                                                                                                                                                                        |
|   .xcix    | Xilinx Core Container file. This is a binary file that contains the .xci data to customize an IP, along with the generated output products of the IP from synthesis, simulation, and any support files. This is used when the original IP repository not longer exists, usually because Vivado updates no longer support it. |
|    .xco    | Xilinx CORE Generator IP core file. This is a legacy filetype for old IPs, and require .ngc netlist files to support implementation.                                                                                                                                                                                         |
|    .xdc    | Xilinx Design Constraints file.                                                                                                                                                                                                                                                                                              |
|    .sdc    | Synopsys Design Constraints file.                                                                                                                                                                                                                                                                                            |
|    .bd     | Block Diagram file. Used mostly with IPs.                                                                                                                                                                                                                                                                                    |
|    .rpt    | Report file. Basically a text file that contains the output information from the various `report_*` commands that Vivado has.                                                                                                                                                                                                |
|    .rpx    | Report file but with extra data to allow the report to be interactively loaded into Vivado. Similar to the text report but with more visual flair.                                                                                                                                                                           |
| .edif/.edf | Electronic Design Interchange Format file. A standard text-based file that represents circuits, typically of a post-synthesis netlist.                                                                                                                                                                                       |
|    .ngc    | Netlist file. This was the proprietary file format output from the Xilinx Synthesis Technology (XST) tool, which contained the post-synthesis netlist.                                                                                                                                                                       |
|    .veo    | Verilog instance declaration template. These are made when you use an IP from the IP catalog. The template gives you text you can copy-and-paste to instantiate the IP module in your design.                                                                                                                                |
|    .vho    | VHDL instance declaration template. Same as .veo but for VHDL IP.                                                                                                                                                                                                                                                            |


