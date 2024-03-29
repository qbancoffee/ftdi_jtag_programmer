# FTDI JTAG Programmer
An inexpensive JTAG programmer for Xilinx CPLD's using an FT232 usb to serial converter

Disclaimer:<br>
This project is not an original creation but rather an effort to organize information gathered from the internet, which proved instrumental in programming the CPLD for the SAMx4 project. The programming procedure detailed here is based on adapting steps from a different project that used a Xilinx XC9536XL CPLD. While the steps are theoretically applicable to other Xilinx CPLDs, I cannot guarantee universal compatibility. The SAMx4 project serves as an example to demonstrate the programming process specifically for the Xilinx XC95144XL CPLD.

## Intro


Introduction:<br>
I'm really into the Tandy Color Computer, especially the hardware side of things. Recently, I discovered that Ciaran Anscomb recreated the SAM chip, which is used in both the Dragon and Tandy Color Computers. Excited about this, I cloned his project and started adapting it for my own purposes.

About Ciaran's Project - SAMx4:<br>
Ciaran's project, called SAMx4, is centered around a Xilinx XC95144XL CPLD. The code was written in VHDL using the Xilinx ISE 14.7 development environment. In the Xilinx ISE, there's a module called Impact that's typically used to program the CPLD using a Xilinx-supported debugger or programmer. Unfortunately, I didn't have a Xilinx programmer, so I needed to find another way.

Challenges and Alternative Approach:<br>
To overcome the lack of a Xilinx programmer, I decided to explore an alternative method. Instead of using a specialized Xilinx programmer, I set out to use an affordable FTDI-based USB-to-Serial converter as a JTAG programmer by bit banging the I/O pins. Specifically, I chose one based on the FT232RL chip. I happened to have a [FTDI Friend](https://learn.adafruit.com/ftdi-friend/overview) by Adafruit lying around, so I used that.

This adaptation allowed me to program the Xilinx XC95144XL CPLD in the SAMx4 project without the need for an expensive Xilinx programmer. It's a cost-effective solution for folks like me who enjoy diving into the hardware side of things.


Here are the steps to compile the project and program the CPLD.

- [Download/clone the SAMx4 project.](#Downloadclone-the-SAMx4-project)
- [Download and install Xilinx ISE 14.7](#Download-and-install-Xilinx-ISE-147)
- [Open the project with Xilinx ISE 14.7](#Open-the-project-with-Xilinx-ISE-147)
- [Implement the top module(compile) to produce a JED file](#Implement-the-top-module-to-produce-a-JED-file)
- [Use Impact CLI to produce an SVF file from the JED file](#Use-Impact-CLI-to-produce-an-SVF-file-from-the-JED-file)
- [Download and install the FTDI D2XX drivers](#Download-and-install-the-FTDI-D2XX-drivers)
- [Compile the CPLD programmer](#Compile-the-CPLD-programmer)
- [Programming the CPLD](#Programming-the-CPLD)


  ### Download/clone the SAMx4 project
  Get the project source files from [here.](https://www.6809.org.uk/dragon/samx4/)
  ### Download and install Xilinx ISE 14.7
  In order to download Xilinx ISE 14.7 you'll have to create and account. Once you've created and account,
  make sure to download the "Full Product Installation" which is a single DVD file.
  
  [Download Xilinx ISE 14.7 for your operating system](https://www.xilinx.com/downloadNav/vivado-design-tools/archive-ise.html)
  
  If you are using Linux watch this video.
  
  [How to install Xilinx ISE on Linux, in 7 easy steps!](https://youtu.be/yzEIQLQZYpk?si=v6nmZXc6_NBRsofR)
  ### Open the project with Xilinx ISE 14.7
  ### Implement the top module to produce a JED file
  ### Use Impact CLI to produce an SVF file from the JED file
  ### Download and install the FTDI D2XX drivers
    

  Since I use Linux I'll go over what I did for my installation.
  I used [this](https://ftdichip.com/Support/Documents/AppNotes/AN_220_FTDI_Drivers_Installation_Guide_for_Linux.pdf) guide to install the libraries and headers but I'll just paste the necessary commands below.
At the time of this writing the D2XX library was version 1.4.27 so you'll have adjust accordingly if using a different version.

  [Download the driver for your OS](https://ftdichip.com/drivers/d2xx-drivers/)

Once downloaded, extract the files and navigate to the direcory where the files were extracted and enter the following commands using a terminal.
```bash
sudo cp release/build/lib* /usr/local/lib
```
Make the following symbolic links and permission modifications in /usr/local/lib:
```bash
cd /usr/local/lib
sudo ln –s libftd2xx.so.1.4.27 libftd2xx.so
sudo chmod 0755 libftd2xx.so.1.4.27   
```
Type the following to see if your user is part of the plugdev group.
```bash
groups
```
If your user is not you can add it by typing the following.
```bash
sudo usermod -aG plugdev $USER
```
You'll need to log out and then back in for this change to take effect however I've noticed that this doesn't always work and a reboot is required.


### Compile the CPLD programmer
Someone wrote a small program that bitbangs the FT232RL chip to be used as a JTAG programmer, you can get it from [here](https://tulip-house.ddo.jp/digital/PROG_CPLD/index.html). For convenience, I've included the program and the source from this site in this repository and you can find it in the "prog_cpld_original" directory.

I slightly modified the source so that it now compiles under Linux. You can find the modified source and an x86 64 bit binary in the "prog_cpld_linux" directory.

You can compile this program under Linux with gcc.
```bash
gcc -o prog_cpld prog_cpld.c -lftd2xx
```
### Wiring the programmer to the SAMx4

The FTDI friend conveniently provides +5V DC which we'll use to power the SAMx4 during programming.

Generic Wiring Diagram<br>
<img src="https://github.com/qbancoffee/ftdi_jtag_programmer/blob/main/images/wiring_1.jpg" width="300">


The FTDI Friend SAMx4 wiring<br>
<img src="https://github.com/qbancoffee/ftdi_jtag_programmer/blob/main/images/wiring_2.jpg" width="600">

The FTDI Friend wired to the SAMx4<br>
<img src="https://github.com/qbancoffee/ftdi_jtag_programmer/blob/main/images/samx4_to_ftdi_friend.jpg" width="600">


### Programming the CPLD
You'll be loading an [SVF](https://en.wikipedia.org/wiki/Serial_Vector_Format) file to the CPLD and to help, I've included a working "samx4.svf" file just to confirm that you've successfully programmed the CPLD. The "samx4.svf" is the SVF file produced for the CPLD in the SAMx4 PCB.

The "samx4.svf" file I've inlcluded is configured for the following:
- 256K banker support
- 4K and 16K DRAM support
- SN74LS785 mode instead of SN74LS783 mode
- Fast video
- Dram refresh

If you need a different configuration you'll have to change that in the source and recompile.

If you are using Windows you'll need to remove the VCP(Virtual COM Port) drivers and install the D2XX direct USB access drivers.
If you are using Linux then you'll need to temporarily unload the VCP(Virtual COM Port) module and install the D2XX direct USB access static and shared libraries.

Plug in the FT232RL based USB to serial converter. At this point, linux will load the built-in VCP module and create a file named
something like /dev/ttyUSB0 which means that the OS recognized it and loaded the FTDI virtual com port module. Since we want direct USB access to the FT232 chip, we'll need to temporarily remove this module.

To see the loaded module type the following
```bash
lsmod | grep ftdi
```

To temporarily remove the module type the following.
```bash
sudo rmmod ftdi_sio
```

Make sure the module is no longer loaded.
```bash
lsmod | grep ftdi
```
Keep in mind that you'll have to repeat these steps if you unplug and plug it back in.

The original program "PROG_CPLD.exe" is a Windows program and although I have not tested it, you should be able to just run it from a command prompt with the following command.
```dos
PROG_CPLD -v -c samx4.svf
```
Similarly you can program the CPLD with the linux executable
```bash
./prog_cpld -v -c samx4.svf
```


You'll see lots of stuff scrolling and if successful, you'll see a maessage that reads.


"<<< All TDO outputs matched to the expected values! >>>"

<img src="https://github.com/qbancoffee/ftdi_jtag_programmer/blob/main/images/successful_programming.png" width="1200">



## Sources
- [How to install Xilinx ISE on Linux, in 7 easy steps!](https://youtu.be/yzEIQLQZYpk?si=v6nmZXc6_NBRsofR)
- [Xilinx XC9536XL CPLD | FT232RL FTDI JTAG programmer | Xilinx ISE 14.7](https://youtu.be/UACzPj62klc?si=p1kzB3-zuSgdYw8j)
- [https://github.com/moustafa-git/Xilinx_XC9536XL_CPLD_FT232RL_JTAG_PROGRAMMER](https://github.com/moustafa-git/Xilinx_XC9536XL_CPLD_FT232RL_JTAG_PROGRAMMER?tab=readme-ov-file)
- [FTDI Drivers Installation Guide for Linux](https://ftdichip.com/Support/Documents/AppNotes/AN_220_FTDI_Drivers_Installation_Guide_for_Linux.pdf)
- [USB CPLD Programmer](https://tulip-house.ddo.jp/digital/PROG_CPLD/index.html)
- [SAMx4, a CPLD replacement for the MC6883 Synchronous Address Multiplexer (SN74LS783/SN74LS785)](https://www.6809.org.uk/dragon/samx4/)

