# This repository is an attempt to make it easier to build the RepRap firmware for the Duet3 for our Arcitype Project

- Build environment is Win11 with Eclipse CDT
- We're testing on 3.5-dev
- Target is the Duet 3D board

## Initial setup

Run through the initial setup [here](https://github.com/Duet3D/RepRapFirmware/wiki/Building-RepRapFirmware#instructions-for-building-under-windows)

Notes: 

- We installed the latest 12.12 ARM compiler toolchain [here](https://developer.arm.com/-/media/Files/downloads/gnu/12.2.mpacbti-rel1/binrel/arm-gnu-toolchain-12.2.mpacbti-rel1-mingw-w64-i686-arm-none-eabi.exe?rev=2c8597f70cd94dfc9603560fb5da44a4&hash=23CEA8B2920A5A0A3BB64F6109902BB227E29729)

*NOTE: That you want to check the installer box to add this to the system path*

- Then the latest Eclipse CDT [here](https://www.eclipse.org/downloads/packages/release/2023-12/r/eclipse-ide-cc-developers)

- For MSYS2 we used the installer [here](https://github.com/msys2/msys2-installer/releases/download/2024-01-13/msys2-x86_64-20240113.exe)

- Run the MSYS shell at the end of the installation and then you need to install the `make` utility with

```
pacman -S make
```

- Then you need to open the Windows System Settings and edit the Environment variable `PATH` to add the path to make to the end

```
existing stuff... ;C:\msys64\usr\bin
```

- Open a new `cmd` box and check you can type `make` and the command runs OK

## Checking out the repo

- Open new cmd box and run `eclipse` from command line in this box (from wherever you downloaded the CDT to)

*NOTE: You have to do this to make sure the system path settings are correctly picked up. Although a reboot might sort things out 

- Import a project, go to the Git->Projects from git with smart import->clone URI. Enter the URL for this repo

- You can use `master` branch. We're currently working with 3.5-dev usptream

- Check `clone submodules (takes time as there's a load of upstream sub-sub modules we don't need)

- Check search for nested projects

- It'll all import and you'll end up showing a number of projects

## Building

- Right click on `RepRapFirmware` project and choose configuration to set active target to `Duet3_MB6XD`

- Next edit the properties of RepRapFirmware project (right click) and go to C/C++ Build->Environment. Edit `PATH` and add the following to the end of the line

```
existing stuff... ;${ProjDirPath}\Tools\CrcAppender\win-x86-64
```

- Build RepRepFirmware (will build dependent projects)

```
Finished building target: Duet3Firmware_MB6XD.elf
 
Generating binary file
arm-none-eabi-objcopy -O binary "C:\Users\ajlen\git\RepRapFirmware-Builder\RepRapFirmware\Duet3_MB6XD/Duet3Firmware_MB6XD.elf" "C:\Users\ajlen\git\RepRapFirmware-Builder\RepRapFirmware\Duet3_MB6XD/Duet3Firmware_MB6XD.bin" && CrcAppender "C:\Users\ajlen\git\RepRapFirmware-Builder\RepRapFirmware\Duet3_MB6XD/Duet3Firmware_MB6XD.bin"
Firmware binary: C:\Users\ajlen\git\RepRapFirmware-Builder\RepRapFirmware\Duet3_MB6XD\Duet3Firmware_MB6XD.bin
CRC32 = 0xF5A26021
```
