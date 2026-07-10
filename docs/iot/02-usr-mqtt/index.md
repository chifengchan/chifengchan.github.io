# 有人304获取正泰电表数据并本地MQTT采集

## 搭建 MQTT 消息服务器 - EMQX

### 使用 `docker` 来部署消息服务器 `EMQX`

创建工作目录

```shell
mkdir -p /home/houzzkit/02-emqx/{log,data}
```

设置目录所有者为 UID 1000（EMQX 容器内默认用户）

```shell
sudo chown -R 1000:1000 /home/houzzkit/02-emqx/
```

给目录设置合适的权限

```shell
sudo chmod -R 755 /home/houzzkit/02-emqx/
```

进入工作目录

```shell
cd /home/houzzkit/02-emqx
```

创建 `docker-compose.yml`

```shell
nano /home/houzzkit/02-emqx/docker-compose.yml
```

内容如下

```yml
services:
  emqx:
    image: 'emqx/emqx:6.2.2'
    container_name: emqx
    volumes:
      - '/home/houzzkit/02-emqx/log:/opt/emqx/log'
      - '/home/houzzkit/02-emqx/data:/opt/emqx/data'
    ports:
      - '18083:18083'
      - '8883:8883'
      - '8084:8084'
      - '8083:8083'
      - '1883:1883'
    healthcheck:
      test: ["CMD", "/opt/emqx/bin/emqx", "ctl", "status"]
      interval: 5s
      timeout: 25s
      retries: 5
    cap_add:
      - SYS_RESOURCE
      - NET_ADMIN
```

启动服务

```shell
sudo docker compose up -d
```

或

```shell
sudo docker-compose up -d
```

访问 EMQX 管理后台：[http://192.168.16.224:18083/](http://192.168.16.224:18083/){target=blank}，默认账号：`admin`，默认密码：`public`

!!! info
    1. 上述 `192.168.16.224` 为消息服务器设备IP，请根据实际修改，
    2. 首次登录会提示修改密码，应尽快修改默认密码！

EMQX 后台管理，如下图所示：

![02-usr-mqtt-01.png](assets/02-usr-mqtt-01.png)

## 有人 MQTT 设置

### 通讯接线示例

![02-usr-mqtt-02.png](assets/02-usr-mqtt-02.png)

!!! info
    正泰电表需要带 `Modbus` 通讯协议，并设置为 `Modbus` 通讯协议

!!! warning "安全提醒"
    本文涉及 220V 交流电操作，接线错误可能导致设备损坏、火灾甚至触电伤亡。

    若您不具备专业电工知识与操作资质，请勿自行操作，务必委托持证电工完成接线。
    
    作者仅提供技术参考，不承担任何因操作不当引发的安全责任。

### 电脑连接有人设备

`有人串口服务器 304` 设备上电并使用网线接入电脑

![02-usr-mqtt-03.png](assets/02-usr-mqtt-03.png)

### 访问有人设备

访问 `http://192.168.0.7`，账号密码均为 `admin`

![02-usr-mqtt-04.png](assets/02-usr-mqtt-04.png)

### 切换中文语言

点击右上角 `中文` 切换中文语言

![02-usr-mqtt-05.png](assets/02-usr-mqtt-05.png)

### DHCP 获取 IP

`网络参数` -> `IP获取方式` -> 调整为 `静态IP` -> `保存` -> `重启模块`

![02-usr-mqtt-06.png](assets/02-usr-mqtt-06.png)

接入网络后重新访问设备管理后台

!!! info "注意"
    上图中的配置请根据实际网络情况填写

### 设置 `端口参数`

修改参数为：

- 波特率：`9600`
- 工作方式：`MQTT`

![02-usr-mqtt-07.png](assets/02-usr-mqtt-07.png)

### 设置 `MQTT网关`

修改参数为：

- 客户端ID内容：`usr-device`
- 服务器域名(IP)：`192.168.16.22`

修改发布配置配置为：

- 勾选 `主题1`，其他默认配置

整体配置如下：

![02-usr-mqtt-08.png](assets/02-usr-mqtt-08.png)

![02-usr-mqtt-09.png](assets/02-usr-mqtt-09.png)

### 设置 `边缘网关`

将 `工作模式` 修改为 `轻边缘网关`

![02-usr-mqtt-10.png](assets/02-usr-mqtt-10.png)

### 配置 `点表参数`

删除全部参数配置

![02-usr-mqtt-11.png](assets/02-usr-mqtt-11.png)

添加一条点表参数

![02-usr-mqtt-12.png](assets/02-usr-mqtt-12.png)

参数修改：

- 设备名称：`u`
- 寄存器类型：`3`
- 寄存器地址：`8193`
- 数据类型：`32位浮点数(ABCD)`
- 倍率：`1`

![02-usr-mqtt-13.png](assets/02-usr-mqtt-13.png)

### 保存并重启模块

![02-usr-mqtt-14.png](assets/02-usr-mqtt-14.png)

!!! info "建议"
    每次保存后若看到 `模块管理`，即可点击 `重启模块`

## 查看 `EMQX` 监控

![02-usr-mqtt-15.png](assets/02-usr-mqtt-15.png)

## MQTT 查看订阅

打开 MQTT 软件

### 添加连接

点击 `Connections`，参数为：

- Name：`chnt-ddsu666`
- Host：`192.168.16.224`

点击 `Connect`

![02-usr-mqtt-16.png](assets/02-usr-mqtt-16.png)

### 添加订阅

点击 `New Subscription`

![02-usr-mqtt-17.png](assets/02-usr-mqtt-17.png)

!!! info "额外操作"
    同时将推送的消息格式调节为 `JSON`

参数为：

- Topic：`/PubTopic1`

点击 `Confirm`

![02-usr-mqtt-18.png](assets/02-usr-mqtt-18.png)

### 查看订阅消息

等待 `30秒` ，即可查看订阅消息

![02-usr-mqtt-19.png](assets/02-usr-mqtt-19.png)

## 有人 MQTT 设置

### 补全点表参数

![02-usr-mqtt-20.png](assets/02-usr-mqtt-20.png)

最终得到的消息

![02-usr-mqtt-21.png](assets/02-usr-mqtt-21.png)

## 额外

### 使用 ESP32-C6 + LED 屏幕 显示电表信息

![02-usr-mqtt-22.png](assets/02-usr-mqtt-22.png)

### 导入 `点表参数`

删除 `点表参数` 中所有配置

本地创建 `config.json` 文件，文件内容如下

```json
{
  "datas": [
    {
      "key": 1,
      "name": "u",
      "sadd": 1,
      "rtype": 3,
      "radd": 8193,
      "dtype": 7,
      "magn": "1",
      "report": 0,
      "ampl": "0"
    },
    {
      "key": 2,
      "name": "i",
      "sadd": 1,
      "rtype": 3,
      "radd": 8195,
      "dtype": 7,
      "magn": "1",
      "report": 0,
      "ampl": "0"
    },
    {
      "key": 3,
      "name": "p",
      "sadd": 1,
      "rtype": 3,
      "radd": 8197,
      "dtype": 7,
      "magn": "1",
      "report": 0,
      "ampl": "0"
    },
    {
      "key": 4,
      "name": "q",
      "sadd": 1,
      "rtype": 3,
      "radd": 8199,
      "dtype": 7,
      "magn": "1",
      "report": 0,
      "ampl": "0"
    },
    {
      "key": 5,
      "name": "s",
      "sadd": 1,
      "rtype": 3,
      "radd": 8201,
      "dtype": 7,
      "magn": "1",
      "report": 0,
      "ampl": "0"
    },
    {
      "key": 6,
      "name": "pf",
      "sadd": 1,
      "rtype": 3,
      "radd": 8203,
      "dtype": 7,
      "magn": "1",
      "report": 0,
      "ampl": "0"
    },
    {
      "key": 7,
      "name": "freq",
      "sadd": 1,
      "rtype": 3,
      "radd": 8207,
      "dtype": 7,
      "magn": "1",
      "report": 0,
      "ampl": "0"
    },
    {
      "key": 8,
      "name": "ep",
      "sadd": 1,
      "rtype": 3,
      "radd": 16385,
      "dtype": 7,
      "magn": "1",
      "report": 0,
      "ampl": "0"
    }
  ]
}
```

进入 `点表参数` 界面中

点击 `选择文件`

上传 `config.json` 文件

点击 `保存` 并 `重启模块`

!!! info "参考文档"
    1. [【说明书】USR-TCP232-30X系列说明书V3.0.2.pdf](https://www.usr.cn/wiki/puba/Br6DMHFlk){target=blank}
    
    2. [正泰单相 DDSU666](https://www.solarahorizon.cn/helpcenter/electric-meter/ddsu666.html){target=blank}

    3. [DDSU666 单相电子式电能表（导轨） 使用说明书 ZTW0.464.0090](https://aiot.chint.com/upload/img/2024-03/6604b8f947f2c.pdf){target=blank}

