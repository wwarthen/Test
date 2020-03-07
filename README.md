# Summary

RomWBW is a complete firmware and software package for many hobbyist
Z80/Z180-based computers. Combined with any of the supported hardware
platforms, RomWBW provides a full CP/M style operating environment with
related utility applications and support for a broad set of hardware
including serial, floppy, hard disk (IDE, CF Card, SD Card), video,
sound, and keyboard. VT-100 escape code processing is built-in for all
video interfaces.

RomWBW supports most of the Z80/Z180 based hardware platforms produced
by the [RetroBrew Computers
Community](https://www.retrobrewcomputers.org) including SBC V1/V2, Zeta
V1/V2, N8, and Mark IV. RomWBW also supports Z80/Z180 based systems from
the [RC2014 Community](https://rc2014.co.uk/).

Most of the functionality is implemented in ROM. In fact, a simple
system with only RAM and ROM is entirely sufficient for a fully
operational CP/M system using RAM for file storage. However, RomWBW
supports many disk storage options which provide for persistent storage
and booting of custom operating systems.

In addition to hardware initialization and bootstrap, RomWBW implements
a robust bank switching memory management system. A hardware driver
framework leverages the bank switching to maximize available application
memory. A complete function interface (HBIOS) allows operating systems
to be adapted to RomWBW without direct knowledge of the hardware. Any
software or operating system written for HBIOS will run on any of the
hardware platforms supported without change.

A simple system monitor can be launched at startup for general hardware
and system debugging. ROM-based implementations of BASIC and Forth are
also provided. RomWBW includes robust adaptations of both CP/M-80 2.2
and Z-System (ZCPR + ZSDOS). Either operating system can be loaded
directly from ROM or installed and loaded from disk storage. A selection
of standard/useful applications is provided via a built-in ROM disk. A
RAM disk is also provided for fast/temporary file storage.

For each of the supported hardware platforms, a pre-built ROM image is
provided which is ready to program onto the ROM for your system. These
standard pre-built ROMs will automatically recognize and use the most
common hardware options for the platform. If desired, system
customization is achieved by making simple modifications to a
configuration file and running a build script to generate a custom ROM
image. All source and build tools are included in the distribution. The
build scripts run under any modern 32 or 64 bit version of Microsoft
Windows.

# Getting Started

The steps documented below will refer to a “modern computer” and a
“RomWBW System”. Modern computer means something like a Windows,
Linux, or Macintosh computer. The modern computer is used to download
and extract the RomWBW distribution files as well as program the ROM and
disk images. RomWBW System refers to your Z80/Z180 computer that will be
running the RomWBW ROM and software applications.

## Procuring RomWBW

RomWBW is maintained on GitHub. The RomWBW repository is found at
[https://github.com/wwarthen/RomWBW](https://github.com/wwarthen/RomWBW%20%22https://github.com/wwarthen/RomWBW%22).
This repository contains all of the source code as well as distribution
releases of the RomWBW software.

Downloading the full release package from GitHub can be a little
confusing because the main repository page has a button labeled “Clone
or Download”. This button will download **only** the source code. To
retrieve the full release distribution including the pre-built ROM
images, you must go to the “Releases” page of the GitHub repository.
From the main RomWBW GitHub repository page, click on the “releases” tab
which will take you to the RomWBW Releases page. On this page, you will
see a list of the RomWBW releases. You may see some releases tagged as
“Pre-release” which you probably do not want unless you specifically
want work in progress. The most recent stable release will be tagged
“Latest release”. To download the desired release package, expand the
Assets dropdown and then choose the entry that ends in “-Package.zip”.

## Distribution Package

The RomWBW distribution is a compressed zip archive file. It is intended
that the contents of the zip file will be extracted on a modern computer
(not your Z80/Z180 computer). Any modern zip file management tool can be
used to do this.

The extracted files are organized in a directory hierarchy. Each of the
high-level directories has it’s own ReadMe.txt file describing the
contents in detail. In summary, these directories are:

| **Directory** | **Contents**                                                                                                                          |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| Binary        | The final output files of the build process are placed here. Most important, are the ROM images with the file names ending in “.rom”. |
| Doc           | Contains detailed documentation including the operating systems, RomWBW architecture, etc.                                            |
| Source        | Contains the source code files used to build the software, ROM images and disk images.                                                |
| Tools         | Contains the MS Windows programs that are used by the build process or that may be useful in setting up your system.                  |

## Installation

To install RomWBW, you will need to program your Z80/Z180 system’s ROM
with the appropriate ROM image from the RomWBW distribution. The
pre-built ROM image files are found in the Binary directory of the
distribution. Consult the following table to determine the correct ROM
image file for your system. It is critical that you pick the correct
image file.

| **Platform** | **ROM Image File** | **Console Port**            |
| ------------ | ------------------ | --------------------------- |
| SBC V1/V2    | SBC\_std.rom       | Onboard 16550 Serial Port   |
| Zeta V1      | ZETA\_std.rom      | Onboard 16550 Serial Port   |
| Zeta V2      | ZETA2\_std.rom     | Onboard 16550 Serial Port   |
| N8           | N8\_std.rom        | Z180 Primary ASCI Port      |
| Mark IV      | MK4\_std.rom       | Z180 Primary ASCI Port      |
| RC2014 Z80   | RCZ80\_std.rom     | ACIA or SIO/2 Serial Module |
| RC2014 Z180  | RCZ180\_nat.rom    | Z180 Primary ASCI Port      |
| RC2014 Z180  | RCZ180\_ext.rom    | Z180 Primary ASCI Port      |
| Easy Z80     | EZZ80\_std.rom     | Onboard SIO/2 Serial Port   |
| SC126        | SCZ180\_126.rom    | Z180 Primary ASCI Port      |
| SC130        | SCZ180\_130.rom    | Z180 Primary ASCI Port      |

Notes:

  - The RC2014 Platforms (Z80 and Z180) **must** include a 512K RAM/ROM
    module.

  - The RC2014 Z80 platform ROM **must** include a serial port module
    (either ACIA or SIO/2). RomWBW will auto detect the the available
    serial port module and the console will be directed to whichever one
    is present. The SIO/2 module will take precedence if both are
    present.

  - The SBC V1 has a known race condition when bank switching. Since
    RomWBW performs frequent bank switching, the SBC V1 may not work
    with RomWBW. This issue was resolved on the SBC V2.

If you do not see any .rom files in the Binary directory of your RomWBW
distribution, you have probably downloaded a “source only” version of
the distribution. Please review the instructions in the “Procuring
RomWBW” section above making sure to get the full release package
distribution.

The pre-built ROM images are configured to automatically detect and
support the most common devices for the associated hardware platform
including serial ports, disk drives, real time clock, video adapters,
etc. without building a custom ROM.

All pre-built ROM images are 512KB. If your system utilizes a larger ROM
size, you can just program the image into the first 512KB of the ROM (a
custom ROM can be created later to utilize all of the available ROM
space).

The ReadMe.txt file in the Binary directory has more detailed
information on the configuration of each pre-built ROM image such as
specific hardware supported.

Unless you already have a working RomWBW system, you will need to use a
ROM programmer attached to your modern computer to program your ROM. If
your RomWBW system is already functional, depending on your system’s
capabilities, you may be able to reprogram the existing ROM in-situ.

If you do have an existing working RomWBW system, it is safest to
program a new ROM chip so that you can return to the known working ROM
chip if the new one fails to boot.

### Using a ROM Programmer

Any commercially available ROM programmer that is compatible with the
ROM chip used in your RomWBW system can be used. Additional guidance on
ROM programmers can usually be found by searching the online support
group postings for your hardware.

You will need to refer to your ROM programmer’s documentation for
detailed instructions. However, in general, you will need to perform the
following steps on your modern computer. These steps assume you have
already attached the ROM programmer to your modern system and installed
the programmer’s software.

1.  Launch the ROM programming software and ensure that the software is
    successfully communicating with your ROM programmer.

2.  Select the ROM chip type. This is critical, but is usually a simple
    matter to looking at the chip label and looking it up in the
    software.

3.  Load the ROM image into the programming software. This is where you
    must be sure to pick the correct file based on the previous table.
    Your programming software may ask about the format of the file being
    loaded. The RomWBW ROM files are raw binary images – they are not
    encoded in any way.

4.  Most programming software has a function to view/edit the loaded
    image. You can use this function to browse the first 256 bytes of
    the loaded file to make sure you see text similar to the following:
    
    “ROMWBW v2.9.2, Copyright (C) 2019, Wayne Warthen, GNU GPL v3”

5.  Mount the ROM chip in the programmer according to the programmer’s
    instructions.

6.  Execute the program function. Normally, the programmer will erase
    the ROM, program the new image, and then verify the image. Confirm
    that all of these steps completed without error.

7.  Remove the ROM chip from the programmer

8.  Make sure that the power is removed from your RomWBW System and
    install the programmed ROM chip in the correct position and
    orientation.

### Programming a ROM In-situ

If you already have a working RomWBW System, you may be able to
reprogram your existing ROM chip in-situ (in place). RomWBW fully
supports this, but your specific hardware must also support it. Your ROM
chip will need to be a Flash or EEPROM device and your system must
support writing to the ROM device. You must also have a working CP/M
drive with at least 512KB of space available. The RAM drive is not large
enough.

Note that this process is inherently dangerous. You will be
reprogramming a working ROM chip with no guarantee that the chip will
work afterwards. If you accidentally choose the wrong ROM image or the
programming process fails in some way, you could end up with a bricked
system. The only way to recover from this is to use a ROM Programmer as
described in the previous section.

Programming in-situ involves two steps: 1) transferring the ROM image
file to your RomWBW system, and 2) running the FLASH application.

There are a few different methods for transferring the ROM image file to
your RomWBW system. Please refer to the section of this document called
“Transferring Files” for instructions and options for doing this. Be
sure to transfer the right ROM image by referring to the table above.

Once the new ROM image is on your RomWBW system, run the FLASH
application to reprogram your existing ROM. The FLASH application is
located on your existing ROM drive (normally B:). Consult
Doc\\Contrib\\Flash4.txt for detailed instructions on using the FLASH
application. The following is an example of using the FLASH application
assuming your new ROM image file has been placed on your C: drive and is
called ROM.IMG.

-----

    B>flash write c:rom.img
    FLASH4 by Will Sowerbutts <will@sowerbutts.com> version 1.2.3
    
    Using RomWBW (v2.6+) bank switching.
    Flash memory chip ID is 0xBFB7: 39F040
    Flash memory has 128 sectors of 4096 bytes, total 512KB
    Write complete: Reprogrammed 13/128 sectors.
    Verify (128 sectors) complete: OK!

-----

Do not interrupt the flashing process once it has started or your ROM
chip will be corrupted.

When the FLASH application is done, your system will still be running on
a RAM copy of the previous ROM. You will need to reboot or power cycle
your system to load your system with the updated ROM software.

## Console Connection

RomWBW requires a console terminal be attached to the primary serial
port for your system. The console terminal is most often a modern
computer running terminal emulation software (e.g., Tera Term). A
classic CRT terminal can also be used if your system supports true
RS-232 connectivity.

The specific cable connection from your RomWBW System to your console
depends on your system’s hardware. Your system hardware documentation
should cover this. If a RS-232 connection is being used, it is likely
that you will need a null-modem cable or adapter. Be sure that you use
the primary serial port connection on your RomWBW system as indicated in
the previous table.

Your console terminal (or communications software) should be configured
as follows:

|              |            |
| ------------ | ---------- |
| Baud Rate    | 38,400 bps |
| Data Bits    | 8          |
| Stop Bits    | 1          |
| Parity       | None       |
| Flow Control | RTS/CTS    |

Notes:

  - The baud rate on some systems is determined by hardware
    configuration. Notably, the RC2014 Z80 platform is normally
    configured for 115,200 baud. In such cases, you will need to use the
    hardware configured baud rate.

  - Some applications distributed with RomWBW use escape sequences to
    produce full screen displays. These applications are normally expect
    VT-100 (or ANSI) terminal text processing. So, if possible, you
    should configure your terminal emulation software for VT-100 or ANSI
    terminal emulation.

  - SBC and Zeta Systems have a configuration jumper which can be used
    to direct console output to a dedicated VGA display with PS/2
    keyboard. It is highly recommended that you get the primary serial
    port working first. So, initially, please ensure that jumper JP2 on
    the SBC or jumper JP1 on the Zeta is *shorted* which will direct
    console output to the serial port.

## System Startup

Refer to your system’s hardware documentation for the startup procedure.
Normally, this is simply a matter of toggling the power switch on. If
the system is already running, a reset pushbutton is usually available
and will perform the same actions as cycling power.

If your system has a real time clock, then RomWBW will take 1-2 seconds
to measure the speed of the CPU **before** you see any output on the
console.

The first phase of RomWBW loads and initializes the RomWBW Hardware BIOS
(HBIOS). The HBIOS contains all of the hardware specific code for
RomWBW. During this phase, the following occurs:

1.  The HBIOS code is copied from ROM to RAM which is where it will
    actually run. RomWBW shadows the hardware BIOS in RAM for execution.

2.  If a real time clock is present, it is used to measure the speed of
    the CPU.

3.  Basic hardware discovery and initialization is performed including
    the serial ports which allow console output to be performed.

4.  A system banner with version information is displayed.

5.  All additional hardware devices are discovered and initialized. As
    this is done, the configuration of each device is printed on the
    console. This is referred to as the boot log.

6.  A summary table of all active device units (disk, character, etc.)
    is displayed. This is referred to as the unit summary table. The
    concept of units is described later in this document.

7.  Finally, the boot loader program is loaded and executed.

The following is an example of the initial output of the system
including the banner and device discovery/initialization boot log. The
contents of your boot log will be different based on your hardware
configuration. This information is primarily of use when troubleshooting
an issue with your system. You should always include the boot log when
requesting assistance with a hardware issue.

-----

    RetroBrew HBIOS v2.9.2, 2019-10-22
    
    SC126 Z8S180-N @ 18.432MHz IO=0xC0
    0 MEM W/S, 2 I/O W/S, INT MODE 2
    512KB ROM, 512KB RAM
    
    ASCI0: IO=0xC0 ASCI W/BRG MODE=38400,8,N,1
    ASCI1: IO=0xC1 ASCI W/BRG MODE=38400,8,N,1
    DSRTC: MODE=STD IO=0x0C Tue 2019-10-22 18:10:23 CHARGE=OFF
    MD: UNITS=2 ROMDISK=384KB RAMDISK=384KB
    SD: MODE=SC OPR=0x0C CNTR=0xCA TRDR=0xCB DEVICES=1
    SD0: SDHC NAME=SE32G BLOCKS=0x03B72400 SIZE=30436MB

-----

After the boot log, you will see the unit summary table. Here is an
example of this. Again, what you see will be different based on your
system hardware.

-----

    Unit        Device      Type              Capacity/Mode
    ----------  ----------  ----------------  --------------------
    Char 0      ASCI0:      RS-232            38400,8,N,1
    Char 1      ASCI1:      RS-232            38400,8,N,1
    Disk 0      MD1:        RAM Disk          384KB,LBA
    Disk 1      MD0:        ROM Disk          384KB,LBA
    Disk 2      SD0:        SD Card           30436MB,LBA

-----

## Boot Loader

The last step of the system startup is loading and executing the boot
loader. This is a simple program that allows you to choose what you want
the system to do. You may select options to start an operating system,
invoke ROM BASIC, run the Debug Monitor, or load an operating system
from a disk device.

It will start by printing a simple menu and then wait for a key to be
pressed indicating what should be loaded. Here is an example of the boot
loader’s output.

-----

    SC126 Boot Loader
    
    ROM: (M)onitor (C)P/M (Z)-System (F)orth (B)ASIC (T)-BASIC
    Disk: (0)MD1 (1)MD0 (2)SD0
    
    Boot Selection?

-----

The specific options listed will depend on your systems hardware
capabilities and configuration. The options following “ROM:” will load
and run your selection directly from the ROM. Each of these options is
briefly described below. The options following “Disk:” indicate the
available disk drives. Selecting a disk drive will load whatever
software has been installed on the boot track of the selected drive. See
the section on “Preparing a Boot Disk” for more information on making a
bootable disk. Note that although MD1: (RAM Drive) and MD0: (ROM Drive)
are listed, they do not currently have a boot track, so attempting to
select them will always result in a “Disk not bootable\!” error.

  - Monitor  
    is a basic hardware debug monitor which has the ability to examine
    and modify memory and I/O ports, load an Intel Hex File, etc. From
    the Monitor’s ‘\>’ prompt, you can enter ‘H’ for a menu of all
    available commands.

  - CP/M  
    is the CP/M-80 v2.2 Operating System from Digital Research which has
    been adapted for operation under RomWBW’s HBIOS. A soft copy of the
    CP/M manual is included in the RomWBW distribution “Doc” directory
    in the file called “CPM Manual.pdf”.
    
    All of the standard CP/M applications documented in the manual are
    included on the embedded ROM Drive (normally drive B:). The ROM
    drive also includes some custom RomwWBW applications that enhance
    the functionality of CP/M under RomWBW – these are documented in the
    “RomWBW Applications” section below.

  - Z-System  
    is a popular alternative operating system that is very compatible
    with CP/M, but offers some nice enhancements such as date/time
    stamping of files, search paths, etc. Z-System is actually made up
    of two components, specifically ZSDOS v1.1 (the DOS portion) and
    ZCPR (the command processor portion).
    
    Soft copies of the ZSDOS and ZCPR manuals are included in the RomWBW
    distribution “Doc” directory in the files called “ZSDOS Manual.pdf”
    and “ZCPR Manual.pdf”. Z-System will run all of the standard CP/M
    applications as well as some ZSDOS specific applications as
    documented in the manuals. The custom RomWBW applications that
    enhance the functionality of RomWBW also run under Z-System.

  - Forth  
    is a standalone implementation of CamelForth for the Zilog Z80. It
    does not provide any mechanism for saving/loading programs or
    accessing disk drives. Credit to Phillip Summers for contributing
    the RomWBW adaptation of this application. Note that commands must
    be typed in uppercase to be recognized.

  - BASIC  
    is a standalone implementation of Microsoft Z80 BASIC. When
    launched, you will receive the prompt “Memory top?”. You may simply
    press \<return\> at this prompt if you want to use maximum available
    memory. It does not provide any mechanism for saving/loading
    programs or accessing disk drives. Credit to Phillip Summers for
    contributing the RomWBW adaptation of this application.

  - T-BASIC  
    is a standalone implementation of Tasty BASIC which is based on Palo
    Alto Tiny Basic. It does not provide any mechanism for
    saving/loading programs or accessing disk drives. Credit to Phillip
    Summers and Dimitri Theulings for contributing the RomWBW adaptation
    of this application.

  - DSKY  
    launches the DSKY interface of the Monitor. This option is only
    presented if your system is configured for a DSKY board. The use of
    the DSKY is documented in the “DSKY.pdf” file in the “Doc” directory
    of the RomWBW distribution.

# Devices and Units

In order to support a wide variety of hardware, RomWBW HBIOS uses a
modular approach to implementing device drivers and presenting devices
to the operating system. In general, all devices are classified as one
of the following:

  - Disk (Hard Disk, CF Card, SD Card, RAM/ROM Disk, etc.)
  - Character (Serial Ports, Parallel Ports, etc.)
  - Video (Video Display/Keyboard Interfaces)
  - RTC/NVRAM (Real Time Clock, Non-volatile RAM)

HBIOS uses the concept of unit numbers to present a complex set of
hardware devices to the operating system. As an example, a typical
system might have a ROM Disk, RAM Disk, Floppy Drives, and Disk Drives.
All of these are considered Disk devices and are presented to the
operating system as generic block devices. This means that the operating
system does not need to understand the difference between a floppy drive
and a ROM disk.

As RomWBW boots, it assigns a unit number to each device. This unit
number is used by the operating system to refer to the device. It is,
therefore, important to know the unit number assigned to each device.
This information is displayed in the unit summary table at startup. Here
is an example:

-----

    Unit        Device      Type              Capacity/Mode
    ----------  ----------  ----------------  --------------------
    Disk 0      MD1:        RAM Disk          384KB,LBA
    Disk 1      MD0:        ROM Disk          384KB,LBA
    Disk 2      FD0:        Floppy Disk       3.5",DS/HD,CHS
    Disk 3      FD1:        Floppy Disk       3.5",DS/HD,CHS
    Disk 4      PPIDE0:     CompactFlash      3815MB,LBA
    Disk 5      PPIDE1:     Hard Disk         --
    Disk 6      PRPSD0:     SD Card           1886MB,LBA
    Char 0      UART0:      RS-232            38400,8,N,1
    Char 1      UART1:      RS-232            38400,8,N,1
    Video 0     CVDU0:      CRT               Text,80x25

-----

In this example, you can see that the system has a total of 7 Disk Units
numbered 0-6. There are also 2 Character Units and 1 Video Unit. The
table shows the unit numbers assigned to each of the devices.

There may or may not be media in the disk devices listed. For example,
the floppy disk devices (Disk Units 2 & 3) may not have a floppy in the
drive. Also note that Disk Unit 4 shows a disk capacity, but Disk Unit 5
does not. This is because the PPIDE interface of the system supports up
to two drives, but there is only one actual drive attached. A unit
number is assigned to all possible devices regardless of whether they
have actual media installed at boot time.

Note that Character Unit 0 is **always** the initial system console by
definition.

If your system has an RTC/NVRAM device, it will not be listed in the
unit summary table. Since only a single RTC/NVRAM device can exist in
one system, unit numbers are not required nor used for this type of
device.

# Using Disk Drives

The operating systems that run on RomWBW (CP/M & Z-System) use letters
to refer to disk drives. As your chosen operating system loads it will
display a table that shows the drive letters that have been assigned to
each disk. Here is an example that matches the unit assignment table
shown in the previous section:

-----

``` 
   A:=MD1:0
   B:=MD0:0
   C:=FD0:0
   D:=FD1:0
   E:=PPIDE0:0
   F:=PPIDE0:1
   G:=PPIDE1:0
   H:=PPIDE1:1
   I:=PRPSD0:0
   J:=PRPSD0:1
```

-----

You can see that drive letter A has been assigned to device MD1. If you
look at the earlier unit assignment table, you can see that MD1 is the
ROM disk so any reference to A: will be directed to the ROM disk.
Similarly, any reference to C: will be directed to floppy device FD0.

The references to devices in the unit assignment table end with a ’:"
followed by a number. RomWBW breaks

You will notice that A: refers to MD1 and B: refers to MD0, which seems
out of order. This is intentional and forces A: to always refer to the
RAM disk. CP/M and similar operating systems assume that drive A: can be
used to write temporary data (SUBMIT files primarily). If drive A: were
assigned to the ROM disk, this would fail. To prevent this problem,
drive A: is assigned to the RAM disk.

## Disk Slices

CP/M and similar operating systems are somewhat limited in the size of
the disk drive they can handle with 8MB being the largest that all of
them support consistently. Since modern disk devices (including CF Cards
and SD Cards) typically provide far larger storage capacity than 8MB,
the concept of slices is used to allow the operating system to utilize
more than 8MB on a single device. You can think of a disk device being
divided up into 8MB slices and each slice can then be utilized as an
independent drive.

In the drive letter assignment table above, you will see a ‘:’ and a
number after each device. The number represents the slice on the disk
device. So, E: refers to the first 8MB slice on the disk attached to
IDE0 and F: refers to the second 8MB slice of the same physical disk.
RomWBW allows up to 256 slices on each disk device meaning that you can
access up to 2GB of a disk. Note that RomWBW is aware that not all types
of disk devices are large enough for slicing. So, you will see that RAM
disks, ROM disks, and floppy disks will never be assigned multiple
slices – they will always use slice 0.

The operating systems support only 16 drive letters (A: - P:). So,
during startup, RomWBW attempts to assign as many drive letters as
possible across all available disk devices. However, since a hard disk
device can have up to 256 slices, there will never be enough drive
letters available to assign to all possible slices. To access the slices
that are not initially assigned, you can use the ASSIGN program to add,
modify, swap, delete, or list drive letter assignments while the
operating system is running.

## Preparing Disks for Data

Depending on the type of disk device you are accessing, you may need to
prepare it before using it for the first time. The exception to this
rule are the RAM and ROM disks. At startup, the RAM disk is checked and
automatically formatted as needed with a blank file system. This is
normally drive letter A: and files can immediately be copied to it.
Since the ROM disk simply refers to an area of the programmed ROM chip,
it is already formatted and contains the standard program files for
RomWBW. This is generally drive letter B: and you can immediately run
programs from this drive.

Hard drives (including CF Cards and SD Cards) do not require a physical
format, but they do require that the directory area be cleared before
files can be placed on it. Note that each slice of a hard disk is an
entirely independent filesystem and must be cleared. So, if you wanted
to use 4 slices on a disk device the slices were assigned to drive
letters E: - H:, you would need to run the clear program 4 times
specifying each of the four drive letters. To clear the directory area
of a drive you would use the CLRDIR program like this:

    B>clrdir e:
    CLRDIR V-0.4 (06-Aug-2012) by Max Scane
    
    Warning - this utility will overwite the directory sectors of Drive: E:
    Type Y to proceed any key other key to exit. Y
    Directory cleared.

This clears the directory area of E: and it would then be ready to copy
files onto it. Note that when using CLRDIR, you must press a capital Y
to confirm clearing the directory. CLRDIR will destroy the previous
contents of the corresponding slice of the disk device, but will not
affect the other slices.

Floppy disks are similar to hard disks, but they do require a physical
format operation. Formatting of floppy disks can be done using the FDU
program provided on the ROM drive. This program can perform a variety of
functions on floppy drives, but one of them is a format operation. You
will need to choose your hardware and the correct disk format. Once a
floppy disk is formatted, it is not necessary to use CLRDIR on it. The
format operation automatically clears the directory area.

Unfortunately, there is no standardization in the disk formats used by
CP/M and similar operating systems. For this reason, disks prepared by
non-RomWBW systems will not be compatible with RomWBW. However, disks
can be moved freely between any system running RomWBW and they will be
compatible.

## Making a Disk Bootable

While RomWBW provides the convenience of loading multiple operating
systems directly from ROM, CP/M was traditionally booted from the system
tracks of a disk. The RomWBW boot loader fully supports this and it may
be useful if you want to install another operating system or modify one
of the default operating systems.

To install an operating system image on the boot tracks of a disk, use
the SYSCOPY program. The ROM disk includes system images for both CP/M
and Z-System. These files are called CPM.SYS and ZSYS.SYS. The following
is an example of using the SYSCOPY command to install a copy of the
Z-System operating system image onto drive E:

    B>syscopy e:=zsys.sys
    
    Transfer system image from B:ZSYS.SYS to E: (Y/N)? y
    Reading image... Writing image... Done

After rebooting the system, you can now choose the device corresponding
to E: to boot from. Here is a sample of this operation:

    SBC Z80 Boot Loader
    
    ROM: (M)onitor (C)P/M (Z)-System (F)orth (B)ASIC (T)-BASIC (D)SKY
    Disk: (0)MD1 (1)MD0 (2)FD0 (3)FD1 (4)PPIDE0 (5)PPIDE1 (6)PRPSD0
    
    Boot Selection? 4
    
    Booting Disk Unit 04...
    
    Reading disk information...
    Loc=D000 End=FE00 Ent=E600 Label=Unlabeled Drive
    
    Loading...

At this time, the RomWBW boot loader will only allow booting from slice
0 of a disk device. So, you must install your custom operating system to
the first slice of the target disk device.

# Transferring Files

In general, all of the standard operating system and RomWBW program
files are included on the ROM disk. As such, you will immediately have
access to most of the tools you would want. However, if you want to run
other applications (perhaps WordStar), you will need to transfer them to
your RomWBW system. There are two basic ways to do this: 1) transfer the
files over your serial port using the XModem protocol, or 2) prepare a
disk image on your Windows or Mac computer. XModem works well for
transferring a small number of files. Using a disk image works best for
more files.

## XModem Transfers

RomWBW includes a standard implementation of the XModem protocol on the
ROM disk. Assuming you are using a communications program on your modern
computer that supports the XModem protocol, it is fairly simple to copy
files to/from your RomWBW system.

The procedure is to start the XModem program on your RomWBW system
first, then invoke the XModem transfer from the communications software
on your modern computer. Here is an example of starting XModem on RomWBW
to receive a file called “SAMPLE.TXT”:

    A>b:xm r sample.txt
    
    XMODEM v12.5 - 07/13/86
    RBC, 06-Jun-2018 [WBW], ASCI0
    
    Receiving: A0:SAMPLE.TXT
    376k available for uploads
    File open - ready to receive
    To cancel: Ctrl-X, pause, Ctrl-X

When using XModem to receive a file, you need to be careful that the
incoming file is not being stored on your ROM drive. Since the ROM drive
cannot be written, the transfer will fail mysteriously. Note in the
example above that the current drive is A: which will be the default
drive for storing the file. The ‘XM’ command is prefixed with B: because
this is the drive here the program is stored.

Be careful that the target drive has enough space for the incoming file.
If it does not, the transfer will fail and you will not get a clear
message indicating the reason.

It is critical that your communication path be working well for XModem
to be reliable. In general, this means that the modern computer cannot
send data faster then your RomWBW system can handle it. This can be
assured either by using a baud rate that is slow enough to prevent
overrun or by making sure that flow control is active and working
properly on the communication channel.

The XModem software automatically uses the primary serial port to
send/receive data. This is the same port that the console uses at
startup (Character Unit 0).

## Disk Image Transfers

The RomWBW distribution includes a program suite called cpmtools. This
toolset allows you to create a file that is a binary image of a CP/M
file system. The image file can then be transferred to a CF Card or SD
Card and subsequently used in a RomWBW system. The following describes
the process for a Windows computer.

First, you need to create the desired disk image. In the RomWBW
distribution, the Source\\Images subdirectory contains instructions in
the ReadMe.txt file for generating a disk image. In summary, you copy
files into a directory structure that corresponds to slices and user
areas of a disk. You then run a script that generates a file that
represents an image of the resultant CP/M disk.

Second, you transfer the image file onto a CF Card or SD Card. The
RomWBW distribution includes a program called Win32DiskImager that can
do this.

Finally, you install the CF Card or SD Card into your RomWBW system and
boot the system normally. At this point, you should find the files you
placed in the image available at the drive letter(s) of the device where
the card is installed.

# Applications

A typical set of CP/M-80 2.2 and Z-System applications are included on
the ROM file system. The Doc directory of the distribution contains the
CP/M, ZSDOS, and ZCPR user manuals which provide the primary usage
information. Note that the Z-System applications will generally not run
under CP/M.

The following custom RomWBW applications are included on the ROM disk to
enhance the operation of RomWBW:

  - ASSIGN  
    Add, change, and delete drive letter assignments. Use `ASSIGN /?`
    for usage instructions.

  - SYSCOPY  
    Copy system image to a device to make it bootable. Use `SYSCOPY`
    with no parms for usage instructions.

  - FDU  
    Format and test floppy disks. The interface is menu driven.

  - OSLDR  
    Load a new OS on the fly. For example, you can switch to Z-System
    when running CP/M. Use `OSLDR` with no parms for usage instructions.

  - FORMAT  
    Will someday be a command line tool to format floppy disks.
    Currently does nothing\!

  - MODE  
    Reconfigures serial ports dynamically.

  - XM  
    XModem file transfer program adapted to hardware. Automatically uses
    primary serial port on system.

  - FLASH  
    Will Sowerbutts’ in-situ EEPROM programming utility.

Please see the [RomWBW
Applications](https://www.retrobrewcomputers.org/doku.php?id=software:firmwareos:romwbw:apps "software:firmwareos:romwbw:apps")
page for more information on using these applications.

Check the
[Errata](https://www.retrobrewcomputers.org/doku.php?id=software:firmwareos:romwbw:errata "software:firmwareos:romwbw:errata")
page for current usage notes.

# UNA Hardware BIOS

John Coffman has produced a new generation of hardware BIOS called UNA.
The standard RomWBW distribution includes it’s own hardware BIOS.
However, RomWBW can alternatively be constructed with UNA as the
hardware BIOS portion of the ROM. If you wish to use the UNA variant of
RomWBW, then just program your ROM with the ROM image called
“UNA\_std.rom” in the Binary directory. This one image is suitable on
**all** of the platforms and hardware UNA supports.

UNA is customized dynamically using a ROM based setup routine and the
setup is persisted in the system NVRAM of the RTC chip. This means that
the single UNA-based ROM image can be used on most of the RetroBrew
platforms and is easily customized. UNA also supports FAT file system
access that can be used for in-situ ROM programming and loading system
images.

While John is likely to enhance UNA over time, there are currently a few
things that UNA does not support:

  - Floppy Drives
  - VT-100 Escape Code Processing
  - Zeta 1 and N8 Systems
  - RC2014 Systems
  - Some older support boards

The UNA version embedded in RomWBW is the latest production release of
UNA. RomWBW will be updated with John’s upcoming UNA release with
support for VGA3 as soon as it reaches production status.

Please refer to the [UNA BIOS Firmware
Page](https://www.retrobrewcomputers.org/doku.php?id=software:firmwareos:una:start "software:firmwareos:una:start")
for more information on UNA.

# CP/M vs. Z-System

There are two OS variants included in this distribution and you may
choose which one you prefer to use. Both variants are now included in
the pre-built ROM images. You will be given the choice to boot either
CP/M or Z-System at startup.

The traditional Digital Research (DRI) CP/M OS is the first choice. The
Doc directory contains a manual for CP/M usage (“CPM Manual.pdf”). If
you are new to the RetroBrew Computer systems, I would currently
recommend using the CP/M variant to start with simply because it has
gone through more testing and you are less likely to encounter problems.

The other choice is to use the most popular non-DRI CP/M “clone” which
is generally referred to as Z-System. Z-System is intended to be an
enhanced version of CP/M and should run all CP/M 2.2 applications. It is
optimized for the Z80 CPU (as opposed to 8080 for CP/M) and has some
significant improvements such as date/time stamping of files. For
further information on the RomWBW implementation of Z-System, see the
wiki page [Z-System
Notes](https://www.retrobrewcomputers.org/doku.php?id=playground:wwarthen:zsystem "playground:wwarthen:zsystem").
Additionally, the official documentation for Z-System is included in the
RomWBW distribution Doc directory (“ZSDOS Manual.pdf” and “ZCPR
Manual.pdf”).

# ROM Customization

The pre-built ROM images are configured for the basic capabilities of
each platform. If you add board(s) to your system, you will need to
customize your ROM image to include support for the added board(s).

Essentially, the creation of a custom ROM is accomplished by updating a
small configuration file, then running a script to compile the software
and generate the custom ROM image. At this time, the build process runs
on Windows 32 or 64 bit versions. All tools (compilers, assemblers,
etc.) are included in the distribution, so it is not necessary to setup
a build environment on your computer.

For those who are interested in more than basic system customization,
note that all source code is provided (including the operating systems).

Complete documentation of the customization process is found in the
ReadMe.txt file in the Source directory.

Note that the ROM customization process does not apply to UNA. All UNA
customization is performed within the ROM setup script.

# Upgrading

Program a new ROM chip from an image in the new distribution. Install
the new ROM chip and boot your system. At the boot loader “Boot:”
prompt, select either CP/M or Z-System to load the corresponding OS
directly from ROM.

If you have spare ROM chips for your system, it is always safest to keep
your existing, working ROM chip and program a new one with the new
firmware. If the new one fails to boot, you can easily return to the
known working ROM.

If you use a customized ROM image, it is recommended that you first try
a pre-built ROM image first and then move on to generating a custom
image.

It is entirely possible to reprogram your system ROM using the FLASH
utility from Will Sowerbutts on your ROM drive (B:). In this case, you
would need to transfer the new ROM image to your system using XModem.
Obviously, there is some risk to this approach since any issues with the
programming or ROM image could result in a non-functional system.

Once you have successfully booted your system with the new ROM, you
should update the system software from previous releases that you may
have copied to disk drives. This is described below.

If your system has any bootable drives, then update the OS image on each
drive using SYSCOPY. For example, if C: is a bootable drive with the
Z-System OS, you would update the OS image on this drive with the
command:

``` code
 B>SYSCOPY C:=B:ZSYS.SYS
```

If you have copies of any of the system utilities on drives other than
the ROM disk drive, you need to copy the latest version of the programs
from the ROM drive (B:) to any drives containing these programs. For
example, if you have a copy of the ASSIGN.COM program on C:, you would
update it from the new ROM using the COPY command:

``` code
 B>COPY B:ASSIGN.COM C:
```

The following programs are maintained with the ROM images and all copies
of these programs should be updated when upgrading to a new ROM version:

  - ASSIGN.COM
  - FORMAT.COM
  - OSLDR.COM
  - SYSCOPY.COM
  - TALK.COM
  - FDU.COM (previously FDTST.COM)
  - XM.COM
  - MODE.COM
  - RTC.COM

# Acknowledgements

While I have heavily modified much of the code, I want to acknowledge
that much of the work is derived or copied from the work of others in
the RetroBrew Computers Community including Andrew Lynch, Dan Werner,
Max Scane, David Giles, John Coffman, and probably many others I am not
clearly aware of (let me know if I omitted someone\!).

I especially want to credit Douglas Goodall for contributing code, time,
testing, and advice. He created an entire suite of application programs
to enhance the use of RomWBW. However, he is looking for someone to
continue the maintenance of these applications and they have become
unusable due to changes within RomWBW. As of RomWBW 2.6, these
applications are no longer provided.

David Giles has contributed support for the CSIO support in the SD Card
driver.

Ed Brindley has contributed some of the code that supports the RC2014
platform.

Phil Summers has contributed a variety of updates including the
ROM-based versions of Forth and BASIC.

UNA BIOS is a product of John Coffman.

# Getting Assistance

The best way to get assistance with RomWBW or any aspect of the
RetroBrew Computers projects is via the community forum.

[https://www.retrobrewcomputers.org/forum/](https://www.retrobrewcomputers.org/forum/ "https://www.retrobrewcomputers.org/forum/")

Also feel free to email Wayne Warthen at
[wwarthen@gmail.com](mailto:wwarthen@gmail.com "wwarthen@gmail.com").
