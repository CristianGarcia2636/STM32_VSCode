# Setup Instructions for Windows

- Download and the zip folder containing our toolchain (mingw-w64-i686) hosted cross toolchains, [arm-gnu-toolchain-12.2.MPACBTI-Rel1-mingw-w64-i686-arm-none-eabi.zip](https://developer.arm.com/downloads/-/arm-gnu-toolchain-downloads) and install the toolchain by running the setup executable.
- Download the latest version of the Windows [ST-LINK driver](https://www.st.com/en/development-tools/stsw-link009.html) and install by running the setup executable.
- Download the latest version of the [MinGW-W64](https://www.msys2.org/) via MSYS2 and install by following the steps included on the website.
- Install the MinGW-W64 toolchain by running the following command in your MinGW terminal: pacman -S --needed base-devel mingw-w64-x86_64-toolchain (br)
Accept the default to install all the members in the toolchain group.(br)
Add the path to your MinGW bin foled to your Windows PATH environment C:\msys64\mingw64\bin
- Install the latest version of [Visual Studio Code](https://code.visualstudio.com/download)(br)
Install the C/C++ Extension from the Extension Marketplace in VS Code. (br)
Install the CMake Tools extension for VS Code from the Extensions Marketplace.
- Install the latest version of Git and clone the [Bare Metal Programming Guide](https://github.com/cpq/bare-metal-programming-guide) into a project directory. (br)
In this project you will want to cd into the 'step' folder that you are following from the guide and the CMake quick start command from the command palette. (Ctrl+Shift+P) (br)
You will then need to make the following edits to your CMakeSystem.cmake:(br)

```set(CMAKE_SYSTEM_NAME "Generic")
set(CMAKE_SYSTEM_PROCESSOR "arm")
set(CMAKE_CROSSCOMPILING "TRUE")
set(CMAKE_TRY_COMPILE_TARGET_TYPE STATIC_LIBRARY)```

and create a settings.json entry in your working directories .vscode folder with the following:(br)

```{
"cmake.generator": "MinGW Makefiles"
}```

and finally create a CMake Tool-kit that will compile using our desired setup by running the command from the command palette and entering the following into your cmake-tools-kits.json:(br)

```{
"name": "GCC for arm-none-eabi 8.2.1",
"compilers": {
"C": "C:\Program Files (x86)\GNU Tools ARM Embedded\8 2018-q4-major\bin\arm-none-eabi-gcc.exe",
"CXX": "C:\Program Files (x86)\GNU Tools ARM Embedded\8 2018-q4-major\bin\arm-none-eabi-g++.exe"
},
}```

Note that you will want to reset your CMake environment while making these changes with the reset option in the command palette.
- From here you can use the none automated commands from our Bare Metal Programming Guide to compile/link/flash our firmware code.(br)
Compile Code: ```arm-none-eabi-gcc -mcpu=cortex-m4 main.c -c```
Reading compiled code: ```arm-none-eabi-objdump -h main.o```
Producing full firmware.elf file with link.ld: ```arm-none-eabi-gcc -T link.ld -nostdlib main.o -o firmware.elf```
Extract sections from fimware.elf into a single binary blob: ```arm-none-eabi-objcopy -O binary firmware.elf firmware.bin```
Flash to hardware with ST-LINK utility: ```\path\to\directory\ST-LINK_CLI.exe -P \path\to\directory\firmware.bin 0x08000000 -V```
