# 【VMware Workstation】Rocky Linux 10 桌面版安装

## 下载系统镜像

访问 [Rocky Linux](https://rockylinux.org/zh-CN/download){target=blank} 官网

![pixpin_2025-07-24_13-46-19.png](assets/pixpin_2025-07-24_13-46-19.png)

下载 `Rocky-10.0-x86_64-dvd1.iso` 系统镜像

!!! info
   下载列表页面：https://rockylinux.org/zh-CN/download

   文件名称：`Rocky-10.0-x86_64-dvd1.iso`

   MD5：`2AE0447BE1353D702B3809C9AB91A17E`

   下载链接：https://download.rockylinux.org/pub/rocky/10/isos/x86_64/Rocky-10.0-x86_64-dvd1.iso

下载 `CHECKSUM` 文件，用于校对系统镜像是否篡改（省略检查过程）

!!! info
   下载列表页面：https://rockylinux.org/zh-CN/download

   文件名称：`CHECKSUM`

   MD5：`52D159B088799A54A389F9914FEDD2A6`

   下载链接：https://download.rockylinux.org/pub/rocky/10/isos/x86_64/CHECKSUM

将上述 2 个文件移动至 `D:\VMware-RockyLinux\01-iso` 目录中

## 安装 Rocky Linux

打开 `VMware Workstation Pro`
点击 `文件` -> `新建虚拟机`

![pixpin_2025-07-24_13-57-11.png](assets/pixpin_2025-07-24_13-57-11.png)

点击 `下一步`

![pixpin_2025-07-24_13-58-12.png](assets/pixpin_2025-07-24_13-58-12.png)

点击 `下一步`

![pixpin_2025-07-24_14-00-03.png](assets/pixpin_2025-07-24_14-00-03.png)

点击 `下一步`

![pixpin_2025-07-24_14-00-13.png](assets/pixpin_2025-07-24_14-00-13.png)

点击 `下一步`

![pixpin_2025-07-24_14-00-33.png](assets/pixpin_2025-07-24_14-00-33.png)

修改信息如下，其中虚拟机名称的 `10.1.1.10` 为设置的虚拟机的IP

虚拟机名称：`RockyLinux10_10.1.1.10`
位置：`D:\VMware-RockyLinux\02-vmHouse\RockyLinux10_10.1.1.10`

![pixpin_2025-07-24_14-04-57.png](assets/pixpin_2025-07-24_14-04-57.png)

每个处理器的内核数量：`4`

![pixpin_2025-07-24_14-10-37.png](assets/pixpin_2025-07-24_14-10-37.png)

此虚拟机的内存：`4096` MB

![pixpin_2025-07-24_14-11-33.png](assets/pixpin_2025-07-24_14-11-33.png)

点击 `下一步`

![pixpin_2025-07-24_14-12-25.png](assets/pixpin_2025-07-24_14-12-25.png)

点击 `下一步`

![pixpin_2025-07-24_14-12-37.png](assets/pixpin_2025-07-24_14-12-37.png)

点击 `下一步`

![pixpin_2025-07-24_14-12-44.png](assets/pixpin_2025-07-24_14-12-44.png)

点击 `下一步`

![pixpin_2025-07-24_14-12-52.png](assets/pixpin_2025-07-24_14-12-52.png)

点击 `下一步`

![pixpin_2025-07-24_14-13-00.png](assets/pixpin_2025-07-24_14-13-00.png)

点击 `下一步`

![pixpin_2025-07-24_14-13-09.png](assets/pixpin_2025-07-24_14-13-09.png)

点击 `自定义硬件`

![pixpin_2025-07-24_14-15-33.png](assets/pixpin_2025-07-24_14-15-33.png)

删除 `USB 控制器`

![pixpin_2025-07-24_14-15-48.png](assets/pixpin_2025-07-24_14-15-48.png)

删除 `声卡`

![pixpin_2025-07-24_14-16-02.png](assets/pixpin_2025-07-24_14-16-02.png)

删除 `打印机`

![pixpin_2025-07-24_14-16-13.png](assets/pixpin_2025-07-24_14-16-13.png)

设置 `新 CD/DVD(IDE)` ，选择 `使用 ISO 映像文件`，选择 `D:\VMware-RockyLinux\01-iso\Rocky-10.0-x86_64-dvd1.iso`

![pixpin_2025-07-24_14-16-53.png](assets/pixpin_2025-07-24_14-16-53.png)

点击 `完成`

![pixpin_2025-07-24_14-17-09.png](assets/pixpin_2025-07-24_14-17-09.png)

点击 `开启此虚拟机`

![pixpin_2025-07-24_14-17-25.png](assets/pixpin_2025-07-24_14-17-25.png)

选择 `Install Rocky Linux 10.0`

![pixpin_2025-07-24_14-25-36.png](assets/pixpin_2025-07-24_14-25-36.png)

选择 `English`，选择 `English(United States)` （检查项）

![pixpin_2025-07-24_14-27-46.png](assets/pixpin_2025-07-24_14-27-46.png)

点击 `Installation Destination`

![pixpin_2025-07-24_14-28-47.png](assets/pixpin_2025-07-24_14-28-47.png)

选中磁盘

![pixpin_2025-07-24_14-29-04.png](assets/pixpin_2025-07-24_14-29-04.png)

点击 `Root Account`

![pixpin_2025-07-24_14-29-48.png](assets/pixpin_2025-07-24_14-29-48.png)

设置密码，并勾选 `Allow root SSH login with password`

![pixpin_2025-07-24_14-30-07.png](assets/pixpin_2025-07-24_14-30-07.png)

点击 `User Creation`

![pixpin_2025-07-24_14-42-27.png](assets/pixpin_2025-07-24_14-42-27.png)

设置用户名，密码

![pixpin_2025-07-24_14-42-47.png](assets/pixpin_2025-07-24_14-42-47.png)

点击 `Time & Date`（检查项）

![pixpin_2025-07-24_14-42-59.png](assets/pixpin_2025-07-24_14-42-59.png)

Region：`Asia`，`City`：`Shanghai`（检查项）

![pixpin_2025-07-24_14-43-28.png](assets/pixpin_2025-07-24_14-43-28.png)

点击 `Begin Installation`

![pixpin_2025-07-24_14-43-39.png](assets/pixpin_2025-07-24_14-43-39.png)

点击 `Reboot System`

![pixpin_2025-07-24_14-48-20.png](assets/pixpin_2025-07-24_14-48-20.png)

重启完成后，显示以下内容则安装成功

![pixpin_2025-07-24_14-49-26.png](assets/pixpin_2025-07-24_14-49-26.png)

点击下图按钮，拍摄此虚拟机的快照

![pixpin_2025-07-24_14-50-35.png](assets/pixpin_2025-07-24_14-50-35.png)

名称：`完成安装系统`

![pixpin_2025-07-24_14-50-59.png](assets/pixpin_2025-07-24_14-50-59.png)

## 跳过使用导航

首次进入系统会进去使用导航模式，点击 `Skip`，进行跳过

![pixpin_2025-07-24_14-59-08.png](assets/pixpin_2025-07-24_14-59-08.png)

## 设置不锁屏幕

点击右上角的 `关机` 图标

![pixpin_2025-07-24_14-59-51.png](assets/pixpin_2025-07-24_14-59-51.png)

选择 `设置` 图标

![pixpin_2025-07-24_15-00-02.png](assets/pixpin_2025-07-24_15-00-02.png)

选择 `Power` -> `Power Saving` -> `Screen Blank` -> 选择 `Never`

![pixpin_2025-07-24_15-00-18.png](assets/pixpin_2025-07-24_15-00-18.png)

## 获取虚拟机的IP，并远程连接

点击左上角的图标

![pixpin_2025-07-24_15-00-31.png](assets/pixpin_2025-07-24_15-00-31.png)

选择 `控制台` 图标

![pixpin_2025-07-24_15-00-39.png](assets/pixpin_2025-07-24_15-00-39.png)

输入命令 `ifconfig`，图是中的 `10.1.1.16` 即是虚拟机IP

![pixpin_2025-07-24_15-00-58.png](assets/pixpin_2025-07-24_15-00-58.png)

使用 `Xshell` 软件进行远程登录（此处忽略操作过程）

![pixpin_2025-07-24_15-11-19.png](assets/pixpin_2025-07-24_15-11-19.png)

## 修改虚拟机地址

已获取虚拟机IP为 `10.1.1.16`，现在将其修改为 `10.1.1.10`，命令如下

```shell
ip addr delete 10.1.1.16/24 dev ens32 && ip addr add 10.1.1.10/24 dev ens32
```

- 上述命令包含2条命令
- `ens32` 为网络接口名称
- `10.1.1.16/24` 为修改前的虚拟机IP
- `10.1.1.10/24` 为修改后的虚拟机IP
- 执行完命令后，马上生效，重新远程登录
- 需要 `root` 权限执行

参考链接
[Rocky Linux 官方文档 - Network Configuration](https://docs.rockylinux.org/guides/network/basic_network_configuration/){target=blank}

## 修改虚拟机的主机名

查看当前主机名信息

```shell
root@localhost:~# hostnamectl
     Static hostname: (unset)                                               
  Transient hostname: localhost
           Icon name: computer-vm
             Chassis: vm 🖴
          Machine ID: b47b4d67597c489dafa4aa7f5647c61d
             Boot ID: a7944f9c26494fd49a45b21ba96ae1e4
        Product UUID: 1ed94d56-8e2b-6d27-d4ec-9ae41ab2fc85
        AF_VSOCK CID: 447937669
      Virtualization: vmware
    Operating System: Rocky Linux 10.0 (Red Quartz)                         
         CPE OS Name: cpe:/o:rocky:rocky:10::baseos
      OS Support End: Thu 2035-05-31
OS Support Remaining: 9y 10month 6d                                         
              Kernel: Linux 6.12.0-55.12.1.el10_0.x86_64
        Architecture: x86-64
     Hardware Vendor: VMware, Inc.
      Hardware Model: VMware Virtual Platform
     Hardware Serial: VMware-56 4d d9 1e 2b 8e 27 6d-d4 ec 9a e4 1a b2 fc 85
    Firmware Version: 6.00
       Firmware Date: Thu 2020-11-12
        Firmware Age: 4y 8month 1w 3d
```

修改主机名，命令如下

```shell
hostnamectl set-hostname "lab10"
```

再次查看主机名信息

```shell
root@localhost:~# hostnamectl
     Static hostname: lab10
           Icon name: computer-vm
             Chassis: vm 🖴
          Machine ID: b47b4d67597c489dafa4aa7f5647c61d
             Boot ID: a7944f9c26494fd49a45b21ba96ae1e4
        Product UUID: 1ed94d56-8e2b-6d27-d4ec-9ae41ab2fc85
        AF_VSOCK CID: 447937669
      Virtualization: vmware
    Operating System: Rocky Linux 10.0 (Red Quartz)                         
         CPE OS Name: cpe:/o:rocky:rocky:10::baseos
      OS Support End: Thu 2035-05-31
OS Support Remaining: 9y 10month 6d                                         
              Kernel: Linux 6.12.0-55.12.1.el10_0.x86_64
        Architecture: x86-64
     Hardware Vendor: VMware, Inc.
      Hardware Model: VMware Virtual Platform
     Hardware Serial: VMware-56 4d d9 1e 2b 8e 27 6d-d4 ec 9a e4 1a b2 fc 85
    Firmware Version: 6.00
       Firmware Date: Thu 2020-11-12
        Firmware Age: 4y 8month 1w 3d 
```

使用 `su` 命令，重新读取新主机名

```shell
root@localhost:~# 
root@localhost:~# su
root@lab10:~# 
```

## 拍摄快照

点击下图按钮，拍摄此虚拟机的快照

![pixpin_2025-07-24_14-50-35.png](assets/pixpin_2025-07-24_14-50-35.png)

名称：`完成IP、主机名、锁屏时间的设置`

![pixpin_2025-07-24_15-56-27.png](assets/pixpin_2025-07-24_15-56-27.png)
