# 有人304通过MQTT获取正泰电表数据并在Home Assistant中采集和可视化

## Home Assistant(ha)

### 使用 `docker` 创建 `ha`

创建工作目录

```shell
mkdir -p /home/houzzkit/03-homeassistant
```

进入工作目录

```shell
cd /home/houzzkit/03-homeassistant
```

创建 `docker-compose.yml`

```shell
nano /home/houzzkit/03-homeassistant/docker-compose.yml
```

内容如下

```yml
services:
  homeassistant:
    container_name: homeassistant
    image: ghcr.io/home-assistant/home-assistant:2026.7.2
    restart: unless-stopped
    volumes:
      # 配置文件目录
      - /home/houzzkit/03-homeassistant/config:/config
      # 时区同步
      - /etc/localtime:/etc/localtime:ro
      # D-Bus 通信系统
      # - /run/dbus:/run/dbus:ro
    environment:
      - TZ=Asia/Shanghai
    network_mode: host
    privileged: true
```

启动服务

```shell
sudo docker compose up -d
```

或

```shell
sudo docker-compose up -d
```

### `ha` 初始化界面

访问 Home Assistant：[http://192.168.16.224:8123/](http://192.168.16.224:8123/){target=blank}

点击 `创建我的智能家居`

![03-usr-homeassistant-01.png](assets/03-usr-homeassistant-01.png)

填写 `姓名`、`用户名`、`密码` 和 `确认密码`，点击 `创建用户`

![03-usr-homeassistant-02.png](assets/03-usr-homeassistant-02.png)

填写 `家的位置`，点击 `下一步`

![03-usr-homeassistant-03.png](assets/03-usr-homeassistant-03.png)

点击 `下一步`

![03-usr-homeassistant-04.png](assets/03-usr-homeassistant-04.png)

点击 `完成`

![03-usr-homeassistant-05.png](assets/03-usr-homeassistant-05.png)

主界面

![03-usr-homeassistant-06.png](assets/03-usr-homeassistant-06.png)

### 学习重新启动 `ha`

位置在 `设置` -> `开发者工具` -> `重启（restart）`

![03-usr-homeassistant-07.png](assets/03-usr-homeassistant-07.png)

![03-usr-homeassistant-08.png](assets/03-usr-homeassistant-08.png)

- 快速重载（无需重启即可加载新的 YAML 配置。）
- 重新启动 Home Assistant（中断所有运行中的自动化和脚本）

!!! info
    或者不再单独介绍，以【快速重载】代表快速重载，以【重新启动 Home Assistant】代表重新启动 Home Assistant

## 社区商店(hacs)

### 安装 hacs

!!! warning "前提条件"
    你并没有阻止 GitHub 或 Cloudflare 的外部请求

进入 `homeassistant` 容器

```shell
sudo docker exec -it homeassistant bash
```

运行 `HACS` 下载脚本

```shell
wget -O - https://get.hacs.xyz | bash -
```

完成安装后，退出 `homeassistant` 容器

```shell
exit
```

操作过程如下图所示

![03-usr-homeassistant-09.png](assets/03-usr-homeassistant-09.png)

!!! info
    config 目录会出现 custom_components/hacs

    ![03-usr-homeassistant-10.png](assets/03-usr-homeassistant-10.png)

重启 `homeassistant` 容器

```shell
sudo docker compose restart
```

或

```shell
sudo docker-compose restart
```

点击 `设置` -> `设备与服务`

![03-usr-homeassistant-11.png](assets/03-usr-homeassistant-11.png)

点击 `添加集成`

![03-usr-homeassistant-12.png](assets/03-usr-homeassistant-12.png)

搜索 `hacs` 并点击 `HACS`

![03-usr-homeassistant-13.png](assets/03-usr-homeassistant-13.png)

勾选全部并点击 `提交`

![03-usr-homeassistant-14.png](assets/03-usr-homeassistant-14.png)

复制授权码并点击跳转链接

![03-usr-homeassistant-15.png](assets/03-usr-homeassistant-15.png)

选择关联账号

![03-usr-homeassistant-16.png](assets/03-usr-homeassistant-16.png)

填写授权码并点击 `Continue`

![03-usr-homeassistant-17.png](assets/03-usr-homeassistant-17.png)

点击 `Authorize hacs`

![03-usr-homeassistant-18.png](assets/03-usr-homeassistant-18.png)

显示关联成功信息

![03-usr-homeassistant-19.png](assets/03-usr-homeassistant-19.png)

回到 `ha`，点击 `跳过并完成`

![03-usr-homeassistant-20.png](assets/03-usr-homeassistant-20.png)

点击 `HACS`，这就是 `hacs` 界面，如下图所示

![03-usr-homeassistant-21.png](assets/03-usr-homeassistant-21.png)

### hacs 中安装 `Frosted Glass` 主题

#### 安装 `Frosted Glass` 主题

点击 `HACS` -> 搜索 `Frosted Glass` -> 选择 `Frosted Glass` 主题 -> 点击 `Download`

![03-usr-homeassistant-22.png](assets/03-usr-homeassistant-22.png)

点击 `Download`

![03-usr-homeassistant-23.png](assets/03-usr-homeassistant-23.png)

#### 设置用户主题

点击 `用户` -> 主题选用 `Frosted Glass` 主题 中的 `Frosted Glass Dark`

![03-usr-homeassistant-24.png](assets/03-usr-homeassistant-24.png)

!!! info
    如果安装 `多个主题` 或者切换回 `默认主题` 也可以在此界面操作

#### 创建仪表盘

点击 `设置` -> `仪表盘`

![03-usr-homeassistant-25.png](assets/03-usr-homeassistant-25.png)

点击 `添加仪表盘`

![03-usr-homeassistant-26.png](assets/03-usr-homeassistant-26.png)

点击 `从新建仪表盘开始`

![03-usr-homeassistant-27.png](assets/03-usr-homeassistant-27.png)

填写 `标题`、`图标` 和 `网站`，点击 `创建`

![03-usr-homeassistant-28.png](assets/03-usr-homeassistant-28.png)

#### 设置默认仪表台

点击 `用户` -> 仪表盘选用 `快乐小屋`

![03-usr-homeassistant-29.png](assets/03-usr-homeassistant-29.png)

#### 仪表台修改标题、添加天气

点击 `快乐小屋` -> 点击编辑按钮

![03-usr-homeassistant-30.png](assets/03-usr-homeassistant-30.png)

点击 `1` 修改标题，点击 `2` 添加卡片

![03-usr-homeassistant-31.png](assets/03-usr-homeassistant-31.png)

点击 `1` 修改标题并点击 `保存`

![03-usr-homeassistant-32.png](assets/03-usr-homeassistant-32.png)

点击 `2` 添加卡片 -> 选中 `日出日落` 中 `Sun`

![03-usr-homeassistant-33.png](assets/03-usr-homeassistant-33.png)

点击修改按钮

![03-usr-homeassistant-34.png](assets/03-usr-homeassistant-34.png)

修改 `标题` 为 `房间`

![03-usr-homeassistant-35.png](assets/03-usr-homeassistant-35.png)

点击 `完成`

![03-usr-homeassistant-36.png](assets/03-usr-homeassistant-36.png)

完成后的效果

![03-usr-homeassistant-37.png](assets/03-usr-homeassistant-37.png)

### hacs 中安装 `Blueprint Studio` 文本编辑器

#### 安装 `Blueprint Studio` 文本编辑器

点击 `HACS` -> 搜索 `Blueprint Studio` -> 选择 `Blueprint Studio` 文本编辑器 -> 点击 `Download`

![03-usr-homeassistant-38.png](assets/03-usr-homeassistant-38.png)

点击 `Download`

![03-usr-homeassistant-39.png](assets/03-usr-homeassistant-39.png)

点击 `设置` -> 点击 `Restart required`

![03-usr-homeassistant-40.png](assets/03-usr-homeassistant-40.png)

点击 `提交`

![03-usr-homeassistant-41.png](assets/03-usr-homeassistant-41.png)

点击 `完成`

![03-usr-homeassistant-42.png](assets/03-usr-homeassistant-42.png)

点击 `设置` -> 点击 `设备与服务`

![03-usr-homeassistant-43.png](assets/03-usr-homeassistant-43.png)

点击 `添加集成`

![03-usr-homeassistant-44.png](assets/03-usr-homeassistant-44.png)

搜索 `Blueprint Studio` -> 点击 `Blueprint Studio`

![03-usr-homeassistant-45.png](assets/03-usr-homeassistant-45.png)

点击 `提交`

![03-usr-homeassistant-46.png](assets/03-usr-homeassistant-46.png)

点击 `完成`

![03-usr-homeassistant-47.png](assets/03-usr-homeassistant-47.png)

完成界面

![03-usr-homeassistant-48.png](assets/03-usr-homeassistant-48.png)

#### 使用

点击 `Blueprint Studio` -> 点击 `目录结构` 按钮，就是 `Blueprint Studio` 主界面

![03-usr-homeassistant-49.png](assets/03-usr-homeassistant-49.png)

#### 备份 `configuration.yaml` 文件

点击 `Blueprint Studio` -> 点击 `目录结构` 按钮 -> 选中 `configuration.yaml` 文件 -> `右击` -> 点击 `复制`

![03-usr-homeassistant-50.png](assets/03-usr-homeassistant-50.png)

填写 `副本名称`

!!! info
    可以按照自己的备份文件命名习惯

![03-usr-homeassistant-51.png](assets/03-usr-homeassistant-51.png)

移动移动

![03-usr-homeassistant-52.png](assets/03-usr-homeassistant-52.png)

## 添加 `MQTT`

点击 `设置` -> 点击 `设备与服务`

![03-usr-homeassistant-53.png](assets/03-usr-homeassistant-53.png)

点击 `添加集成`

![03-usr-homeassistant-54.png](assets/03-usr-homeassistant-54.png)

搜索 `mqtt` -> 选中 `MQTT`

![03-usr-homeassistant-55.png](assets/03-usr-homeassistant-55.png)

点击 `MQTT`

![03-usr-homeassistant-56.png](assets/03-usr-homeassistant-56.png)

修改的参数：

- 代理：`192.168.16.224`
- 【默认】端口：`1883`
- 【默认】MQTT协议：`5`
- 【默认】用户名为空
- 【默认】密码为空

![03-usr-homeassistant-57.png](assets/03-usr-homeassistant-57.png)

点击 `完成`

![03-usr-homeassistant-58.png](assets/03-usr-homeassistant-58.png)

`MQTT` 界面

![03-usr-homeassistant-59.png](assets/03-usr-homeassistant-59.png)

## 添加传感器

点击 `Blueprint Studio` -> 选中 `configuration.yaml` 文件

![03-usr-homeassistant-60.png](assets/03-usr-homeassistant-60.png)

在 `configuration.yaml` 文件追加以下内容，并保存

```yaml
mqtt:
  sensor:
    # 电压 (u)
    - name: "正泰电表电压"
      unique_id: "zheng_tai_voltage"
      # 替换为实际主题
      state_topic: "/PubTopic1"
      value_template: "{{ value_json.params.r_data | selectattr('name', 'equalto', 'u') | map(attribute='value') | first }}"
      unit_of_measurement: "V"
      device_class: "voltage"
      state_class: "measurement"
      # 强制显示1位小数
      suggested_display_precision: 1

    # 电流 (i)
    - name: "正泰电表电流"
      unique_id: "zheng_tai_current"
      state_topic: "/PubTopic1"
      value_template: "{{ value_json.params.r_data | selectattr('name', 'equalto', 'i') | map(attribute='value') | first }}"
      unit_of_measurement: "A"
      device_class: "current"
      state_class: "measurement"
      # 强制显示3位小数
      suggested_display_precision: 3

    # 功率 (p)
    - name: "正泰电表功率"
      unique_id: "zheng_tai_power"
      state_topic: "/PubTopic1"
      value_template: "{{ value_json.params.r_data | selectattr('name', 'equalto', 'p') | map(attribute='value') | first }}"
      unit_of_measurement: "kW"
      device_class: "power"
      state_class: "measurement"
      # 强制显示3位小数
      suggested_display_precision: 3

    # 无功功率 (q)
    - name: "正泰电表无功功率"
      unique_id: "zheng_tai_reactive_power"
      state_topic: "/PubTopic1"
      value_template: "{{ value_json.params.r_data | selectattr('name', 'equalto', 'q') | map(attribute='value') | first }}"
      unit_of_measurement: "kvar"
      device_class: "reactive_power"
      state_class: "measurement"
      # 强制显示3位小数
      suggested_display_precision: 3

    # 视在功率 (s)
    - name: "正泰电表视在功率"
      unique_id: "zheng_tai_apparent_power"
      state_topic: "/PubTopic1"
      value_template: "{{ value_json.params.r_data | selectattr('name', 'equalto', 's') | map(attribute='value') | first }}"
      unit_of_measurement: "kVA"
      device_class: "apparent_power"
      state_class: "measurement"
      # 强制显示3位小数
      suggested_display_precision: 3

    # 功率因数 (pf)
    - name: "正泰电表功率因数"
      unique_id: "zheng_tai_power_factor"
      state_topic: "/PubTopic1"
      value_template: "{{ value_json.params.r_data | selectattr('name', 'equalto', 'pf') | map(attribute='value') | first }}"
      unit_of_measurement: ""
      device_class: "power_factor"
      state_class: "measurement"
      # 强制显示2位小数
      suggested_display_precision: 2

    # 频率 (freq)
    - name: "正泰电表频率"
      unique_id: "zheng_tai_frequency"
      state_topic: "/PubTopic1"
      value_template: "{{ value_json.params.r_data | selectattr('name', 'equalto', 'freq') | map(attribute='value') | first }}"
      unit_of_measurement: "Hz"
      device_class: "frequency"
      state_class: "measurement"
      # 强制显示2位小数
      suggested_display_precision: 2

    # 用电量 (ep)
    - name: "正泰电表总用电量"
      unique_id: "zheng_tai_energy"
      state_topic: "/PubTopic1"
      value_template: "{{ (value_json.params.r_data | selectattr('name', 'equalto', 'ep') | map(attribute='value') | first) | float }}"
      unit_of_measurement: "kWh"
      device_class: "energy"
      state_class: "total_increasing"
      # 强制显示2位小数
      suggested_display_precision: 2

```

【快速重载】

## 在仪表盘中添加传感器

点击 `快乐小屋`

![03-usr-homeassistant-61.png](assets/03-usr-homeassistant-61.png)

点击 `+`

![03-usr-homeassistant-62.png](assets/03-usr-homeassistant-62.png)

点击 `按卡片`

![03-usr-homeassistant-63.png](assets/03-usr-homeassistant-63.png)

点击 `传感器`

![03-usr-homeassistant-64.png](assets/03-usr-homeassistant-64.png)

实体选中 `正泰电表电压`，调整好布局等参数，点击完成

![03-usr-homeassistant-65.png](assets/03-usr-homeassistant-65.png)

!!! info
    其他正泰采集的实体如上述操作，这里不再重复描述

这是最终的仪表盘

![03-usr-homeassistant-66.png](assets/03-usr-homeassistant-66.png)

## 设置能源

点击 `能源` -> 点击 `添加电网连接` 

![03-usr-homeassistant-67.png](assets/03-usr-homeassistant-67.png)

选择 `电网输入的能源`、`成本跟踪` -> 点击 `保存`

![03-usr-homeassistant-68.png](assets/03-usr-homeassistant-68.png)

点击 `下一步`

![03-usr-homeassistant-69.png](assets/03-usr-homeassistant-69.png)

点击 `下一步`

![03-usr-homeassistant-70.png](assets/03-usr-homeassistant-70.png)

点击 `下一步`

![03-usr-homeassistant-71.png](assets/03-usr-homeassistant-71.png)

点击 `下一步`

![03-usr-homeassistant-72.png](assets/03-usr-homeassistant-72.png)

点击 `看看我的能源仪表盘`

![03-usr-homeassistant-73.png](assets/03-usr-homeassistant-73.png)

查看能源仪表盘

![03-usr-homeassistant-74.png](assets/03-usr-homeassistant-74.png)

!!! info "参考链接"
    1. [Platform installation](https://www.home-assistant.io/installation/linux/){target=blank}
    2. [Blueprint Studio for Home Assistant 🚀](https://github.com/ha-china/blueprint-studio){target=blank}