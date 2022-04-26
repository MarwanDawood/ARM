This manual is created based on [Christian's Blog, Getting Started with the TI Stellaris LaunchPad on Linux](https://www.jann.cc/2012/12/11/getting_started_with_the_ti_stellaris_launchpad_on_linux.html)

## Installation

### Install Code Composer Studio (CCS)
```
tar -xf CCS11.2.0.00007_web_linux-x64.tar.gz
sudo apt-get update
sudo apt-get install libtinfo5 libgconf-2-4 libc6-i386
./ccs_setup_11.2.0.00007.run
```

### Install OpenOCD and lm4flash tools
#### Install libusb and gcc compiler for ARM

```
sudo apt-get install libusb-1.0-0-dev
sudo apt-get install gcc-arm-none-eabi
```

#### Install lm4flash tool
```
git clone https://github.com/utzig/lm4tools.git
cd lm4tools/lm4flash/
make
sudo cp lm4flash /usr/bin/
```

#### Build OpenOCD with ICDI support
* Build OpenOCD (the Open On-Chip Debugger) with ICDI (in-circuit debug interface) support to flash and debug the LaunchPad.
* Since ICDI support has been merged into OpenOCD master branch we only need to enable it (--enable-ti-icdi) when configuring OpenOCD.
```
git clone git://git.code.sf.net/p/openocd/code openocd.git
cd openocd.git
./bootstrap
./configure --prefix=/usr --enable-maintainer-mode --enable-stlink --enable-ti-icdi
make
sudo make install
```

#### Compile and flash the project
1. Download and extract SW-EK-LM4F120XL-9453.exe
2. Compile StellarisWare for the Stellaris LM4F120 LaunchPad Evaluation Board
**Note**: StellarisWare project can be found on https://github.com/yuvadm/stellaris.git as well. Both `stellaris/driverlib/` and `stellaris\boards/ek-lm4f120xl/project0/` need to be build.
```
mkdir StellarisWare
cd StellarisWare
unzip SW-EK-LM4F120XL-9453.exe
make
```
3. Add a new udev rule in the new file `/etc/udev/rules.d/10-local.rules` to let normal users access the LaunchPad and program it with OpenOCD
```
ATTR{idVendor}=="15ba", ATTR{idProduct}=="0004", GROUP="plugdev", MODE="0660" # Olimex Ltd. OpenOCD JTAG TINY
ATTR{idVendor}=="067b", ATTR{idProduct}=="2303", GROUP="plugdev", MODE="0660" # Prolific Technology, Inc. PL2303 Serial Port
ATTR{idVendor}=="10c4", ATTR{idProduct}=="ea60", GROUP="plugdev", MODE="0660" # USB Serial
ATTR{idVendor}=="1cbe", ATTR{idProduct}=="00fd", GROUP="plugdev", MODE="0660" # TI Stellaris Launchpad
```
4. Reload the rules
```
sudo udevadm control --reload-rules
```
5. Modify the Makefile in `StellarisWare/boards/ek-lm4f120xl/project0/` and add the flash rule (use a tabulator for indention like in the other make targets)
```
#
# The default rule, which causes the Project Zero Example to be built.
#
all: ${COMPILER}
all: ${COMPILER}/project0.axf

#
# The rule to flash the program to the chip.
#
flash:
	lm4flash gcc/project0.bin
```
6. Compile and flash the application software `project0.c` using `make`, and flash using `make flash`
```
cd boards/ek-lm4f120xl/project0/
make clean
make
sudo make flash
```

## Debug using gdb
1. Instead of installing `arm-none-eabi-gdb`, install `gdb-multiarch`
```
sudo apt-get install gdb-multiarch
```
2. We can also compile our C code with the `-ggdb3` flag which produces additional debugging information for GDB .
3. Open On-Chip Debugger
```
make clean
make DEBUG=1
sudo openocd -f /usr/share/openocd/scripts/board/ek-lm4f120xl.cfg
```
4. in another terminal, run the below command, it already split the layout and show general registers
```
gdb-multiarch gcc/project0.axf -ex 'target remote localhost:3333' -ex 'layout split' -ex 'layout regs'
```
[using gdb debugger and OpenOCD](docs/debugger.png "using gdb debugger and OpenOCD")
