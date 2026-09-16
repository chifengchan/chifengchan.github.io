# 【VMware Workstation】Ubuntu 26.04 桌面版安装

## 下载系统镜像

访问 [Ubuntu](https://ubuntu.com/download/desktop){target=blank} 官网下载网页

![04-ubuntu2604-amd64-desktop-01.png](assets/04-ubuntu2604-amd64-desktop-01.png)

点击 `Download` ，页面自动跳转到 `Thank you` 页面，即可查询 `SHA256 checksum` = `601e30fbf5d97759367c632e2c33630665039b7e2158fd068403da3ccf1bda1f`

![04-ubuntu2604-amd64-desktop-02.png](assets/04-ubuntu2604-amd64-desktop-02.png)

本地下载完成，可以检查 `SHA256 checksum` 是否一致，如下图所示

![04-ubuntu2604-amd64-desktop-03.png](assets/04-ubuntu2604-amd64-desktop-03.png)

!!! info
    下载列表页面：https://ubuntu.com/download/desktop

    文件名称：`ubuntu-26.04.1-desktop-amd64.iso`

    SHA256 checksum：`601e30fbf5d97759367c632e2c33630665039b7e2158fd068403da3ccf1bda1f`

    下载链接：https://releases.ubuntu.com/26.04.1/ubuntu-26.04.1-desktop-amd64.iso

## 安装 Ubuntu 26.04

点击 `文件`

![04-ubuntu2604-amd64-desktop-04.png](assets/04-ubuntu2604-amd64-desktop-04.png)

选择 `自定义(高级)` ，点击 `下一步`

![04-ubuntu2604-amd64-desktop-05.png](assets/04-ubuntu2604-amd64-desktop-05.png)

点击 `下一步`

![04-ubuntu2604-amd64-desktop-06.png](assets/04-ubuntu2604-amd64-desktop-06.png)

选择 `稍后安装操作系统` ，点击 `下一步`

![04-ubuntu2604-amd64-desktop-07.png](assets/04-ubuntu2604-amd64-desktop-07.png)

选择 `Linux` ，选择 `Ubuntu 64` ，点击 `下一步`

![04-ubuntu2604-amd64-desktop-08.png](assets/04-ubuntu2604-amd64-desktop-08.png)

填写 `虚拟机名称` 、 `位置` ，点击 `下一步`

![04-ubuntu2604-amd64-desktop-09.png](assets/04-ubuntu2604-amd64-desktop-09.png)

根据实际情况，填写 `处理器数量` 、 `每个处理器的内核数量` ，点击 `下一步`

![04-ubuntu2604-amd64-desktop-10.png](assets/04-ubuntu2604-amd64-desktop-10.png)

根据实际情况，选择 `此虚拟机的内存` ，点击 `下一步`

![04-ubuntu2604-amd64-desktop-11.png](assets/04-ubuntu2604-amd64-desktop-11.png)

点击 `下一步`

![04-ubuntu2604-amd64-desktop-12.png](assets/04-ubuntu2604-amd64-desktop-12.png)

点击 `下一步`

![04-ubuntu2604-amd64-desktop-13.png](assets/04-ubuntu2604-amd64-desktop-13.png)

点击 `下一步`

![04-ubuntu2604-amd64-desktop-14.png](assets/04-ubuntu2604-amd64-desktop-14.png)

点击 `下一步`

![04-ubuntu2604-amd64-desktop-15.png](assets/04-ubuntu2604-amd64-desktop-15.png)

根据实际情况，填写 `最大磁盘大小` ，点击 `下一步`

![04-ubuntu2604-amd64-desktop-16.png](assets/04-ubuntu2604-amd64-desktop-16.png)

点击 `下一步`

![04-ubuntu2604-amd64-desktop-17.png](assets/04-ubuntu2604-amd64-desktop-17.png)

点击 `自定义硬件`

![04-ubuntu2604-amd64-desktop-18.png](assets/04-ubuntu2604-amd64-desktop-18.png)

操作如下

1. 删除 `打印机` 、 `USB 控制器` 、 `声卡` 的硬件
2. 选中 `新 CD/DVD（SATA）` 中 `使用 ISO 映像文件(M)` ，选中刚刚下载的号的 `ISO` 文件(`ubuntu-26.04.1-desktop-amd64.iso`)

操作完成，点击 `关闭`

![04-ubuntu2604-amd64-desktop-19.png](assets/04-ubuntu2604-amd64-desktop-19.png)

点击 `完成`

![04-ubuntu2604-amd64-desktop-20.png](assets/04-ubuntu2604-amd64-desktop-20.png)

点击 `开启此虚拟机`


![04-ubuntu2604-amd64-desktop-21.png](assets/04-ubuntu2604-amd64-desktop-21.png)

安装程序会给你选择，选择 `Try or Install Ubuntu` （或等待倒数结束）

![04-ubuntu2604-amd64-desktop-22.png](assets/04-ubuntu2604-amd64-desktop-22.png)

进入临时系统，点击 `Live session user`

![04-ubuntu2604-amd64-desktop-23.png](assets/04-ubuntu2604-amd64-desktop-23.png)

选择你的语言，保持默认，点击 `Next`

![04-ubuntu2604-amd64-desktop-24.png](assets/04-ubuntu2604-amd64-desktop-24.png)

选择你需要的任何无障碍设置，保持默认，点击 `Next`

![04-ubuntu2604-amd64-desktop-25.png](assets/04-ubuntu2604-amd64-desktop-25.png)

选择你的键盘布局，保持默认，点击 `Next`

![04-ubuntu2604-amd64-desktop-26.png](assets/04-ubuntu2604-amd64-desktop-26.png)

连接你的网络，保持默认，点击 `Next`

![04-ubuntu2604-amd64-desktop-27.png](assets/04-ubuntu2604-amd64-desktop-27.png)

安装程序会给你选择，是尝试还是安装Ubuntu，保持默认，点击 `Next`

![04-ubuntu2604-amd64-desktop-28.png](assets/04-ubuntu2604-amd64-desktop-28.png)

在互动装置和自动化装置之间选择，保持默认，点击 `Next`

![04-ubuntu2604-amd64-desktop-29.png](assets/04-ubuntu2604-amd64-desktop-29.png)

在 `默认选择` 和 `扩展选择` 之间选择，保持默认，点击 `Next`

![04-ubuntu2604-amd64-desktop-30.png](assets/04-ubuntu2604-amd64-desktop-30.png)

在这里，你可以安装第三方软件。它可以提升设备支持和性能（例如，NVIDIA 图形驱动），并增加对更多媒体格式的支持。

同时启用这两个选项，点击 `Next`

![04-ubuntu2604-amd64-desktop-31.png](assets/04-ubuntu2604-amd64-desktop-31.png)

选择Ubuntu应如何安装在磁盘上，保持默认（Erase disk and install Ubuntu 翻译为：“擦除磁盘”并安装Ubuntu），点击 `Next`

![04-ubuntu2604-amd64-desktop-32.png](assets/04-ubuntu2604-amd64-desktop-32.png)

选择是否加密你的数据，保持默认，点击 `Next`

![04-ubuntu2604-amd64-desktop-33.png](assets/04-ubuntu2604-amd64-desktop-33.png)

创建一个用户，填写 `You name`、`You computer's name`、`Your username`、`Password`、`Confirm password` 后，点击 `Next`

![04-ubuntu2604-amd64-desktop-34.png](assets/04-ubuntu2604-amd64-desktop-34.png)

选择你的时区，选中 `Shanghai(Shanghai, China)` 后，点击 `Next`

![04-ubuntu2604-amd64-desktop-35.png](assets/04-ubuntu2604-amd64-desktop-35.png)

查看安装配置的摘要，点击 `Next`

![04-ubuntu2604-amd64-desktop-36.png](assets/04-ubuntu2604-amd64-desktop-36.png)

安装完成后，点击 `Restart now`

![04-ubuntu2604-amd64-desktop-37.png](assets/04-ubuntu2604-amd64-desktop-37.png)

Ubuntu 镜像文件弹出后，键盘按 `ENTER`

![04-ubuntu2604-amd64-desktop-38.png](assets/04-ubuntu2604-amd64-desktop-38.png)

点击用户

![04-ubuntu2604-amd64-desktop-39.png](assets/04-ubuntu2604-amd64-desktop-39.png)

输入用户密码

![04-ubuntu2604-amd64-desktop-40.png](assets/04-ubuntu2604-amd64-desktop-40.png)

首次安装后的引导页面，点击 `Next`

![04-ubuntu2604-amd64-desktop-41.png](assets/04-ubuntu2604-amd64-desktop-41.png)

保持默认，点击 `Next`

![04-ubuntu2604-amd64-desktop-42.png](assets/04-ubuntu2604-amd64-desktop-42.png)

保持默认，点击 `Next`

![04-ubuntu2604-amd64-desktop-43.png](assets/04-ubuntu2604-amd64-desktop-43.png)

保持默认，点击 `Next`

![04-ubuntu2604-amd64-desktop-44.png](assets/04-ubuntu2604-amd64-desktop-44.png)

保持默认，点击 `Next`

![04-ubuntu2604-amd64-desktop-45.png](assets/04-ubuntu2604-amd64-desktop-45.png)

点击 `Finish`

![04-ubuntu2604-amd64-desktop-46.png](assets/04-ubuntu2604-amd64-desktop-46.png)

完成引导，这就是Ubuntu的桌面

![04-ubuntu2604-amd64-desktop-47.png](assets/04-ubuntu2604-amd64-desktop-47.png)

点击 `拍摄快照`

![04-ubuntu2604-amd64-desktop-48.png](assets/04-ubuntu2604-amd64-desktop-48.png)

编辑快照信息，并点击 `拍摄快照`

- 名称：`完成Ubuntu2604安装`
- 描述：无

![04-ubuntu2604-amd64-desktop-49.png](assets/04-ubuntu2604-amd64-desktop-49.png)

!!! info 
    参考链接：
    
    1. [Install Ubuntu Desktop](https://ubuntu.com/desktop/docs/en/latest/tutorial/install-ubuntu-desktop/){target=blank}