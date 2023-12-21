# ftdi_jtag_programmer
An inexpensive JTAG programmer for Xilinx CPLD's using an FT232 usb to serial converter

Disclaimer:<br>
This is not my own original project but rather an attempt to organize what I found on the internet that ultimately helped me program the CPLD in the SAMx4 project.



## Sources
- [How to install Xilinx ISE on Linux, in 7 easy steps!](https://youtu.be/yzEIQLQZYpk?si=v6nmZXc6_NBRsofR)
- [Xilinx XC9536XL CPLD | FT232RL FTDI JTAG programmer | Xilinx ISE 14.7](https://youtu.be/UACzPj62klc?si=p1kzB3-zuSgdYw8j)
- [https://github.com/moustafa-git/Xilinx_XC9536XL_CPLD_FT232RL_JTAG_PROGRAMMER](https://github.com/moustafa-git/Xilinx_XC9536XL_CPLD_FT232RL_JTAG_PROGRAMMER?tab=readme-ov-file)
- [FTDI Drivers Installation Guide for Linux](https://ftdichip.com/Support/Documents/AppNotes/AN_220_FTDI_Drivers_Installation_Guide_for_Linux.pdf)
- [USB CPLD Programmer](https://tulip-house.ddo.jp/digital/PROG_CPLD/index.html)
- [SAMx4, a CPLD replacement for the MC6883 Synchronous Address Multiplexer (SN74LS783/SN74LS785)](https://www.6809.org.uk/dragon/samx4/)


## Intro
I'm a Tandy Color Computer enthusiast that especially enjoys the hardware side of things. 
When I learned that Ciaran Ascomb had recreated the SAM chip, a chip used in the Dragon and Tandy Color Computer's,
I immediately cloned his repository and began modifying his design for my use.

Ciarans project is called the SAMx4 and it's built around a Xilinx XC95144XL CPLD. The code was written in VHDL using the older Xilinx ISE 14.7 development environment.
Xilinx ISE has a module called Impact which is used to  program the CPLD using a Xilinx supported debugger(programmer). I don't own a Xilinx programmer so I decided to find another approach.

Here are the steps to compile and load the binary onto the CPLD.
- [Download and install Xilinx ISE 14.7](#Download-and-install-Xilinx-ISE-14\.7)
- [Download/clone the SAMx4 project.](#Download/clone-the-SAMx4-project)
- [Open the project with Xilinx ISE 14.7](#Open-the-project-with-Xilinx-ISE-14.7)
- [Implement the top module(compile) to produce a JED file](#Implement-the-top-module(compile)-to-produce-a-JED-file)
- [Use Impact CLI to produce an SVF file from the JED file](#Use-Impact-CLI-to-produce-an-SVF-file-from-the-JED-file)

  ### Download and install Xilinx ISE 14.7
  ### Download/clone the SAMx4 project
  ### Open the project with Xilinx ISE 14.7
  ### Implement the top module(compile) to produce a JED file
  ### Use Impact CLI to produce an SVF file from the JED file

