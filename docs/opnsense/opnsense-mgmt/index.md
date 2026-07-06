# J4125 - OPNsense 设置管理口

## 网络环境

> 当前网络环境

![opnsense-mgmt-16.png](assets/opnsense-mgmt-16.png)

> PC 以 `DHCP` 方式获取 `IP` 地址

## 操作过程

### 访问管理界面

> 访问 http://192.168.1.1 登录管理界面

### 添加网关

> 添加 `192.168.20.254` 网关（可选）

![opnsense-mgmt-1.png](assets/opnsense-mgmt-1.png)

### 修改DHCP

> 进入 `服务: Dnsmasq`

![opnsense-mgmt-2.png](assets/opnsense-mgmt-2.png)

> 修改 `起始地址` 、 `结束地址` 以及 `描述`

![opnsense-mgmt-3.png](assets/opnsense-mgmt-3.png)

> 删除 IPv6 的 DHCP

![opnsense-mgmt-4.png](assets/opnsense-mgmt-4.png)

### 修改MGMT

> 修改 MGMT 接口，内容如下图所示

![opnsense-mgmt-5.png](assets/opnsense-mgmt-5.png)

> 重新获取 IP

![opnsense-mgmt-6.png](assets/opnsense-mgmt-6.png)

### 修改防火墙

> 在 `防火墙: 规则:Migration assistant` 中导出当前规则文件 `download_rules.csv`

![opnsense-mgmt-7.png](assets/opnsense-mgmt-7.png)

> 在 `防火墙: Rules [new]` 中导入 `download_rules.csv` 规则文件

![opnsense-mgmt-8.png](assets/opnsense-mgmt-8.png)

> 选择文件后，点击右侧的按钮，如下图所示

![opnsense-mgmt-9.png](assets/opnsense-mgmt-9.png)

> 查看导入后的规则，如下图所示

![opnsense-mgmt-10.png](assets/opnsense-mgmt-10.png)

> 在 `防火墙: 规则:Migration assistant` 中清空规则

![opnsense-mgmt-11.png](assets/opnsense-mgmt-11.png)

> 在 `防火墙: Rules [new]` 编辑 IPv4 的规则，如下图所示

![opnsense-mgmt-12.png](assets/opnsense-mgmt-12.png)

> 在 `防火墙: Rules [new]` 删除 IPv6 的规则，如下图所示

![opnsense-mgmt-13.png](assets/opnsense-mgmt-13.png)

> 检查 OPNsense 的 MGMT 接口是否能正常联网，如下图所示

![opnsense-mgmt-14.png](assets/opnsense-mgmt-14.png)

> 正常联网如下图所示

![opnsense-mgmt-15.png](assets/opnsense-mgmt-15.png)

!!! info "参考链接"
    1. [OPNsense Transparent Filtering Bridge (v26.1)](https://www.youtube.com/watch?v=ZCDXNxDhrIQ){target=blank}
