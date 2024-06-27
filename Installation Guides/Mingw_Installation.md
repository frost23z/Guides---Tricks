# Mingw Installation

This document will guide you through the installation of Mingw on Windows using msys2.

1.  Download msys2 from [here](https://www.msys2.org/) and install it.

2.  Open the msys2 msys terminal and run the following commands:

    1.  Update the package database and base packages using following command:

            pacman -Syu

    2.  Update rest of the base packages

            pacman -Su

3.  Now open the msys2 mingw terminal and run the following commands to install gcc and g++ for C and C++

    For 64 bit

        pacman -S mingw-w64-x86_64-gcc

    For 32 bit

        pacman -S mingw-w64-i686-gcc

4.  To install the debugger (gdb) for C and C++

    For 64 bit

        pacman -S mingw-w64-x86_64-gdb

    For 32 bit

        pacman -S mingw-w64-i686-gdb

5.  Set the path of the mingw bin directory in the environment variables.

        C:\msys64\mingw64\bin

6.  Run the following command on any terminal to check if the installation was successful:

        gcc --version
        g++ --version
        gdb --version
