---
title: Landing Page
---

This vault discusses the following topics:
- FPGA chips, SoCs, and Development Boards.
- How to use Vivado to develop hardware applications.
- How to use Vitis to program processor cores and interact with the programmable fabric.
- Review of each User Guide produced by AMD.
- Overview of Xilinx terminology relating to FPGAs.

If these topics interest you, you can read on.

## Getting Started 
**[[FPGAs, SoCs, and Development Boards]]**
Start by learning the names of various Xilinx (and optionally Altera and third party) FPGA devices, SoCs, and development boards, and selecting one to begin learning how to use it.

**[[Vivado Quick Tutorial ]]**
Vivado is the overarching tool that allows you to write hardware in SystemVerilog/VHDL and perform simulation, synthesis, implementation, and bitstream generation to actually program the FPGA. This page introduces the relevant terms and UI elements to begin using Vivado. Links to the relevant official Xilinx User Guides are provided within.

**[[Vitis Quick Tutorial ]]**
Vitis (formerly known as Vivado HLS) is the supplementary tool that allows you to create software applications for the hard-logic cores present on SoC devices. Additionally, Vitis High Level Synthesis (HLS) allows you to convert C/C++ code to an HDL description that can be implemented in the programmable logic. This page introduces the relevant terms and UI elements to begin using Vitis. Links to the relevant official Xilinx User Guides are provided within.

**[[PetaLinux Quick Tutorial ]]**
PetaLinux is a Linux distribution based on Yocto that has official development support for most of the SoC based development boards. In short, you can run Linux on the SoC, letting the operating system run on the ARM cores while your custom logic runs in the programmable fabric. Note that this is only relevant for SoC based development boards, such as boards that use the Zynq-7000 or Zynq UltraScale+ MPSoC chips. This page introduces the relevant terms and UI elements to begin using PetaLinux. Links to the relevant official Xilinx User Guides are provided within.

## Hands-on Projects
[[Simple projects to adapt]]
Walk through some relatively simple projects documented by others to become more familiar with design flows, terminology, and capabilities. 


## References 
**[[Xilinx Terminology]]**
This page lists some of the archaic and convoluted terms used by Xilinx in their documents for FPGAs, Vivado, Vitis, and PetaLinux. Useful as a reference when you want to know more about some esoteric term they use.

**[[Xilinx Docs]]**
The official Xilinx documentation page can only be described as "Everything Everywhere All at Once." with so much that seems to be outdated and circular links that do not seem to end. For people trying to learn the tools, it is less than optimal. I try to document the useful ones here as a way of mapping out which ones are helpful for learning which tools, and which ones can be disregarded.
