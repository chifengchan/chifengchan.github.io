# ESP8266(ESPHome)通过RS485转ttl模块获取正泰电表数据并在Home Assistant中采集和可视化

## ESPHome

### 使用 `docker` 创建 `ESPHome`

创建工作目录

```shell
mkdir -p /home/houzzkit/04-esphome
```

进入工作目录

```shell
cd /home/houzzkit/04-esphome
```

创建 `docker-compose.yml`

```shell
nano /home/houzzkit/04-esphome/docker-compose.yml
```

内容如下

```yml
services:
  esphome:
    container_name: esphome
    image: ghcr.io/esphome/esphome:2026.7.0
    volumes:
      - /home/houzzkit/04-esphome/config:/config
      - /etc/localtime:/etc/localtime:ro
    restart: unless-stopped
    privileged: true
    network_mode: host
    environment:
      # PyPI镜像
      - PIP_INDEX_URL=https://pypi.tuna.tsinghua.edu.cn/simple
      - PIP_TRUSTED_HOST=pypi.tuna.tsinghua.edu.cn
      - PIP_DEFAULT_TIMEOUT=600
      
      # ESP-IDF下载优化
      - IDF_GITHUB_ASSETS=dl.espressif.com/github_assets
      - ESPHOME_USE_SUBPROCESS=1
      - ESPHOME_SKIP_VERSION_CHECK=1

      # 时区
      - TZ=Asia/Shanghai

      # 登录账号密码
      # - ESPHOME_USERNAME=test
      # - ESPHOME_PASSWORD=ChangeMe
```

启动服务

```shell
sudo docker compose up -d
```

或

```shell
sudo docker-compose up -d
```

### `ESPHome` 初始化界面

访问 ESPHome：[http://192.168.16.224:6052/](http://192.168.16.224:6052/){target=blank}

点击 `继续`

![04-usr-esphome-01.png](assets/04-usr-esphome-01.png)

选择 `ESPHome 新用户` -> 点击 `继续`

![04-usr-esphome-02.png](assets/04-usr-esphome-02.png)

点击 `也许以后`

![04-usr-esphome-03.png](assets/04-usr-esphome-03.png)

### `ESPHome` 主界面

这就是 ESPHome 主界面

![04-usr-esphome-04.png](assets/04-usr-esphome-04.png)

## ESP8266 和 ESP32-C6 刷写 ESPHome 固件

### 开发板 `实现主要功能` 以及 `注意事项`

1. 刷写 ESPHome 固件。***配置中定义的硬件 如传感器、开关、灯光等，将自动出现在 Home Assistant 的用户界面中***

2. 能通过 `RS485 转 TTL` 模块读取正泰电表 ***ddsu666*** 型号的电表数据。 
    
    2.1. `RS485 转 TTL` 为 ***TTL转RS485模块 485转串口UART电平互转通讯自动流向控制自动双向***

    2.2. 正泰电表型号为 ***ddsu666***、***单相***，并且已经切换为 ***ModBus*** 协议

3. 能控制 ***开发板板载LED***

!!! info
    - 操作电脑需要提前安装好驱动（比如：CH340驱动）
    - 根据实际情况，将开发板使用跳线或者USB数据线连接至电脑
    - 正泰电表需要带 `Modbus` 通讯协议，并设置为 `Modbus` 通讯协议

!!! warning "安全提醒"
    本文涉及 220V 交流电操作，接线错误可能导致设备损坏、火灾甚至触电伤亡。

    若您不具备专业电工知识与操作资质，请勿自行操作，务必委托持证电工完成接线。
    
    作者仅提供技术参考，不承担任何因操作不当引发的安全责任。

### `ESP32-C6` 推荐实验连线

![04-usr-esphome-05.png](assets/04-usr-esphome-05.png)

### `ESP32-C6` 刷写 `ESPHome` (WEB + WIFI + 板载LED + 线刷固件)

#### 创建设备

![04-usr-esphome-06.png](assets/04-usr-esphome-06.png)

#### 选择 `空配置`

点击 `高级设置选项` -> 点击 `空配置`

![04-usr-esphome-07.png](assets/04-usr-esphome-07.png)

输出设备名称 `esp32-c6-power-meter`

![04-usr-esphome-08.png](assets/04-usr-esphome-08.png)

这就是空配置界面

![04-usr-esphome-09.png](assets/04-usr-esphome-09.png)

#### 设置 `密钥`

点击右上角 -> 点击 `密钥`

![04-usr-esphome-10.png](assets/04-usr-esphome-10.png)

将红色区域内的内容

![04-usr-esphome-11.png](assets/04-usr-esphome-11.png)

替换为如下

```yml
# Secrets — referenced from device configs via !secret
# Add Wi-Fi credentials here, or let the create-device
# wizard add them for you.

# ==================== Web 管理界面 ====================
# 通过浏览器访问设备IP时，管理页面的登录凭证
web_login_username: "admin"   # 网页登录用户名（建议设置，留空则无认证）
web_login_password: "admin"   # 网页登录密码（建议使用强密码）

# ==================== WiFi 网络配置 ====================
wifi_ssid: "<<YOUR_WIFI_SSID>>"            # 目标WiFi名称（即路由器SSID）
wifi_password: "<<YOUR_WIFI_PASSWORD>>"        # 目标WiFi密码

# ==================== AP 热点模式配置 ====================
# 当设备无法连接到目标WiFi时，自动开启的应急热点
wifi_ap_password: "12345678"     # 热点密码（至少8位，留空则无密码，建议设置）

# ==================== OTA 无线升级 ====================
# 通过WiFi进行固件升级时所需的认证密码
ota_password: "12345678"         # OTA升级密码（请务必设置，防止未授权刷写）

# ==================== API 加密通信 ====================
# 与Home Assistant等系统通信时，用于加密的预共享密钥
# （可通过 `esphome config <设备名>` 命令生成）
# 可以在 https://esphome.io/components/api/#configuration-variables 生成密钥
api_encryption_key: "/w9meS/Uu0rAK/2+5ai2LFI6V6WCCDj4FxSiGyy/RWA="   # 32字节Base64编码的加密密钥（留空则使用明文通信）
```

!!! info 注意
    - 参数 `wifi_ssid` 和 `wifi_password` 必须根据实际情况填写！
    - 参数 `api_encryption_key` 可在 [https://esphome.io/components/api/#configuration-variables](https://esphome.io/components/api/#configuration-variables){target=blank} 中生成
    - 其他参数根据实际情况修改

编辑完成后点击保存后返回

#### 完整配置（集成 WEB + WIFI + 板载LED）

将红色区域内的内容

![04-usr-esphome-12.png](assets/04-usr-esphome-12.png)

替换为如下

```yml
# ===================================================================
# ESPHome 配置文件 - ESP32-C6 电力监测仪
# ===================================================================
# 硬件平台: ESP32-C6-DevKitC-1
# 主要功能: 电力参数采集 + Web仪表板 + Home Assistant集成
# 维护人员: [请填写你的名字]
# 最后更新: 2026-07-22
# ===================================================================

# ===================================================================
# 1. ESPHome 核心配置
# ===================================================================
esphome:
  name: "esp32-c6-power-meter"  # 设备唯一标识名（编译后不可修改）

  # 启动回调：设备上电后自动执行的初始化动作
  # 此处将板载LED点亮为白色（50%亮度），用于指示设备已正常启动
  on_boot:
    - light.turn_on:
        id: board_led
        brightness: 50%                 # 亮度50%，避免过亮刺眼
        red: 100%                       # 三色全开 → 混色为白色
        green: 100%
        blue: 100%

# ===================================================================
# 2. 硬件平台配置（ESP32-C6）
# ===================================================================
esp32:
  variant: ESP32C6                      # 芯片型号（RISC-V架构）
  framework:
    type: esp-idf                      # 使用ESP-IDF框架（比Arduino更稳定）
  flash_size: 4MB                      # 板载Flash容量（需与实际一致）

# ===================================================================
# 3. 日志系统
# ===================================================================
# 通过串口输出调试日志（波特率默认115200）
# 生产环境可注释掉以减少资源占用
logger:

# ===================================================================
# 4. Web Server（核心功能：提供网页管理界面）
# ===================================================================
# 版本3支持分组排序、认证、OTA等高级特性
web_server:
  version: 3
  port: 80                             # HTTP端口（标准Web端口）
  auth:                                # 基础认证（Base64编码传输，建议配合HTTPS使用）
    type: basic
    username: !secret web_login_username   # 从secrets.yaml读取
    password: !secret web_login_password   # 从secrets.yaml读取

  # 分组定义：对Web界面中的传感器进行逻辑分组，便于阅读
  # 权重越小排序越靠前
  sorting_groups:
    - id: system_group
      name: "系统参数"
      sorting_weight: 10                # 权重10 → 排在第一位

# ===================================================================
# 5. 网络配置
# ===================================================================
wifi:
  ssid: !secret wifi_ssid              # 从secrets.yaml读取（安全）
  password: !secret wifi_password
  
  # 备用热点模式：当WiFi连接失败时，设备自身开启AP热点
  ap:
    ssid: "ESP32-C6 Power Meter"       # 热点名称（可自定义）
    password: !secret wifi_ap_password # 建议设置8位以上复杂密码

# 强制门户认证：连接AP后自动弹出配置页面（简化配网流程）
captive_portal:

# ===================================================================
# 6. OTA 固件升级（两种方式保障）
# ===================================================================
ota:
  - platform: esphome                  # ESPHome原生OTA方式
    password: !secret ota_password
  
  - platform: web_server               # Web界面OTA（通过浏览器上传固件）
                                       # 参考：ESPhome Web Server OTA功能 [citation:2][citation:4]

# ===================================================================
# 7. Home Assistant API（可选集成）
# ===================================================================
# 启用后可将所有传感器自动暴露给Home Assistant
# 加密密钥需与HA侧保持一致
api:
  encryption:
    key: !secret api_encryption_key

# ===================================================================
# 8. 板载LED（状态指示灯）
# ===================================================================
light:
  - platform: esp32_rmt_led_strip
    name: "板载LED"
    id: board_led                      # 供on_boot和其他动作引用
    
    pin: GPIO8                         # ESP32-C6-DevKitC-1板载LED引脚（注意：官方文档确认）
    num_leds: 1                        # 开发板通常只有1个LED
    
    rgb_order: GRB                     # WS2812的颜色通道顺序（GRB是默认）
    chipset: WS2812                    # 明确指定芯片型号（确保驱动正确）
    
    restore_mode: ALWAYS_OFF           # 设备重启后LED保持关闭（避免意外亮起）
    
    # Web界面排序（归入系统参数组，权重7次于信息类）
    web_server:
      sorting_group_id: system_group
      sorting_weight: 7

# ===================================================================
# 9. 文本传感器（静态信息展示）
# ===================================================================
text_sensor:
  - platform: wifi_info
    ssid:
      name: "设备连接的WiFi名称"
      web_server: 
        sorting_group_id: system_group
        sorting_weight: 2               # 权重2 → 优先显示
    ip_address:
      name: "设备IP地址"
      web_server: 
        sorting_group_id: system_group
        sorting_weight: 3
    mac_address:
      name: "设备MAC地址"
      web_server:
        sorting_group_id: system_group
        sorting_weight: 4

# ===================================================================
# 10. 数值传感器（采集电力数据和信号质量）
# ===================================================================
sensor:
  # -------------------- WiFi信号强度（dBm） --------------------
  - platform: wifi_signal
    name: "WiFi信号_dB"
    id: "xinhao1"                       # 作为源传感器供copy使用
    update_interval: 10s                # 每10秒更新一次
    entity_category: "diagnostic"       # 归类为诊断数据（HA中可隐藏）
    web_server:
      sorting_group_id: system_group
      sorting_weight: 5

  # -------------------- WiFi信号强度（百分比） --------------------
  # 通过copy + lambda公式将dBm转换为0-100%的直观表示
  - platform: copy
    source_id: xinhao1
    id: wifi_signal_percent
    name: "WiFi信号_%"
    
    # 转换公式：将-100dBm ~ -50dBm 映射到 0% ~ 100%
    # 解释：x为dBm值，+100后范围0~50，乘以2得0~100，并限幅
    filters:
      - lambda: return min(max(2 * (x + 100.0), 0.0), 100.0);
    
    unit_of_measurement: "%"
    entity_category: "diagnostic"
    device_class: ""                    # 无特定设备类型（不显示图标）
    web_server:
      sorting_group_id: system_group
      sorting_weight: 6
```

编辑完成后点击保存

#### 下载固件二进制文件

点击 `安装` -> 点击 `高级选项` -> 点击 `下载固件二进制文件`

![04-usr-esphome-13.png](assets/04-usr-esphome-13.png)

等待编译完成 -> 点击 `出厂镜像`

![04-usr-esphome-14.png](assets/04-usr-esphome-14.png)

点击 `保留`，得到 `esp32-c6-power-meter-firmware.factory.bin` 出厂镜像的固件

![04-usr-esphome-15.png](assets/04-usr-esphome-15.png)

#### 线刷固件

打开 [https://web.esphome.io/](https://web.esphome.io/){target=blank}

点击 `连接`

![04-usr-esphome-16.png](assets/04-usr-esphome-16.png)

选择串行端口 -> 点击 `连接`

![04-usr-esphome-17.png](assets/04-usr-esphome-17.png)

点击 `上传`

![04-usr-esphome-18.png](assets/04-usr-esphome-18.png)

上传 `esp32-c6-power-meter-firmware.factory.bin` 出厂镜像的固件 -> 点击 `安装`

![04-usr-esphome-19.png](assets/04-usr-esphome-19.png)

安装完成，如下图所示

![04-usr-esphome-20.png](assets/04-usr-esphome-20.png)

#### WEB 界面展示

从路由器中获取 `ESP32-C6` 设备的IP

访问 [http://192.168.16.32/](http://192.168.16.32/){target=blank}，出现登录提醒，如下图

![04-usr-esphome-21.png](assets/04-usr-esphome-21.png)

!!! info
    账号密码是在 `密钥` 设置中的参数 `web_login_username` 和 `web_login_password`

这就是 WEB 界面

![04-usr-esphome-22.png](assets/04-usr-esphome-22.png)

#### 测试 `板载LED`

点击 `关闭`，板载LED熄灭，其他效果自行测试

![04-usr-esphome-23.png](assets/04-usr-esphome-23.png)

#### 在 `ESPHome` 界面中获取在线信息

ESPHome 显示设备在线

![04-usr-esphome-24.png](assets/04-usr-esphome-24.png)

点击设备，下图就是在线设备信息

![04-usr-esphome-25.png](assets/04-usr-esphome-25.png)

### `ESP32-C6` 刷写 `ESPHome` (备注 + OTA更新固件)

#### 添加 `备注` 信息

编辑 `esp32-c6-power-meter`，添加 `备注` 信息，如下图红色区域

![04-usr-esphome-26.png](assets/04-usr-esphome-26.png)

修改为

```yaml
# ===================================================================
# ESPHome 配置文件 - ESP32-C6 电力监测仪
# ===================================================================
# 硬件平台: ESP32-C6-DevKitC-1
# 主要功能: 电力参数采集 + Web仪表板 + Home Assistant集成
# 维护人员: [请填写你的名字]
# 最后更新: 2026-07-22
# ===================================================================

# ===================================================================
# 1. ESPHome 核心配置
# ===================================================================
esphome:
  name: "esp32-c6-power-meter"  # 设备唯一标识名（编译后不可修改）

  # 启动回调：设备上电后自动执行的初始化动作
  # 此处将板载LED点亮为白色（50%亮度），用于指示设备已正常启动
  on_boot:
    - light.turn_on:
        id: board_led
        brightness: 50%                 # 亮度50%，避免过亮刺眼
        red: 100%                       # 三色全开 → 混色为白色
        green: 100%
        blue: 100%

# ===================================================================
# 2. 硬件平台配置（ESP32-C6）
# ===================================================================
esp32:
  variant: ESP32C6                      # 芯片型号（RISC-V架构）
  framework:
    type: esp-idf                      # 使用ESP-IDF框架（比Arduino更稳定）
  flash_size: 4MB                      # 板载Flash容量（需与实际一致）

# ===================================================================
# 3. 日志系统
# ===================================================================
# 通过串口输出调试日志（波特率默认115200）
# 生产环境可注释掉以减少资源占用
logger:

# ===================================================================
# 4. Web Server（核心功能：提供网页管理界面）
# ===================================================================
# 版本3支持分组排序、认证、OTA等高级特性
web_server:
  version: 3
  port: 80                             # HTTP端口（标准Web端口）
  auth:                                # 基础认证（Base64编码传输，建议配合HTTPS使用）
    type: basic
    username: !secret web_login_username   # 从secrets.yaml读取
    password: !secret web_login_password   # 从secrets.yaml读取

  # 分组定义：对Web界面中的传感器进行逻辑分组，便于阅读
  # 权重越小排序越靠前
  sorting_groups:
    - id: system_group
      name: "系统参数"
      sorting_weight: 10                # 权重10 → 排在第一位

# ===================================================================
# 5. 网络配置
# ===================================================================
wifi:
  ssid: !secret wifi_ssid              # 从secrets.yaml读取（安全）
  password: !secret wifi_password
  
  # 备用热点模式：当WiFi连接失败时，设备自身开启AP热点
  ap:
    ssid: "ESP32-C6 Power Meter"       # 热点名称（可自定义）
    password: !secret wifi_ap_password # 建议设置8位以上复杂密码

# 强制门户认证：连接AP后自动弹出配置页面（简化配网流程）
captive_portal:

# ===================================================================
# 6. OTA 固件升级（两种方式保障）
# ===================================================================
ota:
  - platform: esphome                  # ESPHome原生OTA方式
    password: !secret ota_password
  
  - platform: web_server               # Web界面OTA（通过浏览器上传固件）
                                       # 参考：ESPhome Web Server OTA功能 [citation:2][citation:4]

# ===================================================================
# 7. Home Assistant API（可选集成）
# ===================================================================
# 启用后可将所有传感器自动暴露给Home Assistant
# 加密密钥需与HA侧保持一致
api:
  encryption:
    key: !secret api_encryption_key

# ===================================================================
# 8. 板载LED（状态指示灯）
# ===================================================================
light:
  - platform: esp32_rmt_led_strip
    name: "板载LED"
    id: board_led                      # 供on_boot和其他动作引用
    
    pin: GPIO8                         # ESP32-C6-DevKitC-1板载LED引脚（注意：官方文档确认）
    num_leds: 1                        # 开发板通常只有1个LED
    
    rgb_order: GRB                     # WS2812的颜色通道顺序（GRB是默认）
    chipset: WS2812                    # 明确指定芯片型号（确保驱动正确）
    
    restore_mode: ALWAYS_OFF           # 设备重启后LED保持关闭（避免意外亮起）
    
    # Web界面排序（归入系统参数组，权重7次于信息类）
    web_server:
      sorting_group_id: system_group
      sorting_weight: 7

# ===================================================================
# 9. 文本传感器（静态信息展示）
# ===================================================================
text_sensor:
  # ---------- WiFi 网络信息（动态获取） ----------
  - platform: wifi_info
    ssid:
      name: "设备连接的WiFi名称"
      web_server: 
        sorting_group_id: system_group
        sorting_weight: 2               # 权重2 → 优先显示
    ip_address:
      name: "设备IP地址"
      web_server: 
        sorting_group_id: system_group
        sorting_weight: 3
    mac_address:
      name: "设备MAC地址"
      web_server:
        sorting_group_id: system_group
        sorting_weight: 4
  # ---------- 硬件接线备注（静态提示） ----------
  # 【用途】在Web界面显示硬件连接说明，方便现场维护人员快速确认接线
  # 【场景】多人协作/长期运维时，避免遗忘或查错文档
  # 【注意】此传感器仅用于信息展示，不参与任何逻辑判断
  - platform: template
    name: "设备备注"
    
    # 更新间隔：86400秒 = 24小时
    # 由于是静态文本，无需频繁更新，每天刷新一次即可
    update_interval: 86400s
    
    # Lambda表达式：返回要显示的字符串
    # 注意：RS485模块的TX/RX与ESP32的RX/TX为交叉连接（即TX→RX，RX→TX）
    lambda: |-
      return {
        "RS485模块的TX接ESP32-C6的GPIO4，RX接ESP32-C6的GPIO5"
      };
    
    # Web界面排序：归入系统参数组，权重8（排在WiFi信息之后）
    web_server:
      sorting_group_id: system_group
      sorting_weight: 8

# ===================================================================
# 10. 数值传感器（采集电力数据和信号质量）
# ===================================================================
sensor:
  # -------------------- WiFi信号强度（dBm） --------------------
  - platform: wifi_signal
    name: "WiFi信号_dB"
    id: "xinhao1"                       # 作为源传感器供copy使用
    update_interval: 10s                # 每10秒更新一次
    entity_category: "diagnostic"       # 归类为诊断数据（HA中可隐藏）
    web_server:
      sorting_group_id: system_group
      sorting_weight: 5

  # -------------------- WiFi信号强度（百分比） --------------------
  # 通过copy + lambda公式将dBm转换为0-100%的直观表示
  - platform: copy
    source_id: xinhao1
    id: wifi_signal_percent
    name: "WiFi信号_%"
    
    # 转换公式：将-100dBm ~ -50dBm 映射到 0% ~ 100%
    # 解释：x为dBm值，+100后范围0~50，乘以2得0~100，并限幅
    filters:
      - lambda: return min(max(2 * (x + 100.0), 0.0), 100.0);
    
    unit_of_measurement: "%"
    entity_category: "diagnostic"
    device_class: ""                    # 无特定设备类型（不显示图标）
    web_server:
      sorting_group_id: system_group
      sorting_weight: 6
```

#### 下载固件二进制文件

点击 `安装` -> 点击 `高级选项` -> 点击 `下载固件二进制文件`

![04-usr-esphome-27.png](assets/04-usr-esphome-27.png)

等待编译完成 -> 点击 `OTA 更新`

![04-usr-esphome-28.png](assets/04-usr-esphome-28.png)

点击 保留，得到 `esp32-c6-power-meter-firmware.ota.bin` 出厂镜像的固件

![04-usr-esphome-29.png](assets/04-usr-esphome-29.png)

#### OTA 更新固件

访问 `ESP32-C6` 的 `WEB` 界面，[http://192.168.16.32/](http://192.168.16.32/){target=blank}，如下图

点击 `选择文件` -> 上传 `esp32-c6-power-meter-firmware.ota.bin` OTA的固件 -> 点击 `UPDATE`

![04-usr-esphome-30.png](assets/04-usr-esphome-30.png)

等待 `OTA更新` 更新完毕，更新成功页面如下

![04-usr-esphome-31.png](assets/04-usr-esphome-31.png)

#### WEB 界面展示

重新访问 `ESP32-C6` 的 `WEB` 界面，[http://192.168.16.32/](http://192.168.16.32/){target=blank}，更新成功会出现 `备注` 信息，如下图

![04-usr-esphome-32.png](assets/04-usr-esphome-32.png)

### `ESP32-C6` 刷写 `ESPHome` (采集电表指标 + WIFI刷固件)

#### 添加 `采集电表指标`

编辑 `esp32-c6-power-meter`，添加 `采集电表指标` 信息，如下图红色区域

![04-usr-esphome-33.png](assets/04-usr-esphome-33.png)

修改为

```yaml
# ===================================================================
# ESPHome 配置文件 - ESP32-C6 电力监测仪
# ===================================================================
# 硬件平台: ESP32-C6-DevKitC-1
# 主要功能: 电力参数采集 + Web仪表板 + Home Assistant集成
# 维护人员: [请填写你的名字]
# 最后更新: 2026-07-22
# ===================================================================

# ===================================================================
# 1. ESPHome 核心配置
# ===================================================================
esphome:
  name: "esp32-c6-power-meter"  # 设备唯一标识名（编译后不可修改，修改后需重新烧录）

  # 启动回调：设备上电后自动执行的初始化动作
  # 此处将板载LED点亮为白色（50%亮度），用于指示设备已正常启动
  on_boot:
    - light.turn_on:
        id: board_led
        brightness: 50%                 # 亮度50%，避免过亮刺眼
        red: 100%                       # 三色全开 → 混色为白色
        green: 100%
        blue: 100%

# ===================================================================
# 2. 硬件平台配置（ESP32-C6）
# ===================================================================
esp32:
  variant: ESP32C6                      # 芯片型号（RISC-V架构，注意与ESP32/ESP32-S系列不兼容）
  framework:
    type: esp-idf                      # 使用ESP-IDF框架（比Arduino更稳定，支持更多功能）
    version: latest                    # 使用最新稳定版框架（可指定具体版本号）
  flash_size: 4MB                      # 板载Flash容量（需与实际一致，否则可能无法启动）

# ===================================================================
# 3. 日志系统
# ===================================================================
# 通过串口输出调试日志（波特率默认115200）
# 生产环境可注释掉以减少资源占用，或设置日志级别降低输出量
logger:
  level: DEBUG                         # 日志级别: DEBUG/INFO/WARN/ERROR/NONE
  # baud_rate: 115200                  # 默认即可，通常无需修改

# ===================================================================
# 4. Web Server（核心功能：提供网页管理界面）
# ===================================================================
# 版本3支持分组排序、认证、OTA等高级特性，相比版本2有更好的UI体验
web_server:
  version: 3
  port: 80                             # HTTP端口（标准Web端口，可改为其他端口如8080）
  auth:                                # 基础认证（Base64编码传输，建议配合HTTPS使用）
    type: basic
    username: !secret web_login_username   # 从secrets.yaml读取，安全性高于明文
    password: !secret web_login_password   # 建议设置强密码（大小写+数字+特殊字符）

  # 分组定义：对Web界面中的传感器进行逻辑分组，便于阅读
  # 权重越小排序越靠前（数值范围建议1-100）
  sorting_groups:
    - id: system_group
      name: "系统参数"
      sorting_weight: 10                # 权重10 → 排在第一位
    - id: sensor_group
      name: "传感器数据"
      sorting_weight: 20

# ===================================================================
# 5. 网络配置
# ===================================================================
wifi:
  ssid: !secret wifi_ssid              # 从secrets.yaml读取（安全存储，避免硬编码）
  password: !secret wifi_password
  
  # 备用热点模式：当WiFi连接失败时，设备自身开启AP热点
  # 该模式与正常WiFi连接互斥，仅在连接失败时启用
  ap:
    ssid: "ESP32-C6 Power Meter"       # 热点名称（可自定义，建议包含设备标识）
    password: !secret wifi_ap_password # 建议设置8位以上复杂密码，防止未授权访问
    ap_timeout: 5min                   # AP模式超时时间，超时后自动尝试重新连接WiFi

# 强制门户认证：连接AP后自动弹出配置页面（简化配网流程）
# 用户可通过WiFi配置页面输入新的SSID和密码
captive_portal:

# ===================================================================
# 6. OTA 固件升级（两种方式保障）
# ===================================================================
ota:
  - platform: esphome                  # ESPHome原生OTA方式（推荐使用）
    password: !secret ota_password     # 设置OTA密码防止未授权刷写
    port: 3232                         # OTA端口（默认3232，一般无需修改）
  
  - platform: web_server               # Web界面OTA（通过浏览器上传固件）
                                       # 参考：ESPhome Web Server OTA功能 [citation:2][citation:4]

# ===================================================================
# 7. Home Assistant API（可选集成）
# ===================================================================
# 启用后可将所有传感器自动暴露给Home Assistant
# 加密密钥需与HA侧保持一致，否则无法建立连接
api:
  encryption:
    key: !secret api_encryption_key    # 32字节密钥，通过"esphome generate_key"生成
  reboot_timeout: 15min                # HA连接超时后自动重启（避免设备死机）

# ===================================================================
# 8. 板载LED（状态指示灯）
# ===================================================================
light:
  - platform: esp32_rmt_led_strip      # 使用RMT驱动的LED灯带（精度高，资源占用少）
    name: "板载LED"
    id: board_led                      # 供on_boot和其他动作引用
    
    pin: GPIO8                         # ESP32-C6-DevKitC-1板载LED引脚（官方文档确认）
    num_leds: 1                        # 开发板通常只有1个LED（如果有多灯需修改）
    # num_leds: 1                      # 请根据实际硬件确认LED数量
    
    rgb_order: GRB                     # WS2812的颜色通道顺序（GRB是默认，SK6812可能需要改为RGB）
    chipset: WS2812                    # 明确指定芯片型号（确保驱动正确）
    
    restore_mode: ALWAYS_OFF           # 设备重启后LED保持关闭（避免意外亮起）
    
    # 默认转换：设置默认亮度为50%，保护视力的同时显示清晰
    default_transition_length: 0.5s    # 颜色切换过渡时间
    
    # Web界面排序（归入系统参数组，权重7次于信息类）
    web_server:
      sorting_group_id: system_group
      sorting_weight: 7

# ===================================================================
# 9. 文本传感器（静态信息展示）
# ===================================================================
text_sensor:
  # ---------- WiFi 网络信息（动态获取） ----------
  - platform: wifi_info                # 该平台提供WiFi连接状态信息
    ssid:
      name: "设备连接的WiFi名称"
      web_server: 
        sorting_group_id: system_group
        sorting_weight: 2               # 权重2 → 优先显示（仅次于设备名称）
    ip_address:
      name: "设备IP地址"
      web_server: 
        sorting_group_id: system_group
        sorting_weight: 3
    mac_address:
      name: "设备MAC地址"
      web_server:
        sorting_group_id: system_group
        sorting_weight: 4
  
  # ---------- 硬件接线备注（静态提示） ----------
  # 【用途】在Web界面显示硬件连接说明，方便现场维护人员快速确认接线
  # 【场景】多人协作/长期运维时，避免遗忘或查错文档
  # 【注意】此传感器仅用于信息展示，不参与任何逻辑判断
  - platform: template
    name: "设备备注"
    
    # 更新间隔：86400秒 = 24小时
    # 由于是静态文本，无需频繁更新，每天刷新一次即可
    update_interval: 86400s
    
    # Lambda表达式：返回要显示的字符串
    # 注意：RS485模块的TX/RX与ESP32的RX/TX为交叉连接（即TX→RX，RX→TX）
    lambda: |-
      return {
        "RS485模块的TX接ESP32-C6的GPIO4，RX接ESP32-C6的GPIO5"
      };
    
    # Web界面排序：归入系统参数组，权重8（排在WiFi信息之后）
    web_server:
      sorting_group_id: system_group
      sorting_weight: 8

# ----- 关键部分：Modbus RTU 配置 -----
# 【注意】Modbus是串行通信协议，需要正确配置UART参数和RS485控制引脚

# 1. 配置UART总线，连接RS485模块
uart:
  id: uart_modbus                      # UART总线ID，供Modbus模块引用
  tx_pin: GPIO4                        # RS485模块的TX接ESP32-C6的GPIO4
  rx_pin: GPIO5                        # RS485模块的RX接ESP32-C6的GPIO5
  baud_rate: 9600                      # 波特率（与电表设置一致，常见9600/4800）
  data_bits: 8                         # 数据位（Modbus标准为8位）
  stop_bits: 1                         # 停止位（标准为1位）
  parity: NONE                         # 校验位（NONE/EVEN/ODD，电表通常使用NONE）
  # debug:                             # 调试模式（取消注释可查看原始Modbus数据）

# 2. 配置Modbus主机
modbus:
  id: modbus_host
  uart_id: uart_modbus
  # 如果你的RS485模块需要硬件流控引脚（如MAX485），请取消注释并设置
  # flow_control_pin: GPIO4            # RS485收发控制引脚（DE/RE引脚）
  # 注意：某些模块通过自动方向控制，无需硬件流控

# 3. 配置Modbus控制器，指向电表
modbus_controller:
  - id: ddsu666_controller
    modbus_id: modbus_host
    address: 0x01                      # 电表默认Modbus地址（1-247，请根据实际修改）
    update_interval: 10s               # 数据更新间隔（频率不宜过高，避免总线拥堵）
      
# ===================================================================
# 10. 数值传感器（采集电力数据和信号质量）
# ===================================================================
sensor:
  # -------------------- WiFi信号强度（dBm） --------------------
  - platform: wifi_signal
    name: "WiFi信号_dB"
    id: "xinhao1"                      # 作为源传感器供copy使用
    update_interval: 10s               # 每10秒更新一次
    entity_category: "diagnostic"      # 归类为诊断数据（HA中可隐藏）
    web_server:
      sorting_group_id: system_group
      sorting_weight: 5

  # -------------------- WiFi信号强度（百分比） --------------------
  # 通过copy + lambda公式将dBm转换为0-100%的直观表示
  - platform: copy
    source_id: xinhao1
    id: wifi_signal_percent
    name: "WiFi信号_%"
    
    # 转换公式：将-100dBm ~ -50dBm 映射到 0% ~ 100%
    # 解释：x为dBm值，+100后范围0~50，乘以2得0~100，并限幅
    filters:
      - lambda: return min(max(2 * (x + 100.0), 0.0), 100.0);
    
    unit_of_measurement: "%"
    entity_category: "diagnostic"
    device_class: ""                   # 无特定设备类型（不显示图标）
    web_server:
      sorting_group_id: system_group
      sorting_weight: 6

  # -------------------- 电压（V） --------------------
  # Modbus地址0x2000，读取2个寄存器（32位浮点数）
  - platform: modbus_controller
    modbus_controller_id: ddsu666_controller
    id: my_u
    name: "电压_u"
    address: 0x2000                    # 寄存器起始地址（DDSU666电表定义）
    register_count: 2                  # 32位数据需要2个寄存器
    unit_of_measurement: V
    register_type: holding             # 保持寄存器（holding register）
    value_type: FP32                   # 32位浮点数（标准IEEE 754）
    accuracy_decimals: 1               # 显示1位小数（如230.1V）
    device_class: voltage
    state_class: measurement           # 测量值状态类型
    web_server:
      sorting_group_id: sensor_group
      sorting_weight: 1

  # -------------------- 电流（A） --------------------
  - platform: modbus_controller
    modbus_controller_id: ddsu666_controller
    id: my_i
    name: "电流_i"
    address: 0x2002
    register_count: 2
    unit_of_measurement: A
    register_type: holding
    value_type: FP32
    accuracy_decimals: 3               # 显示3位小数（如0.123A）
    device_class: current
    state_class: measurement
    web_server:
      sorting_group_id: sensor_group
      sorting_weight: 2

  # -------------------- 有功功率（W） --------------------
  - platform: modbus_controller
    modbus_controller_id: ddsu666_controller
    id: my_p
    name: "有功功率_p"
    address: 0x2004
    register_count: 2
    unit_of_measurement: W
    register_type: holding
    value_type: FP32
    accuracy_decimals: 1
    filters:
      - multiply: 1000                 # 电表返回单位为kW，乘以1000转换为W
      - median:                        # 中值滤波，消除瞬时波动
          window_size: 3               # 取最近3个采样点的中值
          send_every: 3                # 每3个采样点输出一次
    device_class: power
    state_class: measurement
    web_server:
      sorting_group_id: sensor_group
      sorting_weight: 3

  # -------------------- 无功功率（var） --------------------
  - platform: modbus_controller
    modbus_controller_id: ddsu666_controller
    id: my_q
    name: "无功功率_q"
    address: 0x2006
    register_count: 2
    unit_of_measurement: var
    register_type: holding
    value_type: FP32
    accuracy_decimals: 1
    filters:
      - multiply: 1000                 # 转换为var
    device_class: power
    state_class: measurement
    web_server:
      sorting_group_id: sensor_group
      sorting_weight: 4

  # -------------------- 视在功率（VA） --------------------
  - platform: modbus_controller
    modbus_controller_id: ddsu666_controller
    id: my_s
    name: "视在功率_s"
    address: 0x2008
    register_count: 2
    unit_of_measurement: VA
    register_type: holding
    value_type: FP32
    accuracy_decimals: 1
    filters:
      - multiply: 1000                 # 转换为VA
    device_class: power
    state_class: measurement
    web_server:
      sorting_group_id: sensor_group
      sorting_weight: 5

  # -------------------- 功率因数（PF） --------------------
  - platform: modbus_controller
    modbus_controller_id: ddsu666_controller
    id: my_pf
    name: "功率因数_pf"
    address: 0x200A
    register_count: 2
    register_type: holding
    value_type: FP32
    accuracy_decimals: 3               # 显示3位小数（0.001-1.000）
    device_class: power_factor
    state_class: measurement
    web_server:
      sorting_group_id: sensor_group
      sorting_weight: 5

  # -------------------- 频率（Hz） --------------------
  - platform: modbus_controller
    modbus_controller_id: ddsu666_controller
    id: my_freq
    name: "频率_freq"
    address: 0x200E
    register_count: 2
    unit_of_measurement: Hz
    register_type: holding
    value_type: FP32
    accuracy_decimals: 2               # 显示2位小数（如50.00Hz）
    state_class: measurement
    web_server:
      sorting_group_id: sensor_group
      sorting_weight: 6

  # -------------------- 有功总电能（kWh） --------------------
  - platform: modbus_controller
    modbus_controller_id: ddsu666_controller
    id: my_ep
    name: "有功总电能_ep"
    address: 0x4000
    register_count: 2
    unit_of_measurement: kWh
    register_type: holding
    value_type: FP32
    accuracy_decimals: 2               # 显示2位小数（如123.45kWh）
    device_class: energy
    state_class: total_increasing      # 表示总用电量只增不减（适合累积统计）
    filters:
      - median:                        # 中值滤波，减少读数抖动
          window_size: 3
          send_every: 3
      - debounce: 0.1s                 # 防抖动滤波，0.1秒内变化不更新
    web_server:
      sorting_group_id: sensor_group
      sorting_weight: 7
```

#### WIFI刷固件

点击 `安装` -> 点击 `高级选项` -> 确认 `设备IP` 是否正确 -> 点击 `通过网络安装`

![04-usr-esphome-34.png](assets/04-usr-esphome-34.png)

安装成功后，会输出日志，如下图所示

![04-usr-esphome-35.png](assets/04-usr-esphome-35.png)

#### WBE 界面展示

重新访问 `ESP32-C6` 的 `WEB` 界面，[http://192.168.16.32/](http://192.168.16.32/){target=blank}，更新成功会出现 `传感器数据` 信息，如下图

![04-usr-esphome-36.png](assets/04-usr-esphome-36.png)

### 阶段性总结

!!! success "恭喜你"
    至此，你已经掌握了一些常用 `ESPHome` 的 `操作` 和 `配置`：

    - 刷固件的 `3` 种方式（线刷 + OTA更新 + 网络更新）
    - WEB 需要账号密码登录
    - 学会了 `配置密钥`
    - 学会了 `板载LED` 控制
    - 学会了 添加 `备注`
    - 学会了 采集 `电表指标`
    - 学会了 查看 `在线设备信息`

### `Home Assistant` 接入 `ESP32-C6` 的 `ESPHome` （仪表盘 + 能源）

#### HA 中 ESPHome 添加过程

访问 `Home Assistant`，[http://192.168.16.224:8123/](http://192.168.16.224:8123/){target=blank}，如下图所示

![04-usr-esphome-37.png](assets/04-usr-esphome-37.png)

点击 `设置` -> 点击 `设备与服务`

![04-usr-esphome-38.png](assets/04-usr-esphome-38.png)

点击 `添加集成`

![04-usr-esphome-39.png](assets/04-usr-esphome-39.png)

搜索 `ESPHome` -> 点击 `ESPHome`

![04-usr-esphome-40.png](assets/04-usr-esphome-40.png)

填写 `主机` -> 点击 `提交`

![04-usr-esphome-41.png](assets/04-usr-esphome-41.png)

填写 `加密密钥` -> 点击 `提交`

![04-usr-esphome-42.png](assets/04-usr-esphome-42.png)

选择 `区域` -> 点击 `完成`

![04-usr-esphome-43.png](assets/04-usr-esphome-43.png)

#### HA 中 ESPHome 界面展示

![04-usr-esphome-44.png](assets/04-usr-esphome-44.png)

!!! info 
    本教程默认的密钥为 `/w9meS/Uu0rAK/2+5ai2LFI6V6WCCDj4FxSiGyy/RWA=`

    请根据 `密钥` 配置中的参数 `api_encryption_key` 如实填写

#### HA 中 ESPHome 添加到仪表盘

在 `多实体` 卡片配置如下图所示

![04-usr-esphome-45.png](assets/04-usr-esphome-45.png)

在 `代码编辑器` 中，添加指标的 `名称`，方便阅读，代码如下

![04-usr-esphome-46.png](assets/04-usr-esphome-46.png)

内容如下

```yaml
type: entities
entities:
  - entity: sensor.esp32_c6_power_meter_she_bei_lian_jie_de_wifiming_cheng
    name: "设备连接的WiFi名称"
  - entity: sensor.esp32_c6_power_meter_she_bei_ipdi_zhi
    name: "设备IP地址"
  - entity: sensor.esp32_c6_power_meter_she_bei_macdi_zhi
    name: "设备MAC地址"
  - entity: sensor.esp32_c6_power_meter_wifixin_hao_db
    name: "WiFi信号_dB"
  - entity: sensor.esp32_c6_power_meter_wifixin_hao_db
    name: "WiFi信号_%"
  - entity: light.esp32_c6_power_meter_ban_zai_led
    name: "板载LED"
  - entity: sensor.esp32_c6_power_meter_she_bei_bei_zhu
    name: "设备备注"
  - entity: sensor.esp32_c6_power_meter_dian_ya_u
    name: "电压_u"
  - entity: sensor.esp32_c6_power_meter_dian_liu_i
    name: "电流_i"
  - entity: sensor.esp32_c6_power_meter_you_gong_gong_lu_p
    name: "有功功率_p"
  - entity: sensor.esp32_c6_power_meter_wu_gong_gong_lu_q
    name: "无功功率_q"
  - entity: sensor.esp32_c6_power_meter_gong_lu_yin_shu_pf
    name: "功率因数_pf"
  - entity: sensor.esp32_c6_power_meter_shi_zai_gong_lu_s
    name: "视在功率_s"
  - entity: sensor.esp32_c6_power_meter_pin_lu_freq
    name: "频率_freq"
  - entity: sensor.esp32_c6_power_meter_you_gong_zong_dian_neng_ep
    name: "有功总电能_ep"
```

!!! info
    上述操作在 `仪表盘` 中完成，只展示关键操作过程

#### HA 中 仪表盘 最终效果展示

![04-usr-esphome-47.png](assets/04-usr-esphome-47.png)

#### HA 中 ESPHome 接入能源

点击 `能源` -> 点击 `编辑`

![04-usr-esphome-48.png](assets/04-usr-esphome-48.png)

点击 `添加电网连接`

![04-usr-esphome-49.png](assets/04-usr-esphome-49.png)

选择 `电网输入的能源` -> `成本跟踪` 选择 `使用固定价格` -> `价格` 填写 `0.63`，如下图所示

![04-usr-esphome-50.png](assets/04-usr-esphome-50.png)

#### HA 中 能源 最终效果展示

![04-usr-esphome-51.png](assets/04-usr-esphome-51.png)

### 相同功能迁移到 `ESP8266`

#### `ESP8266` 推荐实验连线

![04-usr-esphome-52.png](assets/04-usr-esphome-52.png)

#### ESPHome 配置

点击 `创建设备` -> 选择 `高级设置选项` -> 选择 `空配置` -> 填写设备名称 `esp8266-power-meter` -> 点击 `完成设置`

编辑 `esp8266-power-meter`，如下图红色区域

![04-usr-esphome-53.png](assets/04-usr-esphome-53.png)

配置内容如下

```yaml
# ===================================================================
# ESPHome 配置文件 - ESP8266 电力监测仪
# ===================================================================
# 硬件平台: ESP8266 (ESP-01 1MB)
# 主要功能: 电力参数采集 + Web仪表板 + Home Assistant集成
# 维护人员: [请填写你的名字]
# 最后更新: 2026-07-22
# ===================================================================

# ===================================================================
# 1. ESPHome 核心配置
# ===================================================================
esphome:
  name: "esp8266-power-meter"           # 设备唯一标识名（修改后需重新烧录）

# ===================================================================
# 2. 硬件平台配置（ESP8266）
# ===================================================================
esp8266:
  board: esp01_1m                       # 开发板型号（ESP-01 1MB版本）
  restore_from_flash: true              # 断电后从闪存恢复状态

# ===================================================================
# 3. 日志系统
# ===================================================================
# 通过串口输出调试日志，波特率默认115200
logger:
  level: DEBUG                         # 日志级别: DEBUG/INFO/WARN/ERROR/NONE

# ===================================================================
# 4. Web Server（提供网页管理界面）
# ===================================================================
web_server:
  version: 3                            # 版本3支持分组排序、认证、OTA等高级特性
  port: 80                              # HTTP端口（可改为8080等其他端口）
  auth:                                 # 基础认证
    type: basic
    username: !secret web_login_username
    password: !secret web_login_password

  # 分组定义：对Web界面中的传感器进行逻辑分组
  # 权重越小排序越靠前
  sorting_groups:
    - id: system_group
      name: "系统参数"
      sorting_weight: 10                # 排在第一组
    - id: sensor_group
      name: "传感器数据"
      sorting_weight: 20                # 排在第二组

# ===================================================================
# 5. 网络配置
# ===================================================================
wifi:
  ssid: !secret wifi_ssid
  password: !secret wifi_password

  # 备用热点模式：WiFi连接失败时自动开启
  ap:
    ssid: "ESP8266 Power Meter"         # 热点名称
    password: !secret wifi_ap_password
    ap_timeout: 5min                    # 超时后自动重连WiFi

captive_portal:                         # 强制门户认证，连接AP后自动弹出配网页面

# ===================================================================
# 6. OTA 固件升级
# ===================================================================
ota:
  - platform: esphome                   # ESPHome原生OTA方式
    password: !secret ota_password
    port: 3232

  - platform: web_server                # Web界面OTA（通过浏览器上传固件）

# ===================================================================
# 7. Home Assistant API
# ===================================================================
api:
  encryption:
    key: !secret api_encryption_key     # 32字节密钥，通过 esphome generate_key 生成
  reboot_timeout: 15min                 # HA连接超时后自动重启

# ===================================================================
# 8. 开关配置
# ===================================================================
switch:
  - platform: gpio
    name: "板载 LED"
    pin:
      number: GPIO2
      inverted: true                    # 低电平点亮（ESP8266板载LED通常为低电平有效）
    id: led_switch
    web_server:
      sorting_group_id: system_group
      sorting_weight: 7

# ===================================================================
# 9. 文本传感器（静态信息展示）
# ===================================================================
text_sensor:
  # ---------- WiFi 网络信息 ----------
  - platform: wifi_info
    ssid:
      name: "设备连接的WiFi名称"
      web_server:
        sorting_group_id: system_group
        sorting_weight: 2
    ip_address:
      name: "设备IP地址"
      web_server:
        sorting_group_id: system_group
        sorting_weight: 3
    mac_address:
      name: "设备MAC地址"
      web_server:
        sorting_group_id: system_group
        sorting_weight: 4

  # ---------- 硬件接线备注 ----------
  - platform: template
    name: "设备备注"
    update_interval: 86400s             # 24小时更新一次（静态信息无需频繁更新）
    lambda: |-
      return {
        "RS485模块的TX接ESP8266的GPIO4，RX接ESP8266的GPIO5"
      };
    web_server:
      sorting_group_id: system_group
      sorting_weight: 8

# ===================================================================
# 10. UART 串口配置（连接RS485模块）
# ===================================================================
uart:
  id: uart_modbus
  tx_pin: GPIO4                         # RS485模块的TX接ESP8266的GPIO4
  rx_pin: GPIO5                         # RS485模块的RX接ESP8266的GPIO5
  baud_rate: 9600                       # 波特率（需与电表设置一致）
  data_bits: 8
  stop_bits: 1
  parity: NONE

# ===================================================================
# 11. Modbus 配置
# ===================================================================
modbus:
  id: modbus_host
  uart_id: uart_modbus
  # 如需硬件流控，取消注释并配置对应引脚
  # flow_control_pin: GPIO4

modbus_controller:
  - id: ddsu666_controller
    modbus_id: modbus_host
    address: 0x01                       # 电表Modbus地址（1-247）
    update_interval: 10s                # 数据更新间隔

# ===================================================================
# 12. 数值传感器（采集电力数据）
# ===================================================================
sensor:
  # -------------------- WiFi信号强度（dBm） --------------------
  - platform: wifi_signal
    name: "WiFi信号_dB"
    id: xinhao1                         # 源传感器，供下方copy使用
    update_interval: 10s
    entity_category: "diagnostic"
    web_server:
      sorting_group_id: system_group
      sorting_weight: 5

  # -------------------- WiFi信号强度（百分比） --------------------
  - platform: copy
    source_id: xinhao1
    id: wifi_signal_percent
    name: "WiFi信号_%"
    filters:
      - lambda: return min(max(2 * (x + 100.0), 0.0), 100.0);  # -100~-50dBm → 0~100%
    unit_of_measurement: "%"
    entity_category: "diagnostic"
    device_class: ""
    web_server:
      sorting_group_id: system_group
      sorting_weight: 6

  # -------------------- 电压（V） --------------------
  - platform: modbus_controller
    modbus_controller_id: ddsu666_controller
    id: my_u
    name: "电压_u"
    address: 0x2000                     # 寄存器起始地址
    register_count: 2                   # 32位数据需2个寄存器
    unit_of_measurement: V
    register_type: holding
    value_type: FP32
    accuracy_decimals: 1
    device_class: voltage
    state_class: measurement
    web_server:
      sorting_group_id: sensor_group
      sorting_weight: 1

  # -------------------- 电流（A） --------------------
  - platform: modbus_controller
    modbus_controller_id: ddsu666_controller
    id: my_i
    name: "电流_i"
    address: 0x2002
    register_count: 2
    unit_of_measurement: A
    register_type: holding
    value_type: FP32
    accuracy_decimals: 3
    device_class: current
    state_class: measurement
    web_server:
      sorting_group_id: sensor_group
      sorting_weight: 2

  # -------------------- 有功功率（W） --------------------
  - platform: modbus_controller
    modbus_controller_id: ddsu666_controller
    id: my_p
    name: "有功功率_p"
    address: 0x2004
    register_count: 2
    unit_of_measurement: W
    register_type: holding
    value_type: FP32
    accuracy_decimals: 1
    filters:
      - multiply: 1000                  # 电表返回kW → 转换为W
      - median:                         # 中值滤波，消除瞬时波动
          window_size: 3
          send_every: 3
    device_class: power
    state_class: measurement
    web_server:
      sorting_group_id: sensor_group
      sorting_weight: 3

  # -------------------- 无功功率（var） --------------------
  - platform: modbus_controller
    modbus_controller_id: ddsu666_controller
    id: my_q
    name: "无功功率_q"
    address: 0x2006
    register_count: 2
    unit_of_measurement: var
    register_type: holding
    value_type: FP32
    accuracy_decimals: 1
    filters:
      - multiply: 1000                  # 转换为var
    device_class: power
    state_class: measurement
    web_server:
      sorting_group_id: sensor_group
      sorting_weight: 4

  # -------------------- 视在功率（VA） --------------------
  - platform: modbus_controller
    modbus_controller_id: ddsu666_controller
    id: my_s
    name: "视在功率_s"
    address: 0x2008
    register_count: 2
    unit_of_measurement: VA
    register_type: holding
    value_type: FP32
    accuracy_decimals: 1
    filters:
      - multiply: 1000                  # 转换为VA
    device_class: power
    state_class: measurement
    web_server:
      sorting_group_id: sensor_group
      sorting_weight: 5

  # -------------------- 功率因数（PF） --------------------
  - platform: modbus_controller
    modbus_controller_id: ddsu666_controller
    id: my_pf
    name: "功率因数_pf"
    address: 0x200A
    register_count: 2
    register_type: holding
    value_type: FP32
    accuracy_decimals: 3
    device_class: power_factor
    state_class: measurement
    web_server:
      sorting_group_id: sensor_group
      sorting_weight: 5

  # -------------------- 频率（Hz） --------------------
  - platform: modbus_controller
    modbus_controller_id: ddsu666_controller
    id: my_freq
    name: "频率_freq"
    address: 0x200E
    register_count: 2
    unit_of_measurement: Hz
    register_type: holding
    value_type: FP32
    accuracy_decimals: 2
    state_class: measurement
    web_server:
      sorting_group_id: sensor_group
      sorting_weight: 6

  # -------------------- 有功总电能（kWh） --------------------
  - platform: modbus_controller
    modbus_controller_id: ddsu666_controller
    id: my_ep
    name: "有功总电能_ep"
    address: 0x4000
    register_count: 2
    unit_of_measurement: kWh
    register_type: holding
    value_type: FP32
    accuracy_decimals: 2
    device_class: energy
    state_class: total_increasing       # 累积电量只增不减
    filters:
      - median:
          window_size: 3
          send_every: 3
      - debounce: 0.1s                  # 防抖动滤波
    web_server:
      sorting_group_id: sensor_group
      sorting_weight: 7
```

#### 下载固件 和 线刷固件

操作过程略

#### WEB 界面展示

![04-usr-esphome-54.png](assets/04-usr-esphome-54.png)

!!! info "参考链接"
    1. [Getting Started with the ESPHome Command Line](https://esphome.io/guides/getting_started_command_line/#esphome-device-builder-docker){target=blank}
    2. [ESP8266 Platform](https://esphome.io/components/esp8266/){target=blank}