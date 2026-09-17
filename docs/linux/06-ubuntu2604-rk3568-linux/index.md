# Ubuntu 26.04 桌面版开发RK3568 Linux驱动 - U-Boot 编译

## 安装 git

安装命令

```shell
sudo apt install -y git
```

检查安装

```shell
porschan@lab10:~/rk3568-sdk$ git -v
git version 2.53.0
```

## 安装 make gcc 等编译工具

安装命令

```shell
sudo apt install -y make gcc \
    build-essential build-essential bison flex libssl-dev \
    bc libncurses5-dev libncursesw5-dev device-tree-compiler
```

## 创建并进入工作目录

```shell
mkdir ~/rk3568-sdk
cd ~/rk3568-sdk
```

## 下载 RK Uboot 仓库

```shell
git clone https://github.com/rockchip-linux/u-boot.git
```

## 下载 RK rkbin 仓库

```shell
git clone https://github.com/rockchip-linux/rkbin.git
```

## 创建工具链目录

```shell
mkdir -p ~/rk3568-sdk/prebuilts/gcc/linux-x86/aarch64
```

## 克隆 Linaro 6.3.1 工具链到工具链目录

```shell
git clone https://github.com/lzqqfkj/gcc-linaro-6.3.1-2017.05-x86_64_aarch64-linux-gnu.git \
  ~/rk3568-sdk/prebuilts/gcc/linux-x86/aarch64/gcc-linaro-6.3.1-2017.05-x86_64_aarch64-linux-gnu
```

顺手加个执行权限，防止权限问题：

```shell
chmod +x ~/rk3568-sdk/prebuilts/gcc/linux-x86/aarch64/gcc-linaro-6.3.1-2017.05-x86_64_aarch64-linux-gnu/bin/*
```

## Uboot 编译

```shell
cd ~/rk3568-sdk/u-boot
./make.sh rk3568
```

## 编译成功的输出

```shell
********boot_merger ver 1.38********
Info:Pack loader ok.
pack loader okay! Input: /home/porschan/rk3568-sdk/rkbin/RKBOOT/RK3568MINIALL.ini
/home/porschan/rk3568-sdk/u-boot

Image(no-signed, version=0): uboot.img (FIT with uboot, trust...) is ready
Image(no-signed): rk356x_loader_v1.26.114.bin (with spl, ddr...) is ready
pack uboot.img okay! Input: /home/porschan/rk3568-sdk/rkbin/RKTRUST/RK3568TRUST.ini

Platform RK3568 is build OK, with new .config(make rk3568_defconfig -j24)
/home/porschan/rk3568-sdk/u-boot/../prebuilts/gcc/linux-x86/aarch64/gcc-linaro-6.3.1-2017.05-x86_64_aarch64-linux-gnu/bin/aarch64-linux-gnu-
Thu Sep 17 02:32:27 PM CST 2026
```

## 查看 uboot 文件 和 loader 文件

```shell
ls -l ~/rk3568-sdk/u-boot/uboot.img
ls -l ~/rk3568-sdk/u-boot/rk356x_loader_v1.26.114.bin
```

至此，已获取到 `uboot.img` 文件和 `rk356x_loader_v1.26.114.bin` 文件，完成 【U-Boot 编译】 阶段

## 知识点1：怎么知道需要 Linaro 6.3.1

```shell
porschan@lab10:~/rk3568-sdk$ cd u-boot/
porschan@lab10:~/rk3568-sdk/u-boot$ ls
Documentation  Licenses     PREUPLOAD.cfg  arch   common     disk     dts       fs       make.sh  scripts          tools
Kbuild         MAINTAINERS  README         board  config.mk  doc      env       include  net      snapshot.commit  usb_update.txt
Kconfig        Makefile     api            cmd    configs    drivers  examples  lib      post     test

porschan@lab10:~/rk3568-sdk/u-boot$ grep -n "CROSS_COMPILE" make.sh
15:CROSS_COMPILE_ARM32=$(pwd)/../prebuilts/gcc/linux-x86/arm/gcc-linaro-6.3.1-2017.05-x86_64_arm-linux-gnueabihf/bin/arm-linux-gnueabihf-
17:    CROSS_COMPILE_ARM64=$(pwd)/../prebuilts/gcc/linux-x86/aarch64/gcc-linaro-6.3.1-2017.05-x86_64_aarch64-linux-gnu/bin/aarch64-linux-gnu-
19:    CROSS_COMPILE_ARM64=$(pwd)/../prebuilts/gcc/linux-x86/aarch64/gcc-arm-10.3-2021.07-x86_64-aarch64-none-linux-gnu/bin/aarch64-none-linux-gnu-
33:# Declare global toolchain path for CROSS_COMPILE, updated in select_toolchain()
136:                    CROSS_COMPILE=*)  # set CROSS_COMPILE
138:                            CROSS_COMPILE_ARM32=${1#*=}
139:                            CROSS_COMPILE_ARM64=${1#*=}
279:    # If no outer CROSS_COMPILE, look for it from CC_FILE.
282:                    CROSS_COMPILE_ARM32=`cat ${CC_FILE}`
283:                    CROSS_COMPILE_ARM64=`cat ${CC_FILE}`
286:                            if [ ! -f "${CROSS_COMPILE_ARM64}gcc" ]; then
287:                                    CROSS_COMPILE_ARM64=$(cd `dirname ${CROSS_COMPILE_ARM64}`; pwd)"/aarch64-linux-gnu-"
290:                            CROSS_COMPILE_ARM32=$(cd `dirname ${CROSS_COMPILE_ARM32}`; pwd)"/arm-linux-gnueabihf-"
296:            TOOLCHAIN=${CROSS_COMPILE_ARM64}
297:            TOOLCHAIN_NM=${CROSS_COMPILE_ARM64}nm
298:            TOOLCHAIN_OBJDUMP=${CROSS_COMPILE_ARM64}objdump
299:            TOOLCHAIN_ADDR2LINE=${CROSS_COMPILE_ARM64}addr2line
301:            TOOLCHAIN=${CROSS_COMPILE_ARM32}
302:            TOOLCHAIN_NM=${CROSS_COMPILE_ARM32}nm
303:            TOOLCHAIN_OBJDUMP=${CROSS_COMPILE_ARM32}objdump
304:            TOOLCHAIN_ADDR2LINE=${CROSS_COMPILE_ARM32}addr2line
466:                    make CROSS_COMPILE=${TOOLCHAIN} envtools
826:make ${ARG_SPL_FWVER} ${ARG_FWVER} CROSS_COMPILE=${TOOLCHAIN} all --jobs=${JOB}

porschan@lab10:~/rk3568-sdk/u-boot$ grep -n "TOOLCHAIN" make.sh
34:TOOLCHAIN=
35:TOOLCHAIN_NM=
36:TOOLCHAIN_OBJDUMP=
37:TOOLCHAIN_ADDR2LINE=
296:            TOOLCHAIN=${CROSS_COMPILE_ARM64}
297:            TOOLCHAIN_NM=${CROSS_COMPILE_ARM64}nm
298:            TOOLCHAIN_OBJDUMP=${CROSS_COMPILE_ARM64}objdump
299:            TOOLCHAIN_ADDR2LINE=${CROSS_COMPILE_ARM64}addr2line
301:            TOOLCHAIN=${CROSS_COMPILE_ARM32}
302:            TOOLCHAIN_NM=${CROSS_COMPILE_ARM32}nm
303:            TOOLCHAIN_OBJDUMP=${CROSS_COMPILE_ARM32}objdump
304:            TOOLCHAIN_ADDR2LINE=${CROSS_COMPILE_ARM32}addr2line
307:    if [ ! `which ${TOOLCHAIN}gcc` ]; then
308:            echo "ERROR: No find ${TOOLCHAIN}gcc"
314:            echo "${TOOLCHAIN}" > ${CC_FILE}
423:                            ${TOOLCHAIN_NM} -r --size ${ELF} | grep -iv 'b' | less
428:                            ${TOOLCHAIN_OBJDUMP} -${ARG} ${ELF} | less
466:                    make CROSS_COMPILE=${TOOLCHAIN} envtools
511:            ${TOOLCHAIN_ADDR2LINE} -e ${ELF} ${FUNCADDR}
826:make ${ARG_SPL_FWVER} ${ARG_FWVER} CROSS_COMPILE=${TOOLCHAIN} all --jobs=${JOB}
829:echo ${TOOLCHAIN}

porschan@lab10:~/rk3568-sdk/u-boot$ grep -n "aarch64" make.sh
16:if [ -f "$(pwd)/../prebuilts/gcc/linux-x86/aarch64/gcc-linaro-6.3.1-2017.05-x86_64_aarch64-linux-gnu/bin/aarch64-linux-gnu-gcc" ]; then
17:    CROSS_COMPILE_ARM64=$(pwd)/../prebuilts/gcc/linux-x86/aarch64/gcc-linaro-6.3.1-2017.05-x86_64_aarch64-linux-gnu/bin/aarch64-linux-gnu-
19:    CROSS_COMPILE_ARM64=$(pwd)/../prebuilts/gcc/linux-x86/aarch64/gcc-arm-10.3-2021.07-x86_64-aarch64-none-linux-gnu/bin/aarch64-none-linux-gnu-
287:                                    CROSS_COMPILE_ARM64=$(cd `dirname ${CROSS_COMPILE_ARM64}`; pwd)"/aarch64-linux-gnu-"

porschan@lab10:~/rk3568-sdk/u-boot$ grep -n "prebuilts" make.sh
15:CROSS_COMPILE_ARM32=$(pwd)/../prebuilts/gcc/linux-x86/arm/gcc-linaro-6.3.1-2017.05-x86_64_arm-linux-gnueabihf/bin/arm-linux-gnueabihf-
16:if [ -f "$(pwd)/../prebuilts/gcc/linux-x86/aarch64/gcc-linaro-6.3.1-2017.05-x86_64_aarch64-linux-gnu/bin/aarch64-linux-gnu-gcc" ]; then
17:    CROSS_COMPILE_ARM64=$(pwd)/../prebuilts/gcc/linux-x86/aarch64/gcc-linaro-6.3.1-2017.05-x86_64_aarch64-linux-gnu/bin/aarch64-linux-gnu-
19:    CROSS_COMPILE_ARM64=$(pwd)/../prebuilts/gcc/linux-x86/aarch64/gcc-arm-10.3-2021.07-x86_64-aarch64-none-linux-gnu/bin/aarch64-none-linux-gnu-
porschan@lab10:~/rk3568-sdk/u-boot$ 
```