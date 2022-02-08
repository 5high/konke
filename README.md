
修改原插件为新版本Home Assistant支持，目前测试支持0.103版本。

[Home Assistant](https://www.home-assistant.io/) component of [Konke](http://www.ikonke.com/) devices

# Supported Devices

- Mini K

![Mini K](https://p5.ssl.qhimg.com/dm/300_300_/t01763cbb0d461968a5.png)
- Mini Pro

![Mini Pro](https://p2.ssl.qhimg.com/dm/300_300_/t01da45d0484178dfab.jpg)
- Smart Plug K(untested)

![K](https://p1.ssl.qhimg.com/dm/300_300_/t016c9c239d8d71fb78.jpg)
- K2 Pro(untested)

![K2](https://p1.ssl.qhimg.com/dm/300_300_/t019a7103eb99573480.jpg)

- (cnct) intelliPLUG (Mini K us version, untested)

![intelliPLUG](https://p5.ssl.qhimg.com/dm/300_300_/t0166baaec86aa83a4b.jpg)

# Install
copy the `custom_components` to your home-assistant config directory.

# config
Add the following to your configuration.yaml file:
```yaml
switch:
  - platform: konke
    name: switch_1
    host: 192.168.0.101
  - platform: konke
    name: switch_2
    host: 192.168.0.102
```

CONFIGURATION VARIABLES:
```
- name
  (string)(Optional)The display name of the device

- host
  (string)(Required)The host/IP address of the device.
```  
遥控使用方法：
控客的遥控器是不支持直接输入遥控编码的，只能通过学习添加遥控器。

- 添加遥控：
进入service界面，选择remote.koneke_ir_learn_command或remote.koneke_rf_learn_command。
{
  "entity_id": 【设备的entity_id】,
  "slot": 【命令id，取值范围1000-99999】,
  "timeout": 【超时时长，默认10s】
}
注意solt参数是int格式，周围不能带引号，timeout参数不带单位，否则命令会不生效。
学习后会在主界面显示通知信息，提示学习的成功或者失败。

- 使用遥控：
调用remote.send_command这个service，data:
{
  "entity_id": 【设备的entity_id】,
  "command": 【遥控类型ir或rf】_【命令id，取值范围1000-99999】,
  "num_repeats": 【发送次数，默认1】,
  "delay_secs": 【两次发射间的延时，默认0.4s】
}

使用示例：

configuration.yaml中添加：
```
remote:
  - platform: konke
    name: k_ir_remote
    model: minik
    host: 192.168.2.162
    hidden: false
    type: ir

学习遥控：

service: remote.koneke_ir_learn
data:
  entity_id: remote.k_ir_remote
  slot: 1001
  timeout: 10

使用遥控：

service: remote.send_command
data:
  entity_id: remote.k_ir_remote
  command: ir_1001
  num_repeats: 1
  delay_secs: 0.4
```
