# FTDI JTAG Programmer
An inexpensive JTAG programmer for Xilinx CPLD's using an FT232 usb to serial converter

Disclaimer:<br>
This is not my own original project but rather an attempt to organize what I found on the internet that ultimately helped me program the CPLD in the SAMx4 project.


## Intro
I'm a Tandy Color Computer enthusiast that especially enjoys the hardware side of things. 
When I learned that Ciaran Ascomb had recreated the SAM chip, a chip used in the Dragon and Tandy Color Computer's,
I immediately cloned his repository and began modifying his design for my use.

Ciarans project is called the SAMx4 and it's built around a Xilinx XC95144XL CPLD. The code was written in VHDL using the older Xilinx ISE 14.7 development environment.
Xilinx ISE has a module called Impact which is used to  program the CPLD using a Xilinx supported debugger(programmer). I don't own a Xilinx programmer so I decided to search for another approach.
The idea is to take an inexpensive FTDI based USB to Serial converter and use it as a JTAG programmer by bit banging the I/O pins.
There are many FTDI bases USB to serial converters but for this one you'll have to use one based on the FT232RL chip. I had a [Black FTDI Friend](https://learn.adafruit.com/ftdi-friend/overview) by Adafruit lying around so I used that.


Here are the steps to compile and load the binary onto the CPLD.
- [Download and install Xilinx ISE 14.7](#Download-and-install-Xilinx-ISE-147)
- [Download and install the FTDI D2XX drivers](#Download-and-install-the-FTDI-D2XX-drivers)
- [Download/clone the SAMx4 project.](#Downloadclone-the-SAMx4-project)
- [Open the project with Xilinx ISE 14.7](#Open-the-project-with-Xilinx-ISE-147)
- [Implement the top module(compile) to produce a JED file](#Implement-the-top-module-to-produce-a-JED-file)
- [Use Impact CLI to produce an SVF file from the JED file](#Use-Impact-CLI-to-produce-an-SVF-file-from-the-JED-file)

  ### Download and install Xilinx ISE 14.7
  [Download Xilinx ISE 14.7 for your operating system](https://www.xilinx.com/downloadNav/vivado-design-tools/archive-ise.html)
  
  ### Download and install the FTDI D2XX drivers
  
  If you are using Windows you'll need to remove the VCP(Virtual COM Port) drivers and install the D2XX direct USB access drivers.
  If you are using Linux then you'll need to temporarily unload the VCP(Virtual COM Port) module and install the D2XX direct USB access static and shared libraries.
  
  [Download the driver for your OS](https://ftdichip.com/drivers/d2xx-drivers/)

  Since I use Linux I'll go over what I did for my installation.
  I used [this](https://ftdichip.com/Support/Documents/AppNotes/AN_220_FTDI_Drivers_Installation_Guide_for_Linux.pdf) guide to install the libraries and header but I'll just paste the necessary commands below.
At the time of this writing the D2XX libraries was version 1.4.27 so you'll have adjust accordingly if using a different version.
Once downloaded, extract the files and navigate to the direcory where the files were extracted and enter the following commands using a terminal.
```bash
sudo cp release/build/lib* /usr/local/lib
```
Make the following symbolic links and permission modifications in /usr/local/lib:
```bash
cd /usr/local/lib
sudo ln –s libftd2xx.so.1.1.12 libftd2xx.so
sudo chmod 0755 libftd2xx.so.1.1.12   
```

  
  ### Download/clone the SAMx4 project
  Get the project source files from [here.](https://www.6809.org.uk/dragon/samx4/)
  ### Open the project with Xilinx ISE 14.7
  ### Implement the top module to produce a JED file
  ### Use Impact CLI to produce an SVF file from the JED file




## Sources
- [How to install Xilinx ISE on Linux, in 7 easy steps!](https://youtu.be/yzEIQLQZYpk?si=v6nmZXc6_NBRsofR)
- [Xilinx XC9536XL CPLD | FT232RL FTDI JTAG programmer | Xilinx ISE 14.7](https://youtu.be/UACzPj62klc?si=p1kzB3-zuSgdYw8j)
- [https://github.com/moustafa-git/Xilinx_XC9536XL_CPLD_FT232RL_JTAG_PROGRAMMER](https://github.com/moustafa-git/Xilinx_XC9536XL_CPLD_FT232RL_JTAG_PROGRAMMER?tab=readme-ov-file)
- [FTDI Drivers Installation Guide for Linux](https://ftdichip.com/Support/Documents/AppNotes/AN_220_FTDI_Drivers_Installation_Guide_for_Linux.pdf)
- [USB CPLD Programmer](https://tulip-house.ddo.jp/digital/PROG_CPLD/index.html)
- [SAMx4, a CPLD replacement for the MC6883 Synchronous Address Multiplexer (SN74LS783/SN74LS785)](https://www.6809.org.uk/dragon/samx4/)

