# Ubuntu 26.04 桌面版一些配置

## 固定IP

点击左下角图标 -> 点击 `Settings`

![05-ubuntu2604-desktop-quickstart-01.png](assets/05-ubuntu2604-desktop-quickstart-01.png)

点击 `Network` -> 点击设置图标

![05-ubuntu2604-desktop-quickstart-02.png](assets/05-ubuntu2604-desktop-quickstart-02.png)

点击 `IPv4`

1. 选中 `Manual`
2. 填写 `Addresses - Address` = `10.1.1.10`
3. 填写 `Addresses - Netmask` = `255.255.255.0`
4. 填写 `Addresses - Gateway` = `10.1.1.2`
5. 填写 `DNS` = `10.1.1.2`

根据实际情况填写

![05-ubuntu2604-desktop-quickstart-03.png](assets/05-ubuntu2604-desktop-quickstart-03.png)

点击 `IPv6`

1. 选中 `Disable`

根据实际情况禁用

![05-ubuntu2604-desktop-quickstart-04.png](assets/05-ubuntu2604-desktop-quickstart-04.png)

重新查看是否配置成功

![05-ubuntu2604-desktop-quickstart-05.png](assets/05-ubuntu2604-desktop-quickstart-05.png)

宿主机查看是否联通

![05-ubuntu2604-desktop-quickstart-06.png](assets/05-ubuntu2604-desktop-quickstart-06.png)

## 关闭屏幕保护

点击左下角图标 -> 点击 `Settings`

![05-ubuntu2604-desktop-quickstart-01.png](assets/05-ubuntu2604-desktop-quickstart-01.png)

点击 `Power` -> 关闭 `Power Saving -> Automatic Screen Blank`

![05-ubuntu2604-desktop-quickstart-07.png](assets/05-ubuntu2604-desktop-quickstart-07.png)

## 更换软件仓库

点击左下角图标 -> 点击 `Terminal`

![05-ubuntu2604-desktop-quickstart-08.png](assets/05-ubuntu2604-desktop-quickstart-08.png)

备份 `ubuntu.sources` 文件

```shell
sudo cp -a /etc/apt/sources.list.d/ubuntu.sources /etc/apt/sources.list.d/bak.ubuntu.sources.20260916.disabled
```

修改 `ubuntu.sources` 文件

```shell
sudo nano /etc/apt/sources.list.d/ubuntu.sources
```

修改内容为

```sources
Types: deb
# 默认配置
# URIs: http://sg.archive.ubuntu.com/ubuntu/
# 清华大学 TUNA 镜像源
URIs: https://mirrors.tuna.tsinghua.edu.cn/ubuntu
Suites: resolute resolute-updates resolute-backports
Components: main restricted universe multiverse
Signed-By: /usr/share/keyrings/ubuntu-archive-keyring.gpg

Types: deb
URIs: http://security.ubuntu.com/ubuntu/
Suites: resolute-security
Components: main restricted universe multiverse
Signed-By: /usr/share/keyrings/ubuntu-archive-keyring.gpg
```

更新软件源索引

```shell
sudo apt update
```

升级系统

```shell
sudo apt upgrade -y
```

!!! info "参考链接："

    1. [Ubuntu 软件仓库 - DEB822 格式](https://mirrors.tuna.tsinghua.edu.cn/help/ubuntu/){target=blank}

    2. [Ubuntu 26.04 国内镜像源配置教程 - 解决 apt upgrade 慢的问题](https://my.oschina.net/ethanleellj/blog/19728245){target=blank}

## 安装 fastfetch

安装 fastfetch

```shell
sudo apt install -y fastfetch
```

内容输出

![05-ubuntu2604-desktop-quickstart-09.png](assets/05-ubuntu2604-desktop-quickstart-09.png)

## 安装 ssh 服务

安装 ssh 服务

```shell
sudo apt install -y openssh-server
```

启动 ssh 服务

```shell
sudo systemctl enable --now ssh
```

查看 ssh 服务

```shell
systemctl status ssh
```

输出内容如下

```shell
porschan@lab10:~$ systemctl status ssh
● ssh.service - OpenBSD Secure Shell server
     Loaded: loaded (/usr/lib/systemd/system/ssh.service; enabled; preset: enabled)
     Active: active (running) since Wed 2026-09-16 15:45:10 CST; 5s ago
 Invocation: 5f6a2d83020e4bcd8785c521c289dd79
TriggeredBy: ● ssh.socket
       Docs: man:sshd(8)
             man:sshd_config(5)
    Process: 16031 ExecStartPre=/usr/sbin/sshd -t (code=exited, status=0/SUCCESS)
   Main PID: 16034 (sshd)
      Tasks: 1 (limit: 15081)
     Memory: 1.6M (peak: 2.2M)
        CPU: 44ms
     CGroup: /system.slice/ssh.service
             └─16034 "sshd: /usr/sbin/sshd -D [listener] 0 of 10-100 startups"

Sep 16 15:45:09 lab10 systemd[1]: Starting ssh.service - OpenBSD Secure Shell server...
Sep 16 15:45:10 lab10 sshd[16034]: Server listening on 0.0.0.0 port 22.
Sep 16 15:45:10 lab10 sshd[16034]: Server listening on :: port 22.
Sep 16 15:45:10 lab10 systemd[1]: Started ssh.service - OpenBSD Secure Shell server.
porschan@lab10:~$ 
```

宿主机连接 ubuntu 成功界面

![05-ubuntu2604-desktop-quickstart-10.png](assets/05-ubuntu2604-desktop-quickstart-10.png)

## 修复 `WindTerm 2.7.0` SSH 连接 `Ubuntu 26.04` 出现 `]3008` 显示问题

使用 `WindTerm 2.7.0` SSH 连接 `Ubuntu 26.04` 出现如下

![05-ubuntu2604-desktop-quickstart-12.png](assets/05-ubuntu2604-desktop-quickstart-12.png)

编辑 `~/.bashrc` 文件

```shell
nano ~/.bashrc
```

末尾追加

```bashrc
# 编辑于 20260917
# 模糊匹配：检测到 WindTerm 或 SSH 连接时，精准拦截 systemd OSC 3008 乱码
if [[ "$TERM_PROGRAM" == "WindTerm" || -n "$SSH_CLIENT" || -n "$SSH_TTY" || -n "$SSH_CONNECTION" ]]; then
    # 仅当系统确实加载了 systemd 的 OSC 函数时才重写，避免污染正常环境
    if declare -f __systemd_osc_context_precmdline >/dev/null; then
        __systemd_osc_context_precmdline() { :; }
        __systemd_osc_context_common() { :; }
        __systemd_osc_context_escape() { :; }
        PS0=""
    fi
fi
```

编辑完成后，重启设备

```shell
sudo reboot
```

显示正常，如下

![05-ubuntu2604-desktop-quickstart-13.png](assets/05-ubuntu2604-desktop-quickstart-13.png)

!!! info "参考链接："

    1. [连接kubuntu2604无法正常显示](https://github.com/kingToolbox/WindTerm/issues/3621){target=blank}

## 拍摄快照

点击 `拍摄快照`，编辑快照信息，并点击 `拍摄快照`

- 名称：`完成《Ubuntu 26.04 桌面版一些配置》`
- 描述：无

![05-ubuntu2604-desktop-quickstart-11.png](assets/05-ubuntu2604-desktop-quickstart-11.png)