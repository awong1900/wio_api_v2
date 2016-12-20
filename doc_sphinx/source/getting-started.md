# 完全通过RESTFul APIs使用Wio v2
本指导仅使用Wio v2 APIs和UDP烧写工具完成Thing的创建，注册，升级。
本例子Wio服务器地址：https://wio.forkthings.com

## 通过SSO注册和登陆用户，获取用户token
在本例中，我们使用的测试服务器是用的是Seeed用户系统，支持SSO。登陆https://wio.seeed.io/login 。输入Seeed账户密码或者第三方验证，获取access_token.
```
=====For Wio v1=====
token: FW1IHmsdGGYUFWhr

=====For Wio v2 (test)=====
sso token: 0705e3546b1132f7a5184f3911a322cb
```
使用sso token。

## 通过token创建Thing
```
curl -X POST -H "Authorization: token 0705e3546b1132f7a5184f3911a322cb" -H "Content-Type: application/json" -d '{
    "board": "Wio Link v1.0"
}' "https://wio.forkthings.com/v2/things"
```
返回
```
{
  "thing_id": "cdf220ab8e9b49b2b23f9449a58cf239",
  "user_id": "210181",
  "firmware_id": null,
  "key": "8a04bd9b02144598877cbdff90e36856",
  "name": "",
  "board": "Wio Link v1.0",
  "registered": false
  "online": false,
  "pp_id": null,
  "page_url": "https://wio.forkthings.com/v2/things/cdf220ab8e9b49b2b23f9449a58cf239/page",
  "created_at": "2016-12-19T09:34:32Z",
  "updated_at": "2016-12-19T09:34:32Z"
}
```
## 烧写Thing ID和Thing key到设备上，并校验激活
下载安装wio-cli工具
    $pip install wio-cli
执行
    $wio upd --send "APCFG: YOUR_WIFI_SSID\tYOUR_WIFI_SSID\t8a04bd9b02144598877cbdff90e36856\tcdf220ab8e9b49b2b23f9449a58cf239\thttps://wio.forkthings.com\thttps://wio.forkthings.com\t\r\n"
返回
ok

等待一会儿，检查Wio板子上的指示灯，当一闪一闪时，代表已经连上服务器。
   
发送校验注册API
> TODO, swagger api + source
当返回`204`代表注册成功。

## 创建Fireware
```
curl -X POST -H "Authorization: token af0cc2340935b6d98c46a247f8facb0c" -H "Content-Type: application/json" -d '{}' "https://wio.forkthings.com/v2/firmwares"
```
返回
```
{
  "firmware_id": "a35c8f86689d4da69df5ebb63a289292",
  "user_id": "210181",
  "name": null,
  "desc": null,
  "firmware_type": "flexible",
  "fixed_url": null,
  "bin_url": null,
  "size": null,
  "sha": null,
  "config": {
    "board_name": "Wio Link v1.0",
    "connections": []
  },
  "created_at": "2016-12-20T06:07:58Z",
  "updated_at": "2016-12-20T06:07:58Z"
}
```
## 修改Firmware配置(可选)
这里修改它的名字和描述，以及Grove的连接配置
```
curl -X PATCH -H "Authorization: token af0cc2340935b6d98c46a247f8facb0c" -H "Content-Type: application/json" -d '{
  "name": "Hello-World",
  "desc": "first firmware with config",
  "config": {
  	"board_name": "Wio Link v1.0",
  	"connections": [
  	  {"sku": "101020019-ffff", "port": "D0"}]
  }
}' "https://wio.forkthings.com/v2/firmwares/a35c8f86689d4da69df5ebb63a289292"
```
返回
```
{
  "firmware_id": "a35c8f86689d4da69df5ebb63a289292",
  "user_id": "210181",
  "name": "Hello-World",
  "desc": "first firmware with config",
  "firmware_type": "flexible",
  "fixed_url": null,
  "bin_url": null,
  "size": null,
  "sha": null,
  "config": {
    "board_name": "Wio Link v1.0",
    "connections": [
      {
        "sku": "101020019-ffff",
        "port": "D0"
      }
    ]
  },
  "created_at": "2016-12-20T06:07:58Z",
  "updated_at": "2016-12-20T06:15:01Z"
}
```
到这里一个简单的固件项目已经创建完毕，下一节展示增加自己的APIs到固件项目。

### 增加ULB逻辑(可选)
```
curl -X PUT -H "Authorization: token af0cc2340935b6d98c46a247f8facb0c" -H "Content-Type: text/plain" -H "Cache-Control: no-cache" -H "Postman-Token: 0f5c77b2-5fdb-abde-4e20-20ed2d1e6086" -d '#include "wio.h"
#include "suli2.h"
#include "Main.h"

void setup()
{
}

void loop()
{

}
' "https://wio.forkthings.com/v2/firmwares/a35c8f86689d4da69df5ebb63a289292/ulbs/test/m.cpp"
```
TODO, 需要一个完整的增加API的例子
返回
```
{
  "firmware_id": "a35c8f86689d4da69df5ebb63a289292",
  "path": "test/m.cpp",
  "name": "m.cpp",
  "content": "#include \"wio.h\"\r\n#include \"suli2.h\"\r\n#include \"Main.h\"\r\n\r\nvoid setup()\r\n{\r\n}\r\n\r\nvoid loop()\r\n{\r\n\r\n}\r\n",
  "encoding": "plain",
  "size": 102,
  "sha": "33d97c3c654b9a0252c97859d102b47f8c882bc9",
  "created_at": "2016-12-20T06:20:05Z",
  "updated_at": "2016-12-20T06:20:05Z"
}
```

### 更新Thing的固件
Firmware项目创建后，并不需要立即生成实际的可执行文件。通过Thing的OTA API可以把Firmware固件写入Thing，后台会自动生成实际的可执行文件。
```
curl -X POST -H "Authorization: token af0cc2340935b6d98c46a247f8facb0c" -H "Content-Type: application/json" -d '{
  "firmware_id": "a35c8f86689d4da69df5ebb63a289292"
}' "https://wio.forkthings.com/v2/things/cdf220ab8e9b49b2b23f9449a58cf239/ota"
```
返回
```
{
  "thing_id": "cdf220ab8e9b49b2b23f9449a58cf239",
  "firmware_id": "a35c8f86689d4da69df5ebb63a289292",
  "ota_at": "2016-12-20T06:24:55Z",
  "status": "queue",
  "status_text": "in Queue, high priority",
  "updated_at": "2016-12-20T06:24:55Z"
}
```
OTA任务已经启动，等待后续完成。

通过状态API，可以查询OTA的过程。
```
curl -X GET -H "Authorization: token af0cc2340935b6d98c46a247f8facb0c" "https://wio.forkthings.com/v2/things/cdf220ab8e9b49b2b23f9449a58cf239/ota"
```

## 查询Thing的可用APIs
```
curl -X GET -H "Authorization: token af0cc2340935b6d98c46a247f8facb0c" "https://wio.forkthings.com/v2/things/cdf220ab8e9b49b2b23f9449a58cf239/ota"
```
返回一个网页，通过列表方式展示可以使用的Grove Web APIs.

完
