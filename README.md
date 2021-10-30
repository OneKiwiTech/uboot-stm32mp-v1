# uboot-stm32mp

## 1. Install ARM Cross Compiler: GCC
- `cd toolchain`
- `wget -c https://releases.linaro.org/components/toolchain/binaries/6.5-2018.12/arm-linux-gnueabihf/gcc-linaro-6.5.0-2018.12-x86_64_arm-linux-gnueabihf.tar.xz`
- `tar xf gcc-linaro-6.5.0-2018.12-x86_64_arm-linux-gnueabihf.tar.xz`
- `vim ~/.bashrc` open file **.bashrc**
- add `export PATH=$PATH:~/toolchain/gcc-linaro-6.5.0-2018.12-x86_64_arm-linux-gnueabihf/bin` into file **.bashrc**
- `source ~/.bashrc` update env
- `arm-linux-gnueabihf-gcc -v` check version

## 2. Build u-boot
- `git clone https://github.com/km-tek/u-boot-stm32mp.git`
- `cd u-boot-stm32mp`
- `export CROSS_COMPILE=arm-linux-gnueabihf-`
- `export ARCH=arm`
- `export KBUILD_OUTPUT=./build`
- `make distclean`
- `make stm32mp15_basic_defconfig` or `make stm32mp15_trusted_defconfig`
- `make DEVICE_TREE=stm32mp15xxaa-kmt -j8 all`

## 3. Output files u-boot
- **6. Output files** https://github.com/STMicroelectronics/u-boot/blob/v2020.01-stm32mp/doc/board/st/stm32mp1.rst
- **Step 5a: Configuring SD card to use custom U-Boot (Basic boot chain)** https://octavosystems.com/app_notes/stm32mp1-cubemx-tutorial-for-osd32mp15x/#post-10385-_Toc45627503

## 4. Reference

1. https://www.digikey.com/eewiki/display/linuxonarm/STM32MP1
2. https://wiki.st.com/stm32mpu/wiki/U-Boot_overview
3. https://wiki.st.com/stm32mpu/wiki/U-Boot_-_How_to_debug
4. https://wiki.st.com/stm32mpu/wiki/How_to_configure_U-Boot_for_your_board

# example

1. add your board device tree: **arch/arm/dts/board.dts** and **board-u-boot.dtsi**:
- copy `/arch/arm/dts/stm32mp157c-ev1.dts` to `arch/arm/dts/stm32mp15xxaa-kmt.dts`
- copy `/arch/arm/dts/stm32mp157c-ev1-u-boot.dtsi` to `arch/arm/dts/stm32mp15xxaa-kmt-u-boot.dtsi`

2. create your board defconfig: **defconfig/board_defconfig**:
- copy `u-boot/configs/stm32mp15_basic_defconfig` to `u-boot/configs/stm32mp15_kmt_defconfig`

3. update arch/arm/dts/Makefile:
```Makefile
dtb-$(CONFIG_STM32MP15x) += \
 	<board>.dtb 
```