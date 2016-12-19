## Thing
## Firmware
## ULB
## OTA
## USER 其实是隐藏了起来的

## 一个简单流程
1. 通过SSO注册和登陆用户，获取用户token和ID
2. 通过token和ID创建Thing
3. 烧写Thing ID和Thing key到设备上，并校验激活
3. 通过token和ID创建Fireware
4. 增加ULB逻辑（可选）
5. 编译OTA固件（可选）
6. 把Firmware OTA到thing，会自动编译固件
7. 查询Thing的可用APIs
8. 通过Github分享Fireware项目
9.

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
<!-- ## 获取用户ID
```
curl -X GET  "https://wio.forkthings.com/v2/user?access_token=0705e3546b1132f7a5184f3911a322cb"
```
返回
```
{
  "user_id": "210181",
  "created_at": "2016-11-25T06:08:18Z"
}
``` -->
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

发送校验API
