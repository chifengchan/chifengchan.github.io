# J4125 - OPNsense 简单使用

## PC 访问 OPNsense

从控制台中查看 `LAN` 网关，注意此处需要记住 `LAN` 和 `igb0` 的关系

![opnsense-init-work-01.png](assets/opnsense-init-work-01.png)

将 PC 和 J4125 使用网线连接，注意每台设备可能出现不一致情况，根据实际情况判断操作

![opnsense-init-work-02.png](assets/opnsense-init-work-02.png)

将 PC 的有线网卡设置为 DHCP 获取IP的方式

!!! info "注意"
    你的电脑将得到一个 IP，IP范围 从 `192.168.1.100` 到 `192.168.1.199`

浏览器访问 `https://192.168.1.1`，账号为 `root`，密码为自己设置的密码

![opnsense-init-work-03.png](assets/opnsense-init-work-03.png)

成功登录界面（跳过设置向导）

![opnsense-init-work-04.png](assets/opnsense-init-work-04.png)

## 设置语言为 简体中文

![opnsense-init-work-05.png](assets/opnsense-init-work-05.png)

## 设置系统语言为 简体中文 以及 UTC + 8

![opnsense-init-work-16.png](assets/opnsense-init-work-16.png)

## OPNsense 接入 局域网（WAN - DHCP）

打开 系统:网关：设置

![opnsense-init-work-06.png](assets/opnsense-init-work-06.png)

配置如下：

![opnsense-init-work-07.png](assets/opnsense-init-work-07.png)

添加完成后，点击 `应用`

将 WAN 口配置至 igb3

![opnsense-init-work-08.png](assets/opnsense-init-work-08.png)

编辑 WAN 使用 DHCP 获取IP

![opnsense-init-work-09.png](assets/opnsense-init-work-09.png)

添加完成后，点击 `应用更改`

将OPNsense 连接局域网

## OPNsense 升级

打开下图，编辑镜像地址

![opnsense-init-work-10.png](assets/opnsense-init-work-10.png)

检查升级

![opnsense-init-work-11.png](assets/opnsense-init-work-11.png)

点击【更新】

![opnsense-init-work-12.png](assets/opnsense-init-work-12.png)

![opnsense-init-work-13.png](assets/opnsense-init-work-13.png)

重启 OPNsense，查看固件版本

![opnsense-init-work-14.png](assets/opnsense-init-work-14.png)

## 删除多余的网关

![opnsense-init-work-15.png](assets/opnsense-init-work-15.png)

## 仪表盘显示温度传感器

设置为 CPU芯片上温度传感器

![opnsense-init-work-17.png](assets/opnsense-init-work-17.png)

打开仪表盘

![opnsense-init-work-18.png](assets/opnsense-init-work-18.png)

![opnsense-init-work-19.png](assets/opnsense-init-work-19.png)

添加温度传感器

![opnsense-init-work-20.png](assets/opnsense-init-work-20.png)

点击保存

![opnsense-init-work-21.png](assets/opnsense-init-work-21.png)

## 仪表盘显示备忘录

打开仪表盘

![opnsense-init-work-22.png](assets/opnsense-init-work-22.png)

点击添加按钮

![opnsense-init-work-23.png](assets/opnsense-init-work-23.png)

添加 Notes

![opnsense-init-work-24.png](assets/opnsense-init-work-24.png)

点击编辑

![opnsense-init-work-25.png](assets/opnsense-init-work-25.png)

添加内容

![opnsense-init-work-26.png](assets/opnsense-init-work-26.png)

点击保存

![opnsense-init-work-27.png](assets/opnsense-init-work-27.png)

!!! info "参考链接"
    1. [OPNsense 官方文档](https://docs.opnsense.org/hardware/quickstart.html){target=blnak}
    2. [OPNsense使用记录01：安装与基本配置](https://takuron.com/post/id0027/){target=blnak}
    3. [OPNsense 防火墙系列一：安装、基础配置（PPPoE、IPv6、更换软件源）](https://www.cnblogs.com/Yogile/p/16642649.html){target=blnak}
