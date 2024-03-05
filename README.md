# This repository is an attempt to make it easier to build the RepRap firmware for the Duet3

# Windows

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

- You can use `master` branch. We're currently working with 3.5-dev upstream

- Check `clone submodules (takes time as there's a load of upstream sub-sub modules we don't need)

- Check search for nested projects

- It'll all import and you'll end up showing a number of projects

- Next you need to right click on each of the projects and set the correct target project as per the table e.g. for Duet3 [here](https://github.com/Duet3D/RepRapFirmware/wiki/Building-RepRapFirmware#instructions-for-building-under-windows)
 
- ## Building

- Right click on `RepRapFirmware` project and choose configuration to set active target to `Duet3_MB6XD`

- Next edit the properties of RepRapFirmware project (right click) and go to C/C++ Build->Environment. Edit `PATH` and add the following to the end of the line

```
existing stuff... ;${ProjDirPath}\Tools\CrcAppender\win-x86-64
```

- You also need to have the dotNet framework installed for the CrcAppender to work. If you get errors (e.g. try running CrcAppender directly from the command line) then it gives a link to install the .NET framework
  
- Build RepRepFirmware (will build dependent projects)

```
Finished building target: Duet3Firmware_MB6XD.elf
 
Generating binary file
arm-none-eabi-objcopy -O binary "C:\Users\ajlen\git\RepRapFirmware-Builder\RepRapFirmware\Duet3_MB6XD/Duet3Firmware_MB6XD.elf" "C:\Users\ajlen\git\RepRapFirmware-Builder\RepRapFirmware\Duet3_MB6XD/Duet3Firmware_MB6XD.bin" && CrcAppender "C:\Users\ajlen\git\RepRapFirmware-Builder\RepRapFirmware\Duet3_MB6XD/Duet3Firmware_MB6XD.bin"
Firmware binary: C:\Users\ajlen\git\RepRapFirmware-Builder\RepRapFirmware\Duet3_MB6XD\Duet3Firmware_MB6XD.bin
CRC32 = 0xF5A26021
```

# Ubuntu 22.04 Linux

- Build environment is Ubuntu 22.04 with Eclipse CDT
- We're testing on 3.5-dev
- Target is the Duet 3D board

## Initial setup

Take a look at the the initial setup [here](https://github.com/Duet3D/RepRapFirmware/wiki/Building-RepRapFirmware#instructions-for-building-under-windows)

Notes: 

- You don't need MSYS2 (that's just for Windows)
  
- We installed the latest 12.3 ARM compiler toolchain for Linux [here](https://developer.arm.com/-/media/Files/downloads/gnu/12.3.rel1/binrel/arm-gnu-toolchain-12.3.rel1-x86_64-arm-none-eabi.tar.xz?rev=dccb66bb394240a98b87f0f24e70e87d&hash=97EE9A221DB712D24F9FB455395AF0D487F61BFE)

- Then the latest Eclipse CDT [here](https://www.eclipse.org/downloads/packages/release/2023-12/r/eclipse-ide-cc-developers)

## Checking out the repo

- Open new terminal box and run `eclipse` from command line in this box (from wherever you downloaded the CDT to)

*NOTE: You have to do this to make sure the system path settings are correctly picked up. Although a reboot might sort things out 

- Import a project, go to the Git->Projects from git with smart import->clone URI. Enter the URL for this repo

- You can use `master` branch. We're currently working with 3.5-dev upstream

- Check `clone submodules (takes time as there's a load of upstream sub-sub modules we don't need)

- Check search for nested projects

- It'll all import and you'll end up showing a number of projects

- You need to do a couple of things for the CrcAppender

- Go to where you checked out the repo and then make the CrcAppender application executable with

```
chmod a+x path to repo/RepRapFirmware-Builder/RepRapFirmware/Tools/CrcAppender/linux-x86_64/CrcAppender
```

- You also need to have dotNet 6.0 installed with

```
sudo apt-get install -y dotnet-sdk-6.0
```

- Next you need to right click on each of the projects and set the correct target project as per the table e.g. for Duet3 [here](https://github.com/Duet3D/RepRapFirmware/wiki/Building-RepRapFirmware#instructions-for-building-under-windows)

## Building

- Right click on `RepRapFirmware` project and choose configuration to set active target to `Duet3_MB6XD`

- Next edit the properties of RepRapFirmware project (right click) and go to C/C++ Build->Environment. Edit `PATH` and add the following to the end of the line

```
existing stuff... ;${ProjDirPath}\Tools\CrcAppender\win-x86-64
```

- Build RepRepFirmware (will build dependent projects)

```
Finished building target: Duet3Firmware_MB6XD.elf
 
17:58:53 **** Incremental Build of configuration Duet3_MB6XD for project RepRapFirmware ****
make -j20 all 
make[1]: Nothing to be done for 'main-build'.
Generating binary file
arm-none-eabi-objcopy -O binary "/home/ajlennon/git/RepRapFirmware-Builder/RepRapFirmware/Duet3_MB6XD/Duet3Firmware_MB6XD.elf" "/home/ajlennon/git/RepRapFirmware-Builder/RepRapFirmware/Duet3_MB6XD/Duet3Firmware_MB6XD.bin" && CrcAppender "/home/ajlennon/git/RepRapFirmware-Builder/RepRapFirmware/Duet3_MB6XD/Duet3Firmware_MB6XD.bin"
Firmware binary: /home/ajlennon/git/RepRapFirmware-Builder/RepRapFirmware/Duet3_MB6XD/Duet3Firmware_MB6XD.bin
CRC32 = 0x18EAFF98
```
