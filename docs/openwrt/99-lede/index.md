# Radxa E20C 升级 LEDE（含编译过程）

## 拍摄快照

点击 `虚拟机` -> `快照` -> `完成IP设置以及关闭屏幕保护`，转到快照状态

![lede-1.png](assets/lede-1.png)

## 设置 DNS

在 `Wired Settings` 中的 `IPv4` 中设置 `DNS` 为网关地址 `10.1.1.2`

![lede-2.png](assets/lede-2.png)

查看是否修改成功，效果如下

![lede-4.png](assets/lede-4.png)

使用系统中自带的火狐浏览器访问其他网站来测试是否连接成功

![lede-5.png](assets/lede-5.png)

拍照快照，快照名称为 `完成 DNS 设置`

![lede-6.png](assets/lede-6.png)

## 停用 CD/DVD 和 添加 LEDE 编译硬盘

在虚拟机设置中，在 `CD/DVD（IDE）` 中取消勾选 `启动时连接` 选项

![lede-7.png](assets/lede-7.png)

点击 `添加`

![lede-8.png](assets/lede-8.png)

点击 `下一步`

![lede-9.png](assets/lede-9.png)

点击 `下一步`

![lede-10.png](assets/lede-10.png)

点击 `下一步`

![lede-11.png](assets/lede-11.png)

`最大磁盘大小` 填写 `50` ，点击 `下一步`

![lede-12.png](assets/lede-12.png)

点击 `完成`

![lede-13.png](assets/lede-13.png)

点击 `确定`

![lede-14.png](assets/lede-14.png)

启动虚拟机，点击底部菜单的 `更多` 图标

![lede-15.png](assets/lede-15.png)

点击 `System`

![lede-16.png](assets/lede-16.png)

点击 `Disks`

![lede-17.png](assets/lede-17.png)

选中新添加的硬盘，点击 `设置` 图标

![lede-18.png](assets/lede-18.png)

点击 `Format Partition...`

![lede-19.png](assets/lede-19.png)

`Volume Name` 填写 `lede.project`，点击 `Next`

![lede-20.png](assets/lede-20.png)

点击 `Format`

![lede-21.png](assets/lede-21.png)

输入管理员密码

![lede-22.png](assets/lede-22.png)

点击 `挂载` 图标

![lede-23.png](assets/lede-23.png)

输入管理员密码

![lede-24.png](assets/lede-24.png)

查看挂载的目录，目录为 `/media/porschan/lede.project`

![lede-25.png](assets/lede-25.png)

拍照快照，快照名称为 `关闭CD自动连接和添加50G硬盘`

![lede-26.png](assets/lede-26.png)

## 编译 LEDE

新建会话，使用 `porschan` 用户登录（porschan替换为您自己账号）

```shell
su - root
```

将 `porschan` 用户拥有 `sudo` 权限（porschan替换为您自己账号）

```shell
sudo usermod -aG sudo porschan
```

关闭当前会话

新建会话，使用 `porschan` 用户登录（porschan替换为您自己账号）

使用 `清华大学开源软件镜像站`

使用命令

```shell
sudo nano /etc/apt/sources.list
```

修改内容为

```shell
# 注释 cd/dvd
# deb cdrom:[Debian GNU/Linux 13.1.0 _Trixie_ - Official amd64 DVD Binary-1 with firmware 20250906-10:24]/ trixie contrib main non-free-firmware

# 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
deb https://mirrors.tuna.tsinghua.edu.cn/debian/ trixie main contrib non-free non-free-firmware
# deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ trixie main contrib non-free non-free-firmware

deb https://mirrors.tuna.tsinghua.edu.cn/debian/ trixie-updates main contrib non-free non-free-firmware
# deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ trixie-updates main contrib non-free non-free-firmware

deb https://mirrors.tuna.tsinghua.edu.cn/debian/ trixie-backports main contrib non-free non-free-firmware
# deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ trixie-backports main contrib non-free non-free-firmware

# 以下安全更新软件源包含了官方源与镜像站配置，如有需要可自行修改注释切换
deb https://security.debian.org/debian-security trixie-security main contrib non-free non-free-firmware
# deb-src https://security.debian.org/debian-security trixie-security main contrib non-free non-free-firmware
```

安装必要的环境依赖

- 更新软件包列表

```shell
sudo apt update -y
```

- 将整个系统和所有软件包升级到最新版本

```shell
sudo apt full-upgrade -y
```

- 安装 LEDE 必要的依赖

```shell
sudo apt install -y ack antlr3 asciidoc autoconf automake autopoint binutils bison build-essential bzip2 ccache clang cmake cpio curl device-tree-compiler flex gawk gcc-multilib g++-multilib gettext genisoimage git gperf haveged help2man intltool libc6-dev-i386 libelf-dev libfuse-dev libglib2.0-dev libgmp3-dev libltdl-dev libmpc-dev libmpfr-dev libncurses5-dev libncursesw5-dev libpython3-dev libreadline-dev libssl-dev libtool llvm lrzsz libnsl-dev ninja-build p7zip p7zip-full patch pkgconf python3 python3-pyelftools python3-setuptools qemu-utils rsync scons squashfs-tools subversion swig texinfo uglifyjs upx-ucl unzip vim wget xmlto xxd zlib1g-dev
```

进入编译目录

```shell
cd /media/porschan/lede.project
```

下载源代码

```shell
git clone https://github.com/coolsnowwolf/lede
```

进入项目目录

```shell
cd lede
```

刷新软件包清单

```shell
./scripts/feeds update -a
```

安装清单中的软件包

```shell
./scripts/feeds install -a
```

配置系统

```shell
make menuconfig
```

这里只配置了以下 3 项内容，可根据自己需求添加额外的功能模块

- `Target System` 选择了 `Rockchip`
- `Subtarget` 默认选择了 `RK33xx/RK35xx boards (64bit)`
- `Target Profile` 选择了 `Radxa E20C`

下载 dl 库（-j 后面是线程数，第一次编译推荐用单线程）

```shell
make download -j8
```

编译固件（-j 后面是线程数，第一次编译推荐用单线程）

```shell
make V=s -j$(nproc)
```

!!! info "注意"
    上述操作建议使用作者推荐的单线程编译

编译完成后文件输出在 `/media/porschan/lede.project/lede/bin/targets/rockchip/armv8`

其中 `openwrt-rockchip-armv8-radxa_e20c-squashfs-sysupgrade.img.gz` 为本次安装 `LEDE` 的系统镜像

移动到本地，并安装 `LEDE` 系统，操作过程参考过往教程，本次略过

将 E20C 正确安装成功并且连接正确网线后，使用浏览器访问 `192.168.1.1`，默认账号：`root` 密码 `password`

![lede-27.png](assets/lede-27.png)

进入系统后的界面如下

![lede-28.png](assets/lede-28.png)

!!! info "参考链接"
    1. [Lean 的 LEDE 源码仓库](https://github.com/coolsnowwolf/lede){target=blank}
    2. [『299』 傻瓜式编译OpenWrt固件全流程丨Ubuntu下基于Lean源码编译融合各种插件](https://www.bilibili.com/video/BV1uh411B78b)
    3. [Lean/大雕/冷雪狼/乐得 精彩演讲纪念版---2025中关村科学城工业软件创新暨开源峰会](https://www.bilibili.com/video/BV1NiLkzXEyq/?vd_source=1c13e6cbcac25df6bc6e36cf7314fde7){target=blank}
    4. [清华大学开源软件镜像站 - Debian 软件源](https://mirrors.tuna.tsinghua.edu.cn/help/debian/){target=blank}

