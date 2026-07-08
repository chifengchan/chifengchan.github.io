# J4125 安装 OPNsense

## 操作过程

### 下载镜像

下载 `OPNsense-26.1.6-dvd-amd64.iso` ，并解压

!!! success "下载链接"
    下载网站：https://opnsense.org/download/

    名称：`OPNsense-26.1.6-dvd-amd64.iso`

    SHA256：`6BA3633D9C0F96D82C792015A45F4B8AAC45EA8FA2BDBA3C5E534D0C90A4F08C`

    链接：https://mirrors.pku.edu.cn/opnsense/releases/26.1.6/OPNsense-26.1.6-dvd-amd64.iso.bz2

下载界面如下图所示

![opnsense-install-01.png](assets/opnsense-install-01.png)

校验码如下图所示

![opnsense-install-2.png](assets/opnsense-install-02.png)

### 清空磁盘分区

!!! info "注意"
    此次使用 ventoy + WePE 来安装 ISO 文件，此次不做操作

清空您的系统盘

![opnsense-install-03.png](assets/opnsense-install-03.png)

### 安装 OPNsense

选择对应的 OPNsense 的镜像文件，默认账号：`installer`，默认密码：`opnsense`

![opnsense-install-04.png](assets/opnsense-install-04.png)

选择 `默认`

![opnsense-install-05.png](assets/opnsense-install-05.png)

选择 `默认`

![opnsense-install-06.png](assets/opnsense-install-06.png)

等待界面

![opnsense-install-07.png](assets/opnsense-install-07.png)

选择 `默认`

![opnsense-install-08.png](assets/opnsense-install-08.png)

选择 `默认`

![opnsense-install-09.png](assets/opnsense-install-09.png)

选择 `默认`

![opnsense-install-10.png](assets/opnsense-install-10.png)

选择 `YES`

![opnsense-install-11.png](assets/opnsense-install-11.png)

等待界面

![opnsense-install-12.png](assets/opnsense-install-12.png)

选择 `ROOT Password`

![opnsense-install-13.png](assets/opnsense-install-13.png)

输出首次密码

![opnsense-install-14.png](assets/opnsense-install-14.png)

输出确认密码

![opnsense-install-15.png](assets/opnsense-install-15.png)

选择 `Complete Install`

![opnsense-install-16.png](assets/opnsense-install-16.png)

选择 `Reboot now`

![opnsense-install-17.png](assets/opnsense-install-17.png)

登录界面，默认账号：`root`，密码为初始化过程中设置的密码

![opnsense-install-18.png](assets/opnsense-install-18.png)

!!! info "注意"
    安装过程中设备全程无连接任何网线！


!!! info "参考连接"
    1. [【赛博堡垒 】PVE 部署 OPNsense 完全指南：打造家庭网络“门神”](https://www.bilibili.com/video/BV19bDGBUELi){target=blank}
