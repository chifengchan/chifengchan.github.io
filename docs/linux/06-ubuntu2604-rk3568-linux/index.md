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
  prebuilts/gcc/linux-x86/aarch64/gcc-linaro-6.3.1-2017.05-x86_64_aarch64-linux-gnu
```

顺手加个执行权限，防止权限问题：

```shell
chmod +x ~/rk3568-sdk/prebuilts/gcc/linux-x86/aarch64/gcc-linaro-6.3.1-2017.05-x86_64_aarch64-linux-gnu/bin/*
```

## Uboot 编译

```shell
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