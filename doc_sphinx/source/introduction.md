# 基础概念介绍
Wio平台是一个基于WiFi的IOT/WOT开发平台。它把大量的Grove(TM)系列的传感器和执行器变成了应用开发友好的APIs。它创造性的实现了动态的添加Grove模块和在线编译以及在线升级功能。

Wio v2版本在第一版基础上做了强力升级。包括更友好的Grove或兼容Grove接口的模块驱动开发。更加方便的用户固件编写和测试。实现了更稳定的设备连接和可独立增加的节点部署，适合商业产品级开发。v2版本也包含很多有用的改进，API更多遵循RESTful风格，支持用户固件的分享和Github导入。

## 用户管理，为什么用SSO
在v1版本中，我们做了一个简单email/password登录系统。但是想集成Seeed原有的账户系统时，遇到了困难，需要很多额外的工作才能把他们集成进来。我们想到了Oauth 2.0(第三方认证，如Facebook)和SSO(Sign Sign On)。

我们更希望Wio是一个隐藏在你的IOT应用后面的基础服务。因此SSO更合适，它只需要你的IOT应用登陆一次，Wio服务被验证。而用Oauth，需要验证两次。

当前默认支持Firebase，Stormpath的SSO系统，可以在配置设置里修改。当然也可以写相应的代码支持自己SSO系统，使用公司已有的账户系统。

## Firmware
在Wio系统里，它被定义为一个Wio固件项目，而不是单纯的可执行固件文件。它包括固件的名字，描述和重要的Grove模组配置信息，ULB(用户逻辑文件)。

### Firmware的Grove配置字段
例子：
```
"config": {
    "board_name": "Wio Link v1.0",
    "connections": [
      {
        "sku": "101020019-ffff",
        "port": "D0"
      }
    ]
    }
```
`board_name`代表Thing的类型，`connections`代表连接的Grove模块的列表。`sku`代表Grove模块的编号。`port`代表连接的端口。

### Firmware的ULB
ULB主要提供一种途径让用户在Grove驱动之上定义更多的APIs。它可以调用基础的Wio库或者多个Grove驱动函数，封装成更有用的APIs。这些APIs会集成到固件提供的API列表中。

### Firmware的编译
```
/v2/firmwares/{firmware_id}/bin
```
调用此API可以把固件项目在线编译为可执行文件。

## Thing
可以理解为物联网节点，终端。在Wio系统里，它被定为一个运行Wio固件，驱动Grove模块和连接Wio服务器的物联网终端设备。

### Thing的注册
当用Wio API创建了Thing，但并不代表实际的终端设备即可工作。这里需要额外的注册操作。首先，把Thing的ID，key和WI-FI的SSID和密码通过UDP工具写入终端设备；稍等一会，待设备连上Wio服务器；然后，调用`/v2/things/{thing_id}/verify-registered`检测是否设备是否注册成功。

### Thing的升级
当前Thing支持立即升级和定时升级。
```
/v2/things/{thing_id}/ota
{
  "firmware_id": "ad144ec3505641e29555e7de25aeaa08",
  "ota_at": "2016-12-28T12:05:05.000Z"
}
```
`firmware_id`代表固件项目ID，系统会自动编译固件，并立即或定时通过OTA给Thing升级。
`ota_at`，ISO8601格式时间戳。当定义时，代表定时升级，如果没有定义，代表立即升级。

### Thing的API列表
```
/v2/things/{thing_id}/page
```
通过HTML页面的方式展示API列表，并有javascript功能，支持在线测试。
