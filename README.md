Preparing the Windows build environment
=======================================

Currently (Qubes R3) all Windows Tools code is built using Visual Studio 2013 and WDK 8.1. Builder machine should run Windows 7 64-bit (other OS versions weren't tested). Prerequisites take a lot of space so 30-40 GB for HDD is recommended.
Builder scripts take care of getting/installing almost all needed prerequisites but some things need to be done by hand:

1. Install Service Pack 1 for Windows 7. Without it Visual Studio won't install.
2. Install Visual Studio 2013 Community [1] [3]. Deselect all optional components. Install in a path without spaces or suffer frustration with makefiles. VS requires .NET Framework 4.5 but I think the setup includes that.
3. Install Windows Driver Kit 8.1 [2].

If you're starting in a clean OS without Qubes Builder, the get-be powershell script initializes the build environment. Download it from here:

https://raw.githubusercontent.com/QubesOS/qubes-builder-windows/master/scripts/get-be.ps1

...and run:
`powershell -ExecutionPolicy bypass -f get-be.ps1`

The script:

* Prepares msys/mingw environment
* Clones qubes-builder
* Installs GPG and verifies code signatures
* Adds a msys shell shortcut to the start menu
* Generates a code signing certificate for Windows binaries (necessary for drivers). Use no password for testing.

Building Qubes Windows Tools
============================

Before building, prepare the appropriate `builder.conf` in the root of qubes-builder. Example config is provided as `windows-tools.conf`.
From the newly launched msys shell run:

* `make get-sources`
* `cd qubes-src\vmm-xen-windows-pvdrivers; git submodule update --init --recursive; cd -`
* `make qubes`

The finished installer will be in `qubes-src\installer-qubes-os-windows-tools`.

Building Qubes Windows Tools from a vanilla Windows install (work in progres)
===========================================================

Install Git for windows in default directory

Install Visual Studio 2013 community edition in c:\vs2013

`mkdir c:\QubesOS`

`cd c:\QubesOS`

Download `qubes-builder-windows/scripts/get-be.ps1` and save it in `c:\QubesOS` (TODO, verify signature)

Launch msys via shortcut

`cd /c/C/QubesOS/qubes-builder`

`cp example-configs/qubes-os-master.conf builder.conf`

edit builder.conf (remove comments for the windows components)

check git is here `ls /c/Program\ Files/Git/bin`

`export PATH=$PATH:/c/Programs\ Files/Git/bin`

`make COMPONENTS="builder-windows" get-sources`

`cp qubes-src/builder-windows/windows-tools.conf builder.conf`

edit builder.conf and uncomment the section mark in comments to be uncommented

`cd qubes-src/vmm-xen-windows-pvdrivers`

`make`

`cd /c/C/QubesOS/qubes-builder`

[1] https://www.visualstudio.com/en-us/products/visual-studio-community-vs.aspx

[2] https://www.microsoft.com/en-us/download/details.aspx?id=42273

[3] https://go.microsoft.com/?linkid=9863609
