# 【VMware Workstation】Debian 13 桌面版安装

## 下载系统镜像

访问 [debian](https://www.debian.org/distrib/){target=blank} 官网下载网页

![debian-os-install-1.png](assets/debian-os-install-1.png)

下载  `debian-13.1.0-amd64-DVD-1.iso` 系统镜像

!!! info
    下载列表页面：https://www.debian.org/distrib/

    文件名称：`debian-13.1.0-amd64-DVD-1.iso`

    MD5：`E883FB27DDC95057000F181E6E9820CC`

    下载链接：https://cdimage.debian.org/debian-cd/current/amd64/iso-dvd/debian-13.1.0-amd64-DVD-1.iso

## 安装 Debian 13

点击 `文件`

![debian-os-install-2.png](assets/debian-os-install-2.png)

点击 `新建虚拟机`

![debian-os-install-3.png](assets/debian-os-install-3.png)

点击 `下一步`

![debian-os-install-4.png](assets/debian-os-install-4.png)

点击 `下一步`

![debian-os-install-5.png](assets/debian-os-install-5.png)

点击 `下一步`

![debian-os-install-6.png](assets/debian-os-install-6.png)

点击 `Linux`，版本选择 `Debian 11.x 64 位`

![debian-os-install-7.png](assets/debian-os-install-7.png)

填写 `虚拟机名称`，位置选择合适目录

![debian-os-install-8.png](assets/debian-os-install-8.png)

根据实际情况，选择 `处理器数量` 和 `每个处理器的内核数量`

![debian-os-install-9.png](assets/debian-os-install-9.png)

选择 `4G` 内存

![debian-os-install-10.png](assets/debian-os-install-10.png)

点击 `下一步`

![debian-os-install-11.png](assets/debian-os-install-11.png)

点击 `下一步`

![debian-os-install-12.png](assets/debian-os-install-12.png)

点击 `下一步`

![debian-os-install-13.png](assets/debian-os-install-13.png)

点击 `下一步`

![debian-os-install-14.png](assets/debian-os-install-14.png)

选择 `40G` 存储

![debian-os-install-15.png](assets/debian-os-install-15.png)

点击 `下一步`

![debian-os-install-16.png](assets/debian-os-install-16.png)

点击 `自定义硬件`

![debian-os-install-17.png](assets/debian-os-install-17.png)

删除红框内的设备

![debian-os-install-18.png](assets/debian-os-install-18.png)

点击 `新 CD/DVD(IDE)`，在 `使用 ISO 映像文件（M）` 中选择 `debian-13.1.0-amd64-DVD-1.iso` 镜像

![debian-os-install-19.png](assets/debian-os-install-19.png)

点击 `完成`

![debian-os-install-20.png](assets/debian-os-install-20.png)

点击 `开启此虚拟机`

![debian-os-install-21.png](assets/debian-os-install-21.png)

点击 `下一步`

![debian-os-install-22.png](assets/debian-os-install-22.png)

点击 `下一步`

![debian-os-install-23.png](assets/debian-os-install-23.png)

选择 `other`

![debian-os-install-24.png](assets/debian-os-install-24.png)

选择 `Asia`

![debian-os-install-25.png](assets/debian-os-install-25.png)

选择 `China`

![debian-os-install-26.png](assets/debian-os-install-26.png)

点击 `下一步`

![debian-os-install-27.png](assets/debian-os-install-27.png)

点击 `下一步`

![debian-os-install-28.png](assets/debian-os-install-28.png)

填写 `lab10`

![debian-os-install-29.png](assets/debian-os-install-29.png)

点击 `下一步`

![debian-os-install-30.png](assets/debian-os-install-30.png)

添加 `root` 密码

![debian-os-install-31.png](assets/debian-os-install-31.png)

填写 `用户名称`

![debian-os-install-32.png](assets/debian-os-install-32.png)

点击 `下一步`

![debian-os-install-33.png](assets/debian-os-install-33.png)

填写 `用户密码`

![debian-os-install-34.png](assets/debian-os-install-34.png)

点击 `下一步`

![debian-os-install-35.png](assets/debian-os-install-35.png)

点击 `下一步`

![debian-os-install-36.png](assets/debian-os-install-36.png)

点击 `下一步`

![debian-os-install-37.png](assets/debian-os-install-37.png)

选择 `Finish partitioning and write changes to disk`

![debian-os-install-38.png](assets/debian-os-install-38.png)

选择 `Yes`

![debian-os-install-39.png](assets/debian-os-install-39.png)

选择 `No`

![debian-os-install-40.png](assets/debian-os-install-40.png)

选择 `No`

![debian-os-install-41.png](assets/debian-os-install-41.png)

选择 `No`

![debian-os-install-42.png](assets/debian-os-install-42.png)

勾选 `SSH server`

![debian-os-install-43.png](assets/debian-os-install-43.png)

选择 `Yes`

![debian-os-install-44.png](assets/debian-os-install-44.png)

选择 `/dev/sda`

![debian-os-install-45.png](assets/debian-os-install-45.png)

点击 `Continue`

![debian-os-install-46.png](assets/debian-os-install-46.png)

点击拍摄快照图标

![debian-os-install-47.png](assets/debian-os-install-47.png)

填写快照名称：`完成系统安装`

![debian-os-install-48.png](assets/debian-os-install-48.png)

点击 `用户`

![debian-os-install-49.png](assets/debian-os-install-49.png)

输入 `用户密码`

![debian-os-install-50.png](assets/debian-os-install-50.png)

点击 `Skip`

![debian-os-install-51.png](assets/debian-os-install-51.png)

点击网络图标

![debian-os-install-52.png](assets/debian-os-install-52.png)

点击网络设置图标

![debian-os-install-53.png](assets/debian-os-install-53.png)

点击 `Wired Settings`

![debian-os-install-54.png](assets/debian-os-install-54.png)

点击 Wired 设置图标

![debian-os-install-55.png](assets/debian-os-install-55.png)

设置如图片的IP等信息

![debian-os-install-56.png](assets/debian-os-install-56.png)

点击 Wired 设置图标

![debian-os-install-57.png](assets/debian-os-install-57.png)

查看IP是否设置正常

![debian-os-install-58.png](assets/debian-os-install-58.png)

宿主机测试是否与虚拟机互联

![debian-os-install-59.png](assets/debian-os-install-59.png)

点击电源图标

![debian-os-install-60.png](assets/debian-os-install-60.png)

点击设置图标

![debian-os-install-61.png](assets/debian-os-install-61.png)

关闭 `Automatic Screen Blank`

![debian-os-install-62.png](assets/debian-os-install-62.png)

点击拍摄快照图标

![debian-os-install-63.png](assets/debian-os-install-63.png)

填写快照名称：`完成IP设置以及关闭屏幕保护`

![debian-os-install-64.png](assets/debian-os-install-64.png)

至此，安装debian系统、设置固定IP和关闭屏幕保护已全部完成！